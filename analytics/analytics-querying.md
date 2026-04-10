# Data Discovery, Search, and Querying

## Learning Goals
* Explain the importance of data catalogs and the semantic layer in organizing cloud data.
* Describe how decoupled compute and storage enable query-in-place architectures.
* Differentiate between serverless interactive querying engines and search and observability suites.

## Vocabulary and Synonyms
| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Data Catalog** | A centralized repository that stores and organizes metadata about an organization's data assets. | Metadata Index | We checked the data catalog to find the exact source table for our new sales report. |
| **Semantic Layer** | A user-friendly architectural layer that simplifies interactions by mapping technical metadata to readable business definitions. | Business Translation Layer | Our semantic layer ensures that the term "revenue" means the same thing across all our dashboards. |
| **Data Crawler** | A tool that automatically scans data sources to infer schema and populate metadata in a data catalog. | Schema Inference Tool | The data crawler helped us quickly register our new S3 bucket in the data catalog by detecting the structure of our log files. |
| **Query-In-Place** | Performing analytics directly against data stored in object storage without moving it to a separate database. | Query-on-Demand | We use query-in-place to analyze logs right in the data lake without setting up a complex ETL pipeline. |

## Organizing Data with Metadata, Catalogs, and the Semantic Layer

Once we ingest massive volumes of data and store it in the cloud, finding and extracting specific insights can feel like searching for a needle in a haystack. Before we can run a successful query, we must first understand what data we have, where it lives, and what it means. We solve this by organizing our metadata.

**Data Catalogs** act as centralized repositories that store and organize metadata about our data assets, such as sources, tables, schemas, and columns. They serve as a single source of truth, enabling us to easily discover data, track its lineage, and govern access securely across the organization.

While highly skilled data engineers understand how to work directly with tables and raw data structures, most business users do not have the deep technical experience needed to extract insights so easily. To bridge this gap, we implement a **Semantic Layer**. This is a user-friendly architectural layer designed to simplify interactions between complex data systems and business users. By mapping technical metadata to readable business definitions, it allows analysts to focus on extracting insights without needing to know the technicalities of data retrieval.

The function of the semantic layer can be implemented in a variety of ways, often through a semantic modeling layer built directly into a Business Intelligence tool, or a set of configurations (e.g., YAML files) that define the mappings between technical metadata and business terms. For example, if we have a database table with the technical name `tbl_sales_data_2024_q1`, the semantic layer might map this to a more user-friendly business term like "Q1 Sales Data". This way, when an analyst wants to query sales data for the first quarter, they can simply ask for "Q1 Sales Data" without needing to know the underlying technical details.

Due to the importance of the data catalog and semantic layer, many organizations use **Data Crawlers** to automatically scan data sources and infer schema information, populating the data catalog with metadata. This helps ensure that new datasets are quickly registered and made discoverable for analysis, but for semantic layers in particular, it's important to have any automated processes reviewed by domain experts to ensure that the business definitions are accurate and consistent across the organization.

Maintaining a well-governed semantic layer is crucial for ensuring that business terms are consistently defined across the organization, which is especially important when integrating AI assistants that rely on this layer to generate accurate insights.

## Enabling Query-on-Demand and In-Place Analytics

Traditionally, analyzing data meant we had to move it from storage into a dedicated processing database, which can be a slow and expensive process. To build more efficient systems, we design our cloud architecture to query data exactly where it already lives.

We achieve this through one of cloud computing's most powerful features: decoupled compute and storage. Modern cloud architectures separate the storage of data (like a data lake in an Object Storage service) from the computing power used to analyze it. This allows our compute resources to scale dynamically only when a query is executed, without needing to pay for processing power when the system is idle.

Because compute is decoupled, we can perform **Query-In-Place** (or **Query-On-Demand**) analytics directly against data stored in object storage. This eliminates the need for complex Extract, Transform, Load (ETL) pipelines that traditionally copied and moved data into separate databases before analysis.

And because we can query data in place, we can analyze vast amounts of raw data without needing to predict in advance which data will be valuable. We can also run many different kinds of queries on the same data, each able to access precisely the information it needs without being limited by the constraints of a traditional data warehouse schema. This flexibility is especially important for handling large volumes of semi-structured or unstructured data, such as logs, click streams, and social media feeds, which often do not fit neatly into a relational schema.

![A diagram showing a storage layer (a data lake) on the bottom and a compute layer on top, separated by a dashed line to indicate decoupling. Arrows point from the compute layer directly into the storage layer to represent query-in-place analytics. A variety of compute services are shown using the same data lake: Exploratory Analysis, Log Auditing, Compliance Reporting, and Federated Queries. Non structured data, including Logs, Click streams, and Social media feeds, are shown feeding into the data lake.](./assets/analytics-compute-storage.png)  
*Fig. Decoupled compute and storage enabling query-in-place analytics. A data lake enables variety of different services to access the data directly where it is stored, performing on-demand transformations per service.*

## Practical System Roles for Search and Querying

Depending on the structure of our data and the questions we are trying to answer, we rely on different types of specialized engines to perform our searches. 

### Interactive Query-in-Place Engines

**Interactive Query-in-Place Engines** enable us to analyze vast amounts of structured and semi-structured data directly in object storage using standard SQL. They can be managed or serverless, giving us options about how much infrastructure management we want to handle ourselves. These engines are designed to be highly scalable and cost-effective, allowing us to run queries on petabyte-scale datasets without needing to provision or manage a separate data warehouse. Not all providers offer this as a distinct service, with some instead providing this functionality as a feature within their data warehousing or data lake services, but the core concept of query-in-place analytics is still present.

Costs usually accumulate based on the amount of data scanned by each query or the compute units used to carry out that request, making it important to optimize our data storage and query patterns to minimize unnecessary scanning. These engines are ideal for ad-hoc exploration and analysis of large datasets, allowing us to quickly extract insights without needing to set up complex data pipelines or move data into a separate database.

In order to query multiple data sources, first, we usually would set up a data source to bridge the gap between the query engine and the data lake, such as a data catalog or a semantic layer. This allows us to run federated queries across multiple data sources, including relational databases, data lakes, and even external APIs, all from a single interface.

For example, we might have an object storage data lake that contains historical purchase data in a plain Parquet file. We can use an interactive query-in-place engine to run SQL queries directly against that to extract insights about user behavior. We would register the data lake in our data catalog, either by setting up schema information manually, or running a data crawler to try to determine the data structure automatically. Once the data is registered, we can use the query engine to run SQL queries that join the historical data with other structured data sources, such as a customer database, to gain deeper insights, all while leaving the actual data in place!

| Provider | Service Name |
| -------- | ------------ |
| AWS | Amazon Athena |
| Azure | Fabric SQL Analytics Endpoint |
| Google Cloud | BigQuery BigLake |
| OCI | Autonomous AI Lakehouse |

### Search and Observability Suites

While SQL is great for tables or other structured data, we often need to analyze unstructured data like application logs or text documents. For this, we use **Search and Observability Suites**. These are distributed search and analytics engines specifically designed for handling semi-structured and unstructured data. Instead of standard SQL tables, these engines rely on complex indexing and tokenization, making them ideal for full-text search, application monitoring, and real-time log aggregation.

Often, these suites are still able to provide SQL-like query interfaces, but they are optimized for different use cases than traditional relational databases. For example, we might use a search and observability suite to analyze application logs in real time, allowing us to quickly identify and troubleshoot issues as they arise. These suites can also be used for more complex search queries across large volumes of unstructured data, such as searching through customer feedback or social media posts to identify trends and sentiment.

| Provider | Service Name |
| -------- | ------------ |
| AWS | Amazon OpenSearch Service |
| Azure | Elastic Cloud |
| Google Cloud | Elastic Cloud |
| OCI | OCI Search Service |

## Summary

We learned that successfully analyzing big data requires us to organize it with data catalogs and a semantic layer, making it discoverable and understandable for all users. By leveraging decoupled compute and storage, we can perform query-in-place analytics directly against our object storage, saving time and resources. Finally, choosing the right tool—whether a serverless interactive SQL engine for structured tables or a search and observability suite for unstructured logs—ensures we can extract the insights we need efficiently.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: c4e891e5-ae2e-4eac-b7a2-ddf0222f1449
* title: Data Discovery, Search, and Querying

##### !question

What is the primary function of a semantic layer in our data architecture?

##### !end-question

##### !options

a| To execute heavy ETL pipelines before data reaches the data lake.
b| To provide full-text search capabilities across unstructured application logs.
c| To simplify interactions between complex data systems and business users by mapping technical metadata to readable business definitions.
d| To automatically provision database servers based on traffic spikes.

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

The primary function of a semantic layer is to simplify interactions between complex data systems and business users by mapping technical metadata to readable business definitions. This allows analysts to focus on extracting insights without needing to understand the technical details of data retrieval.

<br>

Executing heavy ETL pipelines is not the role of a semantic layer; it is more related to data processing. Providing full-text search capabilities is typically the function of search and observability suites, not the semantic layer. Automatically provisioning database servers is a feature of cloud infrastructure management, not related to the semantic layer's purpose.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 308fdb37-9ea3-464b-8531-a4f0483db4d2
* title: Data Discovery, Search, and Querying

##### !question

Which of the following describe the advantages of "query-in-place" and decoupled compute/storage architectures?

##### !end-question

##### !options

a| They require data to be explicitly copied into a relational database before analysis can begin.
b| They allow us to run queries directly against data stored in object storage (like a data lake).
c| Compute resources can scale dynamically only when a query is executed.
d| They eliminate the need for complex pipelines that traditionally moved data prior to analysis.
e| They tightly bind processing power to hard drives, requiring us to scale both simultaneously.

##### !end-options

##### !answer

b|
c|
d|

##### !end-answer

##### !explanation

The advantages of "query-in-place" and decoupled compute/storage architectures include allowing us to run queries directly against data stored in object storage (like a data lake), enabling compute resources to scale dynamically only when a query is executed, and eliminating the need for complex pipelines that traditionally moved data prior to analysis.

<br>

Query-in-place does not require data to be copied into a relational database; it allows us to query data directly where it resides. Decoupled architectures specifically allow us to scale compute and storage independently, rather than tightly binding them together.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
