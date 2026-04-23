# Hybrid and Multi-Cloud Environments

## Learning Goals

- Distinguish between transitional hybrid environments and intentional long-term hybrid architectures.
- Explain why organizations adopt multi-cloud strategies and what tradeoffs come with them.
- Identify governance and operational challenges that arise when systems span multiple environments.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Multi-cloud** | A strategy in which an organization runs workloads across two or more cloud providers | multi-cloud strategy, Polycloud | "We run our machine learning workloads on AWS and our analytics pipeline on Google Cloud; this multi-cloud environment lets us use the best tool for each job." |
| **Vendor lock-in** | The degree to which an organization's systems depend on a single provider's proprietary tools, making switching costly | Provider dependency | "Because we used the provider's proprietary event bus extensively, we hit significant vendor lock-in when we tried to move the service to a different cloud." |
| **Control plane** | The layer of infrastructure responsible for managing, configuring, and orchestrating systems (as opposed to the data plane, which handles actual traffic) | Management layer | "Losing access to the control plane didn't stop traffic from flowing, but it meant we couldn't push any configuration changes until access was restored." |
| **Federation** | The practice of linking separate identity systems so that a principal authenticated in one can be trusted by another | Identity federation, cross-domain trust | "We federated our on-premises Active Directory with the cloud identity provider so that engineers could log into cloud resources with their existing corporate credentials." |
| **Cost attribution** | The practice of tracking and assigning cloud costs to specific teams, services, or business units | Showback, chargeback | "Without cost attribution, it took us three months to realize that one team's overnight batch job was responsible for 40% of our cloud bill." |

## Hybrid Environments

In a hybrid environment, on-premises systems and cloud systems coexist and typically communicate across the boundary we covered in the hybrid networking lesson: VPN tunnels, dedicated connections, or managed transit gateways. 
- From a user perspective, the application may feel like a single system. 
- Behind the scenes, a single user action might touch services in multiple locations. 
  - A request might originate in the cloud, hit a service that runs on-premises, query a database that is also on-premises, and return a response assembled in the cloud!

When we covered cloud migration strategies in the previous lesson, we looked at how most migrations are incremental. Organizations rarely move everything at once; they move workload by workload, team by team. The period between "everything is on-premises" and "everything is in the cloud" is a hybrid environment, and it's a state most organizations spend significant time in. However, some organizations end up in a hybrid environment permanently, and by choice. 

### Transitional vs. Long-Term Hybrid Environments

The most important question to ask about any hybrid environment is: is this temporary, or is it intended to last?

**Transitional hybrid environments** exist because a migration is still in progress. Workloads remain on-premises not by design, but because they're next in the migration queue, waiting on a dependency to be resolved, or lower in priority than the systems that have already moved. The goal is full cloud adoption; the hybrid state is a phase.

In transitional environments, governance decisions can be made with an expiration date in mind. We invest in cross-environment monitoring and access management knowing those investments will be simplified once the remaining workloads migrate.

**Long-term hybrid environments** are intentional. The on-premises component is not a workload waiting to be migrated, it's a permanent part of the architecture. Some of the most common reasons organizations maintain long-term hybrid environments are:
- **Regulatory requirements.** Certain industries are governed by rules that specify where data must be stored and who must have physical access to it. Healthcare organizations, government agencies, and some financial services firms may be required to keep specific data on hardware they directly control—regardless of how capable cloud storage has become.
- **Specialized hardware.** Manufacturing control systems, scientific computing clusters, and certain types of real-time processing run on hardware that has no direct cloud equivalent. Moving those workloads is not a configuration exercise—it may be technically infeasible.
- **Economics.** For workloads with consistent, predictable demand and long expected lifespans, owning the hardware may genuinely be cheaper than paying cloud usage costs indefinitely.

A concrete example: a large hospital network uses cloud services for its patient scheduling system, analytics platform, and public-facing portals. But patient health records governed by strict regulatory requirements stay on-premises on infrastructure the organization controls directly. This is not a migration in progress, it is the intended architecture!

The distinction between temporary and long-term matters operationally. In a long-term hybrid environment, we need permanent solutions for cross-environment identity management, monitoring, and cost governance. We can't treat those as temporary scaffolding waiting to be removed.

![A diagram of the boundaries between self-hosted and cloud infrastructure in a hybrid environment.](assets/hybrid-environment.png)  
*Fig. The boundaries between self-hosted and cloud infrastructure in a hybrid environment. ([Full Size Image](assets/hybrid-environment.png))*

## Multi-Cloud Environments

A **multi-cloud environment** is one in which an organization runs workloads across two or more separate cloud providers simultaneously within the same organization. Each provider hosts a different set of services or workloads, and the organization manages the relationships with multiple vendors.

Multi-cloud is distinct from hybrid environments, though both involve complexity that a single-cloud or fully on-premises setup does not. Where hybrid means mixing on-premises and cloud infrastructure, multi-cloud means mixing providers within the cloud. Many large organizations are doing both at the same time!

### Why Organizations Adopt Multi-Cloud

Multi-cloud adoption usually traces back to a combination of strategic intent and organizational reality:

- **Provider specialization.** Different cloud providers have genuinely different strengths. An organization running large-scale machine learning workloads might find one provider's GPU infrastructure and managed ML tooling significantly more capable than another's. The same organization might rely on a different provider for enterprise identity management because its existing corporate identity systems integrate more cleanly there. Rather than forcing all workloads onto a single provider's tooling, multi-cloud allows teams to use the best available tool for each job.

- **Avoiding vendor lock-in.** Organizations that have built deeply on a single provider's proprietary services sometimes find that switching is more expensive than they anticipated. Proprietary API formats, native data storage systems, data transfer costs, and tight integrations with managed services all create migration friction. Distributing workloads across providers is one way to preserve optionality: no single provider holds the entire relationship.

- **Regulatory distribution.** Some organizations operate in regions or industries where no single provider can meet all regulatory requirements. A financial services firm might need to satisfy data residency requirements across multiple countries, and different providers may be the approved choice in different jurisdictions. 

- **Resilience.** A provider-level outage, while rare, affects every workload running on that provider. Organizations that can tolerate no single failure point across their entire infrastructure sometimes distribute critical workloads across providers so that a failure in one doesn't bring down everything.

- **Acquisition and merger history.** Multi-cloud environments can be an artifact of organizational history rather than a planned architecture. For example, a team might choose a particular cloud vendor, then their organization acquires a company who was using a different cloud vendor. The new org brings its systems and cloud infrastructure along, and the overall system now has multiple cloud relationships to manage.

### Operational Tradeoffs

The advantages of a multi-cloud strategy come with corresponding complexity and costs:

- **Increased operational complexity.** Each provider has different APIs, different tooling, different deployment mechanisms, different networking models, and different pricing structures. Teams that operate across multiple providers need to maintain fluency, and often separate tooling, for each system. A runbook that works for deploying to one provider will not necessarily transfer to another.

- **Harder to achieve consistency.** Security policies, tagging conventions, network configurations, and access models need to be defined and enforced independently across each provider. What is a policy attached to an IAM role in one provider could be a different configuration object with different syntax in another. Maintaining consistent policy enforcement across providers requires deliberate investment.

- **Data transfer costs.** Moving data between cloud providers is not free, and the costs can be significant. An architecture where services in different providers communicate frequently or share large datasets may incur data egress fees that erode the cost benefits of using the better-suited provider.

- **Reduced negotiating leverage.** Concentrating spend with a single provider often unlocks enterprise discount agreements and committed use pricing. Splitting spend across multiple providers may reduce access to those arrangements.

## Operational & Governance Implications

The combination of hybrid environments and multi-cloud adoption creates governance challenges that don't exist in simpler architectures. The following challenges apply in both contexts and tend to compound when they appear together.

### Identity and Access Challenges

In a single cloud environment, access management is handled by one IAM system. When we add an on-premises environment or a second cloud provider, we add identity systems that don't automatically trust each other.

Consider a scenario: an engineer needs to deploy code to a service that runs in the cloud, but that service depends on a database that runs on-premises. To do their job, they need credentials that work in both places. If the organization has not set up identity federation between its on-premises directory and its cloud IAM, that engineer might end up with two separate accounts, two separate login flows, and two separate sets of permissions that don't mirror each other. When they leave the organization, both accounts need to be deprovisioned. If only one is, a stale credential remains active in the other environment.

This is how access management complexity compounds. Multiply that by hundreds of engineers and dozens of services, and maintaining a consistent and auditable access model becomes a significant operational challenge.

**Identity federation**—connecting separate identity systems so that a principal authenticated in one is trusted by another—is the primary mechanism organizations use to manage this. A federated setup benefits both team members and the organization: 
- Accessing resources is unified: Engineers can log in once with their corporate credentials and access resources in both environments without maintaining separate accounts. 
- Deprovisioning is centralized: removing the account in one place removes access everywhere.
- Auditing is unified: a single access log spans both environments.

When federation is absent or partially implemented, the gap becomes a security liability and an operational burden.

In multi-cloud environments, this extends further. Each provider may have a different IAM model with different permission granularity and different policy syntax. Organizations operating across multiple providers typically need either a centralized identity provider that all platforms federate with, or they accept the operational cost of managing separate access models in each provider and reconciling them manually.

The principle of least privilege is harder to maintain when access spans multiple systems. Auditing who has access to what is harder when logs are in separate places that don't aggregate automatically.

### Observability Complexity

In the observability lesson, we covered how distributed systems require teams to correlate signals across multiple services to diagnose a problem. Hybrid and multi-cloud environments extend that problem: signals are now scattered across environments that may have entirely separate monitoring tools, log storage systems, and tracing infrastructure.

Imagine this: a user submits an order through a cloud-hosted web application. The order validation service, also in the cloud, looks fine. But the order reaches the on-premises fulfillment system and fails silently. No alert fires. The cloud monitoring dashboard shows a healthy API. The on-premises monitoring system, which uses entirely different tooling, shows an error that nobody on the cloud operations team can see without logging into a separate console.

This is the observability gap that multi-environment architectures create. A request that crosses the network boundary may cross a monitoring boundary at the same time. Unless we invest in cross-environment observability practices like unified log aggregation, distributed tracing that spans both environments, and alerting that correlates signals regardless of where they originate, we'll have blind spots at exactly the points where complex failures are most likely to appear.

Multi-cloud compounds this. Each provider has its own native monitoring and logging tooling, with different query languages, different retention policies, and different alert mechanisms. Teams operating across providers frequently end up with a fragmented view of their own systems unless they deliberately invest in finding or creating aggregation infrastructure that pulls from all sources to allow a full view of their application and services.

### Cost Visibility

In a single cloud environment, cost visibility is primarily a discipline and tagging problem: if we tag resources consistently and organize accounts by team or service, we can generate reasonably accurate reports of who is spending how much and on what.

In hybrid environments, cloud costs and on-premises costs live in different systems. Cloud spending appears in the provider's billing console, billed by usage. On-premises costs are capital expenditures and operational costs tracked in finance systems with different cadences, different categorizations, and different levels of granularity. Comparing the real cost of running a workload on-premises against running it in the cloud requires pulling data from both systems and translating it into comparable terms. This is harder than it sounds, and frequently doesn't happen until there is a specific reason to investigate.

In multi-cloud environments, cloud costs are distributed across multiple providers' billing systems. An engineering team might have real-time visibility into one provider's spend and only monthly visibility into another's. Data egress fees that get charged when data moves between providers may not appear in either provider's cost allocation in a way that makes the cross-provider data flow obvious.

Without cost visibility, teams find out about budget problems after they've already happened. An architecture that looked efficient at design time may have hidden costs that only surface at billing time. Just like with observability, organizations operating across multiple providers often need dedicated tooling to aggregate cost data into a single view.

### Increased failure surface area

Every integration point between environments is a potential failure point. 
- Within a single cloud environment, a service that depends on another service in the same region travels across the provider's internal network. It's fast, reliable, and subject to the provider's uptime guarantees. 
- When that dependency crosses the network boundary between environments, it becomes subject to additional latency, packet loss, and failure modes that don't exist within a single environment, like VPN gateway restarts or dedicated connection interruptions.

Consider the ordering system example from the "Observability Complexity" section above. When the network connection between the cloud-hosted web application and the on-premises billing system is interrupted, requests don't fail cleanly, they time out. The web application's retry logic kicks in, generating more requests that also time out. The on-premises billing system, when the connection recovers, sees a backlog of duplicate requests. What began as a network blip cascades into a data consistency problem.

This is how the blast radius of failures expands in hybrid architectures. A failure in one environment can propagate into another in ways that aren't obvious from looking at either environment in isolation. Services that communicate across the boundary need explicit failure handling: defined timeouts, circuit breakers to prevent hammering slow or broken dependencies with requests, fallback behaviors, and clear documentation of what happens when the cross-environment dependency is unavailable.

Multi-cloud environments have similar exposure. A service in one provider that depends on a service in another has a cross-provider network path, and a service outage on one side may not be visible to monitoring on the other.

Planning for failure means understanding not just the failure modes within each environment, but the failure modes at the boundary between them. Understanding the failure surface means understanding:
- every place where traffic crosses an environment boundary
- what happens to dependent services when that crossing fails
- whether the monitoring on both sides of the boundary can detect a failure at the boundary itself

These require teams to reason about the system as a whole, not just the slice of it they own.

## Summary

Hybrid and multi-cloud architectures are less about choosing a single approach and more about understanding what is driving the design. A hybrid environment may be a temporary state during a migration, or it may be the permanent, intended design. The distinction matters because it changes how we plan. 

When the hybrid state is transitional, we build cross-environment tooling knowing it will eventually be retired. When it is long-term, those same tools become permanent infrastructure that needs sustained investment. Regulatory constraints, specialized hardware that has no cloud equivalent, and the economics of predictable workloads are all legitimate reasons an organization might remain in a hybrid configuration indefinitely. Treating a long-term hybrid architecture like a migration in progress leads to under-investment in the permanent tooling it actually needs: federated identity, cross-environment monitoring, and cost governance built to last rather than to be dismantled.

Multi-cloud environments introduce a parallel layer of complexity. Rather than bridging on-premises and cloud, we are coordinating across providers who each have their own IAM model, networking primitives, deployment tooling, and monitoring systems. Organizations end up here for reasons ranging from deliberate strategies, like matching workloads to each provider's strengths or avoiding dependence on a single vendor, to organizational history, such as an acquisition that brought a different cloud relationship along. In either case, the operational challenges compound. 

Identity systems that don't automatically trust each other require federation to prevent fragmented access and stale credentials. Monitoring tools that speak different languages across providers create blind spots exactly where failures are most likely to cross boundaries. Cost visibility, already a discipline problem in a single environment, becomes harder when spend is spread across providers with different billing models. The core principle across all these topics is: complexity at the boundaries between environments requires deliberate, sustained, investment and attention; it doesn't manage itself.

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: bK7mR2xQ9nT4pL6wJ3sY8dA1vC5eF0
* title: Hybrid and Multi-Cloud Environments
##### !question

A retail company runs its inventory management system on-premises due to strict internal data governance policies, while its customer-facing storefront runs in the cloud. The team has been told this setup is permanent, not a stepping stone toward full cloud adoption.

What does this distinction most directly affect?

##### !end-question
##### !options

* Whether the company needs to pay for cloud services
* Whether the cross-environment tooling should be treated as long-term or temporary
* Whether the on-premises system needs to be rewritten before it can talk to the cloud
* Whether the company is eligible for enterprise cloud discount programs

##### !end-options
##### !answer

* Whether the cross-environment tooling should be treated as long-term or temporary

##### !end-answer
##### !explanation

When a hybrid environment is intentional and long-term rather than a migration in progress, the tooling that bridges the two environments (identity federation, cross-environment monitoring, cost governance) must be built and maintained as permanent infrastructure. Treating it as temporary leads to under-investment in systems the organization will rely on indefinitely.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: Fh2vM7tL0kB4wR9xC6pN3sJ8eQ1dU5
* title: Hybrid and Multi-Cloud Environments
##### !question

An e-commerce platform hosts its web application in the cloud, which connects to an on-premises payment processing system. When the network link between the two environments goes down briefly, the web app's retry logic floods the payment system with duplicate requests. When the connection recovers, the payment system processes them all, resulting in duplicate charges.

What does this scenario illustrate about failures in hybrid architectures?

##### !end-question
##### !options

* Network outages in hybrid environments are always caused by misconfigured VPN tunnels
* Failures at the boundary between environments can cascade and impact both sides 
* The payment system should have been migrated to the cloud to avoid this issue
* Retry logic should never be used in cloud-connected applications

##### !end-options
##### !answer

* Failures at the boundary between environments can cascade and impact both sides 

##### !end-answer
##### !explanation

In hybrid architectures, what begins as a localized network blip can cascade into larger problems, in this case, a data consistency issue with real business consequences. Services communicating across environment boundaries need explicit failure handling: defined timeouts, circuit breakers, and fallback behaviors. 

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: Dj6aW1yH3nF8tP5kR0mE2vC9bX4qG7
* title: Hybrid and Multi-Cloud Environments
##### !question

A growing financial services firm recently acquired a smaller company that used a different cloud provider. Post-acquisition, engineers from both organizations need access to shared services. Without any integration between the two identity systems, what is the most likely operational result?

##### !end-question
##### !options

* Engineers automatically gain access to resources in both environments through single sign-on
* Engineers end up with separate accounts in each system, making consistent access auditing and deprovisioning difficult
* The acquired company's cloud environment must be shut down immediately
* The firm loses compliance status until both environments are merged into one

##### !end-options
##### !answer

* Engineers end up with separate accounts in each system, making consistent access auditing and deprovisioning difficult

##### !end-answer
##### !explanation

Without identity federation connecting the two systems, engineers require separate accounts and separate credentials for each environment. This compounds access management complexity: when someone leaves the organization, both accounts must be deprovisioned. If one is missed, a stale credential remains active.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->