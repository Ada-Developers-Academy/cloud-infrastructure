# Database Types & Tradeoffs

Whether hosted locally or in the cloud, databases are typically divided into two categories: relational, and non-relational. Since SQL databases are covered in the core curriculum as an example of relational databases, we will spend less time covering their data representation in this lesson. Feel free to revisit the Unit 2 SQL lessons for a refresher if desired!

## Learning Goals

- Describe key differences between relational and non-relational databases.
- Describe common kinds of non-relational databases and their strengths & tradeoffs.
- Describe use cases for relational databases and common kinds of non-relational databases.

## Relational Databases

Relational databases store data in tables made up of rows and columns, with defined schemas that enforce data structure and relationships between tables. Their core strengths are:
- **Strong consistency** - once data is written, every subsequent read is guaranteed to reflect the most recent data.
- **Normalization** - organizing the attributes and connections in tables to remove redundant information. This helps reduce database size and keeps our data more accurate by having a single source of truth. 
- **Complex queries** - JOIN operations across tables enable complex associations that help answer very specific questions about our data. 

These databases excel when data integrity matters most, like financial systems, order management, and user account data.

## Non-relational (NoSQL) Databases

Non-relational databases are a category of databases rather than a specific type. Also called NoSQL databases, these store information using data models with flexible schemas instead of rigid tables and rows. The structure of data is often enforced by the application rather than the database itself. Unlike relational databases, they are optimized for high throughput and low latency by avoiding expensive JOIN operations and relaxing some consistency or normalization constraints. 

There are many types of non-relational databases, some common ones being: key-value, document, graph, and in-memory. Each of these are built for particular data shapes and access patterns. Next, we'll dive further into the non-relational database types we mentioned above!
- There are other kinds of NoSQL databases built for even more specific use cases or professions that we will not cover here; feel free to follow your curiosity if you'd like to know more!

### Key-Value Databases

**Data Representation**

A key-value database is in some ways like a dictionary data structure. Data is stored as a collection of pairs: a unique key and a value associated with it. The key functions like a lookup identifier, and the value can hold almost anything: a string, a number, a JSON blob, or a binary object. There is no enforced schema for the shape of values and the database itself does not support relationships between entries. We ask for a key, and we get back its value. That's the entire model!

**Strengths & Limits**

The primary strength of key-value databases is raw speed and horizontal scalability; they distribute well across many nodes, making them capable of handling enormous volumes of traffic. Because lookups are simple and direct, read and write operations are extremely fast. The main tradeoff is that this simplicity is also a constraint: key-value stores don't support the complex queries, filtering, or joins of relational databases. If we need to find all records that share a particular attribute, we have to know the key ahead of time or build that logic into our application. They're built for speed, not flexibility.

**Use Cases**

Key-value databases can be useful when data needs to be accessed quickly by a known identifier. A very common use case is session management: when a user logs into a web application, their session token becomes the key and their session data is the value, making lookup nearly instantaneous. Another strong use case is shopping cart storage for e-commerce platforms. A user's ID is the key, and their cart contents are the value, which can be read and updated rapidly even during high-traffic sale events. 

### Document Databases

**Data Representation**

Document databases store data as individual documents, typically in a JSON or JSON-like format such as BSON. Unlike rows in a relational table, each document can have its own structure. One document in a collection might have five fields while another has fifteen, and neither has to match a rigid schema. Related data is often embedded directly inside a document rather than split across multiple tables. For example, a blog post document might contain the title, author, body, and an array of comments all nested together as a single record.

**Strengths & Limits**

The flexibility of the document model is its core strength. Because we can represent a rich, nested structure inside one record, reads are often faster than in a relational database, which would often need multiple joins to assemble the same data. Document databases also scale horizontally well and tolerate evolving data shapes, which is useful when product requirements or types change frequently. 

The tradeoff is that this flexibility puts more responsibility on the application layer. Since there's no enforced schema, inconsistent data can creep in if validation isn't handled carefully in code. Complex queries that span many documents or require aggregating data across fields can also be slower and harder to optimize than equivalent SQL queries.

**Use Cases**

Document databases are often seen in content management systems, where different content types, like articles, videos, and product listings, each have different fields but are all queried in similar ways. They're also widely used for user profile storage: a user record might store preferences, settings, notification history, and linked accounts all in one place. The document model lets that structure grow over time without requiring schema migrations. 

### Graph Databases

**Data Representation**

Graph databases store data as a network of nodes and edges, where nodes represent entities (like a person, a product, or a location) and edges represent the relationships between them (like "follows," "purchased," or "located in"). Both nodes and edges can carry properties as metadata attached directly to them. The key distinction from relational databases is that relationships are first-class citizens, stored natively in the structure of the database rather than inferred at query time through JOIN operations. This means traversing deeply connected data is dramatically faster because the database doesn't need to scan large tables to find related records.

**Strengths & Limits**

The strength of graph databases is in queries that require hopping across many layers of relationships, an action that becomes exponentially expensive in a relational database. The tradeoff is that graph databases aren't a general-purpose tool. They perform poorly at the kinds of bulk aggregations and tabular reporting that relational databases handle well. They are also typically harder to scale horizontally than key-value or document stores, and require learing their query languages, like Cypher or Gremlin. 

**Use Cases**

One of the most impactful use cases for graph databases is fraud detection. A financial platform can model customers, accounts, devices, and transactions as a graph, then quickly identify suspicious patterns like multiple accounts sharing a device or a ring of accounts that all transact with each other in a circular pattern. 

Another classic use case is recommendation engines. A streaming platform can model users, content, ratings, and viewing history as a graph to answer questions like "What content have people with similar taste to this user enjoyed but this user hasn't seen yet?" 

### In-Memory Databases

**Data Representation**

In-memory databases are distinct from other kinds of databases because they serve data from RAM rather than from disk. They persist data to disk asynchronously for durability while still serving reads and writes from memory. All other database types ultimately read from and write to a hard disk, which introduces latency at the hardware level. 

RAM access is orders of magnitude faster than even the fastest SSD. In-memory databases take advantage of this by keeping the entire working dataset in memory, resulting in response times measured in microseconds rather than milliseconds. 
- It's important to note that there is a difference between an in-memory database and an in-memory cache; caches don't persist data to disk and treat data loss as acceptable.

**Strengths & Limits**

The defining strength of in-memory databases is speed: no other database category comes close for raw read and write throughput. However, this speed comes with constraints around cost vs data size. RAM is significantly more expensive per gigabyte than disk storage, so in-memory databases are typically reserved for datasets where speed matters more than storage size. 

**Use Cases**

Given their constraints, in-memory databases are best for small amounts of data that need to be read & written constantly. A common use case is application caching: rather than querying a relational database for the same product data on every page load, results are cached in an in-memory store and served directly from there until the cache is invalidated, typically after some period of time. This dramatically reduces load on the primary database during traffic spikes. Another common use case is real-time leaderboards in gaming applications: player scores are written and read constantly, and rankings need to update within milliseconds. 

## Quick Comparison Tables

### Relational vs Non-Relational: Key Differences 

| | Relational | Non-Relational |
|---|---|---|
| **Data Structure** | Tables with rows and columns | Varies by type: key-value pairs, documents, graphs, or in-memory structures |
| **Schema** | Enforced and defined upfront; changes require migrations | Flexible; structure can vary between records and evolve without migrations |
| **Relationships** | First-class, defined explicitly via foreign keys and JOINs | Handled manually, via embedding, or natively in the data structure (graph) |
| **Consistency** | Strong consistency and ACID transactions by default | Varies; often trades consistency for performance and availability |
| **Scaling** | Scales vertically easily; horizontal scaling requires significant strategy to split up or "shard" the database across nodes | Most, but not all, non-relational databases are designed for horizontal scaling across many nodes |
| **Query Language** | Standardized SQL across most engines | Varies by database; no universal query standard |
| **Data Duplication** | Minimized through normalization | Common; data is often duplicated to optimize reads |
| **Best For** | Structured data with complex relationships and strict correctness requirements | Large scale, flexible, or high-performance workloads where the data model varies |

### Database Strengths & Drawbacks Comparison by Type

| Database Type | Key Strengths | Key Drawbacks | Common Use Cases |
|---|---|---|---|
| Relational *(SQL)* | Enforced schema; strong consistency & transactions; powerful querying with JOINs; well-understood and widely supported | Horizontal scaling is complex; rigid schema makes evolving data structures costly; JOIN-heavy queries slow down at massive scale | Financial transactions, order management, user accounts, any domain requiring strict data integrity |
| Key-Value *(Non-Relational)* | Extremely fast reads/writes; scales horizontally with ease; simple operational model | No support for complex queries; must know the key to retrieve data; relationships must be managed in application code | Session management, shopping carts, caching, gaming leaderboards, IoT device state |
| Document *(Non-Relational)* | Flexible schema accommodates evolving data shapes; related data can be embedded in one record reducing reads; scales horizontally well | No schema enforcement risks data inconsistency; complex cross-document queries are slow; data duplication is common | User profiles, content management systems, product catalogs, real-time big data |
| Graph *(Non-Relational)* | Relationships are first-class and stored natively; traversing deeply connected data is fast and constant cost per hop | Not suited for bulk aggregations or tabular reporting; harder to scale horizontally | Fraud detection, recommendation engines, social networks, knowledge graphs |
| In-Memory *(Non-Relational)* | Fastest read/write performance of any database type; purpose-built data structures for use cases like leaderboards and queues | RAM is expensive, limiting dataset size | Real-time leaderboards, session stores, live analytics dashboards |

## In the Cloud 

As we saw in the previous lesson, whatever kind of database we choose, we can select between self-hosting or using a managed service in the cloud. If we self- host, then the choice of database engine type and version is all ours!

If we're using a managed service, major cloud providers offer multiple relational database engines to choose from such as MySQL, PostgreSQL, and SQL Server. Some cloud vendors also offer proprietary relational engines built for high-performance and heavy workloads, meant for demanding production systems. 

Similarly for non-relational databases, cloud vendors often provide at least a couple options for each database type, and may have specific proprietary non-relational database offerings built for high throughput systems. The exact engines, limits, and feature sets will vary by cloud provider.

## Summary

Databases fall into two broad categories: relational and non-relational. Relational databases store data in structured tables with enforced schemas, and their core strengths are strong consistency, normalization to reduce redundant data, and powerful JOIN-based querying. They are the right tool when data integrity is critical, such as in healthcare records or order management. 

Non-relational (NoSQL) databases are not a single type but a family of database models that trade some consistency and structure for flexibility, speed, and scale. Four common types are: key-value, document, graph, and in-memory. Each is optimized for a specific data shape and access pattern:
- key-value stores excel at fast lookups by a known identifier
- document databases handle flexible, nested data structures well
- graph databases make traversing complex relationships fast
- in-memory databases offer the fastest possible read and write speeds by serving data from RAM

No single database type is universally better than another. The right choice depends entirely on our workload! 
- Relational databases are ideal for structured data with complex relationships and strict correctness requirements, but can struggle to scale horizontally. 
- Non-relational databases are typically designed with horizontal scaling in mind, making them better suited for large-scale or high-performance workloads, though each comes with its own tradeoffs around query flexibility, consistency, cost, and operational complexity. 

In practice, many production systems use more than one type of database to meet different needs within the same application. Whether we self-host or use a managed cloud service, understanding these tradeoffs is essential to making database decisions that balance performance, reliability, and cost.

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: bb398606-d54b-4c82-aee7-3c733cd3da22
* title: Database Types & Tradeoffs
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

Relational databases provide strong consistency guarantees, meaning every read reflects the most recent write. Combined with ACID (Atomicity, Consistency, Isolation, Durability) transaction support, this makes relational databases the right tool when data integrity is critical, such as ensuring a withdrawal from one account and a deposit to another account either both succeed or both fail. 

Flexible schemas are a feature of non-relational databases, horizontal scaling without extra configuration is a strength of most NoSQL databases, and in-memory speed is a property of in-memory databases specifically.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: c8f5e478-0bc3-4673-96c1-79935df23a87
* title: Database Types & Tradeoffs
##### !question

A content management system stores many different types of content: articles with a title and body, videos with a duration and transcript, and product listings with a price and inventory count. The team expects new content types to be added frequently. Which of the following is a benefit of using a document database for this use case?

##### !end-question
##### !options

* Each document can have its own structure, allowing different content types without schema migrations
* Document databases enforce a strict schema so all content types remain consistent
* Document databases use JOIN operations to link related content types efficiently
* Document databases store all data in RAM, ensuring fast access for any content type

##### !end-options
##### !answer

* Each document can have its own structure, allowing different content types without schema migrations

##### !end-answer
##### !explanation

Document databases store data as individual JSON-like documents, and each document can have its own structure. This flexibility means different content types like articles, videos, & product listings can coexist in the same collection without requiring every record to match a fixed schema or forcing a migration every time a new content type is added. 

Strict schema enforcement is a feature of relational databases, JOINs are a relational database concept, and in-memory storage is specific to in-memory databases.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 0c7b689b-933a-4731-803b-90f12228f409
* title: Database Types & Tradeoffs
##### !question

A financial services company wants to detect fraudulent activity by identifying patterns such as multiple accounts sharing the same device, or groups of accounts that all transfer money to each other in circular patterns. Which type of database is best suited for this use case?

##### !end-question
##### !options

* Relational database, because it enforces strict schema and strong consistency
* Key-value database, because it can look up account records instantly by ID
* Graph database, because relationships between entities are stored natively and traversed efficiently
* In-memory database, because fraud checks must happen in microseconds

##### !end-options
##### !answer

* Graph database, because relationships between entities are stored natively and traversed efficiently

##### !end-answer
##### !explanation

Graph databases store entities as nodes and relationships as edges, making them ideal for queries that require hopping across many layers of connections. This is exactly what fraud detection requires! 

Identifying a ring of accounts transacting circularly or multiple accounts linked to one device means traversing relationship chains that would require expensive JOIN operations in a relational database. Key-value stores can't express these relationships, and while in-memory databases are fast, speed alone doesn't address the need to model and traverse complex relationship patterns.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

