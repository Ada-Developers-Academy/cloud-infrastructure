# Database Types & Tradeoffs

## Learning Goals

- Describe key differences between relational and non-relational databases
- Describe common cloud patterns for relational databases
- Describe common kinds of non-relational databases and use cases for each kind

## Types of Databases 

Whether hosted locally or in the cloud, databases are typically divided into two categories: relational, and non-relational. Since SQL databases are covered in the core curriculum as an example of relational databases, we will spend less time covering their data representation in this lesson. Feel free to revisit the Unit 2 SQL lessons for a refresher if desired!

## Relational Databases

Relational databases store data in tables made up of rows and columns, with defined schemas that enforce data structure and relationships between tables. Their core strengths are:
- **Strong consistency** - once data is written, every subsequent read is guaranteed to reflect the most recent data.
- **Normalization** - organizing the attributes and connections in tables to remove redundant information. This helps reduce database size and keeps our data more accurate by having a single source of truth. 
- **Complex queries** - JOIN operations across tables enable complex associations that help answer very specific questions about our data. 

These databases excel when data integrity matters most, like financial systems, order management, and user account data.

### In the Cloud 

As we saw in the previous lesson, we have a choice between self-hosting or using a managed service for relational databases in the cloud. If we self- host, then the choice of database engine type and version is all ours!

If we're using a managed service, major cloud providers offer multiple relational database engines to choose from such as MySQL, PostgreSQL, and SQL Server. Some cloud vendors also offer proprietary relational engines built for high-performance and heavy workloads, meant for demanding production systems. The exact engines, limits, and feature sets will vary depending on the provider.

## Non-relational (NoSQL) Databases

Non-relational databases are a category of databases rather than a specific type. Often called NoSQL databases, these store information using data models with flexible schemas instead of rigid tables and rows. The structure of data is often enforced by the application rather than the database itself. Unlike relational databases, they are optimized for high throughput and low latency by avoiding expensive JOIN operations and relaxing some consistency or normalization constraints. 

There are many types, some common ones being: key-value, document, graph, and in-memory, and each of these are built for particular data shapes and access patterns. Next, we'll dive into the non-relational database types we mentioned above!
- There are other kinds of NoSQL databases built for even more specific use cases or professions that we will not cover here; feel free to follow your curiosity if you'd like to know more!

### Key-Value 

**Strengths & Limits**



**Use Cases**



### Document 

**Strengths & Limits**



**Use Cases**


### In-Memory 

**Strengths & Limits**



**Use Cases**


### Graph

**Strengths & Limits**



**Use Cases**



## Summary

### Relational vs Non-relational: Key Differences & Tradeoffs

Describe key differences between relational and non-relational databases

### Non-Relational Databases



## Check for Understanding
 
Which of the following is a benefit of relational databases?
Which of the following is a benefit of NoSQL databases?
Given x requirements, what style of database would fit the feature best?