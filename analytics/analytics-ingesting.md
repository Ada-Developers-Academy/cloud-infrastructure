# Data Ingestion and Processing Paradigms

## Learning Goals
* Differentiate between batch processing and stream processing workloads.
* Compare traditional ETL data pipelines with modern ELT architectures.
* Identify the roles of managed data integration, big data clusters, and real-time streaming platforms in a cloud ecosystem.

## Vocabulary and Synonyms
| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Data Pipeline** | A series of data processing steps that move raw data from its source to a destination where it can be analyzed. | Data Workflow | Our data pipeline ensures that raw records are cleaned and formatted before reaching the reporting team. |
| **ETL (Extract, Transform, Load)** | A traditional data pipeline model where data is extracted from sources, transformed into a desired format, and then loaded into a target storage system. | Traditional Data Pipeline | We use an ETL pipeline to process our daily sales data, transforming it before loading it into the data warehouse. |
| **ELT (Extract, Load, Transform)** | A modern data pipeline model where data is extracted and immediately loaded into a target system, with transformations applied on-demand. | Modern Data Pipeline | Our ELT pipeline allows us to store all raw clickstream data in the data lake and transform it as needed for different analyses. |
| **Batch Processing** | A data processing method that handles large volumes of accumulated data at scheduled intervals. | Scheduled Processing | We run batch processing jobs overnight to generate our monthly financial reports. |
| **Stream Processing** | A data processing method that handles continuous, high-velocity data flows with low latency. | Real-Time Processing | We use stream processing to analyze live sensor data from our IoT devices and trigger alerts for anomalies. |
| **Real-Time Data Streaming Service** | A service that captures, buffers, and processes continuous streams of data with high durability and low latency. | Message Broker | Our real-time streaming platform ensures that we can process millions of events per second without losing any data. |

## Ingestion: The First Step in the Analytics Journey

Before we can analyze data, we must first capture it. The ingestion stage is responsible for collecting raw data from various sources in a reliable and scalable manner. Data can arrive at different speeds which requires different processing paradigms. Data can arrive in different formats, which requires different approaches to how we transform and store it. Data can also arrive in different volumes, which requires different levels of scalability and durability in our storage solutions.

Ingestion must take all of this into account to ensure that we have the necessary data available for downstream processing and analysis.

## Data Pipelines
Raw data must be extracted from its source and formatted before it can be effectively analyzed. We must design data pipelines that efficiently route and clean this information without creating bottlenecks in our operational systems.

**Data pipelines** are the end-to-end flows that move raw data from source systems into a useful, analyzable form. They comprise a series of steps that *extract* data from various sources, *transform* it into a consistent format, and *load* it into a target storage system. The name of this final step "load" can be confusing, since it might sounds like we're loading the data somewhere else. A more intuitive name for this step might be "store" or "save", since the data is being stored in a data lake or data warehouse, not necessarily loaded into another system. However, the term "load" is most commonly used in the industry to refer to this final step of the pipeline.

Taken all together, this process has traditionally been known as ETL (Extract, Transform, Load), where data is transformed before being loaded into storage. However, modern cloud architectures based around data lakes often use ELT (Extract, Load, Transform), where raw data is loaded first and transformed on-demand using the cloud's distributed compute power. Both paradigms are still widely used, and the choice between them depends on the specific requirements of the data and the business needs we aim to address. We will explore both approaches in more detail in the next sections.

### ETL (Extract, Transform, Load)

ETL represents the traditional data pipeline model. Raw data is extracted from a source, transformed into whatever format our schema requires using a dedicated transformation service, and then loaded into a target storage system like a data warehouse. Once in the data warehouse, further computations and aggregations can be performed to prepare the data for analysis. This process can be quite resource-heavy, so jobs are often scheduled during off-peak hours to reduce the strain on the original data source systems.

This approach was very dominant in the era of on-premises data centers, where compute and storage resources were limited and expensive. As mentioned in the discussion of data storage, in order to save on storage costs, organizations made trade-offs by only transforming and storing data that they predicted would be valuable for analysis. This results in large volumes of raw data being discarded or archived without ever being analyzed, potentially leading to missed insight opportunities. As data volumes continue to grow while storage costs decrease, and as the need for more flexible, on-demand analysis increases, the limitations of the ETL model are harder to justify.

However, the ETL model is still widely used for certain workloads, such as generating standard monthly billing reports or performing complex transformations that require a lot of compute power. For more moderate workloads, it is being replaced by the more modern ELT approach.

![A split diagram comparing data pipelines. The left ETL path shows data moving from a Source to a Transformation server, and finally to a Data Warehouse. The right ELT path shows data moving directly from a Source to a Data Lake, with a looping arrow indicating Transformation happening inside the Data Lake itself, often on-demand at query time.](./assets/analytics-etl-elt.png)  
*Fig. Comparing traditional ETL pipelines with modern ELT pipelines.*

### ELT (Extract, Load, Transform)

ELT represents the modern cloud-native approach to ingestion. Raw data is extracted and immediately loaded directly into a highly scalable target system, such as a data lake or lakehouse, in its original state. When organizing our data lake using the Medallion Architecture, this raw, unaltered storage serves as our "Bronze" layer. Transformation steps happen *after* the data is securely stored, producing our "Silver" and "Gold" layers. Some transformations may also happen on-demand, using features of our data catalog to apply business rules and definitions at query time, without needing to create new physical tables. This allows us to leverage the massive, distributed compute power of the cloud storage environment to process the data when needed, providing greater flexibility and scalability compared to traditional ETL pipelines.

The significant advantage of ELT is that it allows us to store all raw data without needing to predict in advance which data will be valuable. This means we can both retain a complete historical record and apply data transformations for different use cases, using the same transformation services that can process our more traditional ETL workloads. But we can do so without being limited by the constraints of a traditional ETL pipeline.

ELT is particularly well-suited for handling large volumes of semi-structured or unstructured data, such as logs, click streams, and social media feeds, which may not fit neatly into a relational schema. Storing them in a data lake in their raw form allows us to apply different transformations and analyses as our needs evolve.

### Transformation Services

Cloud providers offer a variety of managed services to help us transform our incoming data for our analytics needs. These services can handle complex, distributed processing of petabyte-scale datasets. They allow us to focus on writing our data transformation logic while providing options about how much operational management we want to take on.

To manage large-scale data transformation workloads, transformation services typically take the form of managed cluster environments designed to host distributed data processing frameworks, such as Apache Hadoop and Apache Spark. There ability to split workload across multiple nodes allows them to efficiently process massive datasets that would be impractical to handle on a single server.

Many of these services are also available in a serverless format, allowing us to run big data processing jobs without needing to manage the underlying cluster infrastructure. This means we can focus on writing our data transformation logic while the cloud provider handles provisioning, scaling, and maintaining the cluster.

## Velocity and Volume: Choosing the Right Processing Paradigm

In a cloud-based analytics system, raw data rarely arrives perfectly formatted or at a convenient speed. Organizations must handle massive historical dumps as well as continuous, high-speed events. Understanding the volume and velocity of data being ingested is crucial for selecting the most appropriate processing paradigm and tools to ensure that data is transformed and made available for analysis in a timely manner.

If data arrives in large batches or does not require immediate insights, **Batch Processing** is often the most efficient approach. Whereas if data arrives continuously and requires real-time analysis, **Stream Processing** may be necessary to capture and process data to produce rapid insights.

### Batch Processing

**Batch Processing** is an ingestion method used to process large volumes of accumulated data at scheduled, periodic intervals. Certain data processing tasks are compute-intensive and inefficient to run on individual transactions as they occur. Instead, it can be more efficient to collect data over a period of time, such as daily or weekly, and process it all at once. This approach is highly efficient for heavy, complex transformations, but since we only process the data periodically, it introduces latency between when data is generated and when insights are available. As a result, batch processing is best suited for workloads that do not require rapid responses.

For example, consider an e-commerce system that receives orders throughout the day. Instead of processing analytics for every order immediately, the system collects all orders at the end of each day and processes them in one batch for the fulfillment team. Fulfillment requires a comprehensive view of all orders to optimize delivery routes and inventory management, and waiting until the end of the day to process this data is acceptable since it would not introduce significant delays in the fulfillment process.

### Stream Processing

Alternatively, **Stream Processing** is an ingestion method designed to handle continuous, high-velocity data flows with extremely low latency. Data records are captured and processed individually or in micro-batches within milliseconds of being generated. This paradigm is essential for applications requiring immediate action in response to application events.

For example, we can track changes in public sentiment regarding our products by continuously analyzing clickstream data and social media streams in real-time, enabling us to respond promptly. Or we can monitor financial transactions for signs of fraud, where even a delay of a few seconds could result in significant losses. Stream processing allows us to capture data as it arrives, as well as to kick off real-time analytics and alerting.

Neither batch nor stream processing is inherently better than the other; they are simply different tools for different use cases. Our task is to choose the appropriate processing paradigm based on the specific requirements of our data and the business needs we aim to address.

### Streaming Services

Stream processing requires a different set of tools than batch processing. To process fast-moving data without loss, we use highly scalable, fault-tolerant **Real-time Data Streaming Services**. These services robustly buffer, capture, and process continuous streams of data at a massive scale. Apache Kafka is a dominant open source framework that is commonly offered in a managed format by cloud providers, along with provider-specific offerings. These platforms allow us to handle high volumes of streaming data, with low latency and high reliability.

These services are:

- **highly durable** through replication across multiple availability zones,
- **decoupled** through data buffering so that producers and consumers can operate independently, and
- **scalable** through sharding or partitioning to handle varying volumes of data without loss, even during traffic spikes.

## Scenario-Driven Data Pipeline Examples

Understanding the distinct roles of data pipelines is much easier when we see them working together to solve real business problems. Because organizations face different challenges regarding the volume, velocity, and variety of their data, we rarely rely on a single, one-size-fits-all pipeline. 

Let's explore three practical scenarios to see how the different processing paradigms and pipeline architectures come together to handle various data requirements.

### Traditional ETL for High-Volume Relational Data

When we have well-structured, relational data—think massive daily logs of financial transactions or inventory updates—we need a reliable pipeline that cleans and formats this data so it can power complex business reports. In this scenario, we prioritize governance, structure, and reporting speed over real-time flexibility.

In this pipeline, data moves in a scheduled, predictable batch process:

1. **Extraction:** We extract the high-volume relational data from our operational databases during off-peak hours to avoid impacting our daily application performance.
2. **Transformation:** The data flows into a powerful transformation service, which scrubs the data for errors, standardizes formats, and joins related tables.
3. **Loading:** The polished, highly structured data is loaded into a data warehouse. This warehouse is optimized for complex, historical aggregations.

### ELT and the Medallion Architecture for Semi-Structured Data

Modern applications often generate massive volumes of semi-structured data, like JSON files from user clickstreams or mobile app interactions. If we tried to force this massive, flexible data into a rigid relational database right away, our pipeline would bottleneck. Instead, we use an Extract, Load, Transform (ELT) approach built around a data lake and the Medallion Architecture.

In this pipeline, we leverage the vast scale of cloud object storage and apply structure only when we need it:

1. **The Bronze Layer:** We extract the raw JSON clickstream data and load it directly into a highly scalable object store without any modifications. This serves as our data lake's foundational "Bronze" layer, ensuring we never lose the original data.
2. **Cataloging:** A data catalog automatically crawls the raw files in the object store, inferring the schema and registering the metadata so our tools know what information is available.
3. **The Silver Layer:** We use a transformation service to read the Bronze data, clean out any corrupted records, and rewrite the data back to the object store in a highly compressed, columnar format like Apache Parquet. 
4. **The Gold Layer:** We run a final transformation to aggregate the cleaned data into curated, business-level metrics (e.g., "daily clicks per user"), storing this "Gold" data in the object store.

### Real-Time Stream Processing and Anomaly Detection

Some data loses its value if we wait for an overnight batch job to process it. When dealing with high-velocity data, such as a continuous stream of IoT sensor readings or live credit card swipes, we need a pipeline that can react instantly to anomalies while also retaining the data for future historical analysis.

In this pipeline, we split the data stream to serve two different architectural needs simultaneously:

1. **Ingestion:** Thousands of IoT sensors continuously push tiny data payloads to a highly scalable, real-time data stream processor. This service securely buffers the continuous flow so no records are dropped during traffic spikes.
2. **Real-Time Path (Stream Processing):** As the data flows through the stream, a dedicated stream processing engine analyzes the records in milliseconds. It looks for specific conditions—such as a sensor reporting a temperature spike above a safe threshold. If an anomaly is detected, it instantly triggers an alert or an automated response.
3. **Historical Path (Data Lake Storage):** Simultaneously, the stream processor batches the incoming raw data and delivers it to our centralized object store (Data Lake). 

### Choose the Right Pipeline for the Job

By exploring these scenarios, we can see that building a modern ingestion pipeline requires matching the right tools to the shape and speed of our data. We use traditional ETL pipelines and data warehouses for governed, high-volume structured reporting. When handling massive volumes of semi-structured data, we rely on object stores, data catalogs, and the Medallion Architecture to clean our data with greater flexibility. Finally, when we need to catch anomalies the moment they happen, we utilize stream processing engines that react in milliseconds while safely archiving the raw events in our data lake for the future.

## Summary

We explored the differences between how traditional ETL pipelines transform data before loading it into storage, while modern ELT architectures load raw data first and transform it on-demand, leveraging the cloud's distributed compute power. We saw how to process data efficiently by considering batch processing for high-volume, scheduled jobs, and stream processing for continuous, low-latency data flows. Finally, we looked at practical scenarios that demonstrate how different pipeline architectures and processing paradigms come together to solve real business problems, showing that the best approach depends on the specific requirements of our data and the insights we need to generate.

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

Which of the following is NOT a common role of data processing cloud services?

##### !end-question

##### !options

a| Managing multiple layers of data transformation, from raw to business-ready formats, using the Medallion Architecture.
b| Periodically importing massive data dumps for batch processing workloads.
c| Providing a platform for distributed data processing frameworks like Hadoop and Spark.
d| Serving as a highly scalable, fault-tolerant processor for real-time streaming data.
e| Replacing the need for any data transformation by automatically converting all raw data into business-ready insights.

##### !end-options

##### !answer

e|

##### !end-answer

##### !explanation

Cloud data processing services play a crucial role in the data catalogs that help enable multiple layers of data transformation, managing periodic batch processing, providing platforms for distributed processing, and handling real-time streaming data. They do not replace the need for data transformation. Data transformation is a critical step in the analytics process that involves cleaning, structuring, and enriching raw data to make it suitable for analysis.

<br>

Data processing services facilitate this transformation but do not automatically convert all raw data into business-ready insights without any user-defined transformations or configurations.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
