# Problem Set: Governance & Cost

## Directions

Complete all questions below.

## Practice

### !challenge
<!-- Question 1 -->
* type: multiple-choice
* id: f52aaaf2-4913-49df-bebd-739632dab6fa
* title: Lesson Review

##### !question
An organization notices that its monthly cloud bill has increased significantly over the past three months, but cannot determine which teams or projects are responsible for the increase. Which of the following is the most likely root cause?
##### !end-question

##### !options
a| The organization is using pay-as-you-go pricing instead of reserved instances
b| Resources have not been consistently tagged with cost allocation metadata
c| The organization does not have a billing dashboard configured
d| Engineers have been provisioning resources in the wrong environments
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Billing visibility without cost allocation provides only aggregate spending data. Without consistent tagging that attributes resources to teams, projects, or cost centers, it is impossible to break down a cloud bill in a way that identifies who is responsible for which portion of the spend. A billing dashboard can show total costs but cannot produce a meaningful breakdown without the metadata that tags provide.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: checkbox
* id: ca95d375-270c-4592-9d4c-e1e3a35e2a27
* title: Lesson Review

##### !question
Which of the following are consequences of over-provisioning cloud resources? Select all that apply.
##### !end-question

##### !options
a| Resources accrue cost for unused capacity on an ongoing basis
b| Services are more likely to experience reliability issues under load
c| Cost optimization tools may flag the resources as right-sizing candidates
d| Engineers are unable to deploy new resources until existing ones are decommissioned
e| The financial impact accumulates for as long as the resource remains over-provisioned
##### !end-options

##### !answer
a|
c|
e|
##### !end-answer

#### !explanation 
Over-provisioning means paying for capacity that is not being used, which generates ongoing charges for as long as the resource remains at its current size. Cost optimization tools analyze utilization patterns and surface over-provisioned resources as right-sizing candidates. The second option (Services are more likely to experience reliability issues under load), describes under-provisioning, not over-provisioning. The fourth option, "Engineers are unable to deploy new resources until existing ones are decommissioned", is not a consequence of over-provisioning because cloud environments do not typically prevent new resource creation based on existing resource utilization.
#### !end-explanation 
### !end-challenge

<!-- Question # 3 -->
### !challenge
* type: multiple-choice
* id: 4d3be386-8a8b-4718-ba46-f153b6a89a67
* title: Lesson Review

##### !question
A team is running a nightly batch processing job that transforms raw data and writes results to a data warehouse. The job runs for approximately three hours, has run reliably on the same schedule for eight months, and the team has no plans to change the workload. Which pricing model is most appropriate?
##### !end-question

##### !options
a| Pay-as-you-go, because it offers the most flexibility
b| Spot instances, because batch jobs are well suited to interruptible compute
c| Reserved instances, because the workload is stable, predictable, and long-running
d| A combination of reserved and spot instances for maximum cost reduction
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
Reserved instances are most appropriate when a workload has stable, predictable demand and is expected to run continuously over a long period. Eight months of consistent usage and no anticipated changes provides the confidence needed to justify a commitment-based pricing model, which offers significant discounts compared to pay-as-you-go. Spot instances are a reasonable consideration for batch jobs that can tolerate interruption, but the question does not indicate that the job is designed to handle interruption gracefully. Pay-as-you-go would be more expensive for a workload this predictable.
#### !end-explanation 
### !end-challenge

<!-- Question # 4 -->
### !challenge
* type: checkbox
* id: 284d9637-4b02-44e2-9052-2fff4b7c1d8f
* title: Lesson Review

##### !question
Which of the following describe characteristics of an effective tagging strategy? Select all that apply.
##### !end-question

##### !options
a| Tags are applied consistently across every resource in the environment
b| The strategy requires as many tags as possible to maximize data collection
c| Tag keys and acceptable values are defined and communicated in advance
d| Tags enable costs to be attributed to the teams or projects responsible for them
e| The strategy is reviewed and updated as the organization's structure and needs evolve
##### !end-options

##### !answer
a|
c|
d|
e|
##### !end-answer

#### !explanation 
An effective tagging strategy is one that is applied consistently, defined clearly in advance, used to support cost allocation and ownership tracking, and maintained as the organization changes. The second option ("The strategy requires as many tags as possible to maximize data collection") is incorrect because comprehensiveness is not inherently valuable if it comes at the expense of consistency. A smaller set of well-chosen tags applied reliably across every resource provides more value than an extensive set applied sporadically.
#### !end-explanation 
### !end-challenge

<!-- Question # 5 -->
### !challenge
* type: multiple-choice
* id: 38a63d4d-9071-49ed-b5c2-f8b342a92f79
* title: Lesson Review

##### !question
A governance-heavy organization requires manual sign-off from a senior engineer before any cloud resource can be provisioned. Over time, engineers begin under-specifying resource requests to avoid triggering additional review steps, then quietly expanding the resources after provisioning. Which of the following best describes what this behavior illustrates?
##### !end-question

##### !options
a| Engineers are acting in bad faith and need stricter controls to correct the behavior
b| The governance process is functioning as intended by making engineers more deliberate about their resource requests
c| The organization needs more human reviewers to reduce the wait time for approvals
d| Governance controls that function as gates can produce outcomes worse than less rigid governance by motivating engineers to work around them
##### !end-options

##### !answer
d|
##### !end-answer

#### !explanation 
Under-specifying resource requests to avoid review and then expanding them afterward is a workaround that undermines the governance control entirely. The engineers are still provisioning the resources they need, but they are routing around the process to do it. This is one of the core failure modes of governance-heavy cultures when governance is experienced primarily as friction, teams find ways around it rather than engaging with it. 
#### !end-explanation 
### !end-challenge

## Case Study Review

Read the following scenario, then answer the questions below (~5 minutes). The full blog post can be found [here](https://drive.google.com/file/d/1ZGI2zb77oLBVCcbqw8hlckHZVL--dZ1W/view?usp=sharing) ([source](https://blog.duolingo.com/reducing-cloud-spending/)).

Duolingo is a language-learning platform used by hundreds of millions of people worldwide. By early 2024, the company's cloud costs had grown to millions of dollars a year — a direct consequence of years of rapid feature development on AWS. At the start of 2024, engineering leadership issued a company-wide initiative: reduce cloud spending without compromising the product. Dozens of engineers contributed to the effort, and the company achieved 20% annualized savings within a few months.

The investigation began with a visibility problem. The team needed easy access to data that answered the question: how much are we spending, and on what? Using a third-party cost intelligence tool, engineers broke down cloud costs into queryable line items and began identifying anomalies. One early discovery illustrated a common governance failure: a staging environment for one service had been scaled up by an engineer for testing purposes and never scaled back down. As a result, its staging costs exceeded its production costs.

Further investigation uncovered a significant amount of waste from resources that were no longer needed. Engineers found ancient database clusters, entire databases, and at least one complete microservice that were still running and accruing cost despite belonging to legacy features that had been deprecated. The engineers noted that if resource owners had known the literal cost of their technical debt, they might have made different decisions.

Right-sizing was another major opportunity. Most services were over-provisioned relative to their actual demand. One service was running at such low CPU utilization relative to its provisioned task count that optimizing its task allocation alone saved hundreds of thousands of dollars a year. The team also discovered that many services were provisioned for worst-case memory usage rather than typical usage, and that almost all services could run comfortably at 90 to 95 percent of their allocated memory without any reliability impact.

A refined reserved instance strategy contributed additional savings. By tracking usage and allocation across compute resources, the team gained visibility into their baseline capacity needs and used that data to make more informed bulk purchasing commitments.

The team's conclusion was that cost optimization and code health are closely related. Many of the savings came directly from cleaning up technical debt and simplifying complex systems. The engineers recommended embedding cost awareness into engineering culture: including cost estimations in technical specifications, sending weekly cost reports to teams, and conducting quarterly reviews of spending trends.

<!-- Question # 6 -->
### !challenge
* type: multiple-choice
* id: b8e24d73-308e-4e8e-9de3-f7396078ffe1
* title: Case Study Review

##### !question
Early in Duolingo's cost reduction effort, engineers discovered that a staging environment for one service cost more than its production environment. Which of the following best explains how this situation was able to persist without anyone noticing?
##### !end-question

##### !options
a| Staging environments are always more expensive than production environments because they require more compute capacity for testing
b| The team lacked sufficient billing visibility to surface anomalies at the service and environment level before costs were reviewed in detail
c| The engineer who scaled up the staging environment deliberately concealed the change from the rest of the team
d| Production costs had decreased significantly, making staging costs appear higher by comparison
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
The Duolingo post describes the first step of their cost reduction effort as establishing observability so that they could gain easy access to data that answered where money was going and how it was changing over time. The staging environment anomaly was only discovered once the team had tooling in place to break down costs at a granular level. Without that visibility, the overscaled staging environment continued to accrue cost unnoticed. 
#### !end-explanation 
### !end-challenge

<!-- Question # 7 -->
### !challenge
* type: checkbox
* id: efbf08ff-0c87-48b0-8bc7-da19dae1ee62
* title: Case Study Review

##### !question
Duolingo's investigation uncovered several categories of waste. Which of the following practices, if implemented before the cost reduction effort began, would most directly have prevented the waste described in the case study? Select all that apply.
##### !end-question

##### !options
a| Regularly reviewing cloud costs at the team and service level and making that data accessible to all engineers
b| Establishing resource lifecycle policies that flag or decommission idle and legacy resources after a defined period
c| Integrating cost impact estimates into infrastructure planning workflows so engineers can see the cost implications of their decisions
d| Switching all compute resources to spot instances to reduce baseline costs
e| Right-sizing compute resources based on actual utilization patterns rather than worst-case assumptions
##### !end-options

##### !answer
a|
b|
c|
e|
##### !end-answer

#### !explanation 
The waste Duolingo found fell into three categories: resources that were no longer needed (legacy clusters, deprecated services, overscaled staging environments), resources that were over-provisioned relative to actual demand, and costs that engineers were not aware of because cost data was not visible or accessible. The first option addresses visibility. The second option addresses idle and legacy resource accumulation. The third option addresses cost-aware decision making by making cost implications visible to engineers as part of their normal planning process, rather than something discovered after the fact on a bill. The fifth option addresses over-provisioning. The fourth option is incorrect — switching all resources to spot instances is not appropriate for all workloads, particularly production services requiring high availability, and does not address the root causes of the waste described in the case study.
#### !end-explanation 
### !end-challenge

<!-- Question # 8 -->
### !challenge
* type: multiple-choice
* id: 2c7ad9be-5d19-4b59-9ea0-0428cc99dd07
* title: Case Study Review

##### !question
Duolingo found that one service was running at significantly lower CPU utilization than its provisioned task count warranted. Optimizing its task allocation saved hundreds of thousands of dollars a year. Which of the following best describes why this situation is a common pattern in cloud environments, according to the principles covered in this topic?
##### !end-question

##### !options
a| Cloud providers intentionally over-provision resources to ensure reliability, which engineers cannot control
b| Engineers tend to provision for worst-case demand rather than typical demand, and without visibility into utilization data there is no prompt to revisit those decisions
c| Reserved instance commitments prevent engineers from reducing resource sizes once they have been provisioned
d| Over-provisioning is only a problem at the scale Duolingo operates and does not affect smaller organizations
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Over-provisioning is one of the most common and costly patterns in cloud infrastructure, and it occurs for understandable reasons. When engineers are uncertain about future demand, provisioning generously feels like the safer choice. Without visibility into utilization data over time, and without a governance practice or organizational prompt to review capacity decisions, initial allocations tend to persist indefinitely. Duolingo's experience illustrates exactly this pattern: a service provisioned more generously than necessary continued to run at that allocation until engineers specifically looked for over-provisioning opportunities.
 #### !end-explanation 
### !end-challenge