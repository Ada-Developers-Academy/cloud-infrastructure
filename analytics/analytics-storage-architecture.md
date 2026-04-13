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

## Storage Considerations for Analytics

When building cloud analytics systems, we must understand that the data storage architectures used for application operations are fundamentally different from those used for analytics. Applications require fast, transactional databases that can handle high volumes of small, atomic read/write operations. In contrast, analytics systems require storage solutions optimized for complex queries and aggregations over large volumes of historical data.

Further, when we design our analytics storage, we must consider the structure of our data and how we intend to use it. Some data may be highly structured and fit well into a relational schema, while other data may be semi-structured or unstructured, requiring more flexible storage solutions. Because we often need vast amounts of historical data for analytics, we also need to ensure that our storage solutions are scalable and cost-effective. Fortunately, some of these concerns can be addressed using modern cloud storage approaches we've already seen, and others require new architectures designed specifically for analytics workloads!

## Differentiating Data Models for Transactions and Analytics

Storing data for everyday application operations (like processing a shopping cart) requires fundamentally different technology than storing data for complex business analytics (like identifying year-over-year sales trends). To build efficient cloud systems, we must understand the different data models used to manage these distinct workloads.

**Online Transaction Processing (OLTP)** systems are optimized for daily, business-critical application operations. They are designed to handle thousands of concurrent users performing high volumes of small, fast read and write transactions (CRUD operations) using relational tables. Because downtime is potentially costly for business-critical tasks, these systems must be highly available and structured as efficiently as possible to process real-time updates in milliseconds. For example, a large retail company uses an OLTP database to track loyalty points, manage payment information, and update inventory levels the moment a product is sold. 

Conversely, **Online Analytical Processing (OLAP)** systems are optimized for complex business queries, aggregations, and multidimensional analysis over large volumes of historical data. Instead of processing rapid single-row updates, OLAP databases periodically ingest large batches of data to analyze patterns and trends. Returning to our retail example, the company's analysts use an OLAP system to perform complex queries across historical sales data, identifying which products were most popular during a specific time period to optimize future budgets.

![A side-by-side architectural comparison. On the left, an OLTP system shows many small, rapid arrows representing thousands of concurrent user transactions (like shopping carts) updating a relational database (which stores current data via rapid, small transactions). On the right, an OLAP system shows a large arrow representing large-scale data extraction moving massive amounts of data into a data warehouse, which after analytical processing, produces a Business Analytics Report (e.g., Sales Trends).](./assets/analytics-oltp-olap.png)  
*Fig. Comparing the workload patterns and data flow of OLTP and OLAP systems.*

## Analytics Storage Architectures

As we move data out of our transactional systems into analytics systems, we need robust storage architectures to house it. The paradigm we choose depends on the structure of our data and how we intend to use it.

### Data Warehouses

**Data Warehouses** are centralized repositories that store highly structured, relational data using a predefined schema. In order to write any data into the warehouse, we must transform it from whatever its original format was into a structured format that fits the warehouse's schema. Hence, this approach is often referred to as a **schema-on-write** approach. This structure ensures that all data adheres to a consistent format, making it easier to query and analyze. This structure also makes data warehouses highly optimized for fast searching, enterprise reporting, and business intelligence (BI) analytics.

Considering the relational data and use of schemas, the description of data warehouses may sound similar to the relational databases we have previously used, but there are important differences. Rather than being optimized for retrieving entire rows of data (entire records) as in OLTP, data warehouses are organized around columns of data, which allows them to efficiently perform complex queries and aggregations across large datasets (OLAP).

Consider a query that calculates the average sales amount for a specific product category. In an OLTP system, the database would need to read through every row of the dataset to find the relevant records, and from those rows only use the `sales_amount` and `product_category` fields. Records (rows) can be very wide, containing many fields that are not relevant to the query, which results in a lot of unnecessary data being read and processed, adding to the time and cost of the query. Experienced Data Base Administrators (DBAs) can optimize this process by creating indexes on specific columns, or extracting relevant fields into dedicated views for faster access, but this adds complexity and maintenance overhead. Fundamentally, storing data in a way that is optimal for reading entire rows will inevitably lead to inefficiencies when we want to perform analytical queries that only need a subset of the fields.

A data warehouse using **columnar storage**, on the other hand, is able to read only the relevant columns (e.g., `sales_amount` and `product_category`), significantly reducing the amount of data that needs to be processed and improving overall query performance. And while an organization might have multiple OLTP databases for different applications, the data warehouse tends to act as a single, centralized repository for all cleaned and structured data that is used for analysis across the organization. The data warehouse acts as a "single source of truth" that analysts can trust for all their reporting and business intelligence needs.

Even with the optimizations of columnar storage, when businesses hosted their own data warehouses on-premises, they often had to limit the amount of historical data they retained due to storage and performance constraints. When loading data into their warehouse, it wasn't just a matter of cleaning the data and ensuring it fit the schema, they also performed certain aggregations at load-time. For example, they might have only retained the last 3 years of sales data, and pre-aggregated that data into monthly totals instead of keeping the raw daily transactions. These pre-calculated levels of aggregation were known as **data cubes**.

The use of **data cubes** was necessary to ensure that queries could run in a reasonable amount of time given the limitations of the hardware. By pre-aggregating the results, the data size was reduced, and the interactive query performance was improved. If a sales manager wanted to see the average sales for a specific product category in a specific month, the warehouse could quickly return the pre-aggregated monthly totals for that category without needing to scan through every individual transaction. However, this approach also meant that they lost the ability to perform more granular analysis on historical data, requiring them to make trade-offs about what data to keep and how to structure it based on their anticipated reporting needs.

Modern cloud infrastructure has changed this dynamic. With the scalability and cost-effectiveness of cloud storage, organizations can forgo the creation of data cubes, instead retaining much larger volumes of historical data in their data warehouses without worrying about performance degradation. This allows them to keep more granular data (e.g., daily transactions instead of monthly aggregates) and perform more detailed analysis on historical trends without needing to make trade-offs about what data to retain.

The gains of being able to retain more historical and granular data in the warehouse are significant, but unchecked growth of data still leads to performance issues and increased costs when running queries. To optimize query performance, modern cloud analytics systems use **Massively Parallel Processing (MPP)** architectures. Though the warehouse data itself uses columnar storage, MPP systems distribute table rows across multiple compute nodes in chunks called **slices**, allowing them to process complex analytical queries in parallel against petabyte-scale datasets. Each compute node works on its own slice of the data, and the results are combined to produce the final query output. This means that instead of a single server handling all the data processing, multiple servers work together to execute queries much faster, even as the volume of data grows.

Data warehouses are ideal for structured data that fits well into a relational schema and is used for enterprise reporting and business intelligence. However, they are not the only storage solution for analytics. As we've seen in the previous lessons, modern applications can use both structured and unstructured data, across structured and unstructured storage systems. This is where data lakes and data lakehouses come into play.

### Data Lakes

A **Data Lake** is a massive, flexible storage system that retains vast amounts of data in its raw, original format. Rather than performing transformations and enforcing a predefined schema before loading data (as in a data warehouse), data lakes allow us to store structured, semi-structured (e.g., JSON, CSV, logs), and unstructured (e.g., images, videos) data simultaneously without careful upfront design.

But what we've described so far probably just sounds like object storage, and in many ways it is! Data lakes are often built on top of cloud object storage services, which provide the necessary infrastructure to manage petabyte-scale datasets. The key difference that makes object storage into a data lake is the way we use it to store and manage data for analytics.

Rather than requiring our downstream analytics systems to work directly with the raw, object storage layer, we can register the data in a **Data Catalog**. A data catalog is a centralized repository that stores metadata about our data assets, making it more accessible to downstream tools that know how to interact with the catalog. The tools interact with the data catalog to discover and understand the data stored in the data lake, rather than needing to know where in object storage the data is located or its underlying format. Using the data catalog to access our data effectively enforces a schema onto the data in the object storage at the time we read it, hence why this approach is often referred to as a **schema-on-read** approach.

Still, even after the data has been registered in the data catalog, its structure may not be efficient for direct analysis since the data in the lake is stored in its raw form. There are two main strategies for making this data more usable for analysts. The first starts to resemble the traditional data warehouse approach of performing some additional transformations on the data. This leads to a strategy for organizing our data known as the **Medallion Architecture**, which we'll cover in more detail shortly. The second strategy is to create virtual "views" of the data that apply transformations on-the-fly when the data is queried, without needing to physically transform and store the data in a different format. While this can make the data more convenient to work with, it can still suffer from performance issues since the transformations are being applied at query time, so it is often used in combination with the Medallion Architecture to optimize performance while still retaining the flexibility of the raw data in the lake.

With either strategy, our goal is to produce a more structured and optimized version of the data that is easier to query and analyze, while still retaining the raw data in the lake for future use cases. This allows us to leverage the flexibility and scalability of the data lake while also providing a more user-friendly interface for analysts to work with the data.

The following table summarizes some of the core differences between data warehouses and data lakes.

| Feature | Data Warehouse | Data Lake |
| --- | --- | --- |
| **Data Structure** | **Highly Structured:** Data must fit a predefined blueprint. | **Flexible:** Stores raw, semi-structured, and unstructured data. |
| **Schema Approach** | **Schema-on-Write:** Enforced at the time the data is loaded. | **Schema-on-Read:** Enforced at the time the data is queried. |
| **Metadata Location** |	**Internal Schema:** The database engine owns and manages the metadata and the files together. | **External Catalog:** Metadata is stored independently of the underlying raw data files. |
| **Storage Strategy** | **Columnar:** Organized by columns for fast math and aggregations. | **Object Storage:** Retains files in their original, raw format. |
| **Optimization** | **Data Cubes (Legacy):** Relied on pre-aggregating data for speed.<br>**MPP Efficiency (Modern):** Uses raw power and "slices" to calculate results on-the-fly. | **Medallion Architecture:** Progressively refines data for specific analysis use cases. |
| **Primary Goal** | **Single Source of Truth:** Clean, trusted data for insights and reporting. | **Massive Flexibility:** Low-cost storage for all data types and future use. |

### Data Lakehouses

Data warehouses and data lakes each have their own strengths and weaknesses. Data warehouses provide a structured, optimized environment for analysis but can be expensive and inflexible when it comes to handling large volumes of raw data. Data lakes offer massive scalability and flexibility at a lower cost, but they can be difficult to query and manage without additional layers of organization and optimization. What if we could have the best of both worlds? Enter the **Data Lakehouse**.

A **Data Lakehouse** combines the low-cost scalability and flexibility of a data lake with the high-performance query capabilities and data management guarantees of a data warehouse. It accomplishes this by using a unified storage layer (often built on top of object storage) that can handle both raw and structured data, along with a metadata layer that provides the necessary schema and organizational structure for efficient querying.

But isn't that just a data lake? Not quite. While a data lake can store raw data, on its own, it doesn't really provide the performance optimizations and data management features that a data warehouse does. A data lakehouse starts with the same raw storage as a data lake but formalizes a number of strategies so that it can also provide the performance and management features of a data warehouse.

When we perform queries through the adapters provided by our data catalog, the query engine has to read the raw data from object storage and apply transformations on-the-fly to produce the results. By comparison, data warehouses are able to pre-index and pre-cache data to optimize query performance. Data lakehouses use a variety of techniques to bridge this gap, such as using columnar storage formats, automatically indexing data sources, implementing caching layers, and leveraging MPP approaches similar to those used in data warehouses, to ensure that queries can run efficiently even against large volumes of raw data.

They also implement data management features such as ACID transactions, schema enforcement, and data versioning to provide the same level of reliability and consistency that we expect from a data warehouse. This means that with a data lakehouse, we can have the flexibility to store all our raw data while also providing a performant and reliable environment for analysis, without needing to choose between a data lake or a data warehouse.

## Columnar Data in Object Storage

Data warehouses often have their own proprietary internal storage formats that are optimized for analytical queries, based around the idea of **Columnar Storage**. Data lakes, on the other hand, typically store data in its raw format in object storage, which is not optimized for analytical queries. To bridge this gap, we can use columnar storage formats that are designed to work efficiently with object storage while still providing the performance benefits of columnar data.

It may seem strange to think about storing data in a columnar format without having a particular query engine in mind, but we have many such file formats for row-oriented data, such as CSV, tab-delimited text, or even Excel files. There are even row-oriented versions of JSON, such as JSON Lines (JSONL), which is a format where each line of the file is a separate JSON object. These formats are designed to be easy to read and write for row-based data, but they are not optimized for analytical queries that need to read specific columns across many rows.

Having a columnar storage format allows us to store data in a way that is optimized for analytical queries, even when the data is stored in its raw form in object storage. Two of the most popular columnar storage formats are **Apache Parquet** (read "par-KAY") and **Apache ORC** (rhymes with "fork" and stands for Optimized Row Columnar).

These formats are designed to be efficient for both storage and query performance, allowing us to read only the relevant columns when performing analytical queries, which can significantly reduce the amount of data that needs to be processed and improve query performance. Storing data column-by-column also allows these formats to offer high data compression ratios, since all of the data in a single column should have similar characteristics, allowing compression to be extremely effective. By reducing the amount of disk I/O required to aggregate specific fields across large datasets, columnar storage formats can significantly improve the performance of analytical queries, even when the data is stored in its raw form in object storage!

## Organizing Data with the Medallion Architecture

One of the drawbacks to storing data in its raw form in a data lake is that it can be difficult to query and analyze without additional layers of organization and optimization. While being able to store all our raw data is a key feature of data lakes, it can also lead to a disorganized "data swamp" if we don't have a clear strategy for how to organize and manage that data. To address this challenge, we can use the **Medallion Architecture** to logically organize our data and think about how to structure it to make it more usable for analysis.

The **Medallion Architecture** is a logical framework for organizing data in a data lake into three connected stages: Bronze, Silver, and Gold. Each stage represents a different level of refinement and optimization for the data, with the goal of progressively transforming raw data into business-ready insights.

* **Bronze Layer:** This foundational layer ingests and stores raw, unchanged data directly from the source. It maintains the data's original structure without any cleanup or validation, ensuring we always have an unaltered historical record.
* **Silver Layer:** In this intermediate stage, the data has been filtered, cleaned, and validated. It provides a reliable, standardized view of the data that is ready for low-level queries and exploratory analysis. It leverages columnar storage formats to optimize query performance while still retaining enough detail for analysts to perform their work. This layer is ideal for data scientists and analysts who need to work with the data in a more structured format but still want access to the underlying details for their analyses.
* **Gold Layer:** This final layer contains highly curated, business-level aggregated data. It is designed specifically to power executive dashboards, enterprise reports, and machine learning inputs, representing the most refined insights in our system.

![Three connected stages labeled Bronze (Raw Data Ingestion & Historical Record), Silver (Cleaned, Validated & Standardized Data), and Gold (Highly Curated, Aggregated Insights). The Bronze stage shows a mix of raw, unformatted data files. An arrow points to the Silver stage, which displays neatly organized database tables. A final arrow points to the Gold stage, which features a polished bar chart and a business dashboard.](./assets/analytics-medallion.png)  
*Fig. The Medallion Architecture logically organizes the flow of data from raw inputs to business-ready insights.*

Just because data is considered to be in a different layer doesn't mean it has to be stored in a different physical location. In practice, all three layers of the Medallion Architecture can be stored within the same data lake or data lakehouse, using different folders or prefixes to logically separate them. This allows us to maintain a clear organizational structure while leveraging the same underlying storage infrastructure.

External tools needn't be aware of the exact internal structure of the data lake, as they can interact with the data through the data catalog, which abstracts away the underlying storage details. By using the Medallion Architecture to logically organize our data, we can ensure that it remains usable and accessible for analysis while still retaining the flexibility and scalability of a data lake.

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
