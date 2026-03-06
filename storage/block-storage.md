# Block Storage

## Learning Goals

- Describe at a high level how block storage works.
- Describe considerations to keep in mind when choosing block storage.
- Describe common use cases for block storage.
- Describe tradeoffs for block storage regarding Solid State Drives (SSD) and Hard Disk Drives (HDD).

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Solid State Drive | A flash memory based hard drive. | SSD | We needed the performance speed that a solid state drive can provide. |
| Hard Disk Drive | A magnetic spinning disk based hard drive. | HDD | This feature uses hard disk drives for storage since we have a lot of data and it doesn't need the highest access speeds. |
| Throughput | A measurement of bits or bytes per second that can be processed by a storage device. | | Streaming videos requires high throughput because they are large, sequential data transfers. |
| IOPS | The number of read/write operations per second that a storage device can perform. | | Databases require high IOPS to quickly handle many read/write transactions. |

## Data Representation

Block storage maintains data in fixed-size blocks. Blocks can range in size, typically from a few kilobytes to several megabytes. Many cloud providers allow us to predetermine the block size during the configuration process. In cloud environments, block storage volumes can be attached, detached, resized, and snapshotted independently of a compute instance, allowing applications to scale while keeping their data durable and consistent.

Unlike the traditional file systems we're used to on most personal machines, block storage does not rely on a single path to the data. Cloud block storage services work by giving each block a unique address or block number, which is held inside a data lookup table. Cloud storage systems place data blocks wherever is most efficient, meaning that blocks can be spread across different data centers, systems, and environments. When we request data from a cloud block storage service, the relevant blocks of data are pulled together from wherever they are stored, and presented back to us. 

Block storage can provide efficient access when we have large volumes of data because it allows direct access to individual data blocks. We can read or write data to specific blocks without having to retrieve or modify the entire dataset. 

## Considerations & Use Cases

Now that we know a little about how block storage works, let's talk about its strengths and limitations, and when we would use cloud block storage.

### Strengths & Limits

**Fast Performance**: Block storage provides multiple paths to data and uses limited metadata, this reduces the amount of data transferred and allows for efficient data retrieval. 

**Modifying Data**: It's easy to modify just the piece of data or file we need. We only need to modify the specific block or blocks affected when changing a file in block storage.

**Scaling**: Block storage is easy to scale upward, but may eventually reach I/O limits or reach the maximum volume sizes available. Block storage offers scalability by adding more storage volumes or expanding existing volumes. We can add new blocks as needed as capacity needs grow without impacting performance, but scalability depends on the cloud provider's block storage system's ability to handle increased I/O demands and capacity requirements.

**Cost**: Block storage tends to be more expensive, especially since we are billed for our total provisioned capacity, even if we don’t end up using the full storage. 

**Metadata**: Block storage allows very limited use of metadata, we can only include basic file attributes as part of their architecture to maintain high efficiency. This limited metadata structure means block storage does not natively support rich search, tagging, or flexible organization of unstructured data, making it less ideal for massive archives or content-heavy systems where cost and metadata flexibility matter more than raw performance.

### When to choose block storage 

Block storage is a good fit for application-critical workloads that require low latency and frequent changes. Any service that requires fast access to data can work well with block storage. For example, a financial services company running a high-transaction trading database in the cloud would benefit from the low latency and high IOPS characteristics of block storage. When performance and control outweigh cost and metadata flexibility concerns, block storage is the appropriate choice.

Other common use cases include: 
- storage for databases, containers, or transactional workloads
- media rendering
- scale-out analytics
- caching
- backend storage for virtual machines

## Comparing Block Storage Options

We have some options to choose from within cloud block storage, but before we can compare those, we need a shared understanding of the metrics we use to compare the hard drives that back block storage technology.  

### Throughput & IOPS

For hard drives, IOPS measures how many small read/write actions can happen per second. Throughput measures how much total data can be transferred per second. In an ideal world, we'd have both high IOPS and throughput, but restrictions on the physical architecture and costs of hard drives means we often need to make tradeoffs to balance our storage costs with the requirements of a product.

One way to visualize IOPS vs throughput is to think about a coffee shop. IOPS is how many individual drink orders the shop can complete per minute, especially when each order is small and different (i.e. one latte, one espresso, one tea). Throughput is the total volume of liquid served per minute, regardless of how many separate orders that represents. 
- A machine that can quickly switch between drink types and finish many small orders has high IOPS. 
- A setup that can continuously pour large batches of brewed coffee into containers has high throughput. 

In hard drives, high IOPS matters when handling lots of small, random requests, like database queries. High throughput matters when transferring large files in steady streams, like video files.

### Comparing SSDs vs HDDs

When it comes to hard drives and storage, whether we're working in the cloud or building our own PC, we generally have two types of hard drive to choose from: Solid State Drives (SSDs) or Hard Disk Drives (HDDs). Cloud providers will typically give us the choice between SSD backed block storage and HDD backed block storage. Let's look at some key differences between them and talk about when we would choose one over the other.

Solid State Drives (SSDs) and Hard Disk Drives (HDDs) differ primarily in how they store and retrieve data, which directly affects throughput and IOPS (input/output operations per second). SSDs use flash memory with no moving parts, allowing them to deliver significantly higher IOPS and lower latency, making them ideal for workloads with frequent, small, random read/write operations. 

HDDs rely on spinning magnetic platters and mechanical read/write heads, which limits their IOPS and increases latency due to physical movement. In terms of throughput, HDDs can still perform well for large, sequential data transfers, especially in streaming workloads, but SSDs generally outperform them across most performance metrics. The performance gap becomes especially noticeable in transactional systems, where rapid response time and consistent latency are critical.

### Choosing between SSD & HDD

Cost and minimum requirements for an application's IOPS & throughput are the main influencers in the decision between SSDs and HDDs. 

HDDs typically offer a lower cost per gigabyte, making them attractive for storing large volumes of data where performance is not the primary concern. SSDs, while more expensive per gigabyte, often provide more predictable performance over time because they lack mechanical components. However, SSDs do have write endurance limits due to the finite number of write cycles in flash memory, though enterprise-grade SSDs are engineered to handle substantial workloads. 
- In cloud environments, these tradeoffs are abstracted into storage tiers, where SSD-backed volumes are priced higher but optimized for high IOPS, while HDD-backed volumes are more cost-effective for throughput-oriented or infrequently accessed data.

For enterprise applications, SSDs are typically chosen for databases, real-time analytics platforms, high-transaction systems, and any workload requiring consistent low latency and high I/O performance. HDDs are often selected for archival storage, backups, large-scale data lakes, or workloads that primarily involve sequential reads and writes rather than random access. 

In the cloud, this distinction appears in different volume types: 
- SSD-backed options are marketed for transactional or latency-sensitive systems 
- HDD-backed options target big data processing or bulk storage needs. 

Organizations frequently combine both, using SSDs for “hot” data that must be accessed quickly and HDDs for “cold” data that is stored cheaply over long periods. The decision ultimately balances performance requirements, budget constraints, and expected access patterns within the broader architecture.

We should use a solid state drive (SSD) when we: 
- need high IOPS
- deal with frequent read/writes 
- do not need high throughput

A hard disk drive (HDD) is a better choice if we are:
- dealing with data backups or data archives
- throughput-intensive workloads 
- storing high-volume data with infrequent access

Different providers will likely have different types of SSD/HDD available within these categories. The choice between hard drive subcategories will be made around which option best fits an application’s particular needs for IOPS, throughput, and cost. 

## Summary

Block storage is a cloud storage model that organizes data into fixed-size blocks, allowing direct access to individual pieces of data without retrieving an entire file. This design enables high performance, low latency, and efficient modification of specific data blocks, making it well suited for transactional and application-critical workloads such as databases, virtual machines, containers, and caching systems. 

Key performance metrics for evaluating block storage include IOPS (input/output operations per second), which measures how many small read/write actions can occur per second, and throughput, which measures how much total data can be transferred per second; different workloads prioritize one over the other. 

Cloud providers typically offer block storage backed by either solid state drives (SSDs), which use flash memory and deliver higher IOPS and lower latency, or hard disk drives (HDDs), which use spinning magnetic disks and provide lower cost per gigabyte but reduced performance for random operations. Choosing between SSD and HDD options ultimately depends on balancing cost, required IOPS, throughput needs, and expected data access patterns within a system’s architecture.

While block storage offers fast performance, easy data modification, and flexible scaling through volume expansion, it can be more expensive because billing is based on provisioned capacity. 

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: ffdcfe66-7cc8-4a64-bf83-35d2a2d32fc6
* title: Block Storage
##### !question

Imagine that we are building a robust search service that prioritizes effectively finding matching items over instant response times. 

Please select whether the following statement is True or False: 

"Based on the strengths and limits of block storage, block storage is the best choice for this feature."

##### !end-question
##### !options

* True
* False

##### !end-options
##### !answer

* False

##### !end-answer
##### !explanation

Block storage prioritizes fast access, and does not allow for flexible or very descriptive metadata that enables features like rich searches.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 5dc2f923-abc4-48e8-aa9d-7f17e5d49256
* title: Block Storage
##### !question

Imagine that we are building service that needs to stream large amounts of analytics data to process over time so we can better understand our customer's patterns and make data driven business decisions. 

Based on the strengths and limits of SSDs and HDDs, please choose which would be the best fit for this scenario.

##### !end-question
##### !options

* SSD
* HDD

##### !end-options
##### !answer

* HDD

##### !end-answer
##### !explanation

The key points in the prompt are: 
- we're streaming large amounts of data  
- the data will be processed over time, not instantly
- the service is for ongoing business decisions, not customer facing or mission-critical application systems

All of these lend themselves to lower cost HDDs to back our block storage since the fast performance of SSDs is not critical to this task.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->