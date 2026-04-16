# Problem Set: Observability, Reliability & Operations

## Lesson Review

Complete all questions below.

### !challenge
<!-- Question 1 -->
* type: multiple-choice
* id: 2a41d641-5262-4a3f-9cc4-0d0b17cca9f9
* title: Lesson Review

##### !question
A team is investigating a production incident. Their monitoring dashboard shows that error rates for their checkout API have spiked to 12% in the last ten minutes. They need to find out what the error is and which service in their distributed system is causing it. Which combination of signals should they use, and in what order?
##### !end-question

##### !options
a| Check traces first to see the overall error rate trend, then check metrics to identify the specific error message.
b| Check metrics and traces together, since logs are only useful for identifying errors in single-service systems.
c| Check logs first to see the error rate trend, then check traces to confirm the error is real.
d| Use the metrics alert as the starting point, then check logs to identify the specific error and its context, then check traces to pinpoint which service in the distributed system is responsible for the failure.

##### !end-options

##### !answer
d|
##### !end-answer

#### !explanation 
Metrics surface that something is wrong and provide the starting point for investigation. Logs reveal what the error actually is by capturing the specific context around individual events. Traces show how the request traveled through the distributed system and identify which service in the chain is responsible for the failure. Using all three signals in this sequence moves the team from detection to diagnosis to root cause as efficiently as possible.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: multiple-choice
* id: 0e571bc1-2afb-4940-9c17-30fcab06a216
* title: Lesson Review

##### !question
A team's average API response time is 95ms, which is well within their SLO target of 200ms. However, a small number of users are reporting that the application feels extremely slow. The team investigates and finds that their p99 response time is 8 seconds. Which of the following best explains this situation?
##### !end-question

##### !options
a| The SLO target of 200ms is too strict and should be relaxed to account for occasional slow responses.
b| The average response time is masking a small subset of requests that are taking significantly longer than the majority. The p99 measurement reveals that 1% of users are experiencing response times of 8 seconds, which the average alone would never surface.
c| The p99 response time of 8 seconds means the entire system is performing poorly and the average of 95ms must be incorrect.
d| The users reporting slow performance are likely experiencing issues with their own internet connection rather than the application itself.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Averages collapse many individual measurements into a single number and can hide the experience of users whose requests behave very differently from the majority. A p99 response time of 8 seconds means that 1% of requests are taking 8 seconds to complete, even though the average looks healthy. Percentiles surface these outliers and give teams a more accurate picture of how the system is actually performing for the full range of users.
#### !end-explanation 
### !end-challenge

<!-- Question # 3 -->
### !challenge
* type: checkbox
* id: 935bbcdf-4113-49af-9e1c-b8d8c70c4a11
* title: Lesson Review

##### !question
A team is evaluating the reliability of their cloud application. Which of the following are reliability indicators they should track? Select all that apply.
##### !end-question

##### !options
a| Mean Time Between Failures (MTBF)
b| The number of engineers on the team
c| Mean Time to Detect (MTTD)
d| Throughput
e| The size of the team's deployment pipeline
f| Error rate
##### !end-options

##### !answer
a|
c| 
d|
f|
##### !end-answer

#### !explanation 
MTBF measures how frequently failures occur, MTTD measures how quickly failures are detected, throughput measures how many requests the system can handle per unit of time, and error rate measures the percentage of requests that result in an error. Together these indicators give a team a layered view of how their system is performing and where reliability may be degrading. The number of engineers on the team and the size of the deployment pipeline are operational characteristics of the team rather than measurements of system reliability.
#### !end-explanation 
### !end-challenge

<!-- Question # 4 -->
### !challenge
* type: multiple-choice
* id: 33b832cd-bdf1-4322-a2e2-e9e1cc137742
* title: Lesson Review

##### !question
A payment processing service is operational and responding to requests 99.9% of the time. However, due to a bug introduced in a recent deployment, 4% of transactions are being processed incorrectly. Which of the following best describes this system?
##### !end-question

##### !options
a| The system is both reliable and available because it is responding to the vast majority of requests successfully.
b| The system is available but not reliable, because it is operational but not performing its intended function correctly and consistently.
c| The system is reliable but not available, because it is processing most transactions correctly despite occasional downtime.
d| The system is neither reliable nor available, because a 4% error rate means the system is effectively offline for those users.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Availability measures whether a system is operational and responding to requests. Reliability measures whether a system is performing its intended function correctly and consistently. This system is available because it is up and responding, but it is not reliable because it is processing 4% of transactions incorrectly. A system can meet its availability target while still failing its users in ways that availability metrics do not capture.
#### !end-explanation 
### !end-challenge

<!-- Question # 5 -->
### !challenge
* type: checkbox
* id: 5de9f1dd-48e5-472b-a9b4-fb7d803bde9c
* title: Lesson Review

##### !question
A small e-commerce startup is deploying an AIOps monitoring tool for the first time. Due to budget constraints, they cannot afford to monitor all of their services and must prioritize. Their system is made up of the following services:
- A product catalog service that displays available items to users
- A payment processing service that handles all financial transactions
- An order management service that tracks orders from placement to delivery
- An internal reporting dashboard used by the finance team to review monthly revenue summaries
- A promotional email service that sends discount codes to users once a week
- A customer authentication service that handles user login and session management

Select the four services the team should prioritize for AIOps monitoring given their budget constraints.
##### !end-question

##### !options
a| The product catalog service
b| The payment processing service
c| The order management service
d| The internal reporting dashboard
e| The promotional email service
f| The customer authentication service
##### !end-options

##### !answer
a|
b|
c|
f|
##### !end-answer

#### !explanation 
The payment processing service handles all financial transactions, meaning failures directly result in lost revenue and damaged user trust, making it the highest priority for monitoring. The order management service is central to the application's core function and failures here affect every order in the system. The customer authentication service is a dependency for most other user-facing actions, and a failure here would prevent users from accessing the application entirely. The product catalog service is the first point of contact for users browsing the application, and degraded performance here directly affects the user experience and the ability to drive purchases.
<br>  
The internal reporting dashboard and the promotional email service are lower priority because they serve internal users or run infrequently. Failures in these services do not directly affect the real-time experience of customers using the application, making them reasonable candidates to defer when budget is constrained.
#### !end-explanation 
### !end-challenge

## Case Study Review

Read the following scenario, then answer the questions below (~12 minutes). The full postmortem can be found [here](https://drive.google.com/file/d/1yG8qXNdMnHqXao3m8s2s7785UhLRUFVs/view?usp=sharing) ([source](https://blog.cloudflare.com/post-mortem-on-cloudflare-control-plane-and-analytics-outage/)).

In November 2023, Cloudflare experienced a significant outage affecting its control plane and analytics services. The outage lasted approximately 36 hours and was caused by a power failure at one of three data centers the company relied on in the Portland, Oregon area. 

Cloudflare had designed its systems with high availability in mind, distributing its services across all three facilities so that each one was actively handling traffic at the same time. The intention was that if any one facility went offline, the remaining two would continue to operate and users would not be affected. However, the investigation revealed that some newer services had not yet been migrated to the high availability cluster and remained dependent on the affected data center. Additionally, the company's observability tools were unable to detect that the data center had switched from utility power to generator power before the failure occurred, and the data center operator did not notify Cloudflare of the power source change.

<!-- Question # 1 -->
### !challenge
* type: multiple-choice
* id: a6a132a5-0688-41e1-b54e-45e9de2b52e3
* title: Case Study Review

##### !question
The postmortem notes that Cloudflare's observability tools were unable to detect that the data center had switched from utility power to generator power before the failure, and that the data center operator did not inform Cloudflare of the change. Which of the following best describes the observability concept this illustrates?
##### !end-question

##### !options
a| Alert fatigue, because Cloudflare's monitoring tools were firing too many alerts and the team missed the one that mattered.
b| A misleading average, because the power metrics appeared normal on average even though the underlying power source had changed.
c| A silent failure, because the change in power source represented a degraded and risky condition that did not trigger any alert, leaving the team unaware of the increased risk until the failure occurred.
d| A missing trace, because Cloudflare did not have distributed tracing configured for its power infrastructure.
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
A silent failure occurs when a real problem exists but no alert fires to notify the team. The switch from utility power to generator power was a meaningful change in the system's risk profile, but because no monitoring was in place to detect or alert on it, the team had no visibility into the degraded condition. This is a clear example of how gaps in observability coverage can leave teams blind to risks that are already present in their systems.
#### !end-explanation 
### !end-challenge

<!-- Question # 2 -->
### !challenge
* type: multiple-choice
* id: bf9cc68f-3363-4e59-944c-ff3934da5b5e
* title: Case Study Review

##### !question
The Cloudflare outage lasted approximately 36 hours from the initial failure to full restoration. During this time, the team had to manually coordinate across many services and teams to restore the system. Which reliability indicator does the length of this restoration period most directly reflect, and what reliability strategies could help reduce it in the future?
##### !end-question

##### !options
a| MTBF, because a longer restoration period means failures are occurring more frequently. Investing in load balancing would reduce how often failures happen.
b| Uptime, because the system was offline for 36 hours. Investing in more servers would ensure the system stays online longer.
c| MTTR, because the restoration period measures how long it took to recover from the failure. Investing in automated recovery and clearer recovery processes would help reduce this time in the future.
d| Error rate, because the 36-hour outage means that 100% of requests were failing during that period. Investing in better logging would help the team identify errors more quickly.
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
MTTR measures the average time it takes to restore a system to a healthy state after a failure occurs. The 36-hour restoration period is a direct reflection of MTTR. Automated recovery mechanisms reduce MTTR by starting the recovery process immediately when a failure is detected rather than waiting for human intervention. Clearer and well-practiced recovery processes also reduce MTTR by ensuring the team can coordinate and act quickly when a major failure occurs, rather than figuring out the process under pressure during an active incident.
#### !end-explanation 
### !end-challenge