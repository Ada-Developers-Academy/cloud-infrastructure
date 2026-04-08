# Data Storage Architectures for Cloud Analytics

## Learning Goals
*   Differentiate between Online Transaction Processing (OLTP) and Online Analytical Processing (OLAP) data models.
*   Compare the purposes and structures of Data Warehouses, Data Lakes, and Data Lakehouses.
*   Explain how columnar storage and Massively Parallel Processing (MPP) optimize analytical queries.
*   Describe how data is logically organized using the Medallion Architecture.

## Vocabulary and Synonyms
| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **OLTP** | Systems optimized for daily, business-critical application operations handling fast, atomic transactions. | Online Transaction Processing | Our retail website uses an OLTP database to instantly update inventory when a customer checks out. |
| **OLAP** | Systems optimized for complex business queries and aggregations over large volumes of historical data. | Online Analytical Processing | We query our OLAP system to generate the quarterly sales performance reports. |
| **Data Warehouse** | A centralized repository storing highly structured, relational data using a predefined schema optimized for fast searching and reporting. | Single Source of Truth | We load our cleaned financial records into the data warehouse so our business intelligence tools can securely access them. |
| **Data Lake** | A massive, flexible storage system that retains vast amounts of data in its raw, original format without requiring an upfront schema. | Raw Storage Repository | Our application streams millions of unformatted event logs directly into the data lake for future exploratory analysis. |

## Differentiating Data Models for Transactions and Analytics

Storing data for everyday application operations (like processing a shopping cart) requires fundamentally different technology than storing data for complex business analytics (like identifying year-over-year sales trends). To build efficient cloud systems, we must understand the different data models used to manage these distinct workloads.

**Online Transaction Processing (OLTP)** systems are optimized for daily, business-critical application operations. They are designed to handle thousands of concurrent users performing high volumes of small, fast read and write transactions (CRUD operations) using relational tables. Because downtime is potentially costly for business-critical tasks, these systems must be highly available and structured as efficiently as possible to process real-time updates in milliseconds. For example, a large retail company uses an OLTP database to track loyalty points, manage payment information, and update inventory levels the moment a product is sold. 

Conversely, **Online Analytical Processing (OLAP)** systems are optimized for complex business queries, aggregations, and multidimensional analysis over large volumes of historical data. Instead of processing rapid single-row updates, OLAP databases periodically ingest large batches of data to analyze patterns and trends. Returning to our retail example, the company's analysts use an OLAP system to perform complex queries across historical sales data, identifying which products were most popular during a specific time period to optimize future budgets.

![A side-by-side architectural comparison. On the left, an OLTP system shows many small, rapid arrows representing thousands of concurrent user transactions (like shopping carts) updating a relational database (which stores current data via rapid, small transactions). On the right, an OLAP system shows a large arrow representing large-scale data extraction moving massive amounts of data into a data warehouse, which after analytical processing, produces a Business Analytics Report (e.g., Sales Trends).](./assets/analytics-oltp-olap.png)  
*Fig. Comparing the workload patterns and data flow of OLTP and OLAP systems.*

## Structuring Data with Storage Paradigms

As we move data out of our transactional systems for analysis, we need robust storage architectures to house it. The paradigm we choose depends on the structure of our data and how we intend to use it.

**Data Warehouses** are centralized repositories that store highly structured, relational data using a predefined blueprint, known as a schema-on-write approach. Because the data is cleaned, enriched, and transformed before it is stored, the data warehouse acts as a "single source of truth" that users can trust. This structure makes data warehouses highly optimized for fast searching, enterprise reporting, and business intelligence analytics. The systems that power data warehouses are typically custom built to handle the specific needs of analytical workloads, such as columnar storage formats and Massively Parallel Processing (MPP) architectures, which we'll discuss in more detail shortly.

However, data warehouses might not properly handle the raw, unstructured data necessary for modern machine learning workloads. This is where **Data Lakes** come in. A data lake is a massive, flexible storage system that retains vast amounts of data in its raw, original format. By using a schema-on-read approach, we can store structured, semi-structured, and unstructured data simultaneously without careful upfront design. This makes data lakes highly scalable, affordable, and ideal for exploratory data science and machine learning applications. Data lakes are often built on top of cloud object storage services, which provide the necessary infrastructure to manage petabyte-scale datasets.

### !callout-info

## Schema-on-Write vs. Schema-on-Read

Data warehouses use a **schema-on-write** approach, meaning the structure of the data must be defined before it can be loaded. This ensures that all data adheres to a consistent format, making it easier to query but less flexible for unstructured data.

Data lakes, on the other hand, use a **schema-on-read** approach. This means that data can be stored in its raw form without any predefined structure. The schema is applied only when the data is read for analysis, allowing for greater flexibility but requiring more effort to ensure data quality and consistency during analysis.

### !end-callout

More recently, a modern, unified architecture known as the **Data Lakehouse** has emerged. A data lakehouse combines the low-cost scalability and flexibility of a data lake with the high-performance query capabilities and data management guarantees of a data warehouse. Typically, a data lakehouse is built on top of a data lake, using open storage formats and metadata layers that provide details about the structure of objects in the data lake. This allows it to provide the structure and performance of a warehouse while retaining the flexibility of a lake.

## Optimizing Query Performance with Storage Formats

When analyzing petabytes of data, reading information row-by-row is too slow and expensive. To ensure our analytical systems remain secure and efficient, we must use storage formats and processing architectures designed specifically for heavy analytical reads.

While traditional OLTP databases store data row-by-row to optimize random writes, analytical OLAP systems utilize **Columnar Storage** formats, such as Apache Parquet (read as "par-KAY") or ORC (rhymes with "fork" and stands for Optimized Row Columnar). By storing data column-by-column, these formats offer high data compression and optimize query performance by drastically reducing the amount of disk I/O required to aggregate specific fields.

For example, if we want to calculate the average sales amount for a specific product category, a columnar storage format allows us to read only the relevant `sales_amount` and `product_category` columns, rather than scanning through every row of the dataset. This results in significantly faster query execution times and reduced costs when working with large datasets.

In order to query this columnar storage efficiently, modern cloud analytics rely on **Massively Parallel Processing (MPP)** architectures. MPP systems distribute table rows across multiple compute nodes, allowing them to process complex analytical queries in parallel against petabyte-scale datasets. This means that instead of a single server handling all the data processing, multiple servers work together to execute queries much faster.

## Organizing Data with the Medallion Architecture

If we ingest raw data into our storage systems without a logical organizational strategy, our data lake can quickly degrade into a disorganized "data swamp," making it difficult to extract meaningful insights. To prevent this, we organize data logically as it flows through our pipelines, describing its transformation and refinement stages, using the **Medallion Architecture**.

* **Bronze Layer:** This foundational layer ingests and stores raw, unchanged data directly from the source. It maintains the data's original structure without any cleanup or validation, ensuring we always have an unaltered historical record.
* **Silver Layer:** In this intermediate stage, the data has been filtered, cleaned, and validated. It provides a reliable, standardized view of the data that is ready for low-level queries and exploratory analysis.
* **Gold Layer:** This final layer contains highly curated, business-level aggregated data. It is designed specifically to power executive dashboards, enterprise reports, and machine learning inputs, representing the most refined insights in our system.

![Three connected stages labeled Bronze (Raw Data Ingestion & Historical Record), Silver (Cleaned, Validated & Standardized Data), and Gold (Highly Curated, Aggregated Insights). The Bronze stage shows a mix of raw, unformatted data files. An arrow points to the Silver stage, which displays neatly organized database tables. A final arrow points to the Gold stage, which features a polished bar chart and a business dashboard.](./assets/analytics-medallion.png)  
*Fig. The Medallion Architecture logically organizes the flow of data from raw inputs to business-ready insights.*

Just because data is considered to be in a different layer doesn't mean it has to be stored in a different physical location. In practice, all three layers of the Medallion Architecture can be stored within the same data lake or data lakehouse, using different folders or prefixes to logically separate them. This allows us to maintain a clear organizational structure while leveraging the same underlying storage infrastructure.

## Summary

Storing data for daily operations requires fundamentally different technologies than storing data for analytics. We use OLTP databases for fast, transactional application updates, and OLAP systems for complex, historical business queries. To manage analytical workloads efficiently in the cloud, we leverage Data Warehouses for highly structured reporting and Data Lakes for massive, flexible, raw data storage. By utilizing columnar storage formats, Massively Parallel Processing, and the organized layers of the Medallion Architecture, we ensure our data remains performant, organized, and ready to deliver valuable business insights.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: b48b9a22-7c49-4bc7-9fce-25b706b12022
* title: Data Storage Architectures for Cloud Analytics

##### !question

Which data model is specifically optimized to quickly process thousands of small, atomic read/write transactions per minute for a business-critical application?

##### !end-question

##### !options

a| Data Lakehouse
b| Massively Parallel Processing (MPP)
c| Online Transaction Processing (OLTP)
d| Online Analytical Processing (OLAP)

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

OLTP (Online Transaction Processing) systems are designed to handle high volumes of small, fast read/write transactions, making them ideal for business-critical applications that require real-time updates. OLTP systems are typically how we think of traditional relational databases, such as PostgreSQL or MySQL, which are optimized for transactional workloads.

<br>

In contrast, OLAP (Online Analytical Processing) systems are optimized for complex queries and aggregations over large volumes of historical data, while Data Lakehouses combine the flexibility of data lakes with the performance of data warehouses. Massively Parallel Processing (MPP) is an architecture used to optimize query performance in analytical systems but is not a data model itself.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->
<!-- Replace everything in square brackets [] and remove brackets  -->

### !challenge

* type: checkbox
* id: 973077c0-c57a-46ef-a567-c2521ae4c7ab
* title: Data Storage Architectures for Cloud Analytics

##### !question

Which of the following characteristics accurately describe a Data Lake?

##### !end-question

##### !options

a| It retains data in its raw, original format without requiring an upfront schema.
b| It is highly optimized for daily, transactional CRUD operations.
c| It strictly requires a predefined relational schema before data can be loaded (schema-on-write).
d| It can store structured, semi-structured, and unstructured data simultaneously.
e| It is a highly scalable storage system often used as a foundation for machine learning workloads.

##### !end-options

##### !answer

a|
d|
e|

##### !end-answer

##### !explanation

Data Lakes are designed to retain data in its raw, original format without requiring an upfront schema, which is known as a schema-on-read approach. This allows them to store structured, semi-structured, and unstructured data simultaneously, making them highly flexible for various types of data. Additionally, data lakes are often built on top of cloud object storage services, providing the necessary infrastructure to manage petabyte-scale datasets, which is essential for machine learning workloads.

<br>

Being optimized for daily, transactional CRUD operations is a characteristic of OLTP systems, not data lakes. Data warehouses, not data lakes, typically require a predefined relational schema before data can be loaded (schema-on-write), while data lakes allow for more flexibility in how data is stored and accessed.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
