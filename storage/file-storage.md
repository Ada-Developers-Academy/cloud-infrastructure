# File Storage

If we have interacted with files on a computer before, we have used a type file storage! Since we have some familiarity with accessing folders and files, we'll briefly cover how file storage represents data before diving into the big topic of when we might use cloud file storage. 

## Learning Goals

- Describe at a high level how file storage works.
- Describe key considerations when choosing file storage.
- Describe common use cases for  file storage.

## Data Representation

File storage is a hierarchical data storage architecture that uses files and folders to organize data. Data is saved in files and then organized in folders, these folders are then arranged within other folders, sometimes called directories or subdirectories. A file uses its name, the file extension type, and the specific path to the data’s location as unique identifiers, and we access files by their paths through the heirarchy of folders using standard file-level protocols. 

<hierarchical folder system example image>

Each file includes metadata such as name, size, timestamps, and permissions. In addition to the file-level permissions, file storage typically supports other access control tools provided by cloud vendors which enables organizations to control access across multiple users or systems.

File storage is limited in speed compared to other storage strategies due to its hierarchical architecture. Needing to follow a sequential path to find a resources takes more time compared to ID and lookup table based retrieval like we saw with block storage. 
- We can think about this difference in retrieval like the difference between searching for an item in a list vs a dictionary. Searching a list takes longer since we have to go sequentially through the list, similar to following a path to a file in file storage. When we look something up in a dictionary, a hashing algorithm can rapidly return the data, similar to how  block storage uses a unique address in a lookup table for fast access.  

## Cloud vs Traditional Hosting

When we host our own file storage, we are responsible for purchasing, maintaining, and replacing the hardware, as well as installing, managing, and patching the software that runs the file system. We're also responsible for creating any plans to back up and restore data in case of hardware or software failure (more on this in a later lesson!). But, we have full control over everything within that system, and once we have purchased the initial hardware the recurring costs are mostly electrical and maintenance. 

Cloud providers have "fully managed" file storage solutions. Here, "fully managed" means that both the hardware and software, including ongoing maintenance, are handled by the cloud provider. No replacing hardware yourself when it fails, no worring about if the latest security patch was applied. This makes it lower cost to get started with a cloud file storage service, but you pay more for the storage itself.    

### Strengths & Limits

**Ease of Integration**: Most operating systems are built to understand standard file-level protocols, which means that operating systems and applications can interact with cloud file storage without needing new APIs or custom integrations. 

**Concurrent Shared Access**: We mentioned earlier that organizations can define access policies for folders and files. This allows a single company with many teams to share the same file system since boundaries can be set up for team and organizational group's folders and files. File storage is also designed for multiple compute instances, systems, or users to access the same files at the same time, making it well suited for collaborative environments.

**Scaling**: Cloud file storage can rapidly scale in volume, which is great if you have rapidly growing or unpredictably growing data needs. There are limits to data access speed due to file storage's hierarchical nature, we cannot add more resources to access files faster.  

**Cost**: Current cloud providers offer plans based on active storage, so you only pay for space that is used. Cloud file storage can be cheap initially, but monthly fees can accumulate into significant expenses over time. Many cloud vendors also charge "egress fees", which are fees levied when data leaves one of their data regions or leaves their network, which can make it costly to switch cloud providers or to a self hosted solution.

## When to Choose the Cloud

As we can see, there is tradeoff between hosting your own file storage and using a fully managed file storage solution. Hosting your own solution requires a high upfront cost and ongoing labor by IT staff on backups, security patching, and troubleshooting, which can outweigh the cost of cloud subscriptions for small and medium-sized teams. While initial investment is high, after the initial setup, recurring costs are mostly electrical and maintenance, allowing for lower costs when working at massive scale.

Self-hosted file storage is best for companies with: 
- Need for total control over data: This often comes up for industries that need know exactly where their data physically lives for meeting data sovereignty laws (laws that require data to remain within specific national or regional borders).
- High security requirements: for companies that desire or require no third-party access, eliminating the risk of a cloud provider or government agency accessing data through "backdoor" legal requests or terms of service changes.
- High data portability needs or massive datasets that stay relatively static: If we have constantly moving data or a large amount of data that is not changing frequently, but we need to retrieve it constantly we will pay significant costs in data egress fees from cloud providers. 

A fully managed solution gives a low upfront cost but higher ongoing cost for storage used, including extra fees if large amounts of data need to regularly move into and back out of the storage system. Cloud solutions remove the need to worry about hardware failure and maintenance, allowing for a focus on business productivity rather than infrastructure management.

Cloud file storage solutions are best for companies with: 
- Unpredictable growth: If storage needs grow fast or unpredictably, cloud file storage provides necessary flexibility without massive capital expenditure.
- Limited in-house IT staff: If IT staff time limited or expensive, cloud file storage is typically cheaper because it eliminates hours of system maintenance.
- A need for rapid deployment: It takes much less time to deploy a cloud file storage solution than to provision and set up a data center for file storage.

For most startups and small businesses, the labor savings of the cloud outweigh the cost of file storage. For established companies with large data needs, a hybrid approach that uses on-premise for active, large data and cloud solutions for archiving and collaboration often provides the best balance.

## Use Cases 

The most common use cases for file storage solutions are around applications and tasks that require sharing files or collaboration. Some example uses are: 

**Team Collaboration & Remote Work**

Cloud file systems are meant to be interacted with remotely and concurrently, making it an easy tool to use for planning, coordination, and documentation. Here at Ada, the staff uses a shared cloud file system every day to hold team meeting notes, collaborate on curriculum plans, and more!

**Shared Content Repositories**

Some applications or features may need to provide a single source of truth for multiple web servers that need to serve the same website content or assets. This could look like a media company running multiple WordPress web servers that all pull images and stylesheets from a shared cloud file system to ensure consistency.

**Legacy Application Migration**

Some companies may need to move legacy on-premises applications to the cloud without rewriting their code to use cloud-native APIs. An example could be a financial firm moving a decades-old accounting application that requires standard file protocols to function to a cloud provider.

## Summary

File storage is a hierarchical architecture that organizes data into files and folders, where each file is identified by its name, extension, and path. Retrieving data from file storage is slower than ID-based approaches like block storage since it requires navigating a sequential path through the folder hierarchy.

Organizations can either self-host their file storage or use a fully managed cloud solution. Self-hosting requires significant upfront hardware investment and ongoing IT labor, but offers full control and lower recurring costs at scale. This makes it well suited for organizations with strict data sovereignty laws, high security requirements, or large static datasets with frequent retrieval needs. Cloud providers handle all hardware and software management for cloud flie storage solutions, lowering the barrier to entry, but ongoing storage fees and egress charges can accumulate into significant expenses over time.

Cloud file storage offers several advantages around:
- ease of integration via standard file protocols
- native remote and concurrent access of data
- ease of rapid scaling

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 820c4f30-b6c4-43b6-bff4-d770a86f782f
* title: File Storage
##### !question

Which of the options below is a reason that file storage is  generally slower to retrieve data than block storage?

##### !end-question
##### !options

* File storage compresses data by default, requiring decompression before it can be retrieved.
* File storage encrypts every file individually, adding decryption overhead to each retrieval request.
* File storage stores data across multiple physical drives, requiring the system to reassemble pieces before returning them.
* File storage needs to follow a sequential path in a hierarchical structure to retrieve data.
* File storage relies on network requests to a remote server, even when the data is stored locally.

##### !end-options
##### !answer

* File storage needs to follow a sequential path in a hierarchical structure to retrieve data.

##### !end-answer
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: checkbox
* id: 2be20561-b770-4a4d-a28d-3782dcc8028e
* title: File Storage
##### !question

A fast-growing startup has two engineers managing all of its infrastructure. 

Select all of the options below that make cloud file storage a better fit than self-hosted storage at this stage.

##### !end-question
##### !options

* Cloud file storage requires no hardware purchasing or physical maintenance, freeing up the engineers' limited time.
* Cloud file storage can scale rapidly to meet unpredictable growth without requiring capital expenditure on new hardware.
* Cloud file storage guarantees faster data retrieval speeds than any self-hosted solution could provide.
* Cloud file storage eliminates the need for the engineers to manage security patching and system updates themselves.
* Cloud file storage has no recurring costs since startups typically qualify for free tiers at any scale.

##### !end-options
##### !answer

* Cloud file storage requires no hardware purchasing or physical maintenance, freeing up the engineers' limited time.
* Cloud file storage can scale rapidly to meet unpredictable growth without requiring capital expenditure on new hardware.
* Cloud file storage eliminates the need for the engineers to manage security patching and system updates themselves.

##### !end-answer
##### !explanation

Incorrect: Cloud file storage guarantees faster data retrieval speeds than any self-hosted solution could provide.
- Cloud file storage is slower at retrieval than many kinds of storage due to its architecture.

Cloud file storage has no recurring costs since startups typically qualify for free tiers at any scale.
- This is not true of most, if any, cloud providers.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->
