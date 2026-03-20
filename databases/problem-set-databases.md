# Problem Set: Cloud Databases

## Directions

Complete all questions below.

## Practice

Which of the following is a reason to use a self-hosted database over a fully managed database?
Which of the following is handled by the cloud provider when using a fully managed database?
Which of the following is not a benefit of relational databases?
Which of the following is common use case for a(n) X database?
can have questions for each or several of the NoSQL types covered
Given x requirements, what style of database would fit the feature best?
Given x requirements would vertical or horizontal scaling best fit the needs?
Which of the following is a limitation of read replicas?

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