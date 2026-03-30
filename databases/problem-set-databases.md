# Problem Set: Cloud Databases

## Directions

Complete all questions below.

## Practice

<!-- Self-Hosted vs Managed Database questions -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: b5b8d02b-2026-445f-8d7e-e6e4736e2132
* title: Problem Set: Cloud Databases
##### !question

A hospital IT team is using a fully managed cloud database service to store patient appointment records. One morning, a critical security vulnerability is disclosed for the database engine version they are running. Which of the following best describes how the team should respond?

##### !end-question
##### !options

* The team must immediately download and apply the patch manually to the database server
* The team should spin up a new self-hosted instance with the patched version and migrate their data
* The cloud provider will handle patching the database engine; the team should monitor for confirmation and verify application behavior after the update
* The team must open a support ticket and wait for the provider to grant them SSH access so they can apply the patch themselves

##### !end-options
##### !answer

* The cloud provider will handle patching the database engine; the team should monitor for confirmation and verify application behavior after the update

##### !end-answer
##### !explanation

With a fully managed database service, the cloud provider is responsible for applying security patches and engine updates, the team does not need to manually access or patch the underlying server. The team's responsibility is to monitor that the update completed successfully and verify that their application continues to behave as expected afterward. Direct server access via SSH is typically not available on managed services, and migrating to a self-hosted instance would be unnecessary and likely costly here.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 155843a9-c4f3-4ca5-9e8c-b6f7da6e786f
* title: Problem Set: Cloud Databases
##### !question

A media streaming company self-hosts their database on cloud virtual machines. Their lead database administrator leaves the company, and no other team member has experience managing the database infrastructure. Three months later, the database crashes due to a storage volume filling up. The team has no runbook for recovery and is unsure whether any recent backups exist. Which characteristic of self-hosted databases most directly contributed to this situation?

##### !end-question
##### !options

* Self-hosted databases do not support storage auto-scaling under any circumstances
* Self-hosted databases on cloud VMs cannot be monitored with standard alerting tools
* The team bears full responsibility for operational tasks like storage management, backup configuration, and recovery procedures
* Self-hosted databases require a dedicated on-call team available 24/7 to remain compliant with cloud provider terms

##### !end-options
##### !answer

* The team bears full responsibility for operational tasks like storage management, backup configuration, and recovery procedures

##### !end-answer
##### !explanation

With a self-hosted database, every operational responsibility, including monitoring storage capacity, configuring and testing backups, documenting recovery procedures, belongs to the team. There is no provider safety net. When the person who held that knowledge left, the operational risk went unaddressed. Self-hosted databases can be monitored with standard tools and can support storage scaling, but both require the team to configure them.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 966e227a-d531-4d78-a215-a4645c38a7b9
* title: Problem Set: Cloud Databases
##### !question

A three-person engineering team at an early-stage startup is evaluating their database options. A self-hosted database on a cloud VM would cost roughly 30% less per GB of storage than a comparable managed service. A senior engineer argues the team should choose the managed service despite the higher storage cost. Which of the following best supports the senior engineer's position?

##### !end-question
##### !options

* Managed services always outperform self-hosted databases regardless of workload size
* The storage cost difference will shrink as the startup's data volume grows, making managed services cheaper at scale
* For a small team, the time required to handle patching, incident response, and backup management on a self-hosted database could exceed the cost savings from cheaper storage
* Managed services provide dedicated database administrators as part of the service fee, eliminating the need for any internal database expertise

##### !end-options
##### !answer

* For a small team, the time required to handle patching, incident response, and backup management on a self-hosted database could exceed the cost savings from cheaper storage

##### !end-answer
##### !explanation

The higher per-GB cost of a managed service reflects the operational automation and vendor expertise built into the service. For a small team, the engineering hours required to patch, monitor, back up, and respond to incidents on a self-hosted database can significantly outweigh the storage cost savings. Managed services do not provide dedicated database admins, the team is still responsible for schema design, query optimization, and scaling decisions. Performance and cost at scale depend on workload and design, not only the hosting model.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- Database Type questions -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 4062255a-e705-4ad3-b28b-afb6a5ac1237
* title: Problem Set: Cloud Databases
##### !question

A mobile game needs to display a live leaderboard that updates player scores within milliseconds as matches finish. The dataset is relatively small, but the system must handle thousands of score reads and writes per second. Which type of database is best suited for this use case?

##### !end-question
##### !options

* Relational database, because it enforces schema and provides strong consistency for score records
* Document database, because flexible schemas allow different players to have different stat structures
* Graph database, because player rankings reflect relationships between entities
* In-memory database, because data is served from RAM for the fastest possible read and write speeds

##### !end-options
##### !answer

* In-memory database, because data is served from RAM for the fastest possible read and write speeds

##### !end-answer
##### !explanation

In-memory databases serve data from RAM rather than disk, giving them response times measured in microseconds rather than milliseconds. This makes them the best fit for real-time leaderboards, where player scores are written and read constantly and rankings must update nearly instantly. The dataset is also small enough to fit comfortably in memory, which addresses the main cost constraint of in-memory databases. Relational and document databases are not optimized for this access pattern, and a graph database is designed for traversing complex relationships, not for high-throughput score reads and writes.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 2c1c764f-2c92-4ba1-8bc2-eb1bc38e78c0
* title: Problem Set: Cloud Databases
##### !question

A development team is building a banking application where customers transfer money between accounts. Which of the following is a key benefit of using a relational database for this use case?

##### !end-question
##### !options

* Flexible schema that can change without migrations
* Strong consistency and ACID transactions that prevent double-withdrawals
* Horizontal scaling across many nodes without additional configuration
* Data is stored in RAM for extremely fast read and write speeds

##### !end-options
##### !answer

* Strong consistency and ACID transactions that prevent double-withdrawals

##### !end-answer
##### !explanation

Relational databases provide strong consistency guarantees, meaning every read reflects the most recent write. Combined with ACID (Atomicity, Consistency, Isolation, Durability) transaction support, this makes relational databases the right tool when data integrity is critical — such as ensuring a withdrawal from one account and a deposit to another either both succeed or both fail. Flexible schemas are a feature of non-relational databases, horizontal scaling without extra configuration is a strength of most NoSQL databases, and in-memory speed is a property of in-memory databases specifically.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: cd2e3d8a-5a51-4534-81ea-3f8970f6e717
* title: Problem Set: Cloud Databases
##### !question

A social media platform expects user traffic to grow rapidly and unpredictably. Their engineering team wants a database that can scale out by adding more servers as load increases, rather than being limited by the capacity of a single machine. Which of the following best describes this as an advantage of non-relational databases?

##### !end-question
##### !options

* Non-relational databases enforce strict schemas, preventing inconsistent data at scale
* Most non-relational databases are designed for horizontal scaling across many nodes
* Non-relational databases use JOIN operations to keep data normalized across tables
* Non-relational databases require less application-level validation than relational ones

##### !end-options
##### !answer

* Most non-relational databases are designed for horizontal scaling across many nodes

##### !end-answer
##### !explanation

Most non-relational databases are architected for horizontal scaling — distributing data and workload across many nodes as demand grows. This is one of their core advantages over relational databases, which scale vertically more easily but require significant additional strategy (like sharding) to scale horizontally. Non-relational databases typically have flexible or no enforced schemas (putting validation responsibility on the application), and they generally avoid JOINs rather than supporting them.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- Scaling questions -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: b989fa77-c055-4189-a509-3c9693494053
* title: Problem Set: Cloud Databases
##### !question

An engineering team has been vertically scaling their managed relational database for two years. They have just upgraded to the largest instance tier their cloud provider offers. Query latency is starting to climb again as traffic continues to grow. What is the most accurate description of their situation?

##### !end-question
##### !options

* They should delete old records to free up capacity on the current instance
* They have reached the ceiling of vertical scaling and must now consider horizontal scaling strategies
* They should switch to a NoSQL database since relational databases cannot be horizontally scaled
* They should move to a self-hosted database on a virtual machine to access larger hardware

##### !end-options
##### !answer

* They have reached the ceiling of vertical scaling and must now consider horizontal scaling strategies

##### !end-answer
##### !explanation

Vertical scaling has a hard ceiling: once the largest available instance tier is in use, there is nowhere left to scale up. At this point, horizontal scaling strategies, such as adding read replicas or evaluating sharding, become necessary. Deleting records doesn't address performance. Relational databases can be horizontally scaled, though it requires more planning than with some NoSQL database types. Switching to self-hosted hardware doesn't remove the fundamental hardware ceiling.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: c109e515-3904-4c6e-8e8f-68affdb8b0fc
* title: Problem Set: Cloud Databases
##### !question

An IoT company ingests sensor data from millions of devices. Write volume is extremely high and has outgrown what their current single database node can handle. Read latency is acceptable. Which combination of factors best describes the appropriate next step?

##### !end-question
##### !options

* Add read replicas, since read replicas distribute both read and write traffic across nodes
* Vertically scale the database instance, since all writes go to a primary node and adding resources will directly improve write throughput
* Evaluate sharding to distribute write traffic across multiple nodes, since read replicas do not address write throughput
* Migrate to a relational database, since relational databases are optimized for high write throughput at scale

##### !end-options
##### !answer

* Evaluate sharding to distribute write traffic across multiple nodes, since read replicas do not address write throughput

##### !end-answer
##### !explanation

Read replicas only distribute read traffic, all writes still flow through the primary node, so they do not help a write-heavy bottleneck. Vertical scaling is a valid first step but has a ceiling and may already be exhausted. Sharding partitions data across multiple independent nodes, distributing both write traffic and dataset size. This is the right lever when write throughput has outgrown a single instance. Relational databases are actually harder to shard than most NoSQL databases, making a NoSQL key-value or document store potentially a better long-term fit for this workload.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->