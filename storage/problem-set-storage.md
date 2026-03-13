# Problem Set: Cloud Storage

## Directions

Complete all questions below.

## Practice

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: c0954e5b-9d64-45d1-b0c8-f7c913342473
* title: Cloud Storage
##### !question

A company is choosing between SSD and HDD storage for two separate workloads: one is a high-traffic e-commerce checkout system, and the other is a long-term archive of financial records accessed once per quarter. 

Select the recommendation below that best fits the scenario for cost savings.

##### !end-question
##### !options

* Use SSD for both, as SSDs are always preferable regardless of workload
* Use HDD for the checkout system and SSD for the archive
* Use SSD for the checkout system and HDD for the archive
* Use HDD for both, as cost savings outweigh performance needs

##### !end-options
##### !answer

* Use SSD for the checkout system and HDD for the archive

##### !end-answer
##### !explanation

The checkout system needs the low latency that SSDs provides, while the archive is infrequently accessed so we could lower our archive costs by using HDDs.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 30b5a9e8-0ae6-4cd4-b27e-c2a2ef8b3566
* title: Cloud Storage
##### !question

Which of the following options is **not** a typical use case for block storage?

##### !end-question
##### !options

* Hosting the root volume of a virtual machine's operating system
* Storing a relational database that requires fast, consistent read/write access
* Providing persistent storage volumes for a containerized application
* Storing billions of user-uploaded images for a social media platform

##### !end-options
##### !answer

* Storing billions of user-uploaded images for a social media platform

##### !end-answer
##### !explanation

Storing large volumes of unstructured files like user images at scale is a use case for object storage.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: a2a055e3-c482-4727-98c1-12608fb3a9d9
* title: Cloud Storage
##### !question

A startup is building a platform where users upload profile photos, short videos, and documents. The platform expects to scale to millions of users. Uploaded files are read frequently but rarely modified after upload. 

Select the storage option that best fits this scenario.

##### !end-question
##### !options

* Block storage, because it offers low latency for frequent reads
* Local instance storage, because it provides the highest throughput
* Object storage, because it scales horizontally and handles large volumes of unstructured data
* File storage with a hierarchical directory structure to organize uploads by user

##### !end-options
##### !answer

* Object storage, because it scales horizontally and handles large volumes of unstructured data

##### !end-answer
##### !explanation

Object storage is designed for large volumes of unstructured data like media files, scales horizontally nearly without limits, and is well suited for content that is written once and read many times.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: c67c78ca-2342-40fd-b011-7970514fec4d
* title: Cloud Storage
##### !question

A company is migrating an on-premises application to the cloud. The application was built to read and write files using standard operating system file paths and expects a mounted drive it can browse like a local directory. The application has not been modified and the team does not have the resources to refactor it. 

Select the storage option and explanation that best fits the scenario.

##### !end-question
##### !options

* Object storage, because it is the most scalable option and can handle any file type
* Block storage attached directly to a single compute instance, because it provides the fastest possible read/write speeds
* Network file storage, because it can be mounted as a drive and accessed using standard file system conventions without requiring application changes
* Cold tier object storage, because migration workloads are typically infrequent and cost savings should be prioritized

##### !end-options
##### !answer

* Network file storage, because it can be mounted as a drive and accessed using standard file system conventions without requiring application changes

##### !end-answer
##### !explanation

Network file storage can be mounted and accessed like a local drive using familiar file path conventions. This makes it the right choice when lifting and shifting applications that were not built to interact with object storage APIs and cannot be easily refactored.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 2981697f-fff8-462a-888b-c400f2cad69c
* title: Cloud Storage
##### !question

A media company uploads video files that are heavily streamed for the first 14 days after release. After 14 days, viewership drops sharply. After 90 days, videos are rarely accessed. The company's current lifecycle policy keeps all videos in standard storage permanently. 

Select the updated policy that would best optimize costs in this scenario.

##### !end-question
##### !options

* Transition to cool storage after 14 days, then to archive storage after 90 days
* Transition to archive storage immediately after upload to maximize savings
* Delete all videos after 90 days to eliminate ongoing storage costs
* Keep all videos in standard storage but enable intelligent tiering on individual files

##### !end-options
##### !answer

* Transition to cool storage after 14 days, then to archive storage after 90 days

##### !end-answer
##### !explanation

Moving to cool storage after peak viewership ends and archive storage after long-term inactivity matches access patterns and minimizes cost without deleting content.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: b51673f4-4965-4099-8c2d-55ecbecaf310
* title: Cloud Storage
##### !question

A company has data with unpredictable access patterns. Some files that have not been accessed in months are suddenly queried frequently, then go quiet again. Managing lifecycle policies manually has become difficult. 

Select the storage class feature that would best addresses this scenario.

##### !end-question
##### !options

* Archive storage with a 365-day minimum retention period
* Single zone storage for reduced latency during unexpected access spikes
* Variable tiering (also known as "smart" or "intelligent" tiering), which automatically moves data between one or more tiers based on access patterns
* Standard hot storage for all data to ensure it is always immediately available

##### !end-options
##### !answer

* Variable tiering (also known as "smart" or "intelligent" tiering), which automatically moves data between one or more tiers based on access patterns

##### !end-answer
##### !explanation

Variable tiering automatically adjusts storage class based on observed access patterns, making it ideal for unpredictable workloads where manual lifecycle policies are hard to define.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: abe62157-fcc0-4d6d-91a1-3926bf5b7805
* title: Cloud Storage
##### !question

A company runs a web application with the following components: a virtual machine running the application server, a block storage volume hosting a PostgreSQL database, and an object storage bucket containing user-uploaded files. A disaster destroys the primary region entirely. 

Select the option below that represents a backup plan which ensures the application can be fully restored.

##### !end-question
##### !options

* Snapshot the block storage volume only, as the database contains the most critical data
* Back up the object storage bucket only, as user files are the hardest to recreate
* Snapshot the virtual machine boot disk and block storage volume, and replicate the object storage bucket to another region
* Enable intelligent tiering on the object storage bucket and take weekly snapshots of the database

##### !end-options
##### !answer

* Snapshot the virtual machine boot disk and block storage volume, and replicate the object storage bucket to another region

##### !end-answer
##### !explanation

Full restoration requires backups of all stateful components: the compute configuration (boot disk snapshot), the database volume (block storage snapshot), and the user files (object storage replication).

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: b8c9c0f2-e55f-43e2-aa5e-825a875e2e65
* title: Cloud Storage
##### !question

A growing company is evaluating whether to build and manage their own disaster recovery automation or pay for a fully managed disaster recovery service. 

Select the scenario below that would most favor choosing a fully managed service over a self-managed approach.

##### !end-question
##### !options

* The company has a large, experienced infrastructure team with existing automation tooling and well-documented disaster plans
* The company has aggressive RTO and RPO requirements but limited infrastructure engineering resources to build and maintain recovery workflows
* The company runs non-critical internal tools where several hours of downtime is acceptable
* The company wants to minimize all cloud vendor spending and has already invested in custom backup scripts

##### !end-options
##### !answer

* The company has aggressive RTO and RPO requirements but limited infrastructure engineering resources to build and maintain recovery workflows

##### !end-answer
##### !explanation

Fully managed disaster recovery services are most valuable for organizations that need to meet aggressive recovery targets but lack the engineering resources to build, test, and maintain the complex automation required to do so independently.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->