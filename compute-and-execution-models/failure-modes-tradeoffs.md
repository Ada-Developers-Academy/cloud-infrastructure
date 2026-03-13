# Failure Modes and Trade-Offs in Production

## Learning Goals

- Identify the scope of failure across the compute spectrum, from full virtual machine outages to isolated function timeouts.
- Evaluate the cold start impact on system latency and identify strategies to mitigate performance degradation in ephemeral environments.
- Predict and prevent retry storms by implementing exponential backoff and circuit breaker patterns in distributed systems.
- Compare economic trade-offs between different computing models
- Map the operational responsibility boundary to determine which failures are our responsibility versus the cloud provider's.
- Contrast observability strategies for persistent versus ephemeral compute

### The Anatomy of a Crash

When we talk about production environments, we have to move away from the idea that "the code broke" and start thinking systematically. In a distributed cloud environment, a crash is rarely only caused by a bug in code. It is most likely due to a failure of the environment the code lives in. Understanding the blast radius of a failure will help us see how much of our system goes dark when a specific component fails. There are many, many ways that systems in the cloud can fail. We will have a look at the hierarchy of failure by looking at the different layers of compute we have already reviewed in previous lessons. Strategies for detecting these failures and techniques for mitigating these failures are out of scope for this introductory lesson, but will be covered later in the curriculum. 

### Virtual Machine (IaaS) Failures

When we run our code on a virtual machine, we are responsible for almost everything it including the operating system, network configurations, the runtime environment, and the application code. Since we have so much control, there are many ways the system can degrade. We won't look at every single way that IaaS resources can fail, but we can broadly group them into 3 categories of failure.

| Failure Mode | Description | Examples |
| --------- | --------- | -------- | 
| Infrastructure and Platform | These failures originate outside of our control and are caused by the cloud service provider's physical hardware or the hypervisor (the software that runs the VM). Even though the cloud feels infinite, it still runs on physical racks of servers that can break. | A physical power supply or a network switch fails at a data center. Even though your code is perfect, your VM suddenly becomes unreachable because the physical host it lives on has lost power.<br><br> A physical router in the data center rack fails. Your VM is perfectly healthy and running, but it is effectively cordoned off from the internet, making it unreachable by your users. |
| OS and Application | These failures occur within the VM we rent in the cloud, which means we are responsible for the health of the environment our code lives in. | Our application generates a small log for every request. We forgot to set up a "cleanup" script, and after six months, the logs take up 100% of the disk. The OS can no longer create the temporary files it needs to function, and the whole system seizes up.<br><br> An integration script crashes but doesn't fully close its connection. Hundreds of these zombie processes accumulate, eventually hitting the limit of processes allowed, which prevents the web server from handling new incoming requests from users.|
| Configuration and Management | These are failures related to how the VM is deployed, networked, or managed. These failures can often the most challenging to debug because the VM is technically healthy and the code is bug-free but the logic of the cloud setup is broken. | We move our database to a new IP address. The VM’s operating system has cached the old IP address instead of looking up the new one so our app keeps trying to talk to a the database that no longer exists.<br><br> A certificate or SSH key used for automated deployments expires so our CI/CD pipeline can no longer log into the VM to push new code, leaving the production environment stuck on an old, buggy version of our application. |

### Managed Instances Crashes (PaaS)

In a Platform as a Service (PaaS) model, we have traded away control over the OS in exchange for ease of use. We no longer worry about physical hardware or OS patching. Instead, we rent managed instances from the cloud provider. While the platform handles the the infrastructure, our instance can still fail in ways that are unique to this level of abstraction. The below categories of failure are not an exhaustive list, but give us a starting point for understanding the different ways managed instances can fail.

| Failure Mode | Description | Examples |
| --------- | --------- | -------- | 
| Resource and Runtime | Even though the underlying hardware is abstracted away, our instance operates within the bounds of limits on resources like RAM and CPU. If our application exceeds its limits, the platform can intervene by terminating instances to protect the stability of the rest of the cloud infrastructure. | Our code attempts to process a massive CSV file by loading the entire 2GB file into memory. Since our managed instance is only allocated 512MB of RAM, the platform kills the process to prevent it from destabilizing the host.<br><br> Our application is configured to handle 50 simultaneous connections. A sudden spike in traffic sends 500 requests. The instance becomes overloaded and is unable to open new threads, causing the platform to mark the instance as unhealthy and terminate it. | 
| Lifecycle and Health | In a managed environment, the platform is constantly monitoring our instance. If our code doesn't behave according to the platform's expected lifecycle, the platform will assume the instance is dead. | Our app is under heavy load and its CPU is at 100%. When the platform sends performs a health check to see if the app is alive, the app is too busy to respond. The platform assumes the app has frozen and kills a functioning instance.<br><br> We add a lot of initialization logic (like pre-loading a large machine learning model) that takes 90 seconds. The platform expects the app to be ready in 30 seconds. Since the app took too long to initialize, the platform kills it and tries again in an endless loop. | 
| Integrations | In a PaaS environment, the cloud provider handles the underlying operating system and the runtime environment. Since we do not manage the local file system or a local database on these instances, our application is architected to be a central hub for data. Its primary job is to receive a request, fetch data from an external database, perhaps request a file from object storage, and then send a response back to the user. When these external connections fail, the instance remains running according to the cloud provider's dashboard, but it becomes effectively isolated from the resources it needs to function. | Our application is configured to maintain a set number of connections to an external database. If a surge in traffic causes the database to reject new connections, the instance still stays online. However, every subsequent user request that requires a database query will fail, as the instance can no longer reach its data source.<br><br> We update an API key in the platform’s configuration manager. If the running instance fails to pull that updated credential, perhaps due to a brief network hiccup during the configuration sync, it will continue attempting to connect using the old, invalid key. The instance is healthy in terms of CPU and RAM, but it is locked out of the external services it needs to process requests. | 

### Serverless Computing Errors (FaaS)

With serverless computing, the server is abstracted away but the physical constraints of computing remain. There are hard limits on time, memory, and concurrent executions defined by the cloud provider. Serverless computing fails when your code attempts to operate outside these limits. Since the cloud provider manages the underlying infrastructure, it cannot allow one user's code to run indefinitely or consume infinite memory, as this would destabilize the shared hardware. When a limit is exceeded, the provider does not attempt to fix the environment or wait for the code to finish before it terminates the execution thread.

| Failure Mode | Description | Examples |
| --------- | --------- | -------- | 
| Execution Limit | The provider allocates a specific amount of time and memory for a single task. If the code exceeds these limits, the provider terminates the process instantly. | A function is allocated 128MB of RAM. If it attempts to parse a large JSON payload that requires 130MB, the provider kills the environment immediately.<br><br> A function is configured with a 30-second timeout to process a data export. If the dataset grows and the task takes more than 30 seconds, the provider stops the execution environment. | 
| Resource and Concurrency Constraints | While FaaS is designed to scale horizontally to meet demand, it is not truly infinite. Failures occur when the volume of requests hits the ceiling of the provider's capacity. | If a function hasn't been triggered recently, the provider must provision a new environment, which adds several seconds of startup time. If a synchronous system expects a response within 2 seconds but the cold start takes 5 seconds, the system records a failure even though the code eventually runs successfully.<br><br> A cloud account is limited to 1000 concurrent executions. If a sudden traffic spike triggers 1,500 instances of the function at the same moment, the provider reject 500 requests. |
| Integration & Permission Errors | In a serverless environment, functions are event-driven and execute when they receive a specific signal from another part of your cloud infrastructure. This category of failure occurs when the communication lines between your function and the rest of the world are broken, either because the trigger never arrives or because the function lacks the security credentials or permissions required to interact with the resources it needs. | A function is intended to trigger when a file is uploaded to a specific storage folder. If the event is configured with a typo in the folder path, files will be uploaded, but the function will never be alerted. The system fails to act, yet no error is recorded because the code was never invoked.<br><br> A function is triggered by a file upload to Bucket A. The function tries to read the file, but it only has permission to access "Bucket B." The function crashes because it is not authorized to read the data it was triggered to process. |

### Systemic Failure Patterns

Once we understand how individual "units" (VMs, instances, or functions) fail, we have to look at how those failures spread through a system. In the cloud, failures are rarely isolated and when one part of the system behaves unexpectedly, it can create a ripple effect. We can categorize these large-scale issues into three specific patterns.

#### The Cold Start: The Latency Trigger

- Context: This is a performance characteristic unique to ephemeral compute (FaaS) and some managed instances (PaaS) that scale to zero. Because the cloud provider doesn't keep your code running on a server 24/7, the very first request after a period of inactivity must wait for the environment to be built from scratch.
- The Problem: The startup time includes provisioning a container, downloading your code package, and initializing the runtime environment creates a sudden, one-time spike in latency. While your code might normally run in 100ms, for example, a cold start can push that execution time to a few seconds or more.
- The Failure: In a distributed system, latency is often indistinguishable from a crash. If your frontend load balancer or a service has a timeout set to 3 seconds, it will cut the connection and return an error to the user before the cold start even gets a chance to finish. The function eventually runs, but the system has already recorded a failure because the timing boundaries were violated.

#### The Retry Storm: The Helpful Saboteur

- Context: This pattern occurs across all cloud models, but it is most destructive in serverless environments because of the sheer volume of parallel executions. It could be triggered by a temporary problem in a downstream dependency, like a database or a third-party API.
- The Problem: When a service fails, our instinct is to try again. Most systems have automatic retries enabled by default. If 1,000 instances of a function fail because a downstream database is momentarily slow, the system will instantly launch 1,000 new attempts at the exact same time, which will cause a retry storm.
- The Failure: A service that is already struggling or unavailable, is overwhelmed by repeated, automated retry requests from clients. This creates a self-reinforcing loop of traffic that can prevent the service from recovering and can eventually lead to a complete system outage. 

#### Cascading Failures: The Domino Effect

- Context: This happens when the failure of one, often minor, component triggers a chain reaction of failures across interconnected systems. Due to overloaded resources, retry storms, or tightly coupled services, one node's crash forces traffic onto other nodes, which can quickly grow into a system-wide outage.
- The Problem: High availability (HA) systems are designed to share traffic across multiple units. If you have a cluster of 10 instances and 3 of them crash due to a localized error, for example, the load balancer does not stop the traffic. Instead, it will it redirect 100% of the users to the remaining 7 instances.
- The Failure: The remaining healthy instances are now suddenly handling 30% more traffic than they were designed for. This extra load causes their memory and CPU usage to spike, leading them to fail. As they die, the load shifts again to the last few survivors, which fail even faster. Like a row of dominoes, the entire cluster collapses.

We can see that systemic failure patterns represent the shift from local errors to total outages. While individual failures are often caused by a single bug or a resource limit, systemic failures are caused by the interactions between healthy components. Whether it is the latency of a cold start, the persistent retries that lead to a retry storm, or the shifting weight of a cascading failure, these patterns demonstrate that in the cloud, the solution to a problem (such as automatic scaling or automated retries) can often become the catalyst for a larger collapse if they are not carefully managed.

### The Economics of Idle Waste versus Cloud Bursting Premiums

Transitioning from how these systems fail to how they are funded, we encounter the fundamental economic trade-off of cloud computing. Every architectural choice we have discussed so far, from the always-on VMs to the on-demand functions, carries a different financial profile. The goal for a cloud architect is to minimize spending on idle resources without overpaying for cloud resources. 

In the VM and managed instance models, you are typically billed for provisioned capacity. Since these resources are always on, you pay for the specific resources (CPU, RAM, and disk) you have reserved for a set duration, regardless of whether your application is processing many requests per second or sitting completely idle. This creates the risk of cloud waste, where the gap between the capacity you have paid for and your actual utilization represents lost capital, particularly during off-peak hours. However, the trade-off for this potential waste is a significantly lower unit cost for compute power and a high degree of cost predictability. If your application maintains a constant, 24/7 baseline of traffic, these models can be the most cost-effective choice because you avoid the service premiums associated with automated, high-speed scaling.

Conversely, the serverless computing model shifts the economic focus from constant presence to utility, which means customers are billed strictly for the number of requests and the duration of each execution. This approach is ideal for unpredictable or spiky workloads since you only pay for what you need, which effectively eliminating the financial risk of idle hardware. To provide this elasticity, cloud providers offer cloud bursting which is a strategy that enables instant scaling to manage peak loads without requiring permanently investing in excess resources. However, cloud bursting comes at a premium which results in higher per-unit cost for computing when compared to the equivalent power in a fixed VM. While this premium makes FaaS an excellent model for avoiding costs during low-traffic periods, it can become expensive for applications with sustained, high-volume traffic. 

There is a tipping point in the cloud cost analysis. At low volumes, the FaaS model is cheaper because it eliminates idle waste. However, as traffic becomes consistent and high-volume, the cost of pay per function computing begins to outweigh the cost of the always-on resources of the IaaS or PaaS models. An architect must closely consider these costs when building out their systems and making decisions about when to move from one model to another.

### Operational Responsibility Boundaries and Visibility of the System 

Recall that in the cloud, we follow the Shared Responsibility Model, which outlines the responsibilities of cloud service providers (CSPs) and customers for securing the different components of the cloud environment, including hardware, infrastructure, endpoints, data, configurations, settings, operating system , network controls and access rights. As we mentioned in [The Compute Spectrum: Control versus Abstraction](./the-compute-spectrum.md), when we move from IaaS toward FaaS, we trade control for convenience.

The responsibility for handling a failure either falls on the cloud provider or the customer paying for cloud computing services. On one end, the cloud provider is responsible for failures occurring within the physical and virtualization layers. These are systemic issues that you cannot fix, regardless of your compute model. On the other end, the customer, regardless of which model of cloud computing they are utilizing, is responsible for failures caused by your specific code, security settings, configurations and data.

As we move from IaaS toward FaaS, each model abstracts away more of the underlying complexity, shifting the burden of maintenance (patching, scaling, and hardware monitoring) to the provider. However, this shift creates a fundamental trade-off because as our operational burden shrinks, our visibility into the system also shrinks. When a cloud provider manages a layer of the stack to reduce our operational burden, they simultaneously restrict our access to that layer to maintain the security and standardization of their multi-tenant platform. As the management boundary moves higher, the blind spots in your infrastructure grow.

In addition to being able to see into a system, we also have the ability to understand the internal state of a system by looking at the data it produces. This is called observability. The strategy we choose depends entirely on whether our computing resources are persistent or ephemeral. In a VM or a long-running container, you have fixed resources so you can install monitoring agents that can collect data over days, weeks, or months. In serverless computing, there is no "pulse" to monitor because the environment only exists for the duration of a single request. You cannot log into the machine to see what happened because by the time you realize there was an error, the process has completed. The evidence of a failure can be as short-lived as the environment itself!

We will take a deeper dive into observability later in this curriculum to better understand how to use data from a system to investigate why problems occurred in our system. Stay tuned!

## Summary

Modern cloud architecture can be characterized by its tension between control and abstraction, where the transition from persistent Virtual Machines to ephemeral Serverless functions shifts the operational burden of infrastructure maintenance to the provider at the cost of direct system visibility. This shift requires a move from monitoring long-term node health to tracing transient, event-driven transactions, as traditional blind spots grow and local evidence vanishes the moment a process exits. 

To build resilient systems, architects must navigate the Shared Responsibility Model to distinguish between provider-owned hardware outages and customer-owned logic failures, while simultaneously maintaining cloud costs  between the potential for paying for idle resources and the premium cost of cloud bursting for instant scaling.

Ultimately, success in the cloud requires a systemic perspective to mitigate common failure patterns (cold starts, retry storms, and cascading collapses) and ensure that the very features designed for reliability do not become the catalysts for total system failure.

## Check for Understanding

<!-- Question 1 -->
### !challenge
* type: multiple-choice
* id: aa5f292d-733e-4673-a4f1-808d764642ad
* title: Failure Modes & Trade-offs

##### !question
Your team is running a high-traffic API using a serverless computing service. Users suddenly report intermittent 403 Forbidden errors. You check your centralized logging and see that the function is executing, but it is being denied access when trying to write to a specific storage bucket. Which of the following is true? 
##### !end-question

##### !options
* This is a provider failure. The cloud service provider’s virtualization layer is malfunctioning.
* This is a customer failure. The responsibility lies in the system's permissions configuration.
* This is an OS-level failure. You must SSH into the function environment to clear the credential cache.
* This is a blind spot failure. Since you lack visibility into the network stack, you cannot determine the cause.
##### !end-options

##### !answer
* This is a customer failure. The responsibility lies in the system's permissions configuration.
##### !end-answer

#### !explanation 
In a FaaS model, the customer is always responsible for data storage. Since the logs show the function is running, (which proves the provider's infrastructure is healthy) but being denied access, the failure is a misconfiguration of the security roles defined by the developer.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: multiple-choice
* id: 19cabb6c-351f-4586-b25b-4d659366edd1
* title: Failure Modes & Trade-offs

##### !question
A downstream database experiences a 10-second delay in connectivity. Your application, which is configured with default automatic retries, immediately generates a massive surge of new requests. Even after the database recovers, it remains unresponsive because it is being overwhelmed by thousands of re-execution attempts. This is an example of:
##### !end-question

##### !options
* A Cold Start
* A Cascading Failure
* A Retry Storm
* Idle Waste
##### !end-options

##### !answer
* A Retry Storm
##### !end-answer

#### !explanation 
This is a classic retry storm. The automatic retries turned a minor, temporary glitch into a sustained outage by sending a massive volume of traffic that prevented the dependency (the database) from recovering.
#### !end-explanation 
### !end-challenge

<!-- Question # 3 -->
### !challenge
* type: multiple-choice
* id: 36d11775-7a8c-4cb1-a791-9770caff7406
* title: Failure Modes & Trade-offs

##### !question
You are launching a new internal tool that will only be used by employees for 15 minutes every Monday morning to submit timecards. For the rest of the week, the tool will receive zero traffic. Which compute model and economic factor should most heavily influence your choice?
##### !end-question

##### !options
* IaaS because it ensures cost predictability and avoids cloud bursting.
* PaaS because it maintain high visibility into the OS for the weekly 15-minute window.
* FaaS because it eliminates idle resources by scaling to zero during the rest of the week.
* FaaS because cloud bursting is always cheaper than a VM regardless of traffic volume.
##### !end-options

##### !answer
* FaaS because it eliminates idle resources by scaling to zero during the rest of the week.
##### !end-answer

#### !explanation 
For"spiky workloads with long periods of zero activity, FaaS is the most economical choice. While FaaS has a higher unit price due to cloud bursting, it is outweighed by the savings gained from not paying for a VM that would sit idle for 99% of the week.
#### !end-explanation 
### !end-challenge