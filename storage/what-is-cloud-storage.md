# Cloud Storage

## Learning Goals

- Define cloud storage and key benefits.
- Define block, file, and object storage.

## What is cloud storage?

Cloud storage is a term for the services and hardware that allow software systems in the cloud to access disk space and manage data so it stays safe, available, and affordable over time. These cloud storage offerings provide applications and infrastructure with secure, durable, storage without requiring an up-front investment to build and maintain physical data centers.

Storage decisions affect reliability, speed, security, and long-term cost, so we need to choose the right storage patterns for an application's specific infrastructure and data needs to create performant, cost-effective applications. In production environments, engineers encounter these tools and concepts when performing work like:
- attaching storage to compute instances
- sharing files across teams
- storing user-generated content
- creating backups

Understanding storage tradeoffs enables engineers to design scalable, resilient, and cost-efficient systems that grow with the product.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Persistent Storage | Data storage that retains information after a device loses power, restarts, or shuts down | Non-volatile memory | We need persistent storage to host our databases so that we don't lose customer information. |
| Ephemeral Storage | Temporary, high-speed, local storage directly attached to a machine or virtual machine. | Volatile memory | We can use the ephemeral storage to cache data that invalidates quickly. |
| Metadata | Data that defines, describes, or explains other data. For example, in a relational database, metadata includes all the information that defines the database's schema. |  | Some data storage types allow flexible formatting or complex metadata, while others keep minimal or very rigid formats of metadata. |

## Types of Storage

We will talk about 3 common types of storage through this topic: 
- block storage
- file storage 
- object storage

All three approaches support replicating data across regions for reliability, error detection, encryption, network access, and scaling, but they each differ in performance, metadata flexibility, and how applications interact with them. These storage types are briefly introduced below, and we will dive deeper into them in the following lessons.

### Block Storage

Cloud block storage is a type of persistent storage that provides raw storage volumes to virtual machines. It works by presenting storage as independent volumes that behave like local disks to an operating system, even though the underlying infrastructure is distributed across data centers. Cloud block storage separates compute from storage while preserving the familiar abstraction of a disk drive. Ideal for workloads that need low-latency, high-performance disk access, such as relational databases and applications requiring consistent performance.

### File Storage

Cloud file storage provides a shared file system in the cloud. Its primary advantage is ease of use rather than ultra-low latency or extreme scalability. Cloud file systems organize data using familiar folders and file paths, making it intuitive for teams and applications that already rely on traditional file system structures. File storage is designed for shared access and collaboration, which makes it well suited for environments where many systems need concurrent access to the same set of files. Common use cases are: shared repositories, analytics workloads, web serving, and collaborative development projects

### Object Storage

Object storage is designed to store data as discrete objects rather than as blocks on a disk or files in a hierarchical directory. Each object contains the data itself, a unique identifier, and flexible metadata that can include custom attributes to help organize and retrieve it later. Object storage prioritizes massive scalability and durability across distributed systems over low latency performance. These features make object storage especially effective for storing large volumes of unstructured data such as images, videos, backups, and analytics datasets.

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: a123bbe6-4530-44a8-ad38-25d1a5c95117
* title: Cloud Storage
##### !question

We have an application where users can upload one or more profile images. 
Select the storage option below that is best fit for storing large amounts of images.

##### !end-question
##### !options

* Block
* File
* Object

##### !end-options
##### !answer

* Object

##### !end-answer
##### !explanation

Object storage is ideal for storing unstructured data like images that may need to scale quickly if we get an influx of users, but does not require super fast access.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 61de1b8d-ccd6-4e3c-87c0-324e6ac44fd9
* title: Cloud Storage
##### !question

We want to host a SQL database on a cloud compute instance. 
Select the storage type that best fits our needs from the options below.

##### !end-question
##### !options

* Block
* File
* Object

##### !end-options
##### !answer

* Block

##### !end-answer
##### !explanation

A SQL database requires fast disk access as well as space to hold the tables of data, so block storage is ideal in this case.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->
