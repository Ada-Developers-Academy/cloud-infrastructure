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

A company is choosing between SSD and HDD storage for two separate workloads: one is a high-traffic e-commerce checkout system, and the other is a long-term archive of financial records accessed once per quarter. Which of the following recommendations is correct?

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

Which of the following is not a typical use case for block storage?

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

A startup is building a platform where users upload profile photos, short videos, and documents. The platform expects to scale to millions of users. Uploaded files are read frequently but rarely modified after upload. Which of the following storage options best fits this scenario?

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

A company is migrating an on-premises application to the cloud. The application was built to read and write files using standard operating system file paths and expects a mounted drive it can browse like a local directory. The application has not been modified and the team does not have the resources to refactor it. Which of the following storage options is the best fit and why?

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

A media company uploads video files that are heavily streamed for the first 14 days after release. After 14 days, viewership drops sharply. After 90 days, videos are rarely accessed. The company's current lifecycle policy keeps all videos in standard storage permanently. Which updated policy would best optimize costs?

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

A company has data with unpredictable access patterns. Some files that have not been accessed in months are suddenly queried frequently, then go quiet again. Managing lifecycle policies manually has become difficult. Which storage class feature best addresses this?

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
* id: b8c9c0f2-e55f-43e2-aa5e-825a875e2e65
* title: Cloud Storage
##### !question

A company hosts a customer-facing web application on cloud compute instances. The application uses a PostgreSQL database hosted on block storage, stores user profile images in object storage, and runs on virtual machines with OS boot disks. In the event of a disaster, which backup plan includes all the resources needed to fully recover this feature?

##### !end-question
##### !options

* Back up only the database, as it contains the most critical business data
* Back up the database, the object storage bucket containing profile images, and the compute instance boot disk snapshots
* Back up the compute instance boot disks and the database, but exclude profile images since they can be re-uploaded by users
* Back up only the object storage bucket, as profile images are the largest data asset

##### !end-options
##### !answer

* Back up the database, the object storage bucket containing profile images, and the compute instance boot disk snapshots

##### !end-answer
##### !explanation

Full disaster recovery requires backing up all stateful resources: the database, the storage bucket, and boot disk snapshots to restore the OS and application configuration.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->
