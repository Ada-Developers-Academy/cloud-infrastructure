# Scaling Databases

## Learning Goals

- Describe the differences between vertical and horizontal scaling as it relates to databases, including limits for common types of databases.
- Describe patterns like read replicas and full replication that can be used to keep data available and consistent in a multi-region architecture
- Generally describe strong vs eventual consistency models in distributed databases 

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |

Key terms: replication, sharding

## What does it mean to Scale a Database? 

Scaling databases is how we ensure that as our data and customer base grows, our database remains accessible and performant. We've talked about scaling resources, as well as vertical & horizontal scaling in other contexts, and databases follow similar patterns. 

We can think of the issue of scaling a database like a busy restaurant. Most of the day, service is pretty quick, we have plenty of servers and cooks for the number of occupied tables. As the dinner rush approaches and the restaurant gets full, service slows down: we have a fixed number of cook tops in the kitchen and food can only cook so quickly. If the restaurant gets busy enough, customers may face long wait times or be turned away since there are no open tables. 

Databases face a similar issue: if our service gets too busy for the network resources we've provided, we aren't able to serve each request in a timely manner, and connections time out or are refused. Another possible issue is that, as our data grows, we run out of physical storage we've provisioned for the database and can't store new information.   

In both cases, the result is poor performance that impacts our customers' experience or prevents them from accessing our service entirely. Let's explore how we can alleviate these issues with scaling strategies.

## Vertical Scaling

We saw in the compute lessons that **vertical scaling** is the practice of adding more resources to an existing instance. In the context of databases, that means connecting more or larger storage volumes, or adding RAM to an existing instance running the database. In our restaurant example, this would be like adding a physical expansion to the existing restaurant's kitchen so we have more places to cook, and hiring more staff to this location to run it. 

In both cases, this can work to alleviate pressure for a time, but there are hard limits on vertical scaling. At some point we will not be able to connect new storage volumes or additional RAM, and we will have hit the ceiling on vertical scaling for our database. Our restaurant faces similar issues, the physical real estate that the restaurant resides on is a fixed size; we can only expand the kitchen a certain amount before we reach the limitiations of the space.  

### Vertical Scaling Strengths & Limitations

**Strengths**

- *Avoids complexity*: No code changes, no rethinking how our data is organized. Vertical scaling keeps things simple by adding resources to the existing infrastructure, no extra data orchestration is required.
- *Helps most with writes*: Without horizontal scaling strategies that introduce complexity (we'll talk more about these shortly!) all write operations typically go to one primary server. If our database is getting overwhelmed by incoming writes, adding more database servers (horizontal scaling) doesn't directly help, since . Upgrading the server that handles writes is the right move.

**Limitations**

- *Scaling limits*: Cloud providers only offer database instances up to a certain size. Once we hit the largest available option, we've exhausted vertical scaling as a solution.
- *Cost vs performance*: Larger instance tiers cost significantly more, and we don't always get proportionally better performance for the price.
- *Hard to scale back down*: Maybe we need more resources during a holiday surge twice a year, and don't need maximum vertical capacity the rest of the year.  Downsizing a production database requires careful planning and usually some downtime. Teams often stay on oversized instances longer than needed to avoid the hassle, which adds ongoing costs.

Vertical scaling is universally applicable to all database types, but it isn't always the right fit for our application's needs. The table below summarizes some key considerations for vertically scaling the database types we've talked about previously.

| Database Type | Vertical Scaling Fit | Why | Key Constraint |
|---|---|---|---|
| Relational | ✅ Strong | A single node handles all reads, writes, and JOINs; adding CPU and RAM directly improves query throughput and concurrency without architectural changes | All workloads still flow through one node; hardware has a ceiling, and scaling up requires downtime or a brief failover on most managed services |
| Document | ✅ Strong | A larger instance improves throughput for reads and writes without requiring changes to how documents are distributed | Same hardware ceiling as relational; vertical scaling alone won't sustain truly large-scale workloads |
| Key-Value | ⚠️ Moderate | Works and is straightforward to apply, but key-value databases are purpose-built for horizontal scaling; teams rarely reach for vertical scaling first | Hardware ceiling is hit sooner relative to need since these workloads tend to be very high volume |
| In-Memory | ⚠️ Moderate | Adding RAM directly increases the dataset size the database can hold, which is the primary constraint for in-memory databases | RAM is expensive and has a per-instance ceiling; vertical scaling is the main lever but cost escalates quickly |
| Graph | ⚠️ Moderate | More CPU and RAM improves traversal performance and allows larger graphs to be held in memory | Relationship traversal complexity grows with graph size regardless of instance size; vertical scaling buys time but doesn't solve deep scalability challenges |

### Self-Hosted vs. Managed Considerations

Whether our database is self-hosted on a cloud VM or running as a managed service changes how much work vertical scaling requires from our team.

With a managed service, scaling up is built into the platform. We pick a larger tier, and the provider handles the transition. If our database is set up for high availability across multiple availability zones, the upgrade usually happens with less than a minute of interruption.

With a self-hosted database on a cloud VM, vertical scaling means resizing the VM itself. That typically requires stopping the instance, switching to a larger machine type, and restarting. This switchover generally means planned downtime. Even if we set up a new, larger instance first, divert traffic to the new set up, then decommision the smaller instance, there will likely be at least a short service interruption while traffic is diverted to the new instance. When the database comes back up, our team is responsible for confirming everything started correctly: that the database process is healthy, connections are re-established, and performance looks as expected. There's no automated safety net unless our team built one.

The upside of self-hosting is that we have full control over how the database engine is configured for the new hardware. Managed services don't always expose settings like how much memory the database is allowed to use, or how it handles disk I/O. But in exchange, we own every step of the process and everything that can go wrong with it.

## Horizontal Scaling

When we first encountered horizontal scaling in compute, we touched on it at a high level: horizontal scaling is a set of strategies for *scaling out* across multiple nodes or instances rather than scaling up a single instance. 

In our restaurant example, vertical scaling involved increasing our existing kitchen size, and we faced the limit that there is a ceiling to how much we can build in one location. Horizontal scaling for our restaurant might look like opening a new location, which comes with some new logistics to consider and infrastructure to build around deliveries and staffing for multiple locations. However, once that new system is set up, we can open new restaurants and get them running quickly if we start to see service degrade at the existing locations due to high customer load.

Just as a restaurant faces some logistical challenges in the example above, horizontal scaling for databases presents new challenges compared to vertical scaling. A database holds state, and that state has to be correct. When we add database nodes, we immediately face questions like: if a user writes data to one node, can another node read it immediately? Which node accepts writes? What happens if two nodes temporarily disagree about the value of a record? We will not go in-depth on how data conflicts are resolved (feel free to follow your curiosity!) but we will cover the main horizontal scaling strategies that engineers use to answer these questions while distributing load across nodes.

There are two strategies that we'll discuss, *replication* which is aimed at increasing read speed and accessibility, and *sharding* which can help increase write throughput.

## Horizontal Scaling Methods

### Improving Reads: Replication

In most production applications, reads vastly outnumber writes. For example, in e-commerce situations, users browse products far more than they place orders. A read replica is a synchronized copy of the primary database that handles read-only queries. Writes still go to the primary instance, which then propagates changes to replicas asynchronously.

The catch is that "asynchronously" is doing a lot of work in that sentence. Because replicas are updated shortly after writes happen rather than at the exact same moment, a user might write data and immediately query a replica that hasn't yet received the update. For many use cases, like viewing a social feed or browsing a product catalog, this slight staleness is completely acceptable. For other applications, like checking an account balance or confirming that an order was placed, stale information is unacceptable. Understanding which queries can be routed to replicas vs. which must always hit the primary is an application design decision, not just an infrastructure one.

Read replicas are horizontal scaling in a targeted form: we're not distributing all database access, we're only distributing the read load that makes up the majority of database traffic. If a single database instance is struggling under read pressure, adding read replicas is often the first scaling lever teams reach for.

**Multi-Region Replication: Scaling for Geography**

Beyond throughput, scaling can also mean serving users in multiple geographic regions with low latency. Multi-region replication extends the replica concept globally: database nodes are deployed in different regions, and users are routed to the nearest one. Some configurations limit all writes to a single primary region; others support multi-primary writes with conflict resolution logic. This pattern improves availability, since a regional outage doesn't take down the whole system, and reduces read latency for globally distributed users. 

### Improving Writes: Sharding

Read replicas help with read throughput but don't address write throughput or dataset size, since all writes still go through one primary node. When a system has outgrown what a single node can handle for writes, the answer is **sharding**: partitioning the data itself across multiple independent database instances, called shards.

In a sharded system, each shard owns a defined slice of the data. A simple example: an e-commerce platform might shard its orders table by customer region, so orders from North American customers live on one shard and orders from European customers live on another. Each shard handles reads and writes for its own slice independently. The scaling benefit can be immense, we've now distributed both reads and writes across nodes. But sharding introduces complexity that touches our entire stack. 

In order to shard a database we need to decide on a **shard key**, sometimes called a **partition key**, which is the attribute we use to decide which shard a record belongs to. This choice has significant performance consequences. A well-chosen shard key distributes data and traffic evenly. When data and traffic are not evenly distrubuted, it creates a "hot shard" where one node handles most of the traffic while others sit mostly idle, defeating the purpose of sharding. 

Feel free to expand the following content for a brief scenario that shows how a hot shard could occur. 

<br>

<details>
<summary>Hot Shard Scenario</summary>

Consider a scenario where a team is building a social media platform. They choose to shard their posts table by the first letter of the username, figuring that with 26 letters, we'll have 26 shards, and data will be distributed across all of them.

On launch day, the team finds out that the platform's three most-followed creators all have usernames starting with "S," and together they account for 40% of all post reads and writes on the platform. Every time any of their millions of followers loads a feed, that traffic hits the "S" shard. Meanwhile, the "X" and "Q" shards are nearly idle.

The overloaded "S" shard becomes a bottleneck: query times climb, writes start queuing, and users following those top creators experience slowdowns, even though the rest of the platform is fine. The team is stuck managing degraded performance until they can complete a migration to a better suited shard key.

A more even distribution could have been achieved by sharding on something like a hashed user ID instead. A hash function could spread records pseudorandomly across shards regardless of what the usernames look like, making accidental hot shards much less likely.

</details>

If we do create a hot shard we can fix it, but **resharding**, the process of redistributing data across a different number or configuration of shards, is a painful and risky operation for a number of reasons. Feel free to expand the dropdown below if you're interested in some of the specific issues that can make resharding a database difficult.

<br>

<details>
<summary>Resharding Concerns</summary>

1. It involves moving a lot of data. 
   Resharding means redistributing records across a new shard layout. Records that lived on one node have to be copied to different nodes. For a large production database, that can mean moving terabytes of data across the network, which takes a long time, and depending on where and how it is moving, could incur data transit fees.

2. The database can't go offline while it happens. 
   In most production systems we can't just take the database down, reshard, and bring it back up; users and services depend on it continuously. So the migration has to happen while the database is still accepting reads and writes, which means the data we're trying to move keeps changing underneath us. We have to account for new writes that arrive mid-migration and make sure nothing gets lost or duplicated in the process.

3. There's a window where the routing logic is in flux. 
   Our application needs to know which shard to query for a given record. During a reshard, that mapping is changing. If our application sends a query to the old shard location before the routing update has propagated, it either gets stale data or a miss entirely. Coordinating the cutover from old routing to new routing without dropping requests is tricky.

4. Validating correctness is hard. 
   Once the migration is done, how do we confirm every record ended up in the right place and nothing was corrupted or lost? At scale, that verification process is itself a significant effort.

</details>

We also need to consider other kinds of complexity like cross-shard queries. Any query that needs data from multiple shards usually can't be handled entirely by the database. Our application may have to query each relevant shard and assemble the results, adding latency and application complexity.

### Horizontal Scaling Strengths & Limitations

**Strengths**

- Read throughput can scale nearly linearly by adding read replicas
- Write throughput and dataset size can grow beyond single-node limits via sharding
- Multiple nodes eliminate single points of failure — a node failure doesn't bring down the system
- Global replication reduces latency for geographically distributed users

**Limitations**

- Sharding adds significant operational and application complexity
- Asynchronous replication means replicas may serve briefly stale data
- Poor shard key design causes hot partitions that undermine scaling
- Cross-shard queries require application-layer coordination and are slower
- Resharding a live system is expensive and risky

Horizontal scaling is harder to apply to some types of databases than others. The table below summarizes key considerations for horizontally scaling the database types we discussed before.

| Database Type | Horizontal Scaling Fit | Why | Key Constraint |
|---|---|---|---|
| Key-Value | ✅ Strong | Records are isolated with no relationships to other nodes; keys distribute cleanly across shards; many managed services shard automatically | Simple access model limits query flexibility |
| Document | ✅ Strong | Documents are self-contained; no JOIN operations means queries don't need to coordinate across shards | Partition key design still matters; complex cross-document queries remain slow |
| In-Memory | ⚠️ Moderate | Can distribute across nodes for throughput and availability | RAM cost caps dataset size regardless of node count; scaling adds throughput and redundancy, not storage capacity |
| Graph | ❌ Difficult | Relationship traversals frequently cross shard boundaries, paying a network cost each time; harder as the graph grows more interconnected | Better suited for analytical workloads than web-scale traffic |
| Relational | ⚠️ Moderate | Read replicas scale read-heavy workloads well | Sharding breaks or complicates JOIN operations; cross-shard joins must be reassembled in application code |

### Self-Hosted vs. Managed Considerations

Think back to the managed vs. self-hosted distinction from a previous lesson. Horizontal scaling is one of the places where that difference is most visible.

If we're self-hosting a database on virtual machines:
- Setting up read replicas means we provision additional VM instances, install and configure the database engine on each one, configure the replication link from the primary to each replica, verify replication is working, and update our application's connection logic to route reads to replicas and writes to the primary. 
- Sharding means our team designs the partitioning strategy, physically creates the separate shard instances, migrates data into the correct shards, and handles cross-shard query coordination in code. Monitoring, replication lag alerting, and failover logic are all ours to build and maintain.

If we're using a managed cloud database service, the provider handles the infrastructure mechanics: spinning up replica nodes, managing the replication pipeline, and surfacing replication lag metrics. For some managed non-relational databases, sharding is essentially automatic: we configure a partition key and the service distributes data across nodes without our team managing individual shard instances.

What doesn't change regardless of managed or self-hosted, is our team is still responsible for the design decisions that determine whether horizontal scaling actually works. A managed service that automatically shards our data can't save us from a partition key that sends 80% of our traffic to one shard, and an application that ignores read replicas and routes everything to the primary will perform as if we had no replicas at all.

## Choosing a Scaling Strategy

## Choosing a Scaling Strategy

When a database starts showing signs of strain such as slower queries, higher latency, or approaching storage limits, the first question is whether to scale vertically or horizontally. 

Vertical scaling is usually the simpler first move: it requires no changes to how the application connects to the database or how data is structured. For a relational database powering an e-commerce order management system, resizing the instance to add more RAM and CPU is far less disruptive than introducing replicas or sharding. It's a strong fit when traffic is growing predictably and the team wants to avoid added operational overhead. However, once the largest available instance size is no longer sufficient, or when a single node becomes a fault tolerance risk, vertical scaling stops being a viable long-term solution on its own.

Horizontal scaling becomes the right choice when workloads outgrow what a single instance can handle, or when availability and geographic distribution matter. A social platform experiencing rapid user growth might add read replicas to handle the surge in feed and profile lookups without overloading the primary database. A global SaaS application serving users across multiple continents might deploy read replicas in each region to reduce latency for users far from the primary instance. For write-heavy workloads that have hit vertical limits, like an IoT platform ingesting sensor data from millions of devices, sharding data across multiple nodes may be necessary. Non-relational databases are often the better fit here, since most are built with horizontal scaling as a core capability rather than an afterthought.

In practice, most mature production systems use both strategies at different points in their lifecycle or for different parts of their architecture. A startup might vertically scale a relational database through its early growth phase, then layer in read replicas as read traffic increases, and later evaluate sharding only if write throughput becomes a bottleneck. Within a single application, the relational database handling financial transactions might be scaled vertically while a key-value store managing session data scales horizontally across many nodes. The choice is rarely permanent; scaling decisions should be revisited as traffic patterns, data volume, and cost constraints evolve.

| | Vertical Scaling | Horizontal Scaling | Using Both |
|---|---|---|---|
| **Choose when** | Growth is steady and predictable; architecture simplicity is a priority | Workload has outgrown a single instance; availability or geographic distribution is needed | Different parts of the system have different needs; workload has evolved past vertical limits |
| **Common examples** | Resizing an RDS instance for an order management system during steady growth | Adding read replicas for a high-traffic social feed; sharding a key-value store for IoT data ingestion | Vertically scaled relational DB for transactions + horizontally scaled in-memory store for sessions |
| **Key strengths** | No application architecture changes; operationally simple | No hard ceiling on scale; improves fault tolerance and availability | Lets each database layer be optimized independently for its workload |
| **Key limitations** | Hardware ceiling; single node is still a fault risk | Adds complexity around partitioning, routing, and consistency | Requires careful architecture planning to avoid inconsistency between layers |
| **Watch out for** | Treating the largest instance as a permanent solution | Poor partition key design causing uneven load across nodes | Over-engineering early; adding complexity before it's needed |

## Summary



## Check for Understanding

Which of the following is a common limit of vertical scaling?
Which of the following is a concern for horizontally scaling a database?