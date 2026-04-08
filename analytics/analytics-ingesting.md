# Data Ingestion and Processing Paradigms

## Learning Goals
* Differentiate between batch processing and stream processing workloads.
* Compare traditional ETL data pipelines with modern ELT architectures.
* Identify the roles of managed data integration, big data clusters, and real-time streaming platforms in a cloud ecosystem.

## Vocabulary and Synonyms
| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Batch Processing** | A method used to periodically complete high-volume, repetitive data jobs. | Scheduled Processing | We rely on batch processing overnight to calculate our daily sales totals. |
| **Stream Processing** | Handling continuous, high-velocity data flows with extremely low latency. | Real-Time Processing | Stream processing allows our application to detect fraudulent credit card transactions instantly. |
| **ETL** | Extract, Transform, Load: A traditional pipeline where data is transformed on a dedicated server before being stored. | Traditional Pipeline | Our legacy system uses an ETL pipeline to clean customer records before saving them to the data warehouse. |
| **ELT** | Extract, Load, Transform: A modern approach where raw data is loaded directly into storage and transformed later on-demand. | Cloud-Native Pipeline | By adopting ELT, we can securely store raw event logs in the data lake and decide how to format them later. |
| **Data Catalog** | A centralized repository that stores metadata about data assets, making it easier to discover and manage data. | Metadata Repository | We use a data catalog to keep track of all our datasets and their schemas across the organization. |
| **Message Broker** | A platform that securely buffers, captures, and processes continuous streams of data at a massive scale. | Event Bus | We use a message broker to handle the high volume of clickstream data from our website in real-time. |

## Choosing Between Batch and Stream Processing
In a cloud-based analytics system, raw data rarely arrives perfectly formatted or at a convenient speed. Organizations must handle massive historical dumps as well as continuous, high-speed events. This makes it important that we understand how data is collected, routed, and transformed using different processing paradigms and pipeline architectures so that it can be effectively analyzed.

When considering data collection, we first decide how quickly the data needs to be processed.

**Batch processing** is a method used to process large volumes of accumulated data at scheduled, periodic intervals. Certain data processing tasks are compute-intensive and inefficient to run on individual, real-time transactions. Instead, our systems process such tasks in accumulated batches, often during off-peak times when computing resources are more readily available, such as overnight or weekly. This approach is highly efficient for heavy, complex transformations and is best suited for workloads that do not require immediate insights.

For example, consider an e-commerce system that receives orders throughout the day. Instead of processing analytics for every order immediately, the system collects all orders at the end of each day and processes them in one batch for the fulfillment team.

Alternatively, **stream processing** is a method designed to handle continuous, high-velocity data flows with extremely low latency. Data records are captured and processed individually or in micro-batches within milliseconds of being generated. This paradigm is essential for applications requiring immediate action.

For example, we can track changes in public sentiment regarding our products by continuously analyzing clickstream data and social media streams in real-time, enabling us to respond promptly, or use it for live dashboards and credit card fraud detection.

Neither batch nor stream processing is inherently better than the other; they are simply different tools for different use cases. Batch processing is ideal for heavy, complex transformations that can be scheduled during off-peak hours, while stream processing is essential for applications that require immediate insights and actions. Our task is to choose the appropriate processing paradigm based on the specific requirements of our data and the business needs we aim to address.

## Designing Data Pipelines with ETL and ELT
Raw data must be extracted from its source and formatted before it can be effectively analyzed. We must design data pipelines that efficiently route and clean this information without creating bottlenecks in our operational systems.

### ETL (Extract, Transform, Load)

ETL represents the traditional data pipeline model. Raw data is extracted from a source, transformed into a standard format on a dedicated processing server, and then loaded into a target storage system like a data warehouse. Because this process can be resource-heavy, jobs are often scheduled during off-peak hours to reduce the strain on the original source systems.

This approach was very dominant in the era of on-premises data centers, where compute and storage resources were limited and expensive. But as data volumes have exploded and cloud computing has become more prevalent, the ETL model has shown limitations in terms of scalability and flexibility. Only specifically-identified, high value data is transformed and stored. Large volumes of raw data are often discarded or archived without transformation, which can lead to missed opportunities for insights.

However, the ETL model is still widely used for certain workloads, such as generating standard monthly billing reports or performing complex transformations that require a lot of compute power. While in others, it is being replaced by the more modern ELT approach.

![A split diagram comparing data pipelines. The left ETL path shows data moving from a Source to a Transformation server, and finally to a Data Warehouse. The right ELT path shows data moving directly from a Source to a Data Lake, with a looping arrow indicating Transformation happening inside the Data Lake itself, often on-demand at query time.](./assets/analytics-etl-elt.png)  
*Fig. Comparing traditional ETL pipelines with modern ELT pipelines.*

### ELT (Extract, Load, Transform)

ELT represents the modern cloud-native approach. Raw data is extracted and immediately loaded directly into a highly scalable target system, such as a data lake or lakehouse, in its original state. The transformation step happens *after* the data is securely stored, often as the data is queried for analysis. This leverages the massive, distributed compute power of the cloud storage environment to process the data on-demand.

The significant advantage of ELT is that it allows us to store all raw data without needing to predict in advance which data will be valuable. This means we can retain a complete historical record and apply transformations as needed for different use cases, without being limited by the constraints of a traditional ETL pipeline. ELT is particularly well-suited for handling large volumes of semi-structured or unstructured data, such as logs, click streams, and social media feeds, which may not fit neatly into a relational schema.

## Practical System Roles in Data Processing
To implement batch and stream processing or to orchestrate ELT pipelines, we rely on managed cloud services. Rather than provisioning and maintaining the underlying hardware ourselves, we use specialized platforms that handle the heavy lifting. These platforms can be broadly categorized into three roles: managed data integration, big data cluster platforms, and real-time streaming platforms.

While the general categories of platforms are consistent across providers, the specific implementations and features may vary. So as we look into the services provided by a particular provider, we may need to expand our search beyond those listed here to find the best fit for our specific use case.

### Managed Data Integration

We use serverless platforms to automatically discover data, catalog its metadata, and execute ETL or ELT jobs without requiring us to manage the underlying infrastructure.

| Provider | Service Name |
| -------- | ------------ |
| AWS | AWS Glue |
| Azure | Azure Data Factory |
| Google Cloud | Google Cloud Data Fusion |
| OCI | Oracle Data Integration |

### Big Data Cluster Platforms

These are managed cluster environments designed to host distributed data processing frameworks, such as Apache Hadoop and Apache Spark. We use these platforms to distribute massive, petabyte-scale data transformation tasks across multiple compute nodes, analyzing our data in parallel.

| Provider | Service Name |
| -------- | ------------ |
| AWS | Amazon EMR (Elastic MapReduce) |
| Azure | Azure HDInsight |
| Google Cloud | Google Cloud Dataproc |
| OCI | Oracle Big Data Service |

### Real-Time Streaming Platforms

To process fast-moving data without loss, we use highly scalable, fault-tolerant message brokers. These platforms securely buffer, capture, and process continuous streams of data at a massive scale before the data disappears.

| Provider | Service Name |
| -------- | ------------ |
| AWS | Amazon Kinesis, Amazon MSK (Managed Streaming for Apache Kafka) |
| Azure | Azure Event Hubs |
| Google Cloud | Google Cloud Pub/Sub, Managed Service for Apache Kafka |
| OCI | OCI Streaming |

## Summary
We explored how to process data efficiently by considering the differences between batch processing for high-volume, scheduled jobs, and stream processing for continuous, low-latency data flows. We saw how traditional ETL pipelines transform data before loading it into storage, while modern ELT architectures load raw data first and transform it on-demand, leveraging the cloud's distributed compute power. Finally, we identified the roles of managed data integration services, big data cluster platforms, and real-time streaming platforms in orchestrating these processes without the operational burden of hardware management.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 82c05b8a-cd5d-4f99-9d02-acff02842489
* title: Data Ingestion and Processing Paradigms

##### !question

Which of the following best describes the modern ELT (Extract, Load, Transform) pipeline approach?

##### !end-question

##### !options

a| It strictly requires data to be converted into a relational schema before it can be stored.
b| It leverages the distributed compute power of the target cloud system to transform raw data *after* it has been safely loaded.
c| It is primarily used for handling low-latency, real-time data streams rather than historical data.
d| It reduces the need for data cataloging by applying business rules directly at the extraction phase.

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

The ELT (Extract, Load, Transform) pipeline approach is a modern cloud-native architecture where raw data is extracted from its source and immediately loaded into a highly scalable target system, such as a data lake or lakehouse, in its original state. The transformation step happens *after* the data is securely stored, often as the data is queried for analysis. This allows us to leverage the massive, distributed compute power of the cloud storage environment to process the data on-demand, providing greater flexibility and scalability compared to traditional ETL pipelines.

<br>

Converting data into a relational schema before storage is more characteristic of traditional ETL pipelines, not ELT. While ELT can be used for real-time data processing, it is not primarily focused on low-latency streams; that role is better suited for stream processing platforms. Additionally, ELT does not reduce the need for data cataloging; in fact, effective data cataloging is crucial for managing and understanding the raw data stored in the target system.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->
<!-- Replace everything in square brackets [] and remove brackets  -->

### !challenge

* type: checkbox
* id: 3f431a54-b868-4677-a10b-aed44ec85c95
* title: Data Ingestion and Processing Paradigms

##### !question

Which of the following characteristics accurately describe stream processing?

##### !end-question

##### !options

a| It is generally scheduled to run during off-peak hours to save computing resources.
b| It processes data continuously as it arrives, providing near real-time insights.
c| It is the ideal paradigm for generating standard monthly billing reports.
d| It is utilized for use cases requiring low latency, such as live dashboard updates and fraud detection.
e| It processes data in large chunks that have been accumulating over days or weeks.

##### !end-options

##### !answer

b|
d|

##### !end-answer

##### !explanation

Stream processing is designed to handle continuous, high-velocity data flows with extremely low latency. It processes data records individually or in micro-batches within milliseconds of being generated, making it ideal for applications that require immediate insights and actions, such as live dashboard updates and fraud detection.

<br>

In contrast, batch processing is typically scheduled to run during off-peak hours and processes data in large chunks that have been accumulating over time, making it more suitable for generating standard monthly billing reports or other workloads that do not require real-time insights.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: bd38967a-1d75-4d30-8a7c-2b71875e08bc
* title: Data Ingestion and Processing Paradigms

##### !question

Which of the following is NOT a common role of managed cloud services in data processing?

##### !end-question

##### !options

a| Automatically discovering and cataloging data metadata.
b| Orchestrating ETL or ELT jobs without requiring infrastructure management.
c| Providing a platform for distributed data processing frameworks like Hadoop and Spark.
d| Serving as a highly scalable, fault-tolerant message broker for real-time streaming data.
e| Replacing the need for any data transformation by automatically converting all raw data into business-ready insights.

##### !end-options

##### !answer

e|

##### !end-answer

##### !explanation

While managed cloud services play a crucial role in automating data discovery, cataloging, orchestrating ETL/ELT jobs, providing platforms for distributed processing, and handling real-time streaming data, they do not replace the need for data transformation. Data transformation is a critical step in the analytics process that involves cleaning, structuring, and enriching raw data to make it suitable for analysis.

<br>

Managed services facilitate this transformation but do not automatically convert all raw data into business-ready insights without any user-defined transformations or configurations.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
