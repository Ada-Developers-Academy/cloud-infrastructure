# Governance & Cost Driven Architectural Decision Making 

## Learning Goals

- Explain how architectural decisions in the cloud carry cost consequences and describe the trade-offs.
- Define right-sizing and explain why over-provisioning is a common and costly pattern, including how governance policies and cost optimization tools can encourage right-sizing as a practice.
- Compare reserved, pay-as-you-go, and spot pricing models and identify which workload characteristics make each model appropriate.
- Describe how organizations use multiple environments with different cost and governance expectations.

## Cost Implications of Architectural Choices

Every architectural decision made in a cloud environment carries a cost consequence. The choice of how to run a workload, how to structure a service, and how to allocate resources are not purely technical decisions. They determine what gets charged, how that charge is attributed, and who is accountable for it. Understanding these consequences is part of the responsibility of an engineer working in a cloud environment. This does not mean that cost is always the primary consideration. Performance, reliability, security, and developer experience are all legitimate factors in architectural decision making. The point is that cost belongs in that set of considerations from the beginning of the design process, not as an afterthought once a bill arrives.

The following examples illustrate how two common architectural decisions, each reasonable on its own terms, produce meaningfully different cost consequences depending on the workload and context.

### Paying for Capacity vs. Paying for Consumption

The difference between serverless and always-on compute is one of the clearest illustrations of how architectural choices translate directly into cost consequences. An always-on server runs _continuously regardless of whether it is handling requests_. A server provisioned to handle peak traffic at 10,000 requests per hour accrues the same cost at 2am when traffic drops to 50 requests per hour. A serverless function, by contrast, _executes only when invoked and scales to zero when idle_. The cost model shifts from paying for capacity to paying for consumption.

| Characteristic | Always-On Server | Serverless Function | 
| --------- | --------- | -------- | 
| Billing Model | Continuous, regardless of usage | Per invocation and execution duration |
| Cost at Zero Traffic | Full cost accrues | No cost |
| Cost at Peak Traffic | Fixed | Scales with demand |
| Predictability | High | Variable |

Neither model is categorically better. An always-on server offers predictable costs and consistent performance characteristics, which may be appropriate for workloads with steady, high-volume traffic. A serverless function eliminates idle cost and scales automatically, but introduces trade-offs around execution time limits, cold start latency, and the complexity of debugging distributed function invocations. A workload with highly variable traffic and tolerance for occasional latency may be well suited to serverless. A workload requiring consistently low latency and continuous availability may not be. The decision is never purely technical. Cost structure, traffic patterns, team familiarity with the model, and organizational accountability all shape which approach is appropriate in a given context.

### How Processing Architecture Affects Resource Utilization

The choice between synchronous and asynchronous processing is another example of how an architectural decision produces distinct cost consequences. In a synchronous architecture, a request is received, processed, and responded to in a single uninterrupted flow. While the processing is underway, the compute resources handling that request remain occupied and unavailable for other work. If a request requires calling an external service or waiting on a slow database query, those resources are held idle during that wait time. Handling a high volume of concurrent requests in this model requires a proportionally large pool of compute resources to be available at all times.

In an asynchronous architecture, incoming requests are acknowledged immediately and processed separately, rather than handled in a single uninterrupted flow. A separate set of workers processes items from the queue as capacity allows. Since workers are only occupied when actively processing, the same volume of work can often be handled with fewer concurrently running resources.

| Characteristic | Synchronous | Asynchronous | 
| --------- | --------- | -------- | 
| Resource Utilization| Resources held during wait time | Resources used only during active processing |
| Concurrency Requirement | High, proportional to concurrent requests | Lower, decoupled from request rate |
| Cost Profile | Higher at peak demand | More efficient under variable load |
| Complexity | Lower | Higher, requires queue management |

As with serverless vs. always-on, neither model is universally preferable. Synchronous processing is simpler to implement and reason about, and appropriate for workloads where low latency responses are required. Asynchronous processing introduces operational complexity around queue management, error handling, and the additional infrastructure required to support them, but can reduce the compute capacity required to handle the same workload. The cost efficiency of asynchronous processing comes with a trade-off in architectural complexity that must be weighed against the savings it produces.

### Architectural Decisions and Accountability

Architectural choices also determine how costs are attributed and who is responsible for them. A service that shares infrastructure with other services makes cost allocation more complex. A service with clearly bounded resources and consistent tagging makes it straightforward to attribute costs to the team that owns it.

When engineers make architectural decisions without considering how costs will be tracked and attributed, the result is often an environment where spending is visible in aggregate but opaque at the level of individual teams or products. Building with cost accountability in mind, by structuring resources so that ownership is clear and tagging is applied consistently, is as much a part of sound architecture as performance or reliability considerations.

## Right-Sizing Resources

**Right-sizing** is the practice of provisioning cloud resources at a capacity that matches actual demand rather than worst-case assumptions. A right-sized resource has enough capacity to handle its workload reliably without carrying significant unused capacity that generates cost without providing value.

### Over-Provisioning

**Over-provisioning** is one of the most common and costly patterns in cloud infrastructure. It occurs when resources are provisioned at a capacity significantly greater than what the workload requires. This happens for understandable reasons. When engineers are uncertain about future demand, provisioning generously feels like the safer choice. In a physical infrastructure context, over-provisioning was often necessary because adding capacity later was slow and operationally complex. That reasoning does not translate in cloud environments, where capacity can typically be adjusted more quickly, but the habit persists.

The cost consequence of over-provisioning is straightforward. A compute instance running at 10% of its available capacity is paying for the other 90% regardless. Across a large environment with many over-provisioned resources, that waste accumulates significantly. Unlike a one-time procurement expense, over-provisioning in the cloud generates ongoing charges for as long as the resource remains at its current size.

### Under-Provisioning

**Under-provisioning** carries the opposite risk. A resource provisioned below the capacity its workload requires will degrade under load, producing slower response times, failed requests, or service outages. The cost of under-provisioning is not always visible on a cloud bill, but it can be significant in terms of user impact, engineering time spent investigating performance issues, and the operational cost of resolving incidents. Right-sizing requires finding the appropriate capacity in both directions, not simply minimizing resource size.

### Governance and Right-Sizing

Left entirely to individual discretion, right-sizing decisions are often deprioritized. Engineers working under time pressure tend to provision generously and move on, with the intention of revisiting capacity later. That revisit frequently does not happen without an organizational prompt. Governance policies can make right-sizing a more consistent practice by establishing expectations around resource provisioning. A policy might require that compute resources above a certain size include documented justification, or that resources flagged as over-provisioned by a cost optimization tool are reviewed within a defined time period. These policies do not eliminate engineering judgment but create the organizational conditions in which right-sizing decisions are made deliberately rather than deferred indefinitely.

### Cost Optimization Tools and Right-Sizing

Cost optimization tools, introduced in the lesson about [Core Principles of Governance & Cost](./principles-of-governance-and-cost.md), are particularly well suited to identifying right-sizing opportunities. By analyzing resource utilization over time, these tools can identify compute instances, database configurations, and other resources that are consistently operating well below their provisioned capacity and surface recommendations for more appropriate sizes.

These recommendations are a starting point rather than a directive. A resource running at low average utilization may still require its current capacity to handle periodic traffic spikes. Evaluating a right-sizing recommendation requires understanding the workload's traffic patterns, performance requirements, and tolerance for capacity constraints. The tool identifies the opportunity; the engineer determines whether acting on it is appropriate.

## Pricing Models

The pricing model applied to a cloud resource determines how that resource is billed and, by extension, how predictable and controllable its cost is. Selecting an appropriate pricing model is an architectural decision with direct cost consequences. The right choice depends on the nature of the workload, its traffic patterns, and the organization's tolerance for operational risk. There are three primary pricing models available for cloud compute resources.

### Pay-As-You-Go

**Pay-as-you-go (PAYG)** is the default pricing model for most cloud resources. Resources are billed at a standard rate for the duration they are running, with no upfront commitment required. PAYG offers maximum flexibility: resources can be provisioned and decommissioned at any time without financial penalty.

This flexibility comes at a cost. PAYG is typically the most expensive pricing model on a per-unit basis. For workloads with unpredictable or highly variable demand, or for resources that are only needed temporarily, PAYG is often the appropriate choice. For workloads with stable, predictable demand that run continuously, paying the PAYG rate indefinitely is rarely the most cost-efficient option.

### Reserved Instances

**Reserved instances** involve a commitment to use a specific resource for a fixed term, typically one or three years, in exchange for a significant discount compared to PAYG rates. The discount reflects the predictability that the commitment provides to the cloud provider.

Reserved instances are well suited to workloads with stable, predictable demand that are expected to run continuously over the commitment period. A production web application with consistent baseline traffic, a database that is always running, or a batch processing system that runs on a fixed schedule are all reasonable candidates for reserved pricing.

The risk of reserved instances is the commitment itself. If the workload changes significantly, if the resource is no longer needed, or if the team overestimates how much capacity it will require, the organization may be paying for capacity it is not using. Reserved instances require a degree of confidence in future demand that is not always available, particularly for newer workloads or rapidly evolving systems.

### Spot Instances

**Spot instances** make unused cloud capacity available at a substantial discount compared to PAYG rates. The trade-off is that spot instances can be reclaimed by the cloud provider with limited notice when that capacity is needed elsewhere. A workload running on spot instances may be interrupted mid-execution.

This makes spot instances inappropriate for most production workloads where continuity and availability are required. They are well suited to workloads that are designed to tolerate interruption: batch processing jobs that can save their progress and resume from where they left off if interrupted, data pipeline tasks that can be retried if interrupted, or background processing that is not time-sensitive. Using spot instances effectively requires that the workload architecture explicitly accounts for the possibility of interruption.

### Choosing the Right Model

In practice, organizations often use a combination of pricing models across their infrastructure. A production service might run on reserved instances for its baseline capacity, with PAYG instances handling traffic spikes beyond that baseline. Background processing jobs might run on spot instances where interruption is acceptable. The decision for each workload should be based on its demand characteristics, availability requirements, and the organization's confidence in its capacity forecasts.

Pricing model decisions also have governance implications. Committing to reserved instances represents a financial obligation that typically requires organizational approval. A governance framework may define thresholds above which reserved instance commitments require review, ensuring that long-term financial commitments are made deliberately rather than by individual engineers acting without organizational visibility.

## Environments: Cost and Control Trade-offs

Most organizations run their applications across multiple environments. Each environment serves a distinct purpose in the software development lifecycle, and each carries different expectations around availability, governance controls, and cost. Understanding these differences is relevant not only to operations teams but to any engineer making decisions about how and where to deploy resources.

### Production

The **production environment** is where live user traffic is served. It demands the highest level of availability, reliability, and security of any environment in the organization. Resources in production are typically provisioned for high availability, meaning redundant components are in place to ensure the service remains operational if individual resources fail. Access controls in production are strict: the set of people and systems permitted to make changes is deliberately limited to reduce the risk of accidental or unauthorized modification.

These requirements carry a cost. High availability architectures require running multiple instances of the same resource rather than one, which multiplies the baseline cost. Strict access controls and audit logging add operational overhead. The cost of running production infrastructure is generally accepted as a necessary expense, but it should still be subject to governance practices around ownership, tagging, and right-sizing. High cost expectations do not mean unlimited spending.

### Staging

The **staging environment** is intended to closely replicate production conditions for the purpose of testing changes before they are deployed to users. A staging environment that accurately reflects production helps catch issues that would not surface in a less representative environment. This fidelity has a cost implication: staging environments are often provisioned at a scale closer to production than to development, making them one of the more significant non-production cost centers in a cloud environment.

Because staging exists to validate changes rather than serve live traffic, it does not require the same continuous availability as production. Staging environments can often be scaled down or shut down outside of active testing periods, which is a common and effective cost reduction practice. Governance policies can enforce this by defining expected uptime windows for staging resources and flagging or automatically stopping resources that remain running outside of those windows.

### Development

**Development environments** are used by engineers for active feature work, experimentation, and debugging. They are the most permissive of the three environments in terms of access and configuration, since engineers need the freedom to provision, modify, and tear down resources quickly as part of their work. However, this permissiveness makes development environments a frequent source of uncontrolled spending if guardrails are not in place.

Resources provisioned for a specific experiment or feature branch may remain running long after the work is complete. Development environments are often populated with oversized resources because the cost implications feel less significant than in production. Over time, the aggregate cost of many small, forgotten development resources can become substantial. Governance policies applied to development environments typically focus on enforcing resource lifecycle expectations, such as automatic decommission of resources that have been idle for a defined period, and spending limits that trigger alerts when a development environment exceeds a cost threshold.

### Enforcing Differences Between Environments Through Governance

The different cost and control expectations across environments do not enforce themselves. Without deliberate governance policies, environments tend to converge toward the path of least resistance: engineers apply production-like resource sizes everywhere because it is easier than calibrating each environment individually, or development environments accumulate resources because there is no process prompting cleanup.

Governance policies can codify the differences between environments by defining which resource configurations are permitted in each, what access controls apply, and what spending thresholds trigger review. These policies ensure that the distinctions between environments are maintained consistently rather than depending on individual engineers to remember and apply them.

Tagging strategies support this by making environment attribution explicit on every resource. A required environment tag, with defined acceptable values such as `production`, `staging`, and `development`, ensures that every resource can be identified as belonging to a specific environment. This makes it possible to apply environment-specific policies reliably, produce cost reports broken down by environment, and identify resources that may have been provisioned in the wrong environment or left running after their purpose was served.

### The Cost of Treating All Environments the Same

When organizations fail to differentiate cost and governance expectations across environments, the consequences are predictable. A development environment provisioned with production-scale resources generates production-scale costs without production-level justification. A staging environment left running continuously at full scale through periods of low activity accrues charges that a scheduled shutdown policy would have avoided. A permissive production environment where many engineers have broad access increases the risk of accidental or unauthorized changes that carry both operational and financial consequences.

Environment differentiation is not only a governance best practice. It is one of the more straightforward opportunities to reduce cloud spending without affecting the quality or reliability of production systems.

## Summary

Architectural decisions in the cloud are never purely technical. Every choice about how to run a workload, how to size a resource, which pricing model to apply, and how to structure environments will carry cost and governance consequences that affect the organization as a whole. The decision between always-on and serverless compute, or between synchronous and asynchronous processing, determines not only how a system behaves but how it is billed and who is accountable for that cost. 

Right-sizing ensures that resources are provisioned to match actual demand rather than worst-case assumptions, and governance policies create the organizational conditions in which right-sizing is practiced consistently rather than deferred. 

Pricing model selection, whether PAYG, reserved, or spot, requires matching the commitment level of the purchasing decision to the predictability and risk tolerance of the workload. Environment differentiation ensures that the cost and governance controls applied to production, staging, and development reflect the distinct purpose and risk profile of each, rather than defaulting to a single configuration across all contexts. 

## Check for Understanding

### !challenge
<!-- Question 1 -->
* type: multiple-choice
* id: 4d3e31872-ac94-4678-9737-6d75f41fbc6a
* title: Governance & Cost Driven Architectural Decision Making 

##### !question
Which of the following workload characteristics makes a reserved instance the most appropriate pricing model?
##### !end-question

##### !options
a| A workload with highly variable traffic that scales up and down throughout the day
b| A workload that runs continuously with stable, predictable demand over a long period
c| A workload that can be interrupted and restarted without affecting the end result
d| A workload that is being used temporarily for a short-term experiment

##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Reserved instances are most appropriate when a workload runs continuously and its demand is stable and predictable enough to justify a long-term commitment. The discount that reserved instances provide reflects that predictability. A workload with variable traffic is better served by PAYG, which offers flexibility without commitment. A workload that tolerates interruption is a candidate for spot instances. A short-term experiment does not warrant a long-term commitment and is best run on PAYG.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: multiple-choice
* id: 03c0bcd1-e2bf-4a6f-9d3c-00f63b6147d1
* title: Governance & Cost Driven Architectural Decision Making 

##### !question
A team is running three environments: production, staging, and development. At the end of the month, the cloud bill is higher than expected. An audit reveals that several large compute instances in the development environment have been running continuously for two months, long after the feature work they supported was completed. Which of the following governance practices would most directly prevent this situation in the future?
##### !end-question

##### !options
a| Requiring all production resources to use reserved instance pricing
b| Applying stricter access controls to the production environment
c| Enforcing a policy that automatically flags or stops idle development resources after a defined period
d| Requiring development environments to use the same resource sizes as production for consistency
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
The situation describes idle resources in a development environment accumulating cost without providing value. A governance policy that automatically flags or stops resources that have been idle beyond a defined threshold directly addresses this pattern by creating an organizational prompt for cleanup rather than relying on individual engineers to remember to decommission resources they no longer need. Applying reserved instance pricing or stricter production access controls does not address the root cause, and requiring development environments to match production resource sizes would increase costs further rather than reduce them.
#### !end-explanation 
### !end-challenge

<!-- Question # 3 -->
### !challenge
* type: multiple-choice
* id: 7464175e-23e1-47cc-bc6b-7e5cb445afaf
* title: Governance & Cost Driven Architectural Decision Making 

##### !question
A startup is building a new application and expects traffic to be highly unpredictable in the first six months as they grow their user base. A senior engineer suggests committing to reserved instances immediately to reduce costs. A second engineer argues that PAYG is more appropriate for this stage. Which position is better supported by the principles covered in this lesson?
##### !end-question

##### !options
a| The senior engineer, because reserved instances always provide a lower cost per unit than PAYG
b| The second engineer, because PAYG is always the safest choice regardless of workload characteristics
c| The second engineer, because reserved instances require confidence in future demand that an early-stage, unpredictable workload does not yet support
d| The senior engineer, because reducing cost early is always the highest priority for a startup
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
Reserved instances offer a significant discount in exchange for a long-term commitment. That commitment only makes financial sense when there is sufficient confidence in future demand to justify it. An early-stage application with highly unpredictable traffic does not yet have the usage history or demand stability to make that commitment confidently. Committing to reserved instances under these conditions risks paying for capacity that turns out to be unnecessary or incorrectly sized. PAYG preserves flexibility during a period of uncertainty, and the decision to move to reserved pricing can be revisited once demand patterns become more predictable. Neither PAYG nor reserved instances are categorically better in all situations — the choice depends on the workload characteristics.
#### !end-explanation 
### !end-challenge