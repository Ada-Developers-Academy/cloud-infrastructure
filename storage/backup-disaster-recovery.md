# Backup & Disaster Recovery

## Learning Goals

- Describe patterns and types of tools available to support backup and disaster recovery in cloud environments.

## Key Terms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Snapshot | In the context of storage, a snapshot is a read only copy of a storage volume that captures its exact state and data at that moment. | Snapshot Image, Backup Image, Point-in-time Copy | "We create a snapshot of our block storage at midnight each day so that we can restore our data to that point in time in case of disaster." |
| Replica | In the context of storage, a replica is a continuously synchronized duplicate of a storage volume maintained in a separate location. | Clone, Copy | "The application requires no down time, so we keep a replica in another region which allows us to immediately failover if a whole region experiences an outage." |

## What is disaster recovery

Backup and disaster recovery are strategies organizations use to protect data and restore operations in the event of data loss, corruption, or system failure. We're familiar with tools like GitHub that let us back up our programming project data somewhere remote in case our machine crashes or otherwise experiences disaster. When planning to launch an application or service, part of that strategy should include considering the data and resource needs, and creating a plan for how to backup and restore those required resources in case of hardware failure, service outages, or cyberattacks. Most organizations do this through documented backup and disaster recovery plans. 

A backup is a copy of data taken at a specific point in time and stored separately from the original, allowing an organization to restore data to a known good state if something goes wrong. Disaster recovery plans go a step further than backups, by defining the full plan and set of resources needed to restore an entire system or application to working order after a catastrophic event.

We'll dive further into these concepts and the tools cloud vendors provide to enable backups and disaster recovery plans below! 

## Common backup & recovery tools

### Backups & Snapshots

easy to schedule & set policies for, 

AWS Backup is like the recipe book. If something goes wrong, you can refer to the recipe book to recreate the pizza. Understand AWS Backup as a centralized service for automating and managing backups across AWS services. Recognize the importance of automated backup policies and their role in simplifying data protection. Know that data is secure with AWS Backup. And be aware of AWS Backup's capabilities in terms of cross‑region backup, supporting compliance and disaster recovery strategies. 

### Disaster Recovery Services 

continuously updates replica in one or more regions

Cloud providers support these strategies through tools like volume snapshots, cross-region replication, and object versioning, which together allow teams to recover individual files, entire storage volumes, or whole application environments depending on the severity of the incident. 
- A strong backup and disaster recovery plan identifies all stateful resources an application depends on, defines how frequently they are backed up, and establishes clear recovery time objectives so that teams know exactly what steps to take and how quickly they can restore service when disaster strikes.


Elastic Disaster Recovery is like the backup generator for our pizza kitchen. If there's a power outage, it ensures the pizza making continues. Elastic Disaster Recovery allows you to maintain business continuity without disruptions. 

### On-Premise to Cloud 

For organizations with hybrid architectures where some resources may live in self-hosted data centers, most cloud providers have storage transfer services. These storage transfer services are made exactly for moving large volumes of data from an outside system into a cloud provider's storage ecosystem. This allows organizations to continue taking advantage of the resources they already own, while making their system more resilient. By backing up resources to the cloud, even if their own data center goes down completely, operations may be restored through data backed up in the cloud.

There is also a use case for processing data: imagine that there is a company that has a lot of storage in their own data center, but limited compute power. These storage transfer tools enable teams to move data to the cloud for analysis and processing on cloud compute resources, then bring back the processed information or insights to their own servers for storage or further operations.

Within a cloud storage service, there are typically a few offerings aimed at moving different kinds of storage. A single storage transfer service is likely to have options specific to block storage, file storage, and even in-person transfer (where large storage vloumes are delivered to the customer to fill, then physically sent back to the cloud vendor for upload) for massive amounts or sensistive data that would be costly or otherwise not allowed to be sent across networks. 

### Solution Tradeoffs
network gateway for backing up from on-prem to cloud

Creating snapshots as backups (generally across regions and accounts), that can be restored vs keeping an up to date replica that can be quickly promoted in case of disaster in one or more regions
- May have individual tools and/or a centralized service for backing up across cloud offerings
- snapshots: data loss may happen in disaster if last snapshot scheduled was not right before disaster.
- replicas: very little data loss, but more cost overhead of data use 
Certain applications may be tolerant of some data loss (losing some likes on a post) while others are mission critical (records of orders yet to be fulfilled)

## Check for Understanding

Given a set of needs, would snapshotting be acceptable or is a stronger replication strategy required?

Given "x" feature, which resources would we need to include in a backup plan to ensure we don't lose significant time or data in a disaster event?