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

Backup and disaster recovery are strategies organizations use to protect data and restore operations in the event of data loss, corruption, or system failure. We're familiar with tools like GitHub that let us back up our programming project data somewhere remote in case our machine crashes or otherwise experiences disaster. When planning to launch an application or service, part of that strategy should include considering the data and infrastructure resource needs, and creating a plan for how to backup and restore those required resources in case of hardware failure, service outages, or cyberattacks. Most organizations do this through documented backup and disaster recovery plans. 

Backups are fundamental to being able to create a disaster recovery plan. A backup is a copy of data taken at a specific point in time and stored separately from the original, allowing an organization to restore data to a known good state if something goes wrong. Disaster recovery plans go a step further than backups, by defining the set of all resources neccessary to restore an entire system or application to working order after a catastrophic event as well as a full plan for when those backups will be created and how the services will be restored.

A key part of defining a backup and disaster recovery plan is understanding what the Service Level Agreement (SLA) is for the application or service. Many applications have gurantees to users about certain features or uptime that must be maintained to avoid penalties or damage to product operations or the brand's reputation. The most common metrics that help define our disaster recovery plans are our Recovery Time Objective (RTO) and Recovery Point Objective (RPO), since these define the maximum acceptable downtime for a service, and the maximum acceptable amount of data lost. 

## Common Disaster Recovery Patterns

Since our RTO and RPO define the acceptable time windows for service and data restoration, the shorter our RTO or RPO is, the more resources we need to invest in to be able to failover and mantain our service agreements in case of emergency. These can be divided up further, but there are 3 common patterns for disaster recovery:
- Cold recovery, with typical RTO/RPO times of hours
- Warm recovery, with typical RTO/RPO times of minutes
- Hot recovery, with typical RTO/RPO times of seconds to real time 

We'll dive more into the kinds of resources we need to consider and have on standby for each of these patterns below!

### Cold Disaster Recovery

A cold recovery pattern is the most basic and least expensive approach, where backup infrastructure exists but is not running until a disaster occurs. When a failure happens, the team must manually provision and configure resources from scratch using stored snapshots, backups, and infrastructure templates before the application can be restored. 

Key features include:
- Lowest cost, as no standby infrastructure is running or being paid for
- Can require the most manual intervention to restore services
- Longest recovery time, often measured in hours or days
- Best suited for non-critical applications where extended downtime is acceptable

### Warm Disaster Recovery

A warm recovery pattern maintains a partially running standby environment that is kept in sync with the primary environment but is not actively serving traffic. When a disaster occurs, the standby environment is scaled up and traffic is redirected to it, reducing the time needed to restore service compared to a cold approach. 

Key features include:
- Moderate cost, as some standby infrastructure is running at reduced capacity
- Requires some manual intervention or automation to scale up and redirect traffic
- Moderate recovery time, typically measured in minutes to hours
- Best suited for applications where some downtime is tolerable but extended outages are unacceptable

### Hot Disaster Recovery

A hot recovery pattern maintains a fully running duplicate environment that mirrors the primary environment in real time and can immediately take over traffic if the primary environment fails. This approach is often referred to as an active-active or active-passive configuration depending on whether both environments are serving traffic simultaneously. 

Key features include:
- Highest cost, as a full duplicate environment is running at all times
- Minimal to no manual intervention required, as failover is typically automated
- Near-zero recovery time, as failover can happen automatically in seconds
- Best suited for mission-critical applications where any downtime results in significant financial or reputational harm

Next, we'll talk about the tools cloud vendors provide to enable backups and disaster recovery plans! 

## Backup & Recovery Tools

There are two core parts to disaster recovery plans:
1. Backing up our data and infrastructure
2. Restoring our data and services

We won't cover restoring data and services in depth in this curriculum, but it's important to know that there are tools that help automate the backup and restoration process.

### Backups & Snapshots

Most cloud resources allow you to create a backup, or snapshot of their data at a specific point in time. This is true of compute images as well as storage volumes. Backups are great since they are easy to schedule and if we keep them in object storage, it's also easy to set lifecycle and/or versioning policies to control what access tier they live in and how long we keep them to help control costs.  

When we are not using managed tools for disaster recovery, each resource for an application needs to be individually backed up and managed, including data, compute resources, and environment configurations. As part of this, we need to schedule our data backups frequently enough to ensure we do not lose more data than our agreed RPO allows. If we want to enable warm or hot recovery, we also need to create our own automations for failing over to an suspended or active environment, relaunching services and restoring data. Otherwise we must manually set everything back up ourselves, which typically takes time, aligning with cold recovery patterns.

To help ease the pain point of disaster recovery management, the major cloud vendors like AWS, Google, and Azure also have other layers of disaster recovery tools. Many cloud vendors have a centralized "backup service" that allows us to identify our resources across the vendor's services to be backed up, and set policies to manage when those resources are snapshotted, where they are stored, and how long they are kept. These tools are important because they automate the backup process, not just within a single availability zone or region, but cross region as well, helping ensure data and application resources do not get lost. 
- Something important to note, is that a centralized backup service only handles creating and managing backups, it does not typically cover service restoration, or keeping resources or applications in other regions idle and ready for failover. This means we would still need to do some of our own automation to enable warm or hot disaster recovery patterns.

### Disaster Recovery Services 

Moving beyond backup images and backup services are fully managed disaster recovery services. These services are built ontop of the tools we have already talked about like volume snapshots, cross-region replication, and object versioning, which allows teams to recover individual files, entire storage volumes, or whole application environments depending on the severity of an incident. 

Fully managed cloud disaster recovery services simplify the process of protecting applications by automating the continuous replication of servers, storage volumes, and application data without requiring teams to manually build and maintain standby infrastructure. These services typically run lightweight agents on source servers that continuously stream data changes to the cloud, keeping a up to date copy of the environment ready to be launched at a moment's notice. 

When a disaster occurs, teams can trigger a failover that spins up fully functional replicas of their servers and storage in the cloud within minutes, dramatically reducing recovery time compared to traditional backup and restore approaches. Most managed disaster recovery services include testing tools that allow teams to run non-disruptive failover drills, verifying that recovery procedures work as expected without impacting the live production environment. 

By abstracting away the complexity of replication, failover orchestration, and infrastructure provisioning, these services allow organizations to achieve aggressive RTO and RPO targets without requiring deep cloud expertise or dedicating significant engineering resources to maintaining a disaster recovery program.

### On-Premise to Cloud 

For organizations with hybrid architectures where some resources may live in self-hosted data centers, most cloud providers have storage transfer services. These storage transfer services are made exactly for moving large volumes of data from an outside system into or out of a cloud provider's storage ecosystem. This allows organizations to continue taking advantage of the resources they already own, while making their system more resilient. By backing up resources to the cloud, even if their own data center goes down completely, operations may be restored through data backed up in the cloud.

There is also a use case for processing data: imagine that there is a company that has a lot of storage in their own data center, but limited compute power. These storage transfer tools enable teams to move data to the cloud for analysis and processing on cloud compute resources, then bring back the processed information or insights to their own servers for storage or further operations.

Within a storage transfer service, there are typically a few offerings aimed at moving different kinds of storage. A single storage transfer service is likely to have options specific to block storage, file storage, and even in-person transfer (where large storage vloumes are delivered to the customer to fill, then physically sent back to the cloud vendor for upload) for massive amounts of data or sensitive data that would be costly or otherwise not allowed to be sent across networks. 

### Solution Tradeoffs

As with many cloud provider offerings, deciding how to manage a backup and disaster recovery plan depends on: 
- what the application's needs for restoration are (our RTO/RPO)
- how much we are able or willing to manage ourselves 
- how much we can spend on a solution (our budget)

All of the automation that a vendor-managed recovery service provides can be done manually, but that requires having folks with the knowledge and time to schedule backups, create lifecycle policies, scripts, and workflows, then test the backup process and maintain those automations with updates as neccessary. 
- A mature company with a lot of infrastructure resources may find it's more cost effective to create and automate their disaster recovery plan on their own. 
- For many startups or companies low on human resources for infrastructure, a fully managed disaster recovery tool can be cost effective.

## Summary

Backup and disaster recovery planning is a critical part of launching and maintaining any cloud application or service. A backup is a point-in-time copy of data stored separately from the original, while a full disaster recovery plan defines all the resources and procedures needed to restore an entire system after a catastrophic event. Two key metrics shape these plans: the Recovery Time Objective (RTO), which defines the maximum acceptable downtime, and the Recovery Point Objective (RPO), which defines the maximum acceptable amount of data loss. Together, these metrics determine how much an organization needs to invest in its recovery infrastructure.

The core tension between cold, warm, and hot disaster recovery patterns is cost versus recovery speed. 
- Cold recovery minimizes infrastructure costs but accepts the risk of long downtime, making it inappropriate for applications with aggressive RTOs or RPOs. 
- Warm recovery strikes a middle ground, keeping costs manageable while significantly reducing recovery time compared to cold, but still requires some lead time to fully restore service. 
- Hot recovery eliminates nearly all downtime and meets the most demanding RTOs and RPOs, but requires the organization to continuously pay for and maintain a full duplicate environment regardless of whether it is ever needed. 

Organizations must weigh the cost of each recovery pattern against the potential cost of downtime for a given application, as the right choice depends heavily on how critical the application is and how much data loss or service interruption the business can tolerate.

Cloud providers offer a range of tools to support backup and disaster recovery at different levels of complexity and automation. 
- Basic snapshots and scheduled backups allow teams to manually manage point-in-time copies of their data and infrastructure resources. 
- Centralized backup services automate the scheduling and cross-region storage of those snapshots across multiple cloud resources, though they typically stop short of automating service restoration. 
- Fully managed disaster recovery services go the furthest by continuously replicating environments and enabling automated failover, allowing organizations to meet aggressive RTO and RPO targets without deep infrastructure expertise. 
- For organizations with on-premises infrastructure, cloud storage transfer services provide a path to back up self-hosted data to the cloud, adding resilience even for hybrid environments.

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 0a86ca50-3ee1-4db4-8b16-4c19ea5e9879
* title: Backup & Disaster Recovery
##### !question

Which of the following options best describes the difference between a snapshot and a replica?

##### !end-question
##### !options

* A snapshot is stored in block storage while a replica is stored in object storage
* A snapshot is a point-in-time copy of a volume taken at a specific moment, while a replica is a continuously synchronized duplicate maintained in a separate location
* A snapshot supports automated failover while a replica must be manually restored
* A snapshot is used for warm recovery while a replica is used for cold recovery

##### !end-options
##### !answer

* A snapshot is a point-in-time copy of a volume taken at a specific moment, while a replica is a continuously synchronized duplicate maintained in a separate location

##### !end-answer
##### !explanation

A snapshot captures the state of a volume at a specific moment and does not update automatically, while a replica continuously synchronizes with the source to stay current.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: f0952981-6656-414b-9e63-deb45a8fb423
* title: Backup & Disaster Recovery
##### !question

A startup runs a non-critical internal reporting tool that is only used during business hours. The team is small and has a limited infrastructure budget. Losing up to 24 hours of report data in a disaster event is acceptable. 

Select which disaster recovery approach best fits this scenario.

##### !end-question
##### !options

* Hot recovery with continuous replication across two regions
* Warm recovery with a partially running standby environment in a separate availability zone
* Cold recovery using scheduled daily snapshots stored in object storage

##### !end-options
##### !answer

* Cold recovery using scheduled daily snapshots stored in object storage

##### !end-answer
##### !explanation

A non-critical tool with an acceptable 24-hour RPO and limited budget is well suited to cold recovery using inexpensive scheduled snapshots, with no need for standby infrastructure.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: b716d170-266f-4277-916e-fb20c5d06fb5
* title: Backup & Disaster Recovery
##### !question

A company uses a centralized cloud backup service to manage snapshots of their databases and compute instances across multiple regions. During a disaster, they discover the service did not automatically restore their applications in the secondary region. 

Select the option below that is the most likely explanation.

##### !end-question
##### !options

* The centralized backup service failed to replicate data across regions
* Centralized backup services manage the creation and storage of backups but do not typically automate service restoration or failover
* The company should have used cold recovery instead of relying on a backup service
* Snapshots cannot be used to restore compute instances, only storage volumes

##### !end-options
##### !answer

* Centralized backup services manage the creation and storage of backups but do not typically automate service restoration or failover

##### !end-answer
##### !explanation

Centralized backup services automate snapshot creation and cross-region storage, but restoring services and orchestrating failover typically requires additional automation or a fully managed disaster recovery service.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->