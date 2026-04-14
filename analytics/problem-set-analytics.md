# Problem Set: Analytics and Data Processing

## Scenario: ShopSmart's Smart Recommendation Engine

Our company, "ShopSmart," is building a new e-commerce application that will automatically recommend products to our users based on their real-time browsing behavior. The following situations describe various challenges we face as we build out our analytics architecture to support this goal. Read the questions carefully and select the best answers based on the concepts we've covered in this module.

### Planning a Smart Recommendation Engine

Our team is building a new e-commerce application. We want to automatically recommend products to our users based on their real-time browsing behavior, which generates continuous, diverse clickstream data. 

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: b7a36523-9786-49c6-9a05-b52bff91248f
* title: Problem Set: Analytics and Data Processing

##### !question

Which stage of the analytics maturity model best describes our goal of automatically recommending optimal products to individual users to maximize their engagement?

##### !end-question

##### !options

a| Descriptive Analytics
b| Diagnostic Analytics
c| Predictive Analytics
d| Prescriptive Analytics

##### !end-options

##### !answer

d|

##### !end-answer

##### !explanation

Prescriptive Analytics is the stage of the analytics maturity model that goes beyond just describing or predicting what will happen. It provides actionable recommendations on what to do to achieve a desired outcome. In our case, automatically recommending optimal products to users based on their real-time browsing behavior is a classic example of prescriptive analytics, as it suggests specific actions (product recommendations) to maximize user engagement.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: f55701cd-53da-4429-8739-02daa4f9ffc4
* title: Problem Set: Analytics and Data Processing

##### !question

Which of the "5 V's of Big Data" are we primarily addressing when we design our system to handle the massive speed of continuous real-time clicks, as well as the diverse semi-structured JSON formats of the clickstream logs?

##### !end-question

##### !options

a| Volume
b| Velocity
c| Variety
d| Veracity
e| Value

##### !end-options

##### !answer

b|
c|

##### !end-answer

##### !explanation

- **Velocity** refers to the speed at which data is generated and processed. In our case, we need to handle the massive speed of continuous real-time clicks, which is a direct challenge of velocity.
- **Variety** refers to the different types and formats of data. The diverse semi-structured JSON formats of the clickstream logs represent a variety challenge, as we need to design our system to handle and process these different data formats effectively.

<br>

The other V's are not the primary focus in this scenario:
- **Volume** refers to the sheer amount of data, which is relevant but not the main challenge here.
- **Veracity** refers to the trustworthiness and quality of the data, which is important but not the primary focus in this context.
- **Value** refers to the usefulness of the data, which is the ultimate goal but not the specific challenge we are addressing with the real-time clickstream data.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

### Upgrading Our Storage Architecture

Our application is a success! However, our operational database is struggling because analysts are running heavy historical reports on it while customers are simultaneously trying to check out. We decide to build a dedicated analytics storage architecture so we can store all our raw data affordably while still providing clean, reliable datasets for our reporting team.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: ba25f8d4-2548-4c03-a3e1-621eafeb5f5f
* title: Problem Set: Analytics and Data Processing

##### !question

To ensure our customer checkout process remains incredibly fast and reliable, what kind of data model should we use for those daily operations, distinct from our historical analytics?

##### !end-question

##### !options

a| Online Analytical Processing (OLAP)
b| Online Transaction Processing (OLTP)
c| Massively Parallel Processing (MPP)
d| Schema-on-Read

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

**Online Transaction Processing (OLTP)** systems are optimized for handling daily, business-critical application operations that involve high volumes of fast read and write transactions. They are designed to ensure data integrity and support concurrent access, making them ideal for our customer checkout process. In contrast, **Online Analytical Processing (OLAP)** is optimized for complex queries and historical analysis, which is more suitable for our reporting team but not for real-time transactional workloads.

<br>

**Massively Parallel Processing (MPP)** is a type of architecture used in data warehouses for handling large-scale analytics, and **Schema-on-Read** is a flexible approach to data storage that allows for unstructured data but is not specifically designed for transactional workloads.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: f244c1b5-d786-497b-b8ac-451746ff8ec9
* title: Problem Set: Analytics and Data Processing

##### !question

We decide to use the Medallion Architecture to logically organize the historical data in a new data lake. Put the following stages in the correct chronological flow, from raw input to business-ready insight:

##### !end-question

##### !answer

1. Store raw, unchanged JSON clickstream data to maintain an unaltered historical record.
1. Filter, clean, and validate the data to provide a reliable, standardized view for exploratory analysis.
1. Curate and aggregate the data specifically to power our executive BI dashboards.

##### !end-answer

##### !explanation

The descriptions correspond to the three layers of the Medallion Architecture, which organizes data into different stages of refinement:
1. **Bronze Layer**: Store raw, unchanged JSON clickstream data to maintain an unaltered historical record. This layer serves as the source of truth and allows for reprocessing if needed.
2. **Silver Layer**: Filter, clean, and validate the data to provide a reliable, standardized view for exploratory analysis. This layer ensures that the data is trustworthy and ready for analysis.
3. **Gold Layer**: Curate and aggregate the data specifically to power our executive BI dashboards. This layer is optimized for performance and usability, providing business-ready insights for decision-makers.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

### Designing Data Pipelines

We need to move data from our application into our new data lake. We decide to load the raw data directly into the lake first, and then use the cloud's distributed compute power to clean it on-demand. We also need to process certain data instantly to detect fraudulent credit card transactions.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 99263b68-e149-44d4-97e0-c0d0d0de2909
* title: Problem Set: Analytics and Data Processing

##### !question

Which modern data pipeline approach are we using by loading raw data directly into our scalable storage first and transforming it later?

##### !end-question

##### !options

a| Extract, Transform, Load (ETL)
b| Online Transaction Processing (OLTP)
c| Extract, Load, Transform (ELT)
d| Columnar Storage

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

**Extract, Load, Transform (ELT)** is the modern data pipeline approach where raw data is extracted from the source, loaded directly into a target system (like a data lake), and then transformed on-demand using the compute power of the cloud. This allows for greater flexibility and scalability, as transformations can be performed as needed without having to move data multiple times.

<br>

**Extract, Transform, Load (ETL)** is the traditional approach where data is transformed before loading it into the target system, which can be less efficient for large volumes of data. **Online Transaction Processing (OLTP)** is a type of database optimized for transactional workloads, and **Columnar Storage** refers to a storage format that organizes data by columns rather than rows, which is not directly related to the pipeline approach.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 1a782966-342a-4e29-a08c-4f009faf2161
* title: Problem Set: Analytics and Data Processing

##### !question

To handle the instant fraud detection, we implement stream processing. Which of the following accurately describe our stream processing architecture?

##### !end-question

##### !options

a| It buffers continuous data using message brokers to prevent loss during traffic spikes.
b| It processes records individually or in micro-batches in milliseconds.
c| It is the ideal paradigm for generating scheduled, end-of-day billing aggregations.
d| It utilizes highly scalable platforms to decouple data producers from consumers.

##### !end-options

##### !answer

a|
b|
d|

##### !end-answer

##### !explanation

Stream processing is designed to handle high-velocity data flows in real-time. It uses message brokers to buffer continuous data, preventing loss during traffic spikes. It processes records individually or in micro-batches with very low latency, often in milliseconds. Additionally, stream processing architectures utilize highly scalable platforms that decouple data producers from consumers, allowing for flexible and resilient data pipelines.

<br>

The remaining option describes batch processing, which is not suitable for real-time fraud detection.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

### Empowering Teams with Discovery and Search

Our data is securely stored, but our business team is having trouble finding the datasets they need. Plus, our support team wants to search through raw, unstructured application error logs to troubleshoot bugs.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 912b49ae-6ea5-4d05-a127-29fcb579e061
* title: Problem Set: Analytics and Data Processing

##### !question

Which tool should our support team use to perform full-text searches and real-time troubleshooting directly on our unstructured application logs?

##### !end-question

##### !options

a| A Search and Observability Suite
b| A Data Warehouse
c| A Serverless Interactive Query-in-Place Engine
d| A Data Catalog

##### !end-options

##### !answer

a|

##### !end-answer

##### !explanation

A **Search and Observability Suite** is specifically designed to handle unstructured data, such as application logs. These platforms use complex indexing and tokenization techniques to enable full-text search capabilities, making them ideal for real-time troubleshooting and log analysis. They allow support teams to quickly search through large volumes of log data to identify and resolve issues.

<br>

A **Data Warehouse** is optimized for structured data and analytical queries, a **Serverless Interactive Query-in-Place Engine** is designed for querying data in place but may not be optimized for unstructured logs, and a **Data Catalog** is focused on metadata management and discoverability rather than search capabilities.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: dced29ec-6e0e-43cf-8754-fc9134dc7436
* title: Problem Set: Analytics and Data Processing

##### !question

We implement a Data Catalog and a Semantic Layer to help our business team. How do these tools bridge the gap between our raw data and our users?

##### !end-question

##### !options

a| The Data Catalog provides discoverability by indexing metadata like schemas, tables, and sources.
b| The Data Catalog acts as a centralized OLTP database for processing shopping cart checkouts.
c| The Semantic Layer maps complex technical metadata to readable, consistent business definitions.
d| The Semantic Layer allows business users to analyze data without needing deep data engineering skills.

##### !end-options

##### !answer

a|
c|
d|

##### !end-answer

##### !explanation

- The **Data Catalog** provides discoverability by indexing metadata such as schemas, tables, and data sources. This allows users to easily find and understand the datasets available in the data lake.
- The **Semantic Layer** maps complex technical metadata to readable, consistent business definitions, making it easier for non-technical users to understand and work with the data. It also allows business users to analyze data without needing deep data engineering skills, empowering them to access insights directly without relying on technical intermediaries.

<br>

- The Data Catalog does not function as a centralized OLTP database.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

## Delivering Visual Insights

Our executive team wants a global dashboard to view regional sales. They also want to use a generative AI assistant to ask questions like "What was our best-selling product last month?" using plain language instead of writing SQL.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: be6e79d2-eea5-433f-b252-23418559b694
* title: Problem Set: Analytics and Data Processing

##### !question

Which features of our Business Intelligence (BI) platform and analytics architecture will we use to fulfill these requests securely and accurately?

##### !end-question

##### !options

a| Row-Level Security (RLS) to ensure regional managers only see their authorized data within a shared dashboard.
b| Text-to-SQL capabilities to translate natural language questions into database queries.
c| A well-governed semantic layer to ensure the AI uses consistent business terms (like "revenue") to prevent hallucinations.
d| A traditional ETL transformation server to process live credit card transactions.

##### !end-options

##### !answer

a|
b|
c|

##### !end-answer

##### !explanation

- **Row-Level Security (RLS)** allows us to restrict data access on shared dashboards, ensuring that regional managers only see the data they are authorized to view, which is crucial for maintaining data security and compliance.
- **Text-to-SQL capabilities** enable users to ask questions in plain language, which the BI platform can translate into SQL queries to retrieve the relevant data without requiring users to write complex queries themselves.
- A **well-governed semantic layer** ensures that the AI assistant uses consistent business terms and definitions, which helps prevent hallucinations and ensures that the insights provided are accurate and trustworthy.

<br>

- A traditional ETL transformation server is not relevant to the BI features needed for the executive dashboard and AI assistant, as it is more focused on data processing rather than data access and querying.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
