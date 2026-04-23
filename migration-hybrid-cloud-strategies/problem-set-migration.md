# Problem Set: Cloud Migration Strategies

## Practice

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 2hAe6LkT4vXqY8wJnPsB1mGcR5
* title: Problem Set: Cloud Migration Strategies
##### !question

A manufacturing company knows the cloud would benefit their business, but their enterprise resource planning (ERP) system is tightly coupled to a database server whose vendor stopped issuing security patches six months ago. Why might the company still delay migration?

##### !end-question
##### !options

* They are waiting for cloud providers to lower their prices before committing
* The rework required to make the ERP compatible with the cloud introduces cost and risk they haven't yet planned for
* They prefer capital expenditures over operational expenditures
* Cloud environments don't support ERP workloads

##### !end-options
##### !answer

* The rework required to make the ERP compatible with the cloud introduces cost and risk they haven't yet planned for

##### !end-answer
##### !explanation

Knowing cloud migration is beneficial doesn't make it immediately achievable. When a system depends on hardware or software with no direct cloud equivalent, significant rework may be required before it can run reliably in a new environment. The degree of change required — and the cost and risk that change introduces — is one of the central challenges of any migration plan, and it can cause organizations to delay even when the business case is clear.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 5rVw1NjH3sFdO9pCuKyT7eZbX4
* title: Problem Set: Cloud Migration Strategies
##### !question

A media streaming company is expanding into Southeast Asia. Rather than leasing a new data center in the region, their infrastructure team plans to deploy into a cloud provider's local availability zones. Which migration benefit best describes the primary driver for this approach?

##### !end-question
##### !options

* Avoiding the need to write any new application code
* Accessing geographic infrastructure without building or owning physical facilities
* Eliminating the company's software licensing costs
* Replacing their existing CI/CD pipeline with a cloud-native equivalent

##### !end-options
##### !answer

* Accessing geographic infrastructure without building or owning physical facilities

##### !end-answer
##### !explanation

Cloud providers operate data centers globally. Organizations that need to serve users in new regions or meet data residency requirements can do so by selecting a cloud region — without the capital investment of constructing or leasing physical infrastructure in that location. This is a key business driver for migration.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 3mBq8YoW6nGiK2eDtRcX5pJsL9
* title: Problem Set: Cloud Migration Strategies
##### !question

A financial services firm is six months into a cloud migration. Their fraud detection service is fully running in the cloud, but their core transaction database is still on-premises and connected to the cloud environment via a VPN tunnel. What term describes the firm's current infrastructure state?

##### !end-question
##### !options

* A failed migration, since both systems should have moved simultaneously
* A fully cloud-native architecture
* A hybrid environment, where some workloads are in the cloud and others remain on-premises
* A deprecated architecture that must be retired before migration can continue

##### !end-options
##### !answer

* A hybrid environment, where some workloads are in the cloud and others remain on-premises

##### !end-answer
##### !explanation

A hybrid environment is one where some systems run in the cloud and others continue to run on-premises, typically connected through VPN tunnels or dedicated connections. This is a normal and often deliberate state during migration — not a failure. Most organizations spend extended periods in a hybrid state as they work through the technical and organizational complexity of moving each service.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: zE5nT1wCuM8bG3sXoP7jK9qA2r
* title: Problem Set: Cloud Migration Strategies
##### !question

An e-commerce company's order processing service handles millions of transactions annually and is expected to be in use for the next decade. It currently cannot scale horizontally and fails during peak traffic. Engineering leadership is willing to invest 6 months of development time on it. Which migration strategy is most appropriate?

##### !end-question
##### !options

* Retire the service since it can't handle peak load
* Rehost the service to move it quickly and revisit scaling later
* Refactor the service to take advantage of cloud-native scaling and resilience
* Replatform the service by switching to a managed database to reduce costs

##### !end-options
##### !answer

* Refactor the service to take advantage of cloud-native scaling and resilience

##### !end-answer
##### !explanation

Refactoring is justified when a system is high-value, long-lived, and has known limitations that cloud-native architecture can solve. With a decade of expected use and engineering time available, re-architecting for horizontal scaling is a sound investment. Rehosting would move the problem to the cloud without solving it, and replatforming alone wouldn't address the core architectural constraint.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: kJ9oW2rM5bX8nH7tCvA4pG1qY6
* title: Problem Set: Cloud Migration Strategies
##### !question

A healthcare company is audited and discovers that a cloud storage bucket containing patient records has been publicly accessible for four months. No one on the team was aware of the misconfiguration. Which underlying problem does this scenario best illustrate?

##### !end-question
##### !options

* The company chose the wrong migration strategy for its compliance workloads
* The absence of cloud governance allowed a misconfiguration to go undetected
* The cloud provider failed to enforce default security settings on the bucket
* The team should have retained this workload on-premises instead of migrating

##### !end-options
##### !answer

* The absence of cloud governance allowed a misconfiguration to go undetected

##### !end-answer
##### !explanation

Cloud governance includes policies and controls that prevent and detect insecure configurations. Without guardrails in place, a developer can open a storage bucket to the public without any automated check flagging the issue. This is a predictable consequence of moving fast in the cloud without governance: mistakes are easy to make and hard to detect.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: uS3dL7fI0aE9nO5vBq2mK8wR4c
* title: Problem Set: Cloud Migration Strategies
##### !question

A company is migrating a suite of 40 applications to the cloud. Their platform team has identified that 8 applications are no longer used by any team and have had zero traffic for over a year. What is the recommended action for these applications?

##### !end-question
##### !options

* Rehost them anyway to preserve them in case they are needed in the future
* Replatform them so they consume fewer cloud resources
* Retire them to eliminate unnecessary maintenance and reduce migration scope
* Retain them on-premises since they don't justify the migration effort

##### !end-options
##### !answer

* Retire them to eliminate unnecessary maintenance and reduce migration scope

##### !end-answer
##### !explanation

Retire is the appropriate strategy for workloads that are no longer in use or providing value. Migrating unused applications wastes engineering time and would result in ongoing cloud costs for resources with no business return. Identifying and retiring these early reduces complexity and lets the team focus migration effort where it delivers real value.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

## Hybrid & multi-cloud

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: Wz3nP8kE1mX6qR2yH5tB9cA4jL7dV0
* title: Problem Set: Cloud Migration Strategies
##### !question

A logistics company has manufacturing floor systems that control robotic assembly lines. These systems require specialized real-time processing hardware with no equivalent managed service in any cloud provider's catalog. The company is otherwise fully committed to cloud adoption.

Which reason best explains why these systems would remain on-premises indefinitely?

##### !end-question
##### !options

* The company lacks the budget for cloud migration
* The workload has predictable demand, making cloud pricing less attractive
* The hardware has no cloud equivalent, making migration technically infeasible
* The company's engineers are unfamiliar with cloud deployment tools

##### !end-options
##### !answer

* The hardware has no cloud equivalent, making migration technically infeasible

##### !end-answer
##### !explanation

Specialized hardware, such as manufacturing control systems or certain scientific computing clusters, may have no direct cloud equivalent. In these cases, migration is not a matter of cost or skill; it is technically infeasible. This is one of the core reasons organizations intentionally maintain long-term hybrid environments.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: Nq5sG0wD8fU2mK7yP4cT3bJ6rE1xA9
* title: Problem Set: Cloud Migration Strategies
##### !question

A software company uses Cloud Provider A for its machine learning pipelines because of superior GPU infrastructure, and Cloud Provider B for enterprise identity management because it integrates cleanly with its existing corporate directory. 

What best describes the primary operational challenge this setup introduces?

##### !end-question
##### !options

* The company must rewrite all of their applications and services to be provider-agnostic
* Using two providers requires coordination to avoid blind spots and fragmented access
* Using two providers automatically doubles the company's infrastructure costs
* The company loses access to committed use pricing from both providers

##### !end-options
##### !answer

* Using two providers requires coordination to avoid blind spots and fragmented access

##### !end-answer
##### !explanation

Multi-cloud environments multiply operational complexity because each provider has its own identity system, networking model, deployment tooling, and monitoring infrastructure. Without deliberate investment in coordination organizations end up with fragmented access management and monitoring blind spots at exactly the places where failures are most likely to cross boundaries.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: Ym9cX4rT2hK6pB0wE7jA3nS1fQ8vL5
* title: Problem Set: Cloud Migration Strategies
##### !question

A platform engineering team operates services across two cloud providers. Each provider has its own native logging and monitoring tools with different query languages and retention policies. When a critical error occurs in a user transaction that spans both providers, no single dashboard captures the full picture.

What is the term for this gap, and what does it require to address?

##### !end-question
##### !options

* A cost attribution gap; addressed by tagging resources consistently across both providers
* A vendor lock-in problem; addressed by migrating all workloads to a single provider
* An observability gap; addressed by investing in unified log aggregation and distributed tracing that spans both environments
* A federation gap; addressed by linking the two providers' IAM systems

##### !end-options
##### !answer

* An observability gap; addressed by investing in unified log aggregation and distributed tracing that spans both environments

##### !end-answer
##### !explanation

When a request crosses an environment boundary, it may also cross a monitoring boundary. Each provider's native tooling captures only its slice of the transaction. Without cross-environment observability, teams have blind spots at the exact points where complex failures are most likely to emerge.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->