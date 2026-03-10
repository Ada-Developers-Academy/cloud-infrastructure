# Object Storage

## Learning Goals

- Define object storage and its key benefits and limits.
- Describe common classes of storage available within object storage.
- Describe the difference between hot vs cold data and how that relates to storage classes.
- Describe use cases for object storage.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| ----- | ---------- | -------- | ------------------------ |
| Bucket | A fundamental container in cloud object storage. In object storage, we create buckets which hold our individual pieces of data as objects. |  | "After creating a new bucket in the Northern Virginia region, I uploaded the website's static assets to ensure low-latency access for East Coast users." |
| Hot Data | Frequently accessed data, often essential to continuous operations. |  | "Our current month’s sales figures are hot data because our analysts query them hundreds of times a day to track performance." |
| Cold Data | Infrequently accessed or archived data. |  | "As a healthcare company, we have a lot of cold data that we maintain for compliance reasons." |
| Storage Class | A setting within object storage that determines the cost, availability, and performance of storage based on access frequency. Storage classes within object storage range from more expensive "hot" storage to much cheaper "cold" storage. | Storage Tier | "Because the application requires millisecond retrieval speeds, we must keep all active user profile images in the Standard storage class." |
| Hot Storage | High-performance, higher-cost storage tier for frequently accessed, active data requiring immediate retrieval. |  | "Our customer support team needs to access current account balances instantly, so that information is kept in hot storage for immediate retrieval." |
| Cold Storage | Low-cost, lower-performance storage tier designed for rarely accessed, archived, or backup data where longer retrieval times are acceptable. |  | "Since we only need to access them for occasional compliance audits, we moved all customer records from five years ago into cold storage." |
| Data Lifecycle Policy | A set of rules that defines how data is managed, stored, archived, and deleted. | Data Lifecycle Management Policy, Data Retention Policy | "The data lifecycle policy for this bucket will delete backup files that are over a month old." |

## Data Representation

Object storage stores and manages data as discrete units called objects. Each object is made up of: 
- the actual data (such as documents, backup images, videos, etc.)
- an ID or "key" that is unique within the bucket it lives in
- default or custom-defined metadata

The metadata is additional information about the object that makes it easier for us to search and retrieve it. For object storage, metadata can include attributes like the unique identifier, the object's name, size, creation date, and custom tags that we define.
- This is a key difference from block storage where very little metadata is allowed to ensure high performance speeds.

To start out with object storage, we need to create at least one bucket. A bucket is the highest level in object storage, and we generally need to choose a region or availability zone for the bucket to be deployed in at the time of creation. All of our individual objects are stored within a specific bucket of our choosing in object storage, and we can have many buckets if neccessary for organization. 

A crucial differences from file systems that we may be more familiar with, is that object storage systems use a flat namespace. The objects all sit at the same level in a bucket, there is no hierachical path to an object like in a file-based system. Instead, an object’s unique ID or key provides the address for the object within the storage system. 
- Generally, a hashing algorithm is used to generate an ID from the object's content. Similar to hash tables, this ensures that any objects with the same content will have the same identifier.

Object storage normally uses a distributed storage environment, with object data spread across multiple different storage nodes or servers. This, combined with the flat memory structure, means that object storage is much easier to scale horizontally than other types of storage. Data is already organized within a single, global storage pool distributed across multiple hardware devices and geographical locations, so we can easily add and manage new data. 

## Storage Classes

Storage classes are the various tiers of object storage that we can pay for. Cloud vendors organize object storage into classes that are designed for different data access patterns, typically divided into: 
- High-performance "hot data" classes are optimized for data that applications read frequently
- Lower-cost archival "cold data" classes are intended for data that may only be accessed occasionally or after long periods. 

Each vendor will have slightly different fees, but generally storage costs are influenced by factors such as: 
- the amount of data stored
- the number of retrieval requests
- the speed at which the data must be accessed 
- any transfer or access fees

As data becomes less actively used, it can be moved to cheaper storage classes to reduce overall expenses. This tiered approach allows organizations to store large volumes of data while controlling costs based on how the data is actually used.

### Hot Storage Classes

The exact names of tiers will vary by provider, but our hot storage tiers are often called "standard" storage. Hot storage tiers provide the fastest access times for object storage and typically have low to no data access fees, but the cost per gigabyte for the storage itself is higher than cold access tiers. Hot storage is where we want to keep information that is required for operations or otherwise used frequently, such as profile images for users on a social media site.

### Cold Storage Classes

Similar to our hot tiers, the names of the cold tiers are not consistent across providers, but the cold storage tiers often have terms like "cool", "cold", or "archive" in their names.

Cold storage typically has required minimums for how long data must be stored or extra fees are incurred. Generally speaking, the cheaper the storage is per GB, the longer data is required to be held in storage, and the higher fees are to access that data before the minimum storage period has elapsed. 

Cold storage also has longer data retrieval times than hot storage. The time for retrieval depends on the tier, with more expensive tiers returning data faster than long term archive storage, which can take hours to become available after request.

Cloud providers have several tiers of cold storage, based on access speeds and minimum required retention times. Commonly available cold storage tiers are:
- "cool" storage, requiring retention for 30 days. 
- "cold" storage, requiring retention for 90 days
- "archive" storage, requiring retention for 180-365 days

### Other Offerings

In addition to the hot and cold storage classes common among cloud providers, vendors may have specialized storage classes. We touch on 2 of the more commone specialized storage classes below.

**Variable/"Intelligent"/"Smart" Tiering**

Multiple vendors have a storage class that automatically moves data between one or more storage tiers based on the data's access patterns. These kinds of tiers can be useful for applications with unknown or changing access patterns, which are harder to write our own lifecycle policies for.

**Single Zone Storage**

Cloud vendors typically also support access tiers that are restricted to a single availability zone within a region. The data in these access tiers can be more fragile since we do not have built in redundancy across avilability zones, but the tradeoff is generally very high access speeds, which can be important to applications requireing very low latency.

## Lifecycle Policies

We mentioned earlier that as data becomes less actively used, we can move it to less expensive storage classes to reduce costs. Data lifecycle policies are the core mechanism we use to automate moving data between the storage tiers. If we know what the access patterns for our data look like, we can create rules to optimize where we hold our data and for how long, so that we make the best use of our resources. 

The exact syntax of a lifecycle policy will look different between cloud providers, but they are typically formatted as JSON. Lifecycle policies use resource IDs to define the bucket the policy is attached to and the resource or resources that are affected by the policy, and then contain rules for how the identified resources should be moved, updated, or deleted.

For example, imagine a healthcare organization that must keep patient records and backups for many years to meet legal and compliance requirements, even though older data is rarely accessed. Forms that are used every day along with recently used files and backups are kept in a “hot” storage tier so they can be accessed quickly if someone we've seen recently comes in or something goes wrong and data needs to be restored. As patient files or backups become older and less likely to be used, lifecycle policies that the organization defined automatically move them into cheaper “cold” storage tiers to reduce long-term costs.
- If the organization misunderstood how storage tiers work and kept everything in hot storage, their storage bills would quickly grow much larger than necessary. 
- If daily forms along with files and backups were immediately moved to cold storage, retrieving data for daily operations would take much longer and incur extra fees for accessing data before the minimum storage time had passed, and it could take longer to access necessary information and backups in the case of an audit or recovery from a disaster.

## Strengths & Limits

### Data Durability

With most cloud vendors, we can store data:
- in a single availability zone to minimize storage cost or latency
- in multiple availability zones within a single region for resilience against the permanent loss of an entire data center
- in multiple regions to meet geographic resilience requirements

Cloud vendors also support versioning for object storage. Object versioning  is a strategy where, whenever an object is updated, instead of deleting the older version of the file, the older version is also retained in the bucket. This enables us to see the history of our changes to an object and to quickly roll back to a prior version of an object if a change in an update causes a breakage. 

Versioning does come with a cost though, each copy of a file we keep takes up storage space that we are paying for. If we want to use versioning even with extra storage costs, lifecycle policies make this much more feasible! Depending on the nature of the file, we could create rules to only keep a certain number of past versions, or to schedule older versions to be moved to colder storage tiers after a certain amount of time. 

Cloud vendors also support security features like bucket policies and access control lists allowing us to create rules for how individuals or organizations are allowed to interact with a bucket or object.

### Scalability

Object storage systems prioritize quantity of storage over availability. There’s almost no limits on horizontal scalability; object storage can scale to petabytes and billions of objects. However, compared to block or file storage, they typically have higher access latency and lower throughput.
- Because write operations are slower, object storage is generally not well suited for transactional workloads that require fast, frequent updates.

### Versatility & Searchability

We can store pretty much anything in object storage, it's incredibly versatile, being able to hold compute images, videos, hard drive backups and more. Larger companies are more frequently relying on analyzing large volumes of data for predictional analytics and infrastructure decisions,machine learning, and custom AI applications, and object storage is well suited for these applications by making it easy to store and manage large volumes of unstructured data.

Object storage provides more advanced search capabilities, allowing us to search for an object based on its metadata, contents, and other attributes. As we mentioned earlier, object storage metadata can hold large amounts of information about an object including its name, content type, creation date, size, and other custom fields that can help you locate data.

## Costs

Object storage is consumption-based, affordable storage on-demand. We only pay for the storage we use and it can scale as needed, helping to reduce costs to store all our data.

Due to minimum storage time for cold tiers and data access fees, we need to be aware of our data's access patterns. Our role as developers is to choose initial storage tiers and create lifecycle policies that support our application's needs for data retrieval while minimizing uneccessary costs.

### Accessing Data

There are no folders or directions in object storage, making it easy to retrieve data by ID, as you don’t need to have the exact location. However, unlike file based systems, accessing data in object cloud storage with existing applications requires new code, the use of APIs, and direct knowledge of naming semantics. Additionally, we typically can’t modify an object after it’s created; we usually have to recreate the object and upload it if we need to make a change. 

## When to Choose Object Storage

Object storage is best used for large amounts of unstructured data. This is especially true when durability, unlimited storage, scalability, and complex metadata management are relevant factors for overall performance.

This flat memory model makes it ideal for handling large volumes of unstructured data, such as social media content, videos, or sensor data, which is often difficult to store in a hierarchical way. 

Key reasons why it is ideal for unstructured data include:

Massive Scalability: It scales horizontally to exabytes by adding new nodes, removing the limitations of traditional file or block storage.
Rich Metadata & Searchability: Each object includes, or can be tagged with, custom metadata, making it easy to categorize, index, and find data without a separate database.
Flat Address Space: Because there are no folders or complex hierarchies, data can be stored, retrieved, and managed more easily, regardless of volume.
Cost-Effectiveness: It often runs on standard, commodity hardware, providing a lower-cost option for storing vast amounts of data.
High Durability & Resiliency: Data is automatically replicated across multiple devices and geographic locations, ensuring high availability and protection against data loss.
Simple Data Access: Data is accessed via standard HTTP-based APIs (RESTful APIs), making it easy to integrate with web, mobile, and cloud applications

### Use Cases

Common object storage use cases include cloud-native applications, Internet of Things (IoT), big data, rich media storage and delivery, and backups and archiving.

## Summary

A key takeaway for storage classes is that as the cost per GB of storage lowers, the cost to access data and time to retrieve data rises. 

## Check for Understanding

out of the following list, select all options that are use cases for archival storage

Given a description of needs, choose the lifecycle policy that is best fit