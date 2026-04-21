# Cloud Migration Strategies

## Learning Goals
- Identify each of the 7 Rs of migration and describe when each is appropriate.
- Explain the tradeoffs between lift-and-shift and re-architecture approaches across speed, cost, complexity, and long-term value.
- Describe why governance matters in cloud and hybrid environments and explain the shared responsibility concept.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Lift-and-shift** | Moving an application from on-premises to the cloud without making changes to the code or architecture | Rehost, migrate-as-is | "We used a lift-and-shift approach for the payroll system because we needed to be out of the data center by the end of the quarter." |
| **Re-architecture** | Redesigning an application to take advantage of cloud-native features, such as containers, managed services, or serverless compute | Refactor, cloud-native rewrite | "Re-architecting the API layer took three months, but it cut our infrastructure costs by half and made the service horizontally scalable." |
| **Legacy system** | An existing system built with older technologies or architectures that may be difficult to change, integrate with modern tooling, or migrate | Old system, technical debt | "Our billing service is a legacy system running on a 2011 OS version, it's a migration priority because the vendor stopped releasing patches." |
| **Cloud-Native** | An application designed to run in the cloud and take full advantage of cloud capabilities, typically emphasizing scalability, resilience, and automation. | Cloud-Optimized | "We built the order system as a cloud-native service with policies to auto-scale horizontally when traffic spikes around sales and holidays" |
| **Workload** | A unit of work running on a system, such as an application, service, database, or batch job | Application, service, system | "We mapped every workload to one of the 7 Rs before deciding which to migrate in the first phase." |
| **Migration readiness** | An assessment of how prepared an organization's systems, teams, and processes are to support a cloud migration | Migration assessment, cloud readiness | "Before committing to a timeline, our team ran a migration readiness assessment to identify which workloads had unresolved dependencies." |
| **Cloud governance** | The policies, controls, and processes that define how cloud resources can be used, provisioned, and secured within an organization | Cloud policy, cloud guardrails | "Without cloud governance in place, developers were spinning up resources in every region and the monthly bill was impossible to predict." |
| **Shared responsibility model** | The division of security and operational responsibility between a cloud provider and the customer using cloud services | Provider/customer boundary, security division | "Under the shared responsibility model, our team owns the application security and access configuration; the provider handles physical infrastructure." |
| **Policy guardrail** | An automated rule or constraint that prevents cloud resources from being created or configured in ways that violate an organization's policies | Policy boundary, preventive control | "A policy guardrail prevented engineers from opening port 22 to the public internet, even if they tried to do it accidentally." |

## The 7 Rs of Migration

When an organization decides to move to the cloud, a common instinct is to treat migration as a single event: pick up everything on-premises and set it down in the cloud. That rarely works in practice. Organizations have dozens or hundreds of applications, each with different technical complexity, business value, and readiness for the cloud. 
- Some systems are worth investing in and modernizing, while others might be barely used and should be turned off. Other services may need to move quickly for business reasons, with improvements deferred to later.

The **7 Rs** are a framework for thinking through these complexities. Rather than treating every application the same, we assess each workload individually and assign it a migration strategy. The 7 Rs give us a shared vocabulary for those decisions and a way to communicate them clearly across technical and business teams.

### Lift-and-Shift vs. Re-Architecture

Before walking through each R, it helps to understand the two ends of the spectrum that most migration decisions fall on: lift-and-shift and re-architecture.

**Lift-and-shift** 

Lift-and-shift (also called Rehost) means picking up an application as-is and setting it down in a cloud environment. 
- Nothing about the code or architecture changes: the application runs on a cloud virtual machine the same way it ran on an on-premises server. 
- This is typically the fastest path, it requires less cloud expertise, carries a lower upfront cost than refactoring, and brings less risk of introducing new bugs. 

The tradeoff is that the application won't take advantage of cloud-native features like auto-scaling, managed databases, or serverless compute. It will likely cost more to run in the cloud than an equivalent application that was designed for the cloud.

**Re-architecture** 

Re-architecture (also called Refactor) means redesigning the application to take full advantage of what the cloud offers. 
- An application that was built as a monolith might be decomposed into microservices. 
- A database that was managed in-house might be replaced with a fully managed cloud equivalent. 

These changes can take significantly longer and require more resources, but they can transform how a system scales, how it recovers from failures, and what it costs to run at scale. Re-architecting delivers the most long-term value for high-traffic, business-critical applications.

Neither approach is universally correct, the right choice depends on the workload, the business constraints, and the team's capacity. Most real migration plans include both strategies applied to different workloads. Lift-and-shift handles the applications that need to move quickly or that don't justify significant investment. Re-architecture handles the systems where cloud-native investment will pay long-term dividends.

| | Lift-and-Shift (Rehost) | Re-Architecture (Refactor) |
|---|---|---|
| **Speed** | Fast (weeks to months) | Slow (months to years) |
| **Cost to migrate** | Low | High |
| **Long-term cloud costs** | Higher (not cloud-optimized) | Lower (optimized for cloud) |
| **Risk** | Lower | Higher |
| **Cloud-native benefits** | Low to None | Full |
| **Best when** | Speed or compliance pressure; applications that will be replaced soon | High-value, long-lived systems where cloud benefits justify the investment |

### 1. Retire

> **A Running Scenario: FieldOps Inc.**
>
> To make the 7 Rs concrete, we'll follow a fictional mid-sized company called FieldOps Inc. as we explore the 7 Rs. FieldOps provides field service management software to utility companies. They have 40+ workloads running across aging on-premises servers, a data center contract expiring in 18 months, and a leadership team that wants to move entirely to the cloud before renewal. Their CTO has just finished a portfolio assessment and is assigning each workload to a migration strategy.

The first question to ask of any workload is: does it still need to exist at all?

As organizations grow and evolve, applications accumulate. An internal tool built for a project that ended three years ago. A reporting dashboard that duplicates something a newer platform provides. A service kept running because no one has confirmed it's safe to turn off. These workloads don't deliver value, but they do carry cost in infrastructure, maintenance overhead, security surface area, and the cognitive burden of tracking systems that no one is responsible for.

Before investing time to migrate a workload, it's worth asking whether migration is the right decision. **Retiring** a workload removes it from the migration plan entirely and simplifies the resulting cloud environment.

Let's see how this applies to our example organization! At FieldOps Inc., the portfolio assessment turns up a legacy employee scheduling tool built in 2014. 
- When the team pulls access logs, they find it hasn't been used by anyone in 11 months. The company now uses a commercial HR platform that handles scheduling natively. 
- The CTO assigns this workload to be retired. The server is decommissioned, and the migration plan shrinks by one workload.

```
[DIAGRAM PLACEHOLDER]
Before: [Legacy Scheduling App] on on-premises server — consuming maintenance, patching, server costs
After: [Decommissioned] — no server, no maintenance, no migration needed
```

### 2. Retain

Some workloads aren't ready to migrate, and that's a legitimate outcome of a migration assessment. Some systems have dependencies, constraints, or circumstances that make migration impractical *right now*.

A workload might depend on specialized hardware that has no cloud equivalent. It might be deeply integrated with other on-premises systems in ways that would make isolation too costly or risky right now. It might be scheduled for replacement within the next year, making migration a poor investment. Or the team simply may not have the bandwidth to migrate everything at once, and this workload doesn't justify reprioritizing other work.

**Retaining** a workload means keeping it on-premises intentionally, not indefinitely. This is not the same as ignoring the workload, it means acknowledging the constraints, documenting them, and scheduling a future reassessment rather than letting the workload quietly persist without a plan.

Over at FieldOps Inc., one of their workloads is a real-time telemetry processing service that ingests data from physical sensors on utility equipment. The service runs on bare-metal servers with specialized network interface cards that the cloud can't replicate.
- The vendor has a cloud-compatible version on their roadmap, but it won't be available for at least 12 months. 
- The CTO assigns this workload to be retained and schedules a migration readiness review for the following year.

```
[DIAGRAM PLACEHOLDER]
Before: [Telemetry Service] on specialized on-premises hardware
After: [Telemetry Service] still on-premises — flagged for reassessment when vendor roadmap delivers
```

### 3. Rehost (Lift and Shift)

As we saw earlier in this lesson, rehosting or "lifting and shifting" is the fastest migration path and the one that requires the least cloud expertise. 
- It's particularly appropriate when speed is the primary constraint: a regulatory deadline, a contract obligation, an expiring data center lease. 
- It's also a reasonable starting point when an organization wants to exit on-premises infrastructure quickly and is prepared to optimize once the application is in the cloud.

The tradeoffs are that a rehosted application likely:
- won't benefit from cloud-native features
- will cost more to run than an equivalent application designed for the cloud
- will carry the same architectural limitations it had on-premises

Let's check in with FieldOps Inc.. Their contract management application is a ten-year-old Java application that processes field service contracts. It works reliably, but it was designed as a single-instance application with no scaling logic. 
- The data center lease expires in 18 months, and redesigning the application isn't in the current budget. 
- The CTO assigns this workload to be rehosted. It will move to a cloud virtual machine as-is. Optimization is logged as future work.

```
[DIAGRAM PLACEHOLDER]
Before: [Contract Mgmt App Server (on-prem)] → [Database Server (on-prem)]
After:  [Contract Mgmt App VM (cloud)] → [Database VM (cloud)]
        Same topology. Same configuration. Running in cloud infrastructure.
```

### 4. Replatform (Lift, Tinker, and Shift)

Replatforming sits between rehosting and re-architecture; it takes more time than rehosting but less than a full re-architecture. We move the application to the cloud, but we make targeted improvements along the way to take meaningful advantage of cloud capabilities, without redesigning the application from scratch. 
- This can deliver meaningful reductions in operational overhead, particularly through managed services that eliminate the need to patch and maintain database servers or operating systems.

Common replatforming moves include switching from a self-managed database to a managed cloud database service, upgrading to a supported operating system version, or moving from a self-hosted application server to a platform-as-a-service environment. The core application logic stays the same; what changes is the infrastructure it runs on.

In our example comanpy FieldOps Inc., their customer portal uses a relational database that the operations team manages manually on-premises. During the migration planning, the engineering lead points out that the database server requires significant hands-on maintenance every time there's a version update, and the team has had two unplanned outages in the past year due to storage running out. 
- The CTO assigns the portal to be replatformed: the application moves to a cloud virtual machine, but the database is migrated to a managed cloud database service. 
- The application requires minor code changes for the new connection configuration, but no logic changes.

```
[DIAGRAM PLACEHOLDER]
Before: [Customer Portal App Server (on-prem)] → [Self-Managed DB Server (on-prem)]
After:  [Customer Portal App VM (cloud)] → [Managed Cloud Database Service]
        Application unchanged. Database maintenance is now the provider's responsibility.
```

### 5. Refactor / Re-Architect

We saw earlier in the lesson that refactoring in this context means redesigning the application to take full advantage of cloud-native capabilities. Where rehosting and replatforming treat the cloud as a better place to run the same application, refactoring treats the cloud as an opportunity to build a better application.

This might mean:
- decomposing a monolith into microservices that can be scaled independently
- replacing a batch processing job with an event-driven architecture that processes records in real time
- containerizing services so they can be deployed and scaled through an orchestration platform
- adopting serverless compute for components with highly variable traffic patterns

Refactoring requires more time and resources than any other migration strategy. The engineering team needs to understand not just the existing system but also the cloud-native patterns that will replace it. The risk of introducing bugs or regressions is higher because the scope of change is larger.

The return on that investment, for the right workload, can be substantial. Applications that have been properly refactored for the cloud can scale to handle orders-of-magnitude more traffic, recover automatically from failures, and may cost significantly less to operate than their on-premises equivalents.

In our example company FieldOps Inc., their dispatch and routing service is the core of their product. It processes thousands of field job assignments per day, and on peak days (after a storm event, when utilities need emergency repairs across a wide area) it gets hit with ten times the normal volume. The current monolithic application can't scale fast enough for those spikes. 
- The CTO assigns it to be refactored. The team breaks the service into independently deployable components: job intake, routing logic, and notifications. 
- The routing component, the most compute-intensive, is moved to serverless compute that scales automatically with demand.

```
[DIAGRAM PLACEHOLDER]
Before: [Dispatch Monolith on a large provisioned VM]
        — scales poorly, entire app must scale together, expensive to over-provision

After:  [Job Intake Service] → [Routing Function (serverless)] → [Notification Service]
        — each component scales independently; routing auto-scales on demand
```

### 6. Repurchase (Drop and Shop)

Sometimes the most efficient way to move a workload to the cloud is to stop maintaining it and subscribe to a commercial alternative that already does what we need.

This is most common for general-purpose business software: CRM platforms, email systems, HR tools, accounting software, document management. If an organization is self-hosting a CRM on-premises, migrating that infrastructure to the cloud might require more effort than simply switching to a commercially hosted CRM that's maintained by a dedicated vendor and already runs in the cloud.

Repurchasing eliminates the migration problem by eliminating the workload. The infrastructure goes away, and responsibility for uptime, updates, and security patches shifts to the vendor. The tradeoffs are a loss of customization, potential complexity in migrating existing data to the new platform, and an ongoing subscription cost.

Let's look at how FieldOps Inc. handled migrating their CRM tool. Their sales team uses a self-hosted CRM that was originally chosen because it could be customized. Over the years, most of those customizations have been worked around using other tools, and the CRM runs on aging hardware that's going to need significant attention. 
- The sales team evaluates a commercial hosted CRM and finds it covers everything they need. 
- The CTO assigns the self-hosted CRM to be repurchased: the team migrates the customer data to the commercial platform and decommissions the on-premises CRM server.

```
[DIAGRAM PLACEHOLDER]
Before: [CRM Tool hosted inside the on-premises network] — Self-hosted CRM on an on-premises server. Internal team responsible for hosting, backups, upgrades, and security patches.
After:  [CRM Tool hosted in the cloud and accessed by services in the on-premises network] — Commercial hosted CRM. Data migrated. No infrastructure to maintain. Vendor handles uptime and updates.
```

### 7. Relocate

Relocate is sometimes listed as optional in the 7Rs because it applies to a narrower set of scenarios than the other Rs. It is a specialized strategy for organizations that are already running virtualized workloads and want to move them to the cloud with minimal disruption. This generally requires compatible virtualization platforms on both ends.

Rather than copying an application and setting it up fresh in the cloud, relocation transfers virtual machine images directly between environments, keeping the same virtual infrastructure configuration. This approach is designed for speed and minimal reconfiguration. It's particularly useful when an organization uses the same virtualization platform in both their on-premises environment and the cloud environment they're migrating to. 
- No application code changes, no reconfiguration of the guest operating system — the VM moves and continues running!

At our example company FieldOps Inc., their infrastructure team manages a cluster of virtual machines running on a hypervisor platform that their cloud provider also supports. 
- Rather than setting up new cloud VMs and reinstalling each application, the team exports the VM images and imports them directly into the cloud environment. 
- The workloads come up running without any reconfiguration. The team estimates this saves three weeks compared to a traditional rehost approach.

```
[DIAGRAM PLACEHOLDER]
Before: [VM Image A] [VM Image B] [VM Image C] — on-premises hypervisor cluster
After:  [VM Image A] [VM Image B] [VM Image C] — same images, now running on cloud infrastructure. No OS changes. No application changes. Direct transfer.
```

### Choosing the Right Strategy

Migration strategies aren't mutually exclusive. A real migration plan will use several of them simultaneously across different workloads. The decision for each workload comes down to a few core questions:

- Is this workload still valuable? If not, **Retire** it.
- Is this workload ready to move at all? If not, **Retain** it.
- Does this workload need to move fast? If yes, **Rehost** or **Relocate**.
- Can we get meaningful operational wins with targeted improvements? If yes, **Replatform**.
- Is this a high-value, long-lived system where cloud-native investment pays off? If yes, **Refactor**.
- Does a better commercial product exist for this function? If yes, **Repurchase**.

The goal is to make a deliberate decision for each workload rather than applying the same strategy across the board. Lift-and-shifting everything into the cloud and we pay cloud prices for on-premises thinking. Refactor everything and we run out of time and budget before we move anything. A thoughtful mix gets workloads moved efficiently while investing engineering effort where it delivers the most long-term value.


## Cloud Governance

Why governance matters
Policy, cost control, security guardrails
Shared responsibility concept
Why governance increases in importance in hybrid systems

## Summary


## Check for Understanding

Which strategy prioritizes speed over optimization?
When is refactoring usually justified?

Reflection Discussion question in class:
A company needs to exit a data center in 6 months. Which strategy would you recommend first and why?