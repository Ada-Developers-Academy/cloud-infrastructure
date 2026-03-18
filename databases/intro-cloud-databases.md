# Intro to Cloud Databases

Let's start by taking a moment to think about a website you visit frequently. Outside of files like images, what other content is there? We're likely to see a variety of content: ads with text, products with reviews, posts with comments or likes. We can see that there is a lot of data we need to power applications that isn’t necessarily best stored as an object or file using the cloud storage options we've seen before.

To deploy enterprise applications, we need to create, connect to, and manage various kinds of databases. Throughout this topic we’ll dive into the differences in responsibility between fully managed vs self hosted databases, describe commonly available types of databases offered by cloud providers, and discuss their performance and scaling tradeoffs. Along the way we’ll discuss how we can keep data consistent, especially in a multi-region system, and talk about use cases for several kinds of databases.

## Learning Goals

- Describe what it means for a database to be fully managed vs self-hosted and be able to speak to tradeoffs of each.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Fully Managed Database | A cloud-based database solution provided by a cloud service provider, where the underlying infrastructure, including virtual machines, storage, and database software, is entirely managed by the service provider. | -------- | "We used a fully managed database solution during our start-up phase since we didn't have a lot of dedicated team members to manage security updates and other maintenance." |
| Self-Hosted Database | A database management system that is installed, configured, and maintained by an organization. A self-managed database may be hosted on-premises or on cloud infrastructure. | -------- | "The company chose a self-hosted database so they could enforce strict data sovereignty requirements and keep all records within their own data center." |

## What is a cloud database

During Unit 2 of the core curriculum we install PostgreSQL and run it on our local machines to develop against. This is an on-premises database! We installed a database on hardware under our control and we are responsible for all aspects of keeping it up and running. In contrast, a cloud database is a database that runs on cloud infrastructure and is accessed over the internet. This means they can be reached from anywhere with an internet connection. 

Before cloud infrastructure existed, companies had to purchase servers, allocate physical space, and employ staff to keep systems running. Scaling up a traditional database also required buying and configuring additional hardware, which is both costly and time-consuming. However, cloud databases can typically scale up or down on demand with just a few clicks or even automatically based on workload. 

When it comes to cloud databases, we have a lot of choice! The variety of offerings from cloud vendors enables us to self-host databases using cloud compute and storage resources we've seen eariler, or to tap into fully managed database services that handle the day to day maintenance for us. 
- Depending on what infrastructure our organization already has or our application's needs, we could even design a hybrid on-premises & cloud system! One example could be that we keep sensitive data in the datacenter that we already own to best use our existing resources, and we store less critical or sensitive data in a cloud database.

## Fully-Managed vs Self-Hosted

When we deploy databases in the cloud, we face a foundational choice: use a fully managed database service provided by the cloud vendor, or self-host a database on virtual machines we control. 
- A **fully managed database** means the cloud provider takes ownership of the underlying infrastructure (the virtual machines, storage, networking, and database software) and handles the operational heavy lifting on our behalf. 
- A **self-hosted database**, by contrast, means our team installs, configures, and maintains the database software on cloud compute resources, giving us full control but also full responsibility.

### Responsibility Tradeoffs

There are significant differences in day-to-day responsibilities between fully managed and self-hosted databases. In a managed service, the provider automates routine tasks such as daily backups, OS patching, and version upgrades, as well as backup and restoration tasks. Engineers using a managed service don't need to manually schedule backup jobs, create and test restore procedures, or worry about whether the latest security patch has been applied to the database engine. With a self-hosted database on a virtual machine, our team owns all of these tasks.

Similar to cloud storage services, high availability is baked into fully managed cloud database services. A managed database service typically offers built-in multi-availability zone (multi-AZ) deployment, and the ability to maintain a standby replica and initiate failover in case the primary instance goes down. A self-hosted setup requires our team to design, configure, and test that replication and failover logic manually. This includes setting up load balancing (which we'll talk more about when we cover networking), replication strategies, and automated recovery scripts.

Security responsibilities follow the same pattern: managed databases handle encryption at rest using vendor-provided key management systems and automatically apply security patches, whereas self-hosted teams must manually configure database-level encryption, manage firewall rules, and stay current on vulnerability disclosures.

An important note is that "fully managed" does not mean there is no engineering responsibility. The managed service handles the infrastructure layer, but the application and data layer remain firmly in our hands. We remain responsible for schema design, query optimization, and making scaling decisions. A schema that does not fit the data well on a managed database can still perform badly, and unoptimized queries can drive up costs.  

### Cost & Control Tradeoffs

Key deciding factors between self-hosting and fully managed services are often costs and our required level of control over the database.

We pay for the data we use with managed services, and the cost per GB of storage is higher for managed services than for raw cloud storage volumes. However, when we self-host a database on cloud infrastructure, we pay for the full amount of whichever cloud storage volume tier we are using, whether or not our database is using the full space. 

Managed services carry a higher per GB cost because we are paying a premium for the operational automation and vendor expertise embedded in the service. There are many cases where the benefits of a fully managed service can offset the longer term costs of self-hosting: engineer time spent on patching, backup management, incident response, and capacity planning, which can be substantial. 
- Self-hosted databases can be cheaper, particularly if a team already has strong database administration expertise. 
- Particularly for smaller teams and startups, the staffing and operational costs of self-hosting a database can outweigh the benefits.

Where self-hosting truly shines is fine grain control over our system. Depending on our exact needs, a managed service may simply not have our required configuration available. A self-hosted database allows us to choose any operating system, any database engine version, and any configuration. This matters for organizations with security & compliance requirements such as: 
- demanding specific software versions
- teams running highly customized database configurations
- workloads that need the absolute maximum IOPS that only direct SSD block storage configurations can provide

Managed services, by design, abstract away the hardware layer and impose some limits on database configuration access. 

## Summary

Cloud databases run on cloud infrastructure and are accessed over the internet. Unlike traditional on-premises databases, cloud databases can scale up or down on demand, eliminating the need to purchase and configure additional hardware. Organizations can also design hybrid systems that combine on-premises and cloud databases depending on their data sensitivity and existing infrastructure.

When deploying a database in the cloud we can choose to use a fully managed service or self-host a database on virtual machines our team controls. With a fully managed service, routine tasks like backups, patching, security updates, and failover are automated by the provider. With a self-hosted database, our team owns all of those responsibilities.

Managed services cost more per GB of storage but those can be offset by the significant engineer time that would otherwise be spent on tasks like maintenance and incident response. When we use a fully managed database service, our team is still responsible for schema design, query optimization, and scaling decisions. Treating a managed cloud service as a "set it and forget it" solution is a common and often costly mistake in production systems.

Self-hosted databases offer the greatest control, making them a strong fit for teams with:
- strict compliance needs
- highly customized configurations
- specialized performance requirements 

## Check for Understanding

Which of the following is the user responsible for when using a self-hosted database?
Which of the following is a user responsible for when using a fully managed database?


<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: c97f446a-49dd-4c3d-93bf-c023a1bda9b1
* title: Intro to Cloud Databases
##### !question

A startup team deploys a self-hosted database on a cloud virtual machine. Several months later, a security vulnerability is discovered in the database engine. Which team is responsible for applying the patch?

##### !end-question
##### !options

* The cloud provider, because the database runs on their infrastructure.
* The startup's engineering team, because they own and manage the database software.
* No action is needed because cloud infrastructure is inherently secured.

##### !end-options
##### !answer

* The startup's engineering team, because they own and manage the database software.

##### !end-answer
##### !explanation

With a self-hosted database, the engineering team owns all operational responsibilities, including applying security patches and staying current on vulnerability disclosures. The cloud provider manages the underlying physical infrastructure, but the database software installed on the virtual machine is entirely the team's responsibility. 

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: cloud-db-high-availability
* title: 1651a69a-24d5-4c46-b30f-0fdfb691b589
##### !question

An engineering team is evaluating whether to use a fully managed database service or self-host a database on virtual machines. Their top priority is automatic failover to a standby instance if the primary database goes down. Which option best supports this requirement with the least operational overhead?

##### !end-question
##### !options

* Self-hosted, because the team has full control over failover configuration.
* Fully managed, because built-in multi-AZ deployment and failover are handled by the provider.
* Self-hosted, because cloud virtual machines automatically restart failed processes.
* Fully managed, but the team must still manually trigger failover when the primary goes down.

##### !end-options
##### !answer

* Fully managed, because built-in multi-AZ deployment and failover are handled by the provider.

##### !end-answer
##### !explanation

Fully managed database services typically include built-in multi-availability zone (multi-AZ) deployment and make it easy to maintain a standby replica and initiate automatic failover. A self-hosted setup requires the engineering team to design, configure, and test all replication and failover logic manually.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: checkbox
* id: cb961347-90f7-4e5d-a1b0-a144061ef954
* title: Intro to Cloud Databases
##### !question

Which of the following scenarios are good cases for choosing a self-hosted database over a fully managed cloud database service?

##### !end-question
##### !options

* The team wants to avoid writing backup and restore scripts.
* The organization requires a specific database engine configuration that the managed service does not support.
* The organization has stong IT infrastructure and already pays for cloud storage resources that could be shared to better their utilize resources.
* The organization wants to reduce the time engineers spend on database maintenance.

##### !end-options
##### !answer

* The organization requires a specific database engine configuration that the managed service does not support.
* The organization has stong IT infrastructure and already pays for cloud storage resources that could be shared to better their utilize resources.

##### !end-answer
##### !explanation

Self-hosted databases allow fine-grained control the cloud vendors cannot always provide. Managed services abstract away the hardware layer and impose some limits on configuration access. 

If an organization has the IT resources to host set up and maintain their own database and already has underutilised cloud storage that they could use without further costs, it would cost them less to self-host than to pay for additional storage through the managed database service.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->