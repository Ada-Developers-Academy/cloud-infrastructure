# The Compute Spectrum: Control versus Abstraction

## Learning Goals

- Explain what it means for code to run in the cloud.
- Differentiate between cloud compute models such as **virtual machines (IaaS)**, **managed application platforms (PaaS)**, and **serverless computing (FaaS)**.
- Identify responsibility boundaries between the developer and the cloud provider in different compute models.
- Evaluate tradeoffs between control and operational burden when choosing a compute model for an application.
- Recognize how compute choices influence scalability, reliability, cost, and system design.

### Who Manages the Machine?

When developers run code locally, the program executes directly on their personal computer. The developer controls the environment, which can include the operating system, installed software, networking configuration, and available hardware.

In cloud computing, the code runs somewhere else. Instead of executing on a local machine, it runs on remote computers owned and managed by a cloud provider. The following table lists some of the most common cloud providers and platforms.
	
Provider | Cloud Platform
--- | ---
Amazon | Amazon Web Services (AWS)
Google | Google Cloud Platform (GCP)
Microsoft | Microsoft Azure
Oracle | Oracle Cloud Infrastructure (OCI)

The remote systems managed by cloud providers supply the infrastructure required for applications to run in production environments. From the developer’s perspective, the cloud allows them to define how their application should run, while the cloud platform handles the provisioning and management of the underlying resources.

This approach solves several practical problems that arise in real-world systems:
- Applications may receive unpredictable traffic.
- Systems must remain available even when hardware fails.
- Infrastructure must scale up or down based on demand.
- Teams need to deploy and update applications quickly.

Cloud platforms address these challenges by offering different compute execution models, each providing a different balance between developer control and operational complexity. 

### Infrastructure as a Service: Virtual Machine-Based Compute

Infrastructure as a Service (IaaS) is a form of cloud computing that delivers IT infrastructure like compute, storage, and network resources over the internet. Paying for IaaS enables businesses to avoid or move away from the complexities that come along with managing on-premises systems.

There are three categories of infrastructure services that cloud providers make available to their customers: compute, storage, and networking. We will briefly review each category in this section, but later Learn lessons will go dive deeper into each individual topic.

#### Compute

In the IaaS model, "compute" refers to the raw processing power required to run your code. While we often think of "the cloud" as an abstract concept, it is physically made up of massive, powerful computers sitting in a data center.

In the early days of the web, if you wanted to run an application, you had to buy a physical server, plug it in, and manage it. With the cloud, companies no longer need to own and manage physical servers because they can pay for computing services. The computing services are powered by "virtual machines" (VMs), which are software-based emulations of a physical computer. 

### !callout-info

## Curious About VMs?

[Virtualization](https://en.wikipedia.org/wiki/Virtualization) is a technology that uses software to create virtual representations of physical IT resources like servers, storage, and networks. We call these virtual representations virtual machines. 

Through a technology called a [hypervisor](https://en.wikipedia.org/wiki/Hypervisor), a cloud provider can take one powerful physical server and partition it into smaller, isolated "virtual machines" (VMs). VMs are software-based emulations of a physical computer. 

We won't go into more detail about how virtualization and hypervisors work in this curriculum, but if you are interested in learning more, we invite you to explore your curiosity!

### !end-callout

In industry terms, when you actually go to a cloud provider's dashboard and "turn on" a virtual machine, it is called an "instance". An instance is a single, running copy of a VM and when you "spin up an instance" you are essentially turning on a computer that lives in the cloud. IaaS gives you the most control among the cloud service models, which means you will need to appropriately configure the internals of your instances based on your application needs. The following are examples of resources that can be configured:
- Central processing unit (CPU)
- Computer memory (RAM)
- Computer data storage
- Operating system

#### Storage

In the cloud, storage is not "one size fits all". Are you running a database, hosting images for a website, or sharing configuration files across many different servers? The choice of what kind of storage to use depends on your application's needs. 

IaaS offers three types of cloud storage:
- **Block storage**: Block storage takes any data, like a file or database entry, and divides it into blocks of equal sizes. This system then stores the data block on underlying physical storage for fast access and retrieval. Developers might choose block storage if they require require high-speed, low-latency reading and writing.
- **File storage**: Think of file storage like the hierarchical system we are all used to on our personal computers that have folders inside folders. In the cloud, this usually refers to network-attached storage (NAS), which is a file system that can be accessed by multiple instances at the same time. An example of when file storage would be the most appropriate kind of storage would be if you have multiple web servers that all need to access the same set of configuration files or user-uploaded documents.
- **Object storage**: Object Storage is a flat system where every file (an "object") is stored with its data and a unique identifier. Imagine object storage like a giant bucket where you throw a file in, and the cloud gives you a unique link to get it back. It is ideal for storing, backing up and managing high volumes of static unstructured data reliably and efficiently. It is the most common type of cloud storage. 

We will look at each in more detail in the [Storage](../storage/what-is-cloud-storage.md) lesson.

#### Networking

IaaS infrastructure from third-party providers also includes networking resources like routers, switches, firewalls and load balancers. Networking is what allows you to define who is allowed into your system, how your data travels within it, and how your application reaches the rest of the world.

Cloud instances are like isolated islands that cannot talk to each other or the outside world. Networking provides the "virtual wires" and "security fences" required to turn a collection of individual computers into a functioning system. You use these resources to create a private, protected space for your application where your database can safely chat with your web server without being exposed to the open internet. When resources are connected in a network, we can define entry points for users using traffic-management tools to ensure that when thousands of people visit a site at once, they are directed to a healthy server so the system doesn't get overwhelmed.

We will learn more about networking in the cloud in subsequent lessons. 

### Platform as a Service: Managed Platforms

Platform as a Service (PaaS) is another cloud computing service model where the service provider manages the hardware and software resources needed to run an application. PaaS abstracts the underlying infrastructure, which allows developers to focus on writing code, deploying applications, and managing data. We can think about the IaaS model as providing developers with the raw materials to build a house and the PaaS as providing developers with a pre-built house where the utilities are already hooked up and the maintenance is handled. 

Beyond just hosting code, PaaS offers built-in features designed to speed up the development lifecycle. These platforms typically include integrated runtimes, meaning developers do not need to manually install or update versions of Node.js, Python, or Java.

PaaS resources also include automated deployment pipelines, which means the cloud provider's platform can automatically detect changes in a GitHub repository and "rebuild" an application in production as new changes are merged in. 

Additionally, PaaS environments often include managed services like pre-configured databases, logging tools, and security certificates (SSL). Instead of a developer spending time configuring a firewall or a load balancer, these features are usually "one-click" additions or are automatically handled by the platform as an application's traffic grows.

### Function as a Service: Serverless Compute

Serverless computing, or Function as a Service (FaaS) is an execution where cloud providers manage, provision, and scale infrastructure. This means that developers run code without managing any cloud instances. It is event-driven, designed to be very scalable and customers are only charged for resources consumed during execution, not idle time. The name is a bit of a misnomer because servers do still exist, but they are entirely invisible to the developer from a cloud provider's platform. This model moves further up the spectrum of abstraction, in comparison to IaaS and PaaS, and means that a developer's responsibilities related to managing an application are even further reduced.

Unlike a traditional application that is "always on" and waiting for a request, a serverless application is event-driven. The developer must think in terms of triggers:
- An HTTP request to an API.
- A file being uploaded to a storage bucket.
- A new row being added to a database.
- A scheduled timer (cron job).

While FaaS removes the burden of server management, it introduces new constraints. Most serverless functions are stateless, which means they forget everything once they finish running. A developer cannot save a file to the local disk or store a variable in memory and expect it to be there the next time the function runs. Additionally, developers must account for "cold starts", which are delayed initial executions. If a function has not been used for some time, the provider may have spun down the resources and the next time it is triggered, there is a slight delay while the provider initializes a new environment to run the code.

### Responsibility Boundaries of Different Cloud Computing Models

In cloud computing, the division of labor between the cloud provider and the developer is known as the Shared Responsibility Model. This model defines which party is responsible for maintaining, securing, and patching specific layers of the technology stack. As we moves from IaaS, to PaaS, then to Serverless, the "boundary" shifts upward, offloading more of the operational burden to the cloud provider. Some examples of operational responsibilities are hardware maintenance, operating system updates, or configuring load balancing to distribute incoming traffic. However, a critical rule of the cloud is that the developer is always responsible for the security and logic of the application code and the integrity of the data it handles, regardless of the compute model chosen.

The following table illustrates where these boundaries typically fall across the spectrum of compute models introduced in this lesson:
| Layer | VM-Based (IaaS) | Managed (PaaS) | Serverless (FaaS) |
| --------- | --------- | -------- | --------- |
| Physical Hardware | Provider | Provider | Provider |
| Virtualization | Provider | Provider | Provider |
| Operating System | Developer | Provider | Provider |
| Runtime | Developer | Provider | Provider |
| Application Code | Developer | Developer | Developer |
| Data & Access Identity | Developer | Developer | Developer |

### Tradeoffs Between Compute Models: Control versus Operational Burden

The choice of a compute model dictates where a developer will spend their time. This is often visualized as a spectrum: on one end is maximum control and on the other is maximum abstraction. When a team chooses a compute model that enables them to to control much of the computing resources available, they gain the ability to customize the environment to exact specifications. However, this control comes with a higher operational burden, which means that the developer might be spending time on tasks that do not directly add value to the end user.

Conversely, when a team chooses a compute model with higher levels of abstraction, they trade away low-level control in exchange for speed and simplicity. The cloud provider handles the "heavy lifting" which allows developers to focus entirely on the application code. 

The following table compares how these tradeoffs manifest across the three models:
| Feature | IaaS (High Control) | PaaS (Somewhere In the Middle) | FaaS (High Abstraction) |
| --------- | --------- | -------- | --------- |
| Customization | Full control over the OS and hardware specs. | Limited to supported runtimes and configurations. | Restricted to specific functions and provider constraints. |
| Maintenance | Higher: Developer manages updates, security, and scaling. | Moderate: Provider manages the OS. Developer manages the application. | Lower: Provider manages everything except the code. |
| Speed to Market | Slower: Requires time to configure the environment. | Faster: Quick deployment of full applications. | Fastest: Instant deployment of individual features. |
| Cost Model | Pay for the instance, even if idle. | Pay for the platform capacity. | Pay only for the milliseconds the code runs. |
| Platform Lock-in | Lower: The same instance configuration could be done in any other provider, or even self-hosted. | Lower: The same platform services are often available from other providers. | Moderate: Similar features are often available elsewhere, but there is likely to be no direct migration path. |

## Summary

The spectrum of computing resources represents the evolution of how software is delivered in the cloud. Understanding the balance between control and abstraction is a fundamental skill for the modern software engineer. Below is a brief summary of the different models we covered in this lesson:

- **IaaS**: Provides the most flexibility. It can be used in complex, custom, or legacy systems where the developer needs to manage the entire virtual machine.
- **PaaS**: Provides an environment like a pre-built house. It can be used for standard web applications and APIs where the developer wants to avoid server management but still needs a persistent application environment.
- **FaaS**: Provides the highest level of abstraction. It is best for event-driven tasks, background processing, and applications with highly unpredictable traffic, as it scales automatically and costs nothing when not in use.
Ultimately, the goal of moving to the cloud is to reduce the time between writing a line of code and delivering value to a user. By choosing the right execution model, a team ensures that their system is not only functional but also scalable, cost-effective, and maintainable.

## Check for Understanding

<!-- Question 1 -->
### !challenge
* type: multiple-choice
* id: ce4918cc-f919-4648-ad3c-8082a8f29e31
* title: The Compute Spectrum

##### !question
A security vulnerability is discovered in the underlying Linux kernel of a production environment. Under the Shared Responsibility Model, which compute model places the burden of patching this vulnerability on the developer?
##### !end-question

##### !options
a| Serverless (FaaS)
b| Managed Platforms (PaaS)
c| Infrastructure as a Service (IaaS)
d| None. The cloud provider always patches the operating system.
	
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
IaaS is the only model where the developer manages the operating system, which means the burden of patching falls on developer. In PaaS and Serverless, the provider handles kernel-level updates.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: multiple-choice
* id: 855db302-5e03-4468-9769-66e80c49ac82
* title: The Compute Spectrum

##### !question
A developer is building a background job that only runs for five minutes once per day. Why would Serverless (FaaS) be the most cost-effective choice for this system design?
##### !end-question

##### !options
a| Serverless functions are always cheaper per minute than virtual machines.
b| Serverless computing resources are automatically shut down when they are not in use. This means they would only be charged for the five minutes of execution rather than a 24-hour server rental.
c| Serverless eliminates the need for any networking or storage costs.
d| Providers offer Serverless functions for free if they run for less than ten minutes.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Serverless is the only model that shuts down resources that are not being used. This prevents the developer from paying for idle time when the job isn't running.
#### !end-explanation 
### !end-challenge

<!-- Question 3 -->
### !challenge
* type: multiple-choice
* id: 60d7d410-cbad-458c-86b0-f1c9311ebf3d
* title: The Compute Spectrum

##### !question
When a developer moves "up" the spectrum from control to abstraction (from IaaS toward Serverless), which of the following statements is true regarding tradeoffs?
##### !end-question

##### !options
a| Operational burden increases while control over the environment increases.
b| Operational burden decreases while control over the environment decreases.
c| Scalability decreases as the level of abstraction increases.
d| The developer becomes responsible for more layers of the technology stack.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
When a team decides to use serverless compute, the tradeoff is that they give up the control to tweak the system in exchange for lower operational burden.
#### !end-explanation 
### !end-challenge