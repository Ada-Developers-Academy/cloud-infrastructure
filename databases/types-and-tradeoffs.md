# Database Types & Tradeoffs

## Learning Goals

- Describe common cloud patterns for relational databases
- Describe common kinds of non-relational databases and use cases for each kind
- Describe key differences between relational and non-relational databases

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Relational Database | A database that organizes data into structured tables with predefined schemas, using rows and columns, and enforces relationships between tables through foreign keys. | -------- | "The e-commerce platform used a relational database to link its customers, orders, and products tables, making it easy to generate complex sales reports with JOIN queries." |
| Non-relational Database | A category of databases rather than a specific type. A database that stores data in flexible formats such as documents, key-value pairs, graphs, or wide columns. | NoSQL Database | "A non-relational database was chosen for the product catalog because each item had a different set of attributes that didn't fit neatly into a fixed table structure." |

## Types of Databases 

Whether hosted locally or in the cloud, databases are typically divided into two categories: relational, and non-relational. We will talk about how cloud hosting affects both types, but since SQL databases are covered in the core curriculum as an example of relational databases, we will spend less time covering their data representation through the lessons in this topic. Feel free to revisit the SQL lessons for a refresher if desired!

### Relational

Relational databases store data in tables made up of rows and columns, with defined schemas that enforce data structure and relationships between tables. Their core strengths are strong consistency, transactional guarantees, and the ability to perform complex queries using JOIN operations across related tables. They excel in use cases where data integrity matters most, like financial systems, order management, and user account data.

### Non-relational

Non-relational databases are a category of databases rather than a specific type. Often called NoSQL databases, these store data using flexible, schema-less data models instead of rigid tables and rows. The structure of data is enforced by the application rather than the database itself. Unlike relational databases, they are optimized for high throughput and low latency by avoiding expensive JOIN operations and relaxing some consistency constraints. There are many types, some common ones being: key-value, document, graph, and in-memory, and each of these are built for particular data shapes and access patterns. We will go more in depth on these listed NoSQL databases shortly!

###  Key Differences & Tradeoffs

Describe key differences between relational and non-relational databases

## Relational Databases in the Cloud 

Describe common cloud patterns for relational databases

Can create self hosted & managed instances, or use fully managed offerings from cloud providers

May have diff types of SQL or relational databases to choose from, with limits that vary slightly by provider

## Non-relational (NoSQL) Databases

Describe common kinds of non-relational databases and use cases for each kind

Can mention that there are other types made for more specific purposes like time series and ledger that folks can follow their curiosity on

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



## Check for Understanding
 
Which of the following is a benefit of relational databases?
Which of the following is a benefit of NoSQL databases?
Given x requirements, what style of database would fit the feature best?