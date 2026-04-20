# Problem Set: Migration - Hybrid & Enterprise Cloud Strategy

Materials to prepare: 
Provide an explanation of a company and their workloads (example provided below)
Include a diagram of current environment
Describe any constraints (example provided below)

Prompt: Ask students to write a migration approach they would recommend. Students need to include:
benefits and tradeoffs of the approach they come up with
identify at least 1 governance consideration
be prepared to discuss in class. 

Follow up in live class session:
To bring this discussion into the classroom and answer lingering questions, class could begin by reviewing the problem set. Have students share out their recommendations/benefits/tradeoffs. 

Example scenario we could use: 
Community Health Partners is a regional healthcare provider operating 12 clinics across two states. The company has grown steadily over 15 years and currently serves approximately 200,000 active patients.
They run the following systems in a single physical location (on-prem):
Patient records system
Appointment scheduling web app
Billing and insurance processing system
Internal reporting and analytics tool
File storage for medical documents and imaging
Drivers for change:
The data center lease expires in 9 months.
Hardware is 6 years old and nearing end-of-life.
Telehealth demand has increased significantly.
The executive team wants to reduce capital expenses.
The company must comply with healthcare data regulations.
IT staff is very small (5 engineers total).
Constraints and context:
Leadership wants migration completed within 6–9 months.
Budget is moderate but not unlimited.
Security and compliance cannot be compromised.
The organization cannot afford extended service outages.
No prior cloud experience among IT staff.
The analytics tool is rarely used but important for audits.

## Intro to Cloud Migration

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
