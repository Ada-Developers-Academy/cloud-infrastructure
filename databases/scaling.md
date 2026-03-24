# Scaling Databases

## Learning Goals

- Describe the differences between vertical and horizontal scaling as it relates to databases, including limits for common types of databases.
- Describe patterns like read replicas and full replication that can be used to keep data available and consistent in a multi-region architecture
- Generally describe strong vs eventual consistency models in distributed databases 

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |

Key terms: Vertical scaling, horizontal scaling, read replica

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

- Easy to apply & avoids complexity: No code changes, no rethinking how our data is organized. Vertical scaling keeps things simple by adding resources to the existing infrastructure, no extra data orchestration is required.
- Helps most with writes: If our database is getting overwhelmed by incoming writes, adding more database servers (horizontal scaling) doesn't directly help, since all writes typically go to one primary server. Upgrading that server is the right move.

**Limitations**

- Scaling limits: Cloud providers only offer database instances up to a certain size. Once we hit the largest available option, we've exhausted vertical scaling as a solution.
- Cost vs performance: Larger instance tiers cost significantly more, and we don't always get proportionally better performance for the price.
- Hard to scale back down: Maybe we need more resources during a holiday surge twice a year, and don't need maximum vertical capacity the rest of the year.  Downsizing a production database requires careful planning and usually some downtime. Teams often stay on oversized instances longer than needed to avoid the hassle, which adds ongoing costs.

### Self-Hosted vs. Managed Considerations

Whether our database is self-hosted on a cloud VM or running as a managed service changes how much work vertical scaling requires from our team.

With a managed service, scaling up is built into the platform. We pick a larger tier, and the provider handles the transition. If our database is set up for high availability across multiple availability zones, the upgrade usually happens with less than a minute of interruption.

With a self-hosted database on a cloud VM, vertical scaling means resizing the VM itself. That typically requires stopping the instance, switching to a larger machine type, and restarting. This switchover generally means planned downtime. Even if we set up a new, larger instance first, divert traffic to the new set up, then decommision the smaller instance, there will likely be at least a short service interruption while traffic is diverted to the new instance. When the database comes back up, our team is responsible for confirming everything started correctly: that the database process is healthy, connections are re-established, and performance looks as expected. There's no automated safety net unless our team built one.

The upside of self-hosting is that we have full control over how the database engine is configured for the new hardware. Managed services don't always expose settings like how much memory the database is allowed to use, or how it handles disk I/O. But in exchange, we own every step of the process and everything that can go wrong with it.

### Horizontal Scaling

What is horizontal scaling
methods of horizontal scaling

## Scaling Self-Hosted vs Fully Managed Databases

### Self-Hosted 



### Fully-Managed



## Choosing a Scaling Strategy

limitations & tradeoffs of each
clarify read replicas vs replication in other regions (read only scaling vs read & write, include cost tradeoffs)

## Summary



## Check for Understanding

Which of the following is a common limit of vertical scaling?
Which of the following is a concern for horizontally scaling a database?