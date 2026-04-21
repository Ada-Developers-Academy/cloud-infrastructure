# Cloud Migration Strategies

## Learning Goals
- Identify each of the 7 Rs of migration and describe when each is appropriate.
- Explain the tradeoffs between lift-and-shift and re-architecture approaches across speed, cost, complexity, and long-term value.
- Describe why governance matters in cloud and hybrid environments and explain the shared responsibility concept.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Lift-and-shift** | Moving an application from on-premises to the cloud without making changes to the code or architecture | Rehost, migrate-as-is | "We chose a lift-and-shift approach for the billing service so we could meet the compliance deadline without rewriting the application." |
| **Re-architecture** | Redesigning an application to take advantage of cloud-native features, such as containers, managed services, or serverless compute | Refactor, cloud-native rewrite | "Re-architecting the API layer took three months, but it cut our infrastructure costs by half and made the service horizontally scalable." |
| **Legacy system** | An existing system built with older technologies or architectures that may be difficult to change, integrate with modern tooling, or migrate | Old system, technical debt | "Our billing service is a legacy system running on a 2011 OS version, it's a migration priority because the vendor stopped releasing patches." |
| **Cloud-Native** | An application designed to run in the cloud and take full advantage of cloud capabilities, typically emphasizing scalability, resilience, and automation. | Cloud-Optimized | "We built the order system as a cloud-native service with policies to auto-scale horizontally when traffic spikes around sales and holidays" |
| **Workload** | A unit of work running on a system, such as an application, service, database, or batch job | Application, service, system | "We mapped every workload to one of the 7 Rs before deciding which to migrate in the first phase." |
| **Migration readiness** | An assessment of how prepared an organization's systems, teams, and processes are to support a cloud migration | Migration assessment, cloud readiness | "Before committing to a timeline, our team ran a migration readiness assessment to identify which workloads had unresolved dependencies." |
| **Cloud governance** | The policies, controls, and processes that define how cloud resources can be used, provisioned, and secured within an organization | Cloud policy, cloud guardrails | "Without cloud governance in place, developers were spinning up resources in every region and the monthly bill was impossible to predict." |
| **Shared responsibility model** | The division of security and operational responsibility between a cloud provider and the customer using cloud services | Provider/customer boundary, security division | "Under the shared responsibility model, our team owns the application security and access configuration; the provider handles physical infrastructure." |
| **Policy guardrail** | An automated rule or constraint that prevents cloud resources from being created or configured in ways that violate an organization's policies | Policy boundary, preventive control | "A policy guardrail prevented engineers from opening port 22 to the public internet, even if they tried to do it accidentally." |

## The 7 Rs of Migration

Call out Lift-and-Shift vs Re-Architecture and explain when each is appropriate and their tradeoffs (speed, cost, complexity, long-term value)
Include before/after architecture diagram of each approach

1. Retire: remove outdated, unused applications. Eliminates unnecessary maintenance and reduces complexity in the migration process. 
2. Retain: keep certain workloads on-prem for now since they might not be ready for migration or don’t provide enough value to justify the effort. They retain on-prem till a better opportunity for migration arises.
3. Rehost: lift and shift, move apps to the cloud with minimal or no changes. Fast and cost effective, but doesn’t leverage cloud native features. An org might do this for speed in order to ensure compliance with new regulations without overhauling the application
4. Replatform: lift, tinker, and shift. This allows for optimizations to apps during migration (like upgrading DBs or OS to improve performance in the cloud).This could help reduce operational costs.
5. Refactor or re-architecting: redesigning apps to take full advantage of cloud-native features (such as microservices, cloud computing, containers, etc). This requires more time and resources, but delivers significant long term benefits.
6. Repurchase: replace application entirely with a cloud-based alternative aka drop and shop. Example: Migrate your customer relationship management (CRM) system to Salesforce.com.
7. Relocate: Optional, relocating transfers workloads by moving virtual machines directly between environments without modifying applications. It is designed for speed with minimal disruption and no reconfiguration.


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