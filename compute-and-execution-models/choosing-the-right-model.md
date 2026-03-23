# Choosing the Right Model

## Learning Goals

- Analyze workload traffic patterns to determine the best technical fit and most cost effective compute model.
- Evaluate the operational costs versus the cloud computing costs.
- Justify a specific computing model based on technical requirements and business constraints.
- Compare the financial implications of fixed-cost versus variable-cost models at different scales.
- Design a basic architecture that delegates different parts of a system to different compute models.

### Selection Considerations

Selecting a compute model is a technical decision-making process driven by architectural requirements, traffic patterns, and financial constraints. In production environments, the objective is to identify the model that provides the necessary level of control while minimizing both operational overhead and infrastructure spend. To achieve this, we need to consider the following: technical requirements, workload characteristics, operational capacity, and cost efficiency.

#### Technical Requirements

While technical requirements often serve as the primary filter for selecting a compute model, the boundary between "compatible" and "incompatible" is not always strictly binary. In many cases, an engineer faces a choice between choosing a model that natively supports a requirement or adapting the application and the infrastructure configuration to fit a higher-abstraction model. This introduces a spectrum of engineering friction where the question is not just if a model can satisfy a requirement, but at what cost to complexity and performance.

Technical constraints can be categorized into hard limitations and configuration challenges. Hard limitations are typically dictated by the cloud provider’s architecture and represent true disqualifiers. For example, if an application requires a specific Linux kernel version or a custom network protocol that the provider does not expose in their FaaS or PaaS offerings, that model is effectively disqualified. Similarly, if a workload requires direct access to physical hardware like a specific GPU architecture for specialized parallel processing, and the provider only offers that hardware via dedicated IaaS instances, the more abstracted computing models cannot be used regardless of configuration.

However, many constraints that initially appear to be disqualifiers can be resolved through configuration or adaptation. Most modern PaaS and FaaS providers allow for custom runtimes or the use of containers, which enables developers to package specific language versions or binaries that are not provided by default. In these instances, the compute model is not disqualified because the developer accepts a higher level of configuration responsibility to maintain the abstraction. While this provides wiggle room, the developer must manage parts of the environment they initially hoped to delegate to the provider.

The decision to adapt a system to a compute model (such as refactoring a long-running monolithic process into smaller, event-driven functions to fit FaaS execution limits) is a common architectural trade-off. While the system can be configured or rewritten to satisfy the requirements of a specific model, engineers must evaluate if the effort of adaptation outweighs the benefits of the model itself. If a team spends significant engineering effort forcing a complex application to run in a restricted environment, they may find that moving to a lower-abstraction model like IaaS actually provides a more stable and less complex path forward, even if it requires more operational maintenance.

#### Workload Characteristics

Once technical viability is established, the compute model must be matched to the workload’s resource consumption patterns. This involves analyzing the utilization baseline to see if the application processes requests at a consistent, steady state or remains idle for long periods. Scaling velocity is another critical factor. FaaS can scale near-instantaneously per request, whereas IaaS instances often take several minutes to initialize and pass health checks. Finally, execution duration must be considered, as many high-abstraction platforms impose strict timeouts that make them unsuitable for long-running batch jobs or complex data migrations.

### Engineering and Operational Overhead

Every infrastructure choice also carries an operational tax, which is the engineering time required to maintain the environment. This operational capacity can include tasks like operating system patching, security hardening, and load balancer configuration. While these are managed by the user in an IaaS model, PaaS and FaaS shift this work to the cloud provider. Engineering opportunity cost is often the deciding factor here because for every hour a developer spends managing a virtual machine, they are not writing application code. Consequently, teams often prioritize higher abstraction models to maximize their development velocity, especially when engineering headcount is limited.

### Economic Models: Fixed versus Variable Costs

Cost efficiency in the cloud is a measure of resource utilization rather than a simple price-per-hour calculation. Provisioned billing models, typical of IaaS and PaaS, require payment for a set amount of CPU and RAM regardless of whether the application is utilizing them, which is cost-effective for high, steady-state workloads. In contrast, consumption-based billing models like FaaS charge only for the duration of execution and the number of requests, making them ideal for intermittent workloads where a provisioned server would sit idle. A comprehensive evaluation must ultimately consider the total cost of ownership (TCO), which is the total expense associated with adopting, operating, and provisioning cloud services.

### Selection Matrix

The following selection matrix synthesizes the technical, operational, and economic criteria discussed in previous sections into a unified decision framework. A selection matrix is a criteria-based matrix that is used to compare and evaluate options. This tool allows engineers to compare IaaS, PaaS, and FaaS across the key dimensions that impact system architecture. By evaluating a workload against these variables, you can determine which model provides the necessary technical capabilities with the least amount of management friction.

| Selection Variable | IaaS | PaaS | FaaS |
| --------- | --------- | -------- | --------- |
| Technical Control | Full | Partial | Minimal |
| Typical Traffic Characteristic | Steady-State / High Volume | Predictable / Steady | Intermittent / Spiky |
| Scaling Velocity | Minutes | Seconds to Minutes | Milliseconds to Seconds |
| Operational Effort | High | Moderate | Low |
| Billing Model | Fixed | Fixed | Variable |
| Execution Limits | No default timeout | Configurable timeouts | Strict provider timeouts |

When interpreting this matrix, the primary trade-off is between granularity of control and developer velocity. IaaS offers the most granular control over the environment, which could be necessary for specialized technical requirements or strict security compliance, but it requires the most engineering time to maintain. Conversely, FaaS offers the highest velocity by abstracting away almost all infrastructure management, but it imposes strict limitations on execution time and environmental customization.

The matrix also highlights the financial implications of different traffic patterns. For a workload that runs consistently at 80% utilization, the provisioned billing of IaaS or PaaS is usually more efficient because the unit cost of compute is lower. For a workload that is idle 90% of the time, the consumption-based billing of FaaS prevents paying for unused capacity. However, as mentioned in the economic analysis, the cost of engineering hours to manage an IaaS instance must always be weighed against its lower direct cloud cost.

In practice, engineers use a matrix like this as a starting place. One way to approach the decision making around choosing a model is to look at starting development at the highest level of abstraction (FaaS or PaaS) that meets the application's basic requirements. We only move to a lower-abstraction model when we encounter a specific technical constraint that the higher-abstraction models cannot satisfy. This strategy minimizes the operational tax on the team while maintaining the flexibility to migrate to more complex models only when necessary. 

There are other ways to go about approaching this decision too. How this exact decision making process works depends on the team's capacity and business needs. 

### Multi-Model Architecture Integration

In production environments, it is rare for an entire system to utilize a single compute model. As systems grow in complexity, engineers decompose applications into smaller, specialized services that may each require a different level of abstraction. Using a multi-model integration strategy allows a team to optimize for cost, performance, and control simultaneously.

Consider an e-commerce platform. The core API and checkout service might run using PaaS. This provides a balance of steady-state performance and automated scaling without requiring the team to manage the underlying OS. However, the image processing service (which only runs when a vendor uploads a new product photo) is a perfect fit for FaaS. By using FaaS here, the company only pays for the few seconds it takes to resize an image, rather than paying for a server to sit idle for most of the day.

Simultaneously, the platform might rely on a legacy inventory database that has specific hardware requirements. Since this a requirement that PaaS and FaaS cannot satisfy, the database is hosted using IaaS. By mixing these models, the engineering team uses lower-abstraction infrastructure only where the technical requirements demand it.

The challenge of using multiple models for a system is managing the communication and security between these different layers. Engineers must ensure that a serverless function can securely access a VM database across the network (we will look at how this can be done in future lessons in this curriculum). The ability to architect and maintain these cross-model integrations is a core competency of a full-stack engineer.

## Summary

Choosing a compute model is one of the most consequential decisions a team makes, as it dictates the long-term operational burden, cost, and scalability of a system. Rather than relying on personal preference or industry trends, professional developers use an objective framework to evaluate infrastructure through the lens of these key considerations:
- **Technical Requirements**: Identifying constraints such as OS-level dependencies or specialized hardware, that may disqualify high-abstraction models.
- **Workload Characteristics**: Matching the compute model to the frequency and duration of traffic to ensure the system scales efficiently.
- **Operational Capacity**: Accounting for the cost of operating different models in the cloud and prioritizing developer velocity by delegating some responsibilities to the cloud provider whenever possible.
- **Cost Efficiency**: Balancing direct cloud expenditures against the total cost of ownership (TCO), including the cost of software developer efforts to maintain the infrastructure.

In modern production environments, this often results in a multi-model architecture, where different components of a system (such as APIs, background workers, and legacy databases) could be hosted on different compute tiers to optimize for performance and cost. Applying this decision-making process allows an engineer to build systems that are not only functional but are also sustainable and scalable in a professional environment.

## Check for Understanding

<!-- Question 1 -->
### !challenge
* type: multiple-choice
* id: 9d5c1a7a-59ae-4bc0-8476-93ee358c5223
* title: Choosing the Right Model

##### !question
A startup launches a new API using the FaaS model to save money while traffic is low. Six months later, the API is processing a constant, heavy stream of requests 24/7. The monthly cloud bill has become significantly higher than the cost of a standard virtual machine. According to the selection framework, what is the most likely reason for this?
##### !end-question

##### !options
a| The team failed to account for cold starts.
b| The workload shifted from intermittent to to a most steady flow making consumption-based billing less efficient than provisioned billing.
c| FaaS models have strict execution timeouts that increase costs over time.
d| The team is paying an operational tax that only applies to FaaS.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
FaaS is cost-effective for intermittent traffic because you scale to zero. However, for steady-state, high-volume workloads, the unit cost per request in FaaS is higher than the cost of pre-paying for fixed capacity (IaaS/PaaS). At this intersection point, migrating to a provisioned model reduces the cloud bill.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: multiple-choice
* id: b3b43e83-d15d-495c-a3cc-c76e25feb8de
* title: Choosing the Right Model

##### !question
A financial services team is building an application that requires a custom, proprietary encryption library that must be installed as a specific Linux kernel module. Which compute model should the team select?
##### !end-question

##### !options
a|FaaS, because it offers the highest developer velocity for security features.
b| PaaS, because the provider handles the runtime and security patches.
c| IaaS, because it provides the OS-level access required to install kernel-level dependencies.
d| Using multiple models, using FaaS for the library and PaaS for the database.
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
This is a technical requirement that must be met. FaaS and PaaS abstract away the OS and do not allow users to modify the kernel. Only IaaS provides the access and control over the OS necessary to install custom kernel modules or specialized system drivers.
#### !end-explanation 
### !end-challenge

<!-- Question 3 -->
### !challenge
* type: multiple-choice
* id: d71ad339-dae8-4e6f-b1c9-23ee4d94ea3b
* title: Choosing the Right Model

##### !question
A small team of three full-stack developers needs to ship a standard Node.js CRUD application with a tight deadline. They have no dedicated devOps or infrastructure engineers. Which approach best minimizes their efforts spent on operational overhead?
##### !end-question

##### !options
a| Using IaaS, so they have full control of the VMs to fix any bugs that arise in the OS.
b| Using PaaS, to delegate OS patching and runtime maintenance of their instances to the provider, allowing the team to focus on application code.
c| Using multiple models for the integration of five different FaaS functions to maximize granularity.
d| Building a custom container orchestration system on top of raw IaaS instances.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
In this scenario, the team's primary constraint is their operational capacity. PaaS reduces their responsibilities by managing the OS and scaling logic. While IaaS offers more control, the time spent on maintenance would detract from their ability to meet the product deadline.
#### !end-explanation 
### !end-challenge