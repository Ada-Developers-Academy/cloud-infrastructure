# Hybrid and Multi-Cloud Environments

## Learning Goals

- Distinguish between transitional hybrid environments and intentional long-term hybrid architectures.
- Explain why organizations adopt multi-cloud strategies and what tradeoffs come with them.
- Identify governance and operational challenges that arise when systems span multiple environments.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Multi-cloud** | A strategy in which an organization runs workloads across two or more cloud providers | , multi-cloud strategy, Polycloud | "We run our machine learning workloads on AWS and our analytics pipeline on Google Cloud; this multi-cloud environment lets us use the best tool for each job." |
| **Vendor lock-in** | The degree to which an organization's systems depend on a single provider's proprietary tools, making switching costly | Provider dependency | "Because we used the provider's proprietary event bus extensively, we hit significant vendor lock-in when we tried to move the service to a different cloud." |
| **Control plane** | The layer of infrastructure responsible for managing, configuring, and orchestrating systems (as opposed to the data plane, which handles actual traffic) | Management layer | "Losing access to the control plane didn't stop traffic from flowing, but it meant we couldn't push any configuration changes until access was restored." |
| **Federation** | The practice of linking separate identity systems so that a principal authenticated in one can be trusted by another | Identity federation, cross-domain trust | "We federated our on-premises Active Directory with the cloud identity provider so that engineers could log into cloud resources with their existing corporate credentials." |
| **Cost attribution** | The practice of tracking and assigning cloud costs to specific teams, services, or business units | Showback, chargeback | "Without cost attribution, it took us three months to realize that one team's overnight batch job was responsible for 40% of our cloud bill." |

## Hybrid Environments
On-prem and cloud coexistence
Transitional vs long-term hybrid environment
Include hybrid system diagram with clear boundary markers.

## Multi-Cloud Environments
What it a Multi-Cloud Environment?
Why organizations might adopt it
Operational tradeoffs

## Operational & Governance Implications
Identity and access challenges
Observability complexity
Cost visibility
Increased failure surface area

## Summary


## Check for Understanding
What is a primary challenge of hybrid systems?
What increases as environments become more distributed?

Written Reflection Discussion question in class:
Describe what new operational risks could appear when a company operates in both on-prem and cloud environments?