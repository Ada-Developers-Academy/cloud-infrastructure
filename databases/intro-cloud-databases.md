# Intro to Cloud Databases

Let's start by taking a moment to think about a website you visit frequently. Outside of files like images, what other content is there? We're likely to see a variety of content: ads with text, products with reviews, posts with comments or likes. We can see that there is a lot of data we need to power applications that isn’t necessarily best stored as an object or file using the cloud storage options we've seen before.

To deploy enterprise applications, we need to create, connect to, and manage various kinds of databases. Throughout this topic we’ll dive into the differences in responsibility between fully managed vs self hosted databases, describe commonly available types of databases offered by cloud providers, and discuss their performance and scaling tradeoffs. Along the way we’ll discuss how we can keep data consistent, especially in a multi-region system, and talk about use cases for several kinds of databases.

## Learning Goals

- Describe what it means for a database to be fully managed vs self-hosted and be able to speak to tradeoffs of each.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Fully Managed Database | A cloud-based database solution provided by a cloud service provider, where the underlying infrastructure, including virtual machines, storage, and database software, is entirely managed by the service provider. | -------- | "We used a fully managed database solution during our start-up phase since we didn't have a lot of dedicated team members to manage security updates and other maintenance." |
| Self-Hosted Database | A database management system that is installed, configured, and maintained by an organization. A self-managed database may be hosted on-premises or on cloud infrastructure. | -------- | "The company chose a self-hosted database so they could enforce strict data sovereignty requirements and keep all records within their own data center." |
| Relational Database | A database that organizes data into structured tables with predefined schemas, using rows and columns. Enforces relationships between tables through foreign keys and are typically queried using SQL. | -------- | "The e-commerce platform used a relational database to link its customers, orders, and products tables, making it easy to generate complex sales reports with JOIN queries." |
| Non-relational Database | A category of databases rather than a specific type. A database that stores data in flexible formats such as documents, key-value pairs, graphs, or wide columns. Designed for scalability, speed, and handling unstructured or rapidly changing data. | NoSQL Database | "A non-relational database was chosen for the product catalog because each item had a different set of attributes that didn't fit neatly into a fixed table structure." |

## What is a cloud database

Discuss what it means for a database to be fully managed vs self-hosted

Responsibilities of self-hosting a database vs a managed database: tradeoffs of cost vs granularity of ownership vs management responsibilities

## Types of databases 

### Relational
Short description of common features and strengths, folks have seen this before, can refer folks to review older lessons if we give them access

### Non-relational

Also often called "No-SQL"
Short description of common features and strengths, will go more in depth on kinds of NoSQL databases later in this topic

## Summary



## Check for Understanding

Which of the following is the user responsible for when using a self-hosted database?
Which of the following is a user responsible for when using a fully managed database?