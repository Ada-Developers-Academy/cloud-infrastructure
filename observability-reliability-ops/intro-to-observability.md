# Introduction to Observability

## Learning Goals
- Distinguish between a development environment and a production environment and explain why production systems require a different approach to understanding system behavior.
- Explain what observability is and describe the problem it solves in production environments.
- Define metrics, logs, and traces and describe the type of question each signal is best suited to answer.
- Explain why all three signals are necessary and what gaps exist when any one of them is missing.
- Identify common ways that observability signals can mislead, including alert fatigue, silent failures, and misleading averages.

### What Is Observability and Why Does It Matter?

When a developer writes and runs code locally, they have direct access to everything happening inside the application. They can add print statements to inspect the value of a variable, pause execution with a debugger, restart the process in seconds, and observe the results immediately. If something goes wrong, the feedback loop is short and the tools for investigating the problem are close at hand.

Production is a fundamentally different environment. In production, code runs on remote infrastructure managed by a cloud provider, serves real users, and cannot be paused or restarted without consequences. A developer cannot attach a debugger to a running production server or add a print statement and wait for the output. The code is running somewhere else, handling live traffic, and the team observing it from the outside has no direct window into what it is doing at any given moment.

This gap between development and production is not a flaw in how teams work. It is a structural reality of how modern software is deployed. Code that runs on a developer's laptop eventually has to run on shared infrastructure at scale, and the tools that work in development do not follow it there. When a production system behaves unexpectedly, a team faces two distinct challenges. The first is detecting that something is wrong. The second is understanding why. These are different problems, and solving the first does not automatically solve the second.

A team might notice that response times have increased or that users are reporting errors, but knowing that something is wrong is only the beginning. Is the problem in the application code? A database? A third-party service the application depends on? A network issue between two services? In a modern cloud application made up of many services running across many servers, a single user request might touch dozens of components before a response is returned. Without a way to see inside that system, the team is left guessing.

**Observability** is the ability to understand the internal state of a system by examining the data it produces. An observable system does not require a developer to log in to a server and poke around to understand what is happening. Instead, the system continuously emits data about its own behavior, and teams use that data to detect problems, investigate their causes, and understand how the system is performing over time.

When something goes wrong in a production system, what the team first notices is a symptom. A **symptom** is an observable sign that something is not working as expected. Slow response times, elevated error rates, and failed requests are all symptoms. They tell you that something is wrong, but they do not tell you what caused it or where in the system to look.

To investigate a symptom, teams rely on signals. A **signal** is a stream of data that a system continuously produces about its own behavior. Signals are the raw material of observability. They are what allow a team to move from "something is wrong" to "here is what is wrong, here is where it is happening, and here is why."

### The Three Pillars of Observability

When a production system behaves unexpectedly, a team needs data to understand what is happening. That data comes in three distinct forms: **metrics**, **logs**, and **traces**. Each one answers a different kind of question, and no single signal gives a complete picture on its own. Teams use all three together to detect problems, investigate their causes, and understand how a request traveled through a system.

#### Metrics

A **metric** is a numerical measurement collected over time that describes the state or behavior of a system. Metrics are typically collected at regular intervals and tracked as a series of data points that can be graphed, compared, and analyzed over time. Common examples of metrics include the number of requests a server is handling per second, the percentage of those requests that result in an error, how much CPU or memory a service is consuming, and how long requests are taking to complete on average. These numbers give a team a high-level view of how a system is performing at any given moment and how that performance changes over time.

Metrics are well suited to detecting that something is wrong. When error rates spike or response times climb, metrics surface that change quickly and can trigger an alert that notifies the team. They are also useful for tracking trends, for example noticing that memory consumption has been climbing steadily over the past week, which might indicate a memory leak before it causes a failure. What metrics cannot tell you is _why_ something is wrong. A spike in error rate tells you that errors are happening, but it does not tell you what the error is, which part of the code is producing it, or which service in a distributed system is responsible. 

In practice, production systems can expose hundreds or even thousands of different metrics. Knowing which ones to pay attention to is a challenge in itself. A widely used framework in the industry offers a practical starting point: the four golden signals. They were introduced in Google's Site Reliability Engineering book as a way to identify the metrics that matter most in any production system. The idea is that regardless of how complex a system is, four measurements give a team a meaningful baseline view of how it is behaving. These four golden signals are: 

- **Latency** is the time it takes to serve a request. Tracking latency helps a team understand whether the system is responding quickly enough for users and whether performance is degrading over time.
- **Traffic** is the amount of demand being placed on the system, typically measured as the number of requests per second. Traffic provides context for other signals. For example, a spike in errors means something different if traffic has also tripled than if traffic has remained constant.
- **Errors** are the rate at which requests are failing. This includes explicit failures such as HTTP 500 responses as well as implicit failures such as requests that return a successful response but with incorrect data.
- **Saturation** is a measure of how "full" or constrained a system's resources are. A server whose CPU is running at 95% capacity is saturated, meaning it has little room to absorb additional load. Saturation is often a leading indicator of problems because a system approaching its limits is more likely to fail under a sudden spike in traffic.

These four signals do not replace other metrics, but they provide a consistent and practical framework for understanding the health of a system at a glance. A team that monitors latency, traffic, errors, and saturation has a solid foundation for detecting when something is wrong, even before they have built out more detailed monitoring for their specific application.

### !callout-info

## Monitoring and Metrics

**Monitoring** is the practice of continuously watching the signals a system produces and responding when something crosses a defined threshold. Metrics are the signal most commonly associated with monitoring because they produce a continuous stream of numerical data that can be watched and compared against thresholds over time. When a metric crosses a threshold, an alert fires and notifies the team. Monitoring is what teams *do* with observability data. Observability gives you the signals. Monitoring is the act of watching them and deciding when to act. We will return to monitoring in the next lesson when we look at how teams use it to measure and maintain the reliability of their systems.

### !end-callout

#### Logs

A **log** is a timestamped record of a discrete event that occurred within a system. Every time something meaningful happens inside an application, the application can write a log entry describing what happened, when it happened, and any relevant context. Common examples of log entries include:
- A record of a user logging in
- A database query that was executed
- An error message with a stack trace
- A payment transaction that was processed
- A request that was rejected because the user was not authorized

Logs are the most detailed of the three signals because they capture the specific context around individual events rather than summarizing system behavior as a number. Logs are well suited to answering the question of what happened and when. When a team receives an alert that error rates have spiked, logs are where they look to find out what the error actually is. A log entry might reveal that a specific database query is failing with a timeout error, or that a particular API endpoint is returning a permissions error for a subset of users. That level of detail is not available in a metric.

What logs cannot tell you is how a problem traveled through a distributed system. A log entry records what happened inside one service at one moment in time. If a user request touches five services before returning a response, the logs from each of those services exist independently. Connecting them into a coherent picture of what happened to that specific request requires tracing, which we will review in the next section. 

Production systems can generate an enormous volume of log data. A busy application might write millions of log entries per day, which introduces challenges around storage, search, and noise. Not every event is worth logging, and care must be taken in other cases to ensure that personally identifiable information (PII) is not captured. One of the ongoing challenges of observability is making intentional decisions about what to log, so that the important signals do not get buried in irrelevant data, and customer privacy policies are respected.

#### Traces

A **trace** is a record of the path a single request takes as it travels through a distributed system. Where metrics describe the overall behavior of a system and logs capture individual events within a single service, a trace follows one specific request from the moment it enters the system to the moment a response is returned, recording every service it touches along the way. A trace is made up of individual units called spans. Each **span** represents one unit of work performed by one service as part of handling the request. For example, a single API request might generate a trace that includes:

- A span for the initial HTTP handler
- A span for a call to an authentication service
- A span for a database query
- A span for a call to a caching layer

Each span records how long that unit of work took and whether it succeeded or failed. This structure makes traces uniquely useful for diagnosing problems in distributed systems. If a user request is slow, a trace can show exactly which span took the longest, which tells the team which service or operation is responsible for the slowdown. If a request fails, a trace can show which span failed and what the failure was, even if the error originated deep in a chain of services.

What traces cannot tell you is how the system is behaving overall. A trace describes one request. It does not tell you how many requests are failing, what the error rate is across the system as a whole, or whether a trend is developing over time. For that, teams return to metrics.

![A diagram titled "Three Pillars of Observability: Metrics, Logs and Traces" showing three symbols side by side. The first symbol is labeled "Metrics" with the description "Metrics are numerical values over time." The second symbol is labeled "Logs" with the description "A log is a timestamped record of a discrete event that occurred within a system." The third symbol is labeled "Traces" with the description "A trace is a record of the path a single request takes as it travels through a distributed system."](assets/metrics-logs-traces.png)  
*Fig. The three pillars of observability: metrics, logs, and traces. Each signal answers a different kind of question about system behavior.*

#### How the Three Signals Work Together

Metrics, logs, and traces are most powerful when used together. Each signal answers a different question, and the gaps in one are filled by the others.

Consider a scenario where a team's application begins experiencing a problem. 

- The **metrics** alert the team that the error rate for a specific API endpoint has climbed from less than 1% to 8% over the past ten minutes. That tells the team _something is wrong, but not what or why_. 
- The team turns to the **logs** and finds entries showing that a database query is failing with a connection timeout error. That tells them _what the error is, but not which part of the system_ triggered the query or how the failure is affecting users. 
- The team pulls up a **trace** for one of the failing requests and can see the full path the request took: it passed through an authentication service, then called an order processing service, which attempted a database query that timed out, causing the request to fail. The trace _shows where in the chain the failure occurred_ and how long each step took before the timeout.

With all three signals, the team has moved from "something is wrong" to "here is what is wrong, here is where it is happening, and here is the specific operation that is failing." That is the value of using metrics, logs, and traces together rather than relying on any one of them alone.

### When Signals Mislead

Observability data is only useful if it is interpreted carefully. Having metrics, logs, and traces in place does not guarantee that a team will detect problems quickly or diagnose them accurately. Each of the three signals can mislead teams in specific ways, and understanding these failure modes is just as important as understanding the signals themselves.

#### Noisy Logs and Alert Fatigue

More data is not always better. When a system logs every event indiscriminately or fires alerts for every minor deviation from normal behavior, the important signals get buried in noise. Alert fatigue occurs when teams are exposed to so many alerts that they begin to treat them as background noise. If an alert fires dozens of times a day, many of them for conditions that turn out to be harmless, engineers learn to ignore them. The risk is that when a critical alert fires, it looks the same as all the others and goes unnoticed until users start reporting problems.

The same problem applies to logs. A system that writes a log entry for every minor event can generate so much data that finding the log entries that actually matter becomes difficult and time consuming. Searching through millions of log entries to find the one that explains a failure is significantly harder than searching through a smaller, more intentional set of logs.

Useful observability requires deliberate decisions about what to log and what to alert on. The goal is not to capture everything but to capture the appropriate events, which is an ongoing process of refinement as a team learns more about how their system behaves in production.

#### Misconfigured Alerts and Silent Failures

The opposite problem is equally dangerous. An alert that is configured too conservatively may never fire even when a real problem exists. This is known as a silent failure, which is a failure that occurs without triggering any notification, leaving the team unaware that anything is wrong until a user reports a problem or the failure escalates into something more serious.

Silent failures are particularly risky because they can persist for a long time without anyone noticing. A team that assumes their system is healthy because no alerts have fired may be missing failures that are happening just below their alert thresholds. Consider an alert configured to fire when the error rate for a payment flow exceeds 10%. If the actual error rate is 4%, the alert never fires. But a 4% error rate on a payment flow means that one in every twenty-five transactions is failing, which is a significant problem that could affect a large number of users and result in lost revenue. The threshold was set with the intention of catching serious problems, but it was set too high to catch this one.

Effective alerting requires understanding which thresholds are meaningful for each specific part of a system. A threshold that is appropriate for one service may be completely wrong for another, and thresholds that made sense when a system was first deployed may need to be revisited as the system grows and its traffic patterns change.

#### Misleading Averages

Averages are one of the most common ways that teams summarize metric data, and one of the most common sources of misunderstanding. An average collapses many individual data points into a single number, and in doing so it can hide the experiences of a subset of users whose requests behave very differently from the majority.

Consider a scenario where most API requests complete in 50 milliseconds, but 2% of requests take 8 seconds to complete. The average response time across all requests might still look acceptable, perhaps 150 milliseconds, well within a team's defined performance targets. However, the users whose requests are taking 8 seconds are experiencing something completely different. From their perspective, the application is broken. The average response time metric gave the team no indication that anything was wrong.

This is why the industry commonly uses percentiles rather than averages to measure response time. A percentile describes the experience of a specific portion of users. The 99th percentile, often written as p99, is the response time below which 99% of requests fell. Put another way, only 1% of requests took longer than the p99 value. For example, if your p99 is 400ms then that means 99 out of every 100 users received a response within 400ms, but 1 out of every 100 experienced something slower. That 1% might sound small, but on a system handling 100,000 requests per minute that is 1,000 users every minute experiencing your slowest responses.

![A line chart titled "Request Latency Percentiles" showing four lines plotted over dates from August 3 to August 9, 2025. The y-axis is labeled "Milliseconds" and ranges from 0 to 20. The p99 line shows significantly higher and more variable latency than the p50, p90, and p95 lines, indicating that while most requests are fast and consistent, a small percentage of requests experience much slower response times](assets/percentiles.png)  
*Fig. Request latency percentiles over a one-week period. The p99 line shows significantly higher and more variable latency than the p50, p90, and p95 lines, indicating that while most requests are fast and consistent, a small percentage of requests experience much slower response times.*

These measurements surface the outliers that averages hide, giving teams a more accurate picture of how the system is actually performing for real users. Percentiles are introduced here as a concept to be aware of rather than something to configure. The important takeaway is that averages can mask performance problems affecting a small but real subset of users, and that the choice of how to summarize metric data has real consequences for what a team is able to see.

## Summary

**Observability** is the ability to understand the internal state of a production system by examining the data it produces. Since production environments cannot be paused or inspected directly the way a local development environment can, teams rely on three categories of signals to detect and diagnose problems: 
- **Metrics**, which are numerical measurements that surface trends and can be used to trigger alerts.
- **Logs**, which are timestamped records of discrete events that reveal what happened and when.
- **Traces**, which follow a single request through a distributed system to show where a failure or slowdown occurred. 

The four golden signals — latency, traffic, errors, and saturation — provide a practical starting point for deciding which metrics to watch. Observability data is only useful when interpreted carefully. Too many alerts leads to alert fatigue, misconfigured thresholds can allow failures to go undetected, and averages can mask poor experiences for a small but real subset of users. Percentiles offer a more accurate view of how a system is actually performing across the full range of user experiences.

## Check for Understanding

### !challenge
<!-- Question 1 -->
* type: multiple-choice
* id: 23048d8d-f9fd-40f0-b22e-6fde7c290a42
* title: Introduction to Observability

##### !question
A developer's application works as expected on their laptop but behaves unexpectedly after being deployed to production. The developer wants to understand what is happening inside the running application. Which of the following best describes what observability provides in this situation?
##### !end-question

##### !options
a| A way to pause the running application and inspect variables directly, the same way a debugger works in development.
b| A way to understand the internal state of the system by examining the data it continuously produces about its own behavior.
c| A way to automatically fix errors in the application code when something goes wrong in production.
d| A way to replicate the production environment exactly on the developer's laptop so the issue can be reproduced locally.

##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Observability gives teams a way to understand what a production system is doing by examining the data it emits, such as metrics, logs, and traces. Unlike a development environment where a developer can pause execution and inspect the code directly, a production system cannot be stopped or attached to a debugger. Observability fills that gap by making the system's internal behavior visible through the data it produces.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: multiple-choice
* id: fdd1988d-0fb8-4881-b10a-93ccf80ced81
* title: Introduction to Observability

##### !question
A team receives an alert that error rates have spiked. They check their metrics dashboard and confirm the spike is real. Which combination of signals should they use next, and in what order, to identify what the error is and where in the system it originated?
##### !end-question

##### !options
a| Check traces first to see overall error rate trends, then check metrics to find the specific error message.
b| Check logs to find the specific error message and context, then check traces to see which service in the chain caused the failure.
c| Check metrics again using a different time range, then restart the affected servers to clear the error.
d| Check traces first to find the error message, then check logs to see which service caused the failure.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Metrics are good at detecting that something is wrong, but they cannot tell you _what the error is_ or _where it originated_. Logs are the right next step because they capture the specific context around individual events, including error messages and stack traces. Once the team knows what the error is, traces can show exactly which service in a distributed system triggered it and how the failure propagated through the request chain.
#### !end-explanation 
### !end-challenge

<!-- Question # 3 -->
### !challenge
* type: multiple-choice
* id: fc4e3f9f-61e9-48f3-bb4e-fe59423282ba
* title: Introduction to Observability

##### !question
Your team is investigating a report that a checkout flow is failing for some users. You have access to metrics, logs, and traces. Which signal would be most useful for determining exactly which service in the checkout flow is responsible for the failure, and why?
##### !end-question

##### !options
a| Metrics, because they show the overall error rate across all services and can identify which service has the highest number of failures.
b| Logs, because they capture timestamped records of every event in the system and can show the exact sequence of steps a user took during checkout.
c| Traces, because they follow a single request through every service it touches and can show exactly where in the chain the failure occurred.
d| Metrics and logs together, because neither signal alone can identify which service is responsible for a failure in a multi-service flow.
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
Traces are designed specifically for distributed systems where a single request touches multiple services. A trace follows one request from start to finish, recording each service it passed through as a span. When a failure occurs somewhere in that chain, the trace shows exactly which span failed and how long each step took before the failure. Metrics can tell you that failures are happening, and logs can tell you what the error was, but only a trace can show you the full path of the request and pinpoint which service in the checkout flow is responsible.
#### !end-explanation 
### !end-challenge