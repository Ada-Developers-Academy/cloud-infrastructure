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

```
_Alt. A side-by-side architectural comparison. On the left, an OLTP system shows many small, rapid arrows representing thousands of concurrent user transactions (like shopping carts) updating a relational database. On the right, an OLAP system shows a few large arrows extracting massive amounts of aggregated historical data for a business report._
*Fig. Comparing the workload patterns and data flow of OLTP and OLAP systems.*
```

## Structuring Data with Storage Paradigms

As we move data out of our transactional systems for analysis, we need robust storage architectures to house it. The paradigm we choose depends on the structure of our data and how we intend to use it.

**Data Warehouses** are centralized repositories that store highly structured, relational data using a predefined blueprint, known as a schema-on-write approach. Because the data is cleaned, enriched, and transformed before it is stored, the data warehouse acts as a "single source of truth" that users can trust. This structure makes data warehouses highly optimized for fast searching, enterprise reporting, and business intelligence analytics.

However, data warehouses might not properly handle the raw, unstructured data necessary for modern machine learning workloads. This is where **Data Lakes** come in. A data lake is a massive, flexible storage system that retains vast amounts of data in its raw, original format. By using a schema-on-read approach, we can store structured, semi-structured, and unstructured data simultaneously without careful upfront design. This makes data lakes highly scalable, affordable, and ideal for exploratory data science and machine learning applications. 

### !callout-info

## Schema-on-Write vs. Schema-on-Read

The key difference between data warehouses and data lakes lies in how they handle schemas.

Data warehouses use a **schema-on-write** approach, meaning the structure of the data must be defined before it can be loaded. This ensures that all data adheres to a consistent format, making it easier to query but less flexible for unstructured data.

Data lakes, on the other hand, use a **schema-on-read** approach. This means that data can be stored in its raw form without any predefined structure. The schema is applied only when the data is read for analysis, allowing for greater flexibility but requiring more effort to ensure data quality and consistency during analysis.

### !end-callout


Recently, a modern, unified architecture known as the **Data Lakehouse** has emerged. A data lakehouse combines the low-cost scalability and flexibility of a data lake with the high-performance query capabilities and data management guarantees of a data warehouse. 

> *Cloud Provider Callout: Amazon S3, Google Cloud Storage, and Azure Blob Storage are commonly used as the foundational object storage for building highly durable data lakes.*

## Optimizing Query Performance with Storage Formats

When analyzing petabytes of data, reading information row-by-row is too slow and expensive. To ensure our analytical systems remain secure and efficient, we must use storage formats and processing architectures designed specifically for heavy analytical reads.

While traditional OLTP databases store data row-by-row to optimize random writes, analytical OLAP systems utilize **Columnar Storage** formats, such as Apache Parquet (read as "par-kay") or ORC (rhymes with "fork"). By storing data column-by-column, these formats offer high data compression and optimize query performance by drastically reducing the amount of disk I/O required to aggregate specific fields.

Modern cloud analytics rely on **Massively Parallel Processing (MPP)** architectures to query this columnar storage. MPP systems distribute table rows across multiple compute nodes, allowing them to process complex analytical queries in parallel against petabyte-scale datasets. 

> *Cloud Provider Callout: For MPP data warehousing, fully managed cloud services like Amazon Redshift, Google BigQuery, and Azure Synapse Analytics are industry standards capable of handling massive analytical workloads.*

## Organizing Data with the Medallion Architecture

If we ingest raw data into our storage systems without a logical organizational strategy, our data lake can quickly degrade into a disorganized "data swamp," making it difficult to extract meaningful insights. To prevent this, we organize data logically as it flows through our pipelines using the **Medallion Architecture**.

*   **Bronze Layer:** This foundational layer ingests and stores raw, unchanged data directly from the source. It maintains the data's original structure without any cleanup or validation, ensuring we always have an unaltered historical record.
*   **Silver Layer:** In this intermediate stage, the data has been filtered, cleaned, and validated. It provides a reliable, standardized view of the data that is ready for low-level queries and exploratory analysis.
*   **Gold Layer:** This final layer contains highly curated, business-level aggregated data. It is designed specifically to power executive dashboards, enterprise reports, and machine learning inputs, representing the most refined insights in our system.

```
_Alt. Three connected stages labeled Bronze, Silver, and Gold. The Bronze stage shows a mix of raw, unformatted data files. An arrow points to the Silver stage, which displays neatly organized database tables. A final arrow points to the Gold stage, which features a polished bar chart and a business dashboard._
*Fig. The Medallion Architecture logically organizes the flow of data from raw inputs to business-ready insights.*
```

## Summary

Storing data for daily operations requires fundamentally different technologies than storing data for analytics. We use OLTP databases for fast, transactional application updates, and OLAP systems for complex, historical business queries. To manage analytical workloads efficiently in the cloud, we leverage Data Warehouses for highly structured reporting and Data Lakes for massive, flexible, raw data storage. By utilizing columnar storage formats, Massively Parallel Processing, and the organized layers of the Medallion Architecture, we ensure our data remains performant, organized, and ready to deliver valuable business insights.

## Check for Understanding

1. **Multiple Choice:** Which data model is specifically optimized to quickly process thousands of small, atomic read/write transactions per minute for a business-critical application?
   * A) Massively Parallel Processing (MPP)
   * B) Online Transaction Processing (OLTP)
   * C) Online Analytical Processing (OLAP)
   * D) Data Lakehouse

2. **Choose all that apply:** Which of the following characteristics accurately describe a Data Lake?
   * A) It retains data in its raw, original format without requiring an upfront schema.
   * B) It is highly optimized for daily, transactional CRUD operations.
   * C) It strictly requires a predefined relational schema before data can be loaded (schema-on-write).
   * D) It can store structured, semi-structured, and unstructured data simultaneously.
   * E) It is a highly scalable storage system often used as a foundation for machine learning workloads.

3. **Reordering:** Arrange the layers of the Medallion Architecture in the correct chronological order of data flow, starting from the initial ingestion of data to the final reporting layer.
   * Gold Layer (curated, business-level aggregated data)
   * Bronze Layer (raw, unchanged data)
   * Silver Layer (filtered, cleaned, and validated data)