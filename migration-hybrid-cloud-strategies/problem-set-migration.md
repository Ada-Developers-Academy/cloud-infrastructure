# Problem Set: Cloud Migration Strategies

## Scenario-based Reflection

### Deliverable

Using the scenario below, create a migration plan for the services listed. 
- Determine which of the 7 R's best applies to each workload
- Discuss the benefits and tradeoffs of the approach you decide
- Identify at least 1 governance consideration

Students should be prepared to discuss their migration plan in-class. 

### Scenario

Community Health Partners is a regional healthcare provider operating 12 clinics across two states. The company has grown steadily over 15 years and currently serves approximately 200,000 active patients.

They run the following systems in a single physical location (on-prem):
- Patient records system
- Appointment scheduling web app
- Billing and insurance processing system
- Internal reporting and analytics tool
- File storage for medical documents and imaging

Organizational drivers for change:
- The data center lease expires in 9 months.
- Hardware is 6 years old and nearing end-of-life.
- Telehealth demand has increased significantly.
- The executive team wants to reduce capital expenses.
- The company must comply with healthcare data regulations.
- IT staff is very small (5 engineers total).

Further constraints and context:
- Leadership wants migration completed within 6–9 months.
- Budget is moderate but not unlimited.
- Security and compliance cannot be compromised.
- The organization cannot afford extended service outages.
- No prior cloud experience among IT staff.
- The analytics tool is rarely used but important for audits.

<!-- prettier-ignore-start -->
### !challenge
* type: paragraph
* id: 68e0b915-c75e-4fe0-a7be-3c31585dd4f3
* title: Problem Set: Cloud Migration Strategies
##### !question

Submit your migration plan below when complete.

##### !end-question
##### !placeholder

To migrate these services I would...

##### !end-placeholder
### !end-challenge
<!-- prettier-ignore-end -->

## Practice

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 2hAe6LkT4vXqY8wJnPsB1mGcR5
* title: Introduction to Cloud Migration
##### !question

A manufacturing company knows the cloud would benefit their business, but their ERP system is tightly coupled to a database server whose vendor stopped issuing security patches six months ago. Why might the company still delay migration?

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
* title: Introduction to Cloud Migration
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
* title: Introduction to Cloud Migration
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

## Strategies

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: zE5nT1wCuM8bG3sXoP7jK9qA2r
* title: Cloud Migration Strategies
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
* title: Cloud Migration Strategies
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
* title: Cloud Migration Strategies
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