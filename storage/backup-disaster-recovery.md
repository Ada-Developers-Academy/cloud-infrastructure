# Backup & Disaster Recovery

## Learning Goals

- Describe patterns and types of tools available to support backing up data and disaster recovery in cloud environments.

## Key Terms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Snapshot | In the context of storage, a snapshot is a read only copy of a storage volume that captures its exact state and data at that moment. | Snapshot Image, Backup Image, Point-in-time Copy | "We create a snapshot of our block storage at midnight each day so that we can restore our data to that point in time in case of disaster." |
| Replica | In the context of storage, a replica is a continuously synchronized duplicate of a storage volume maintained in a separate location. | Clone, Copy | "The application requires no down time, so we keep a replica in another region which allows us to immediately failover if a whole region experiences an outage." |
| Service Level Agreement (SLA) | A formal contract between a service provider and a customer that defines the expected level of service, including guarantees around uptime, performance, and support response times. | -------- | "The team chose a premium storage tier to meet the 99.9% uptime guarantee specified in their SLA." |
| Recovery Time Objective (RTO) | Defines the maximum acceptable downtime to restore systems after a failure. |  | "After a storage outage took down their e-commerce platform, the team was able to meet their two-hour RTO by restoring the application from snapshots and bringing the site back online before the deadline." |
| Recovery Point Objective (RPO) | Defines the maximum acceptable amount of data loss after a failure, measured in time (e.g., 4 hours of data lost) | -------- | "Because the company had set a one-hour RPO, they configured automated database backups to run every hour so that no more than sixty minutes of transaction data could ever be lost in a disaster." |

## What is disaster recovery

Backup and disaster recovery are strategies organizations use to protect data and restore operations in the event of data loss, corruption, or system failure. We're familiar with tools like GitHub that let us back up our programming project data somewhere remote in case our machine crashes or otherwise experiences disaster. When planning to launch an application or service, part of that strategy should include considering the data and resource needs, and creating a plan for how to backup and restore those required resources in case of hardware failure, service outages, or cyberattacks. Most organizations do this through documented backup and disaster recovery plans. 

Backups are fundamental to being able to create a disaster recovery plan. A backup is a copy of data taken at a specific point in time and stored separately from the original, allowing an organization to restore data to a known good state if something goes wrong. Disaster recovery plans go a step further than backups, by defining the full plan and set of resources needed to restore an entire system or application to working order after a catastrophic event.

A key part of defining a backup and disaster recovery plan is understanding what the Service Level Agreement (SLA) is for the application or service. Many applications have gurantees to users about certain features or uptime that must be maintained to avoid penalties or damage to product operations or the brand's reputation. The most common metrics that help define our disaster recovery plans are our Recovery Time Objective (RTO) and Recovery Point Objective (RPO), since these define the maximum acceptable downtime for a service, and the maximum acceptable amount of data lost. 

## Common Disaster Recovery Patterns

Since our RTO and RPO define the acceptable time windows for service and data restoration, the shorter our RTO or RPO is, the more resources we need to invest in to be able to failover and mantain our service agreements in case of emergency. These can be divided up further, but there are 3 common patterns for disaster recovery:
- Cold recovery, with typical RTO/RPO times of hours
- Warm recovery, with typical RTO/RPO times of minutes
- Hot recovery, with typical RTO/RPO times of seconds to real time 

We'll dive more into the kinds of resources we need to consider and have on standby for each of these patterns below!

### Cold Disaster Recovery

### Warm Disaster Recovery

### Hot Disaster Recovery


Next, we'll talk about the tools cloud vendors provide to enable backups and disaster recovery plans! 

## Backup & Recovery Tools

### Backups & Snapshots

easy to schedule & set lifecycle policies for
each resource needs to be individually managed, including compute resources and environment confugurations
- includes both backing up the data and restoring from backups during a disaster

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

As with many cloud provider offerings, deciding how to manage a backup and disaster recovery plan depends on: 
- what are the application's needs for restoration (does down time need stay under seconds, or is it tolerant of minutes or hours?)
- how much we are able or willing to manage ourselves 
- how much we can spend on a solution (our budget)

All of the automation that a vendor-managed recovery service provides can be done manually, but that requires having folks with the knowledge and time to schedule backups, create lifecycle policies, scripts, and workflows, then test the backup process and maintain those automations with updates as neccessary. 
- A mature company with a lot of infrastructure resources may find it's more cost effective to create and automate their disaster recovery plan on their own. 
- For many startups or companies low on human resources for infrastructure, a fully managed disaster recovery tool can be cost effective.

## Summary

Creating snapshots as backups (generally across regions and accounts), that can be restored vs keeping an up to date replica that can be quickly promoted in case of disaster in one or more regions
- May have individual tools and/or a centralized service for backing up across cloud offerings
- snapshots: data loss may happen in disaster if last snapshot scheduled was not right before disaster.
- replicas: very little data loss, but more cost overhead of data use 
Certain applications may be tolerant of some data loss (losing some likes on a post) while others are mission critical (records of orders yet to be fulfilled)

## Check for Understanding

Given a set of needs, would snapshotting be acceptable or is a stronger replication strategy required?

Given "x" feature, which resources would we need to include in a backup plan to ensure we don't lose significant time or data in a disaster event?