# Introduction to Reliability

## Learning Goals

- Explain what reliability means in the context of cloud systems and why failure is an expected and normal property of distributed systems rather than an exceptional event.
- Distinguish between reliability and availability and explain how each is measured.
- Describe strategies teams use to implement reliability.
- Identify the key tradeoffs involved in reliability decisions, including cost, complexity, speed, latency, and data consistency.

## What is Reliability?

When software runs in production, the expectation is that it works consistently. A system that processes a user's request correctly nine times out of ten is not a reliable system. A system that is fast on Tuesday but slow on Wednesday without any apparent reason is not a reliable system. **Reliability** is the ability of a system to perform its intended function correctly and consistently over time, even under conditions that are less than ideal.

Reliability is different from correctness. A system can produce the right result and still be unreliable if it does so inconsistently. A payment service that correctly processes transactions when traffic is low but begins failing when traffic increases is correct under controlled conditions but unreliable when the load is unpredictable, hardware fails, or the network is disrupted. Building for production means building for those less than ideal conditions that production environments regularly encounter.

### Reliability versus Availability

While reliability and availability are closely related they are not interchangeable. **Availability** is the percentage of time a system is operational and accessible to users. A system that is up and responding to requests is available. Availability is typically expressed as a percentage over a given time period, and teams often describe it in terms of "nines". A system with 99% availability is down for approximately 3.65 days per year. A system with 99.9% availability is down for approximately 8.7 hours per year. A system with 99.99% availability is down for approximately 52 minutes per year.

Reliability is broader than availability. A system can be highly available and still be unreliable if it returns errors or produces incorrect results while it is running. Consider a search service that is online and responding to every request but returning incorrect results due to a bug in its ranking algorithm. That system is available but unreliable in a way that directly affects users.
- Availability asks: Is the system up? 
- Reliability asks: Is the system doing what it is supposed to do?

###  Failure Is Expected in Distributed Systems

A single well-maintained server can be made highly reliable. However, modern cloud applications are not single servers. They are distributed systems made up of many services, databases, caches, load balancers, and network connections, all running across many physical machines in data centers around the world.

Each component in a distributed system has its own probability of failure. A single component might fail very rarely, but as the number of components grows, the probability that at least one of them is experiencing a problem at any given moment increases significantly. For a system with dozens of interdependent services, it's not a question of _if_ something will fail but _when_.

This is a fundamental shift in how reliability engineering approaches the challenge of keeping systems running when individual components fail. Rather than trying to build components that never fail, the goal is to build systems that continue to function correctly even when individual components do fail. This means designing for failure from the start. We should assume that any service can become unavailable, that any network call can time out, and that any server can crash, and build the system so that these events are handled gracefully rather than causing a complete outage.

This mindset, sometimes called designing for failure, is one of the defining characteristics of modern cloud architecture. The systems that are most reliable in production are the ones built on the assumption that something will go wrong.

## Reliability Indicators

Before a team can improve the reliability of a system, they need to be able to measure it. Reliability indicators are the metrics teams use to understand how their system is performing over time, identify where it is falling short, and track whether improvements are having the intended effect. Without these measurements, reliability becomes a matter of guesswork.

The indicators below are not a comprehensive list, but a selection of widely used metrics that each measure a different aspect of reliability. Together they give teams a layered view of how their system behaves under normal conditions, how quickly problems are detected, and how effectively the team is able to respond and recover when something goes wrong.

### Uptime

**Uptime** is the amount of time a system has been continuously running without interruption. It is the raw measurement that availability percentage is calculated from. Where availability expresses reliability as a proportion of total possible time, uptime is the underlying count of operational time that produces that percentage.

Teams track uptime as a reliability indicator because it provides a direct measure of how consistently a system stays running. A system with frequent short outages may show a healthy availability percentage over a long period while still creating a poor experience for users who encounter those outages. Tracking uptime alongside availability percentage gives teams a more complete picture of how stable their system actually is.

### Mean Time Between Failure (MTBF)

**Mean Time Between Failure (MTBF)**, is the average amount of time between two failure incidents. A higher MTBF indicates that failures are occurring less frequently, which is a sign of a more reliable system. MTBF is useful for understanding the overall failure rate of a system over time. If a team notices that MTBF is decreasing, it means failures are becoming more frequent, which may indicate that technical debt is accumulating, infrastructure is degrading, or recent changes have introduced instability.

### Mean Time to Detect (MTTD)

**Mean Time to Detect (MTTD)**, is the average amount of time between a failure occurring and the team becoming aware of it. A lower MTTD means the team finds out about problems quickly, which directly reduces the amount of time users are affected before recovery begins. MTTD is closely tied to the observability practices introduced in the previous lesson. A team with strong monitoring and well-configured alerts will detect failures faster than a team that relies on users to report problems. Reducing MTTD is one of the most direct ways that investment in observability translates into improved reliability outcomes.

### Mean Time to Repair (MTTR)

**Mean Time to Repair (MTTR)**, is the average amount of time it takes to restore a system to a healthy state after a failure occurs. A lower MTTR indicates that a team is able to detect and recover from failures quickly. MTTR is often more actionable than MTBF because teams have more direct control over how quickly they respond to and recover from failures than they do over whether failures happen at all. Investments in monitoring, automated recovery, and having a well-defined process for diagnosing and resolving failures all contribute to reducing MTTR. A team that cannot prevent failures entirely can still significantly reduce their impact by minimizing the time users are affected.

### Error Rate

**Error rate** is the percentage of requests that result in an error over a given period. A rising error rate is often the first sign that reliability is degrading, even before users begin reporting problems. As discussed in [Introduction to Observability](./intro-to-observability.md), measuring errors is one of the four golden signals and is one of the most important metrics a team can track.

Error rate is particularly useful because it is directly tied to the user experience. A system with a high error rate is failing its users in a measurable way, regardless of whether the system is technically considered available.

### Latency

**Latency** is the time it takes for a system to respond to a request. Reliability is not only about whether a system responds but about whether it responds within a time that is acceptable to users. A system that takes 30 seconds to load a page is technically available but provides a poor user experience.

Latency is also a leading indicator of broader reliability problems. A system under stress often shows increasing latency before it begins returning errors or going offline entirely. Tracking latency over time gives teams an early warning that something may be wrong before it escalates into a more serious failure.

### Throughput

**Throughput** is the number of requests a system can successfully handle per unit of time. A system that performs reliably under normal load but begins failing or slowing down significantly when traffic increases has a throughput reliability problem.

Understanding throughput helps teams anticipate how their system will behave under load and plan accordingly. A team that knows their system can handle 500 requests per second reliably but begins to degrade at 800 requests per second has a concrete data point to inform capacity planning and scaling decisions.

### !callout-info

## SLOs and SLAs

Reliability indicators are most useful when they are tied to concrete targets that a team is working to meet. Two terms that appear frequently in discussions of reliability are **Service Level Objectives (SLOs)** and **Service Level Agreements (SLAs)**. 

An SLO is an internal target that a team sets for how reliable their system should be, expressed in terms of the reliability indicators described in this section. For example, a team might set an SLO that 99.9% of requests must complete successfully within 300ms. 

An SLA is a formal commitment made to users or customers that defines the level of reliability they can expect, often with consequences if the target is not met. For example, a cloud provider might guarantee in their SLA that their storage service will be available 99.95% of the time each month, and that customers will receive a credit on their bill if that target is not met. Together, SLOs and SLAs give reliability indicators a purpose by turning measurements into concrete targets that a team works to meet.

### !end-callout

## How to Implement Reliability

Understanding what reliability means and how to measure it is only part of the picture. The other part is knowing how to build and operate systems that are reliable in practice. Reliability does not happen by chance. It is the result of deliberate decisions about architecture, tooling, and process. Teams implement reliability through a combination of monitoring, architectural patterns, and automated recovery mechanisms.

### Monitoring

**Monitoring** is the practice of continuously watching the signals a system produces and responding when some metric crosses a defined threshold. Observability gives teams the signals they need to understand what a system is doing. Monitoring is what teams do with those signals in practice.

In the context of reliability, monitoring is how teams know whether their system is meeting its reliability targets. A team that has defined an SLO that 99.9% of requests must complete successfully needs a way to continuously measure whether that target is being met. Without monitoring, a team has no way of knowing whether reliability is degrading until users begin reporting problems, by which point the impact on users may already be significant.

Monitoring also connects directly to MTTD. A team with well-configured monitoring and alerts will detect failures faster than a team that relies on user reports, which means recovery can begin sooner and the overall impact of the failure is reduced. 

### Load Balancing

Load balancing was introduced in an earlier lesson as a way to distribute traffic across multiple servers. In the context of reliability, **load balancing** is a strategy a team can employ because it addresses one of the most common sources of unreliability: single points of failure. A single point of failure is any component in a system whose failure would cause the entire system to stop working. 

Load balancing eliminates this risk by distributing traffic across multiple servers. When one server fails, the load balancer detects that it is no longer responding to health checks and removes it from rotation. Traffic continues flowing to the remaining healthy servers and users are unaffected. The system has gone from a state where one failure causes a complete outage to a state where one failure reduces capacity instead.

This improvement in reliability scales with the architecture. Distributing servers across multiple availability zones means that even a failure affecting an entire data center does not bring the application down, because the load balancer can route traffic to servers running in other zones. Each layer of redundancy that load balancing enables is another layer of protection against the failures that distributed systems will inevitably encounter.

### Circuit Breakers

In a distributed system, services depend on each other. An API might call an authentication service, which calls a database, which calls a caching layer. When one service in that chain begins to fail or respond slowly, requests that depend on it start to back up. If those requests continue to pile up while the struggling service tries to recover, the load can prevent it from recovering at all and cause the failure to spread to other parts of the system.

A **circuit breaker** is a pattern that addresses this problem by stopping requests to a failing service before the situation escalates. The name comes from the electrical component that cuts power to a circuit when it detects a dangerous overload, protecting the rest of the system from damage. A software circuit breaker works similarly: when a service begins failing beyond a defined threshold, the circuit breaker opens and requests to that service either fail immediately or are redirected to a fallback, rather than waiting for a timeout that may never come. This gives the struggling service time to recover without being overwhelmed by continued traffic. Protecting a struggling service from additional traffic is just as important as detecting that it is struggling in the first place.

### Automated Recovery

Even with monitoring and load balancing in place, failures will still occur. The question is how quickly a system can recover from them. Manual recovery requires a human to notice the problem, diagnose it, and take corrective action, all of which takes time. During that time, users are affected. **Automated recovery** is the practice of configuring systems to respond to common failure scenarios without waiting for human intervention. 

When a server crashes, an automated process can restart it or launch a replacement. When a health check fails, the load balancer can automatically remove the unhealthy server from rotation. When an availability zone goes offline, traffic can be automatically rerouted to servers in other zones. The primary benefit of automated recovery is its effect on MTTR. By starting the recovery process the moment a failure is detected rather than waiting for a human to respond, automated recovery significantly reduces the time users are affected. It does not replace human judgment for complex or novel failures, but it handles the most common scenarios faster and more consistently than manual intervention can.

## Reliability Tradeoffs

Building a more reliable system is rarely a straightforward decision. Every strategy that improves reliability introduces costs and complexities elsewhere. The tradeoffs below are some of the most common ones teams encounter, but they are not exhaustive. Every system is different, and the specific tradeoffs a team faces will depend on their architecture, their users, and the consequences of failure for their particular application. Understanding these tradeoffs is what allows engineers to make informed decisions rather than simply adding reliability mechanisms because they seem like good practice.

### Cost versus Redundancy

Redundancy is the practice of running multiple copies of a component so that if one fails, others can take over. It is one of the most effective reliability strategies available, and it can also be on of the most costly. Running three servers instead of one costs three times as much. Distributing those servers across multiple availability zones adds data transfer costs on top of the infrastructure costs. Adding automated recovery mechanisms, monitoring infrastructure, and load balancers all add further expense.

For teams building systems where downtime has serious consequences, such as a payment platform, a healthcare application, or an e-commerce site during a peak sales period, the cost of redundancy is usually justified by the cost of the outage it prevents. For teams building internal tools, development environments, or early-stage applications with a small user base, the same level of redundancy may not be necessary or financially practical. The decision comes down to a simple question that is not always simple to answer: what does an hour of downtime actually cost, and is that cost greater than the cost of the redundancy needed to prevent it?

### Complexity and Management Overhead

Each reliability mechanism a team adds to a system is another component that needs to be configured, monitored, maintained, and understood. A system with load balancers, circuit breakers, automated recovery, and multi-zone deployments is significantly more complex to operate than a single-server deployment. 

This complexity has real costs. More components mean more potential failure points. A misconfigured load balancer or a circuit breaker with the wrong threshold can introduce failures rather than prevent them. More complexity also means that engineers need a deeper understanding of how the system works in order to diagnose problems when they occur, which can slow down recovery and increase MTTR.

Teams have to weigh the reliability benefits of each mechanism against the operational burden it introduces. Adding reliability infrastructure that the team does not fully understand or cannot maintain effectively can make a system less reliable rather than more.

### Speed

Investing in reliability takes time. Designing for failure, implementing load balancing, configuring automated recovery, and setting up monitoring all require engineering effort that could otherwise be spent building product features. For teams under pressure to ship quickly, this tradeoff is often felt acutely.

The tension here is that reliability problems also slow teams down in a different way. A system that fails frequently requires engineers to spend time responding to and recovering from incidents rather than building new features. A system that is hard to diagnose because it lacks observability takes longer to fix when something goes wrong. Investing in reliability upfront can reduce the amount of unplanned work a team faces later, but the payoff is not always immediate or easy to quantify.

### Latency

Some reliability strategies introduce additional latency into a system. Routing a request through a load balancer adds a small amount of time. Checking a circuit breaker state before making a request adds a small amount of time. Replicating data across multiple availability zones for redundancy means that write operations may need to wait for confirmation from multiple locations before completing.

In most systems these overheads are small enough to be negligible. But in systems where latency is a critical requirement, such as financial trading platforms or real-time communication tools, even small additions to response time can be unacceptable. Teams building latency-sensitive systems have to measure the overhead of each reliability mechanism carefully and decide whether the reliability benefit justifies the latency cost.

### Data Consistency

Distributing data across multiple servers or availability zones is a common reliability strategy because it ensures that data remains accessible even if one location fails. However, distribution introduces a challenge that does not exist in a single-server system: keeping all copies of the data consistent with each other.

When a user writes data to one server, that change needs to be propagated to the other servers that hold copies of the same data. This propagation takes time, and during that window there is a period where different servers may have different versions of the data. If a user writes data to one server and then immediately reads it from another, they may see an outdated version of data.

Teams have to choose between two broad approaches to this problem. **Strong consistency** ensures that all reads reflect the most recent write, but achieving this requires coordination between servers that adds latency and can reduce availability. **Eventual consistency** accepts that reads may temporarily return slightly outdated data, but offers better performance and availability in exchange. The right choice depends on the specific requirements of the application and how tolerant users are of seeing temporarily inconsistent data.

## Summary

**Reliability** is the ability of a system to perform its intended function correctly and consistently over time. It is different from **availability**, which measures whether a system is operational, because a system can be available while still returning errors or producing incorrect results. In distributed cloud systems, failure is not an exceptional event but an expected property of systems made up of many interdependent components. The goal of reliability engineering is not to eliminate failure entirely but to design systems that continue to function when individual components fail.

Teams measure reliability using indicators such as **uptime**, **MTBF**, **MTTD**, **MTTR**, **error rate**, **latency**, and **throughput**, each of which captures a different dimension of how a system performs over time. **SLOs** and **SLAs** give these measurements purpose by turning them into concrete targets that teams commit to meeting. 

Reliability is implemented through a combination of **monitoring**, **load balancing**, **circuit breakers**, and **automated recovery**, each of which addresses a different aspect of how systems detect, withstand, and recover from failure. These strategies come with tradeoffs around cost, complexity, speed, latency, and data consistency, and the right balance depends on the specific needs of the system and the consequences of downtime for its users.

## Check for Understanding

### !challenge
<!-- Question 1 -->
* type: multiple-choice
* id: 42137ca5-45ff-482c-b0bf-9fd1f247c028
* title: Introduction to Reliability

##### !question
A payment processing service is operational and responding to requests 99.9% of the time. However, due to a bug introduced in a recent deployment, 4% of transactions are being processed incorrectly. Which of the following best describes this system?
##### !end-question

##### !options
a| The system is both reliable and available because it is responding to the vast majority of requests successfully.
b| The system is available but not reliable, because it is operational but not performing its intended function correctly and consistently.
c| The system is reliable but not available, because it is processing most transactions correctly despite occasional downtime.
d| The system is neither reliable nor available, because a 4% error rate means the system is effectively offline for those users.

##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Availability measures whether a system is operational and responding to requests. Reliability measures whether a system is performing its intended function correctly and consistently. This system is available because it is up and responding, but it is not reliable because it is processing 4% of transactions incorrectly. A system can meet its availability target while still failing its users in ways that availability metrics do not capture.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: multiple-choice
* id: 34aa9fc0-4d8b-4d7f-a4ff-9a526e7fad8a
* title: Introduction to Reliability

##### !question
A team is designing a new cloud application that will be made up of fifteen interdependent services running across multiple servers. A team member suggests that since each individual service is well-built and has been thoroughly tested, the system as a whole should be highly reliable. Which of the following best explains why this reasoning is flawed?
##### !end-question

##### !options
a| The team member is correct. If each individual service is reliable, the system as a whole will be reliable because reliability is additive across components.
b| Thoroughly tested services never fail in production, so the team should focus on improving their deployment process rather than designing for failure.
c| In a distributed system, the probability that at least one component is experiencing a problem at any given moment increases as the number of components grows. A reliable system is designed to keep functioning when individual components fail, not on the assumption that they never will.
d| The team should reduce the number of services from fifteen to a smaller number, because fewer services means fewer potential failure points and a more reliable system overall.
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
A single well-built component can be made highly reliable, but a distributed system made up of many interdependent components introduces many more potential failure points. Even if each individual service fails very rarely, the probability that at least one of them is experiencing a problem at any given moment grows with the number of components. Modern reliability engineering starts from the assumption that failures _will happen_ and focuses on building systems that continue to function correctly when they do, rather than assuming that thorough testing alone is sufficient to prevent failures in production.
#### !end-explanation 
### !end-challenge

<!-- Question # 3 -->
### !challenge
* type: multiple-choice
* id: 7ef9f999-6296-4436-88b6-9f1c39688d10
* title: Introduction to Reliability

##### !question
A small startup is considering whether to deploy their early-stage application across three availability zones with a load balancer and automated recovery. A team member argues that the added complexity and cost are not justified at this stage. Which of the following best reflects the tradeoffs involved in this decision?
##### !end-question

##### !options
a| The team member is wrong. Every production application should be deployed across multiple availability zones with automated recovery regardless of its stage or user base.
b| The team member has a point. Multi-zone deployments with automated recovery increase infrastructure costs and operational complexity, and for an early-stage application with a small user base the cost of that redundancy may outweigh the reliability benefit.
c| The team member is wrong. The cost of a multi-zone deployment is always lower than the cost of downtime, so the investment is always justified.
d| The team member has a point. Load balancing and automated recovery always introduce too much latency for early-stage applications to use effectively.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Reliability mechanisms like multi-zone deployments and automated recovery improve a system's ability to withstand failures, but they come with real costs in terms of infrastructure spend and operational complexity. For an early-stage application with a small user base, the consequences of downtime may be limited enough that a simpler, lower-cost deployment is the more practical choice. Reliability tradeoffs are not one-size-fits-all decisions. The right level of investment depends on the specific system, its users, and the consequences of failure for the business.
#### !end-explanation 
### !end-challenge