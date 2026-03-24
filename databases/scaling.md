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

### Strengths & Limitations

**Strengths**

- *Avoids complexity*: No code changes, no rethinking how our data is organized. Vertical scaling keeps things simple by adding resources to the existing infrastructure, no extra data orchestration is required.
- *Helps most with writes*: Without horizontal scaling strategies that introduce complexity (we'll talk more about these shortly!) all write operations typically go to one primary server. If our database is getting overwhelmed by incoming writes, adding more database servers (horizontal scaling) doesn't directly help, since . Upgrading the server that handles writes is the right move.

**Limitations**

- *Scaling limits*: Cloud providers only offer database instances up to a certain size. Once we hit the largest available option, we've exhausted vertical scaling as a solution.
- *Cost vs performance*: Larger instance tiers cost significantly more, and we don't always get proportionally better performance for the price.
- *Hard to scale back down*: Maybe we need more resources during a holiday surge twice a year, and don't need maximum vertical capacity the rest of the year.  Downsizing a production database requires careful planning and usually some downtime. Teams often stay on oversized instances longer than needed to avoid the hassle, which adds ongoing costs.

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

**Multi-Region Replication: Scale for Geography**

Beyond throughput, "scale" can also mean serving users in multiple geographic regions with low latency. Multi-region replication extends the replica concept globally: database nodes are deployed in different regions, and users are routed to the nearest one. Some configurations limit all writes to a single primary region; others support multi-primary writes with conflict resolution logic. This pattern improves both availability (a regional outage doesn't take down the system) and read latency for globally distributed users. 

### Improving Writes: Sharding

### Strengths & Limitations

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

### Self-Hosted vs. Managed Considerations


differences in responsibility during horizontal scaling for self hosted vs managed databases
comparison of which types of databases are ideal vs struggle with horizontal scaling and why

## Choosing a Scaling Strategy

limitations & tradeoffs of each
clarify read replicas vs replication in other regions (read only scaling vs read & write, include cost tradeoffs)

## Summary



## Check for Understanding

Which of the following is a common limit of vertical scaling?
Which of the following is a concern for horizontally scaling a database?