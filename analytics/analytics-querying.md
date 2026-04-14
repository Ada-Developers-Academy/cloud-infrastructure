# Data Discovery, Search, and Querying

## Learning Goals
* Explain the importance of data catalogs and the semantic layer in organizing cloud data.
* Describe how decoupled compute and storage enable query-in-place architectures.
* Differentiate between serverless interactive querying engines and search and observability suites.

## Vocabulary and Synonyms
| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Data Catalog** | A centralized repository that stores metadata about data assets, making it easier to discover and manage data. | Metadata Repository | We use a data catalog to keep track of all our datasets and their schemas across the organization. |
| **Semantic Layer** | A user-friendly architectural layer that simplifies interactions by mapping technical metadata to readable business definitions. | Business Translation Layer | Our semantic layer ensures that the term "revenue" means the same thing across all our dashboards. |
| **Data Crawler** | A tool that automatically scans data sources to infer schema and populate metadata in a data catalog. | Schema Inference Tool | The data crawler helped us quickly register our new S3 bucket in the data catalog by detecting the structure of our log files. |
| **Query-In-Place** | Performing analytics directly against data stored in object storage without moving it to a separate database. | Query-on-Demand | We use query-in-place to analyze logs right in the data lake without setting up a complex ETL pipeline. |
| **Search and Observability Suite** | A distributed search and analytics engine designed for handling semi-structured and unstructured data, often with a SQL-like interface. | Log Analytics Platform | Our search and observability suite allows us to analyze application logs in real time to quickly identify issues. |

## Organizing Data with Metadata, Catalogs, and the Semantic Layer

Once we ingest massive volumes of data and store it in the cloud, finding and extracting specific insights can feel like searching for a needle in a haystack. Before we can run a successful query, we must first understand what data we have, where it lives, and what it means. We solve this by building metadata management systems that make our data discoverable and understandable for all users.

**Data Catalogs** act as centralized repositories that store and organize metadata about our data assets, such as sources, tables, schemas, and columns. We've already discussed how data catalogs provide a layer of abstraction over our raw data in object storage, transforming a chaotic collection of files into a useful data lake. But data catalogs can provide a range of critical functions beyond just registering data.

- **Discoverability:** By cataloging metadata, we can make our data assets searchable, allowing analysts to quickly find the datasets they need for their analysis.
- **Lineage:** Data catalogs can track the lineage of data, showing where it came from and how it has been transformed. This is important for understanding how relevant that data is to the question an analyst is trying to answer, as well as its overall quality and reliability.
- **Impact Analysis:** By understanding how data is related to other data, not just from a "join" perspective, but also in terms of its processing history, we can assess the potential impact of changes to the data on downstream analytics and reporting. This is especially important in large organizations where data is generated across multiple teams and systems, making it difficult to keep track of everything without a centralized catalog.
- **Direct Querying:** Data catalogs can also serve as a bridge between our raw data and our query engines, allowing us to run queries directly against the data in object storage without needing to move it into a separate database. This is a key feature both for the data lake approach to analytics, and for allowing analysts to run ad-hoc queries while investigating new datasets.

Highly skilled data engineers understand how to work directly with tables and raw data structures, most business users do not have the deep technical experience needed to make use of such direct access to data. To bridge this gap, we implement a **Semantic Layer**. This is an architectural layer designed to simplify interactions between complex data systems and business users by mapping technical metadata to readable business definitions. It enables users with business expertise to query and analyze data without also being experts in data engineering.

The function of the semantic layer can be implemented in a variety of ways. It may be provided as a feature of a data catalog, or it could exist as a set of configuration files (e.g., YAML files) that define the mappings between technical metadata and business terms. However it's implemented, the job of the semantic layer is to provide a consistent layer of business language on top of the technical metadata. For example, if we have a database table with the technical name `tbl_sales_data_2024_q1`, the semantic layer might map this to a more user-friendly business term like "Q1 Sales Data". This way, when an analyst wants to query sales data for the first quarter, they can simply ask for "Q1 Sales Data" instead of needing to know the original underlying column names and table structures.

Due to the importance of the data catalog and semantic layer, many organizations use **Data Crawlers** to automatically scan data sources and infer schema information, populating the data catalog with metadata. This helps ensure that new datasets are quickly registered and made discoverable for analysis. For semantic layers in particular, it's important to have any automated processes reviewed by domain experts to ensure that the business definitions are accurate and consistent across the organization.

Maintaining a well-governed semantic layer is crucial for ensuring that business terms are consistently defined across the organization, which is especially important when integrating AI assistants that rely on this layer to generate accurate insights.

## Enabling Query-on-Demand and In-Place Analytics

Traditionally, analyzing data meant we had to move it from storage into a dedicated processing database, which can be a slow and expensive process. Think of a traditional SQL query in a system like PostgreSQL or MySQL. The SQL query engine is a part of the overall database, which knows how to perform queries on data stored in its own tables. If we want to analyze data using the database's query engine, we would first need to load that data into the database; a potentially complex and time-consuming process that would also involve duplicating the original data in the database's storage layer.

In modern cloud architectures, we can make use of the decoupling of compute and storage to enable **Query-In-Place** (or **Query-On-Demand**) analytics directly against data stored where it already lives. Modern cloud architectures separate the storage of data (like a data lake in an object storage service) from the computing power used to analyze it. This allows analysts to apply any compute resources they have available to query the data directly in place, without needing to move it into a separate database first, opening up new possibilities for how we can analyze data at scale.

We can analyze vast amounts of raw data without needing to predict in advance which data will be valuable. We can also run many different kinds of queries on the same data, each able to access precisely the information it needs without being limited by the constraints of a traditional data warehouse schema. Some tools and analyses might benefit more from particular tools, but the same data can be accessed by all of them without needing to create multiple copies or move it around.

![A diagram showing a storage layer (a data lake) on the bottom and a compute layer on top, separated by a dashed line to indicate decoupling. Arrows point from the compute layer directly into the storage layer to represent query-in-place analytics. A variety of compute services are shown using the same data lake: Exploratory Analysis, Log Auditing, Compliance Reporting, and Federated Queries. Non structured data, including Logs, Click streams, and Social media feeds, are shown feeding into the data lake.](./assets/analytics-compute-storage.png)  
*Fig. Decoupled compute and storage enabling query-in-place analytics. A data lake enables variety of different services to access the data directly where it is stored, performing on-demand transformations per service.*

## Search and Query Tools

Depending on the structure of our data and the questions we are trying to answer, we rely on different types of specialized engines to perform our searches. 

### Interactive Query-in-Place Engines

**Interactive Query-in-Place Engines** enable us to analyze vast amounts of structured and semi-structured data directly in object storage using standard SQL. Typically either managed or serverless, they give us options about how much infrastructure management we want to handle ourselves. These engines are designed to be highly scalable and cost-effective, allowing us to run queries on petabyte-scale datasets without needing to provision or manage a separate data warehouse. Not all providers offer this as a distinct service, with some instead providing this functionality as a feature within their data warehousing or data lake services, but the core concept of query-in-place analytics is still present.

Costs usually accumulate based on the amount of data scanned by each query or the compute resources used to carry out that request, making it important to optimize our data storage and query patterns to minimize unnecessary scanning. These engines are ideal for ad-hoc exploration and analysis of large datasets, allowing us to quickly extract insights without needing to set up complex data pipelines or move data into a separate database.

The ability of these tools to access data directly where it is stored is typically enabled by the data catalog and semantic layer. This allows us to run federated queries across multiple data sources, including relational databases, data lakes, and even external APIs, all from a single interface.

For example, we might have an object storage data lake that contains historical purchase data in a plain Apache Parquet file. We can use an interactive query-in-place engine to run SQL queries directly against that to extract insights about user behavior. We would register the data lake in our data catalog, either by setting up schema information manually, or running a data crawler to try to determine the data structure automatically. Once the data is registered, we can use the query engine to run SQL queries that join the historical data with other structured data sources—such as a customer database—to gain deeper insights, all while leaving the actual data in place!

### Search and Observability Suites

While SQL is great for tables or other structured data, we often need to analyze semi-structured data like application logs, or unstructured data like text documents. For this, we use **Search and Observability Suites**. These are distributed search and analytics engines specifically designed for handling semi-structured and unstructured data. Instead of standard SQL tables, these engines process the incoming data as documents, relying on complex indexing and tokenization to make their contents accessible. This approach makes them ideal for full-text search, application monitoring, and real-time log aggregation.

While the data they work best with is not organized internally as tables, these suites can still provide SQL-like query interfaces. This is usually accomplished by translating SQL queries into the underlying search engine's query language. Providing a SQL interface allows users familiar with SQL to interact with the data without needing to learn a new query language. But it's important to keep in mind that these tools are optimized for different use cases than traditional relational databases. A search and observability suite can be used to analyze application logs in real time, allowing us to quickly identify and troubleshoot issues as they arise. These suites can also be used for more complex search queries across large volumes of unstructured data, such as searching through customer feedback or social media posts to identify trends and sentiment.

For example, we might have an application that generates a large volume of logs, and we want to analyze those logs to identify patterns in user behavior or troubleshoot issues. Instead of trying to fit those logs into a structured format and load them into a data warehouse, we can use a search and observability suite to analyze the logs directly in their raw form. We would set up a pipeline to stream our application logs into the search engine, which would index the data for fast retrieval. We could then run queries against the indexed logs to identify patterns, such as error rates or user behavior, without needing to structure the data in a traditional relational format.

## Summary

We learned that successfully analyzing big data requires us to organize it with data catalogs and a semantic layer, making it discoverable and understandable for all users. By leveraging decoupled compute and storage, we can perform query-in-place analytics directly against our object storage, saving time and resources. Finally, choosing the right tool—whether an interactive query-in-place engine for more structured data, or a search and observability suite for unstructured text—ensures we can extract the insights we need efficiently.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 909f0f88-c5a6-4fe9-aff0-b98a57166b63
* title: Data Discovery, Search, and Querying

##### !question

Your company is migrating petabytes of diverse, raw files (CSV, JSON, and Parquet) into a centralized cloud object storage data lake. Your analytics team wants to easily find specific datasets and run ad-hoc SQL queries against these files directly where they live, without having to build complex pipelines to copy the data into a traditional relational database first.

What Data Catalog features would be most critical to enable this use case?

##### !end-question

##### !options

a| **Discoverability:** It acts as a centralized metadata repository that stores information about data sources, schemas, and columns, making raw data assets searchable and understandable for analysts.
b| **Lineage and Impact Analysis:** It tracks where data originated and how it was transformed, allowing teams to assess how changes to a specific dataset might impact downstream reports and dashboards.
c| **High-Velocity Data Buffering:** It acts as a fault-tolerant message broker that temporarily buffers continuous, real-time streaming data from IoT devices to prevent data loss before it hits the data lake.
d| **Enabling Direct Querying:** It provides a layer of abstraction that serves as a bridge between the raw files in object storage and interactive query engines, allowing analysts to run SQL queries directly against the data lake.
e| **In-Memory Data Visualization:** It provides a high-performance, in-memory calculation engine used to render highly formatted, interactive visual dashboards directly to executive stakeholders.

##### !end-options

##### !answer

a|
b|
d|

##### !end-answer

##### !explanation

The most critical features of a Data Catalog for this use case would be **Discoverability**, **Lineage and Impact Analysis**, and **Enabling Direct Querying**.
- **Discoverability** is essential for making the raw data assets in the data lake searchable and understandable for analysts, allowing them to quickly find the datasets they need for their analysis.
- **Lineage and Impact Analysis** is important for understanding where the data originated and how it was transformed, which helps teams assess how changes to a specific dataset might impact downstream reports and dashboards.
- **Enabling Direct Querying** is crucial for allowing analysts to run SQL queries directly against the data lake without needing to build complex pipelines to copy the data into a traditional relational database first.

<br>

The other options, **High-Velocity Data Buffering** and **In-Memory Data Visualization**, are not typically features of a Data Catalog. High-velocity data buffering is more related to data ingestion and streaming services, while in-memory data visualization is a feature of business intelligence platforms rather than data catalogs.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

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
