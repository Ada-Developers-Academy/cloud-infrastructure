# The Need for Event-Driven Architecture & Distributed Systems

## Learning Goals

* Identify the limitations of synchronous, tightly coupled systems in high-traffic scenarios.
* Explain how dependencies between services can lead to system-wide failures.
* Describe the core purpose of Distributed Systems and Event-Driven Architecture (EDA) in solving scaling friction.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| :--- | :--- | :--- | :--- |
| **Synchronous** | A communication style where the sender waits for a response before moving on. | Real-time, blocking, request-response. | We use a **synchronous** call when we need an immediate confirmation from the database before showing a "Success" page. |
| **Asynchronous** | A communication style where the sender moves to the next task without waiting for a response. | Non-blocking, background processing. | By making the email notification **asynchronous**, our checkout process doesn't slow down while waiting for the mail server. |
| **Tight Coupling** | A state where system components are highly dependent on one another's internal logic or availability. | Interdependent, intertwined. | Because our payment and inventory services are **tightly coupled**, the whole site goes down if the inventory database crashes. |
| **Decoupling** | The process of separating system components so they can operate and change independently. | Isolation, Unbinding | By **decoupling** the payment service from the inventory service, we ensure a bug in one doesn't crash the other. |

## The Challenge of Scaling with Dependencies

When we first start building full-stack applications, we often create "monoliths"—single, unified units where the frontend, backend, and database are all tightly connected. In this environment, communication is typically **synchronous**. For example, when a user clicks "Buy," the web server calls the payment service, waits for a "Success" message, then calls the shipping service, and finally tells the user the order is complete. This works well enough when traffic is low and every part of the system is healthy.

However, as our applications grow, this tight coupling creates significant friction. If we experience a sudden spike in traffic—like an e-commerce site on Black Friday—every single dependency in that chain must be able to scale at the exact same speed. If the shipping service slows down under the weight of thousands of requests, it creates a "bottleneck" that forces the payment service to wait. The user's browser itself is also waiting for a response, which can lead to timeouts and a poor user experience.

In a tightly coupled system, a failure or slowdown in one small area can trigger a domino effect, potentially bringing down the entire application as the next service in the chain waits indefinitely for a response. This is why we need to rethink how our services communicate and operate.

## Moving Toward Independent Systems

To build systems that are truly resilient and efficient, we need to move away from the idea that every part of our application must wait for every other part. This is where **Distributed Systems** and **Event-Driven Architecture (EDA)** enter the picture. Instead of one giant program running on one server, a distributed system allows us to spread our application across many independent "nodes" or programs that work together. 

By leveraging EDA, we change the way these nodes talk to each other. Instead of Service A telling Service B exactly what to do and waiting for it to finish, Service A simply announces that something happened—an **event**. Other services can then choose to "listen" to that event and react whenever they are ready.

This shift allows us to scale parts of our system independently. If our shipping department is overwhelmed, it can process its backlog at its own pace without preventing the payment department from taking new orders. This decoupling is the secret to building modern, global-scale cloud applications that remain secure and available even when parts of the network fail.

## Summary

In this lesson, we explored why traditional, tightly coupled applications struggle to keep up with the demands of the modern web. We learned that **synchronous communication** creates dependencies that can lead to system-wide failures if one component becomes a bottleneck. By introducing **Distributed Systems** and **Event-Driven Architecture**, we provide a way for services to operate independently. This allows our systems to be more "elastic," reacting to events in real-time and scaling specific components without requiring the entire application to grow at the same rate. This practice of decoupling our system is essential for anyone building reliable services in the cloud.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 265ea4d2-4264-4070-b1e0-786265f8002b
* title: The Need for Event-Driven Architecture & Distributed Systems

##### !question

Which of the following best describes a "bottleneck" in a tightly coupled system?

##### !end-question

##### !options

a| A security feature that prevents unauthorized access to the database.
b| A single service that slows down, causing all services waiting on it to also slow down or fail.
c| A specialized tool used to increase the speed of network requests.
d| A method for distributing data across multiple cloud providers like AWS and Azure.

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

A "bottleneck" occurs when one service in a tightly coupled system becomes overwhelmed or fails, causing all other services that depend on it to also slow down or fail. This is a common issue in synchronous communication models where services must wait for responses from each other, leading to a domino effect of failures if one component cannot keep up with the demand.

<br>

The following options do not describe a bottleneck:
- While security features may be the source of some performance issues if misconfigured, it does not describe a bottleneck.
- A bottleneck situation tends to degrade performance. It is not a specialized tool for increasing the speed of network requests.
- Distributing data across cloud providers is a strategy for redundancy and scalability, which can help avoid bottlenecks. It is not a bottleneck itself.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: b15c2bef-3a01-48f5-a743-600fc82aaba9
* title: The Need for Event-Driven Architecture & Distributed Systems

##### !question

Why is synchronous communication often a disadvantage during high-traffic events?

##### !end-question

##### !options

a| It requires the sender to wait for a response, which can lead to timeouts if the receiver is busy.
b| It forces all connected services to scale at the same time to maintain performance.
c| It makes the system more secure by ensuring only one request happens at a time.
d| It creates a "domino effect" where one slow service impacts the entire user experience.

##### !end-options

##### !answer

a| 
b|
d|

##### !end-answer

##### !explanation

The following are reasons why synchronous communication can be a disadvantage during high-traffic events:
- If the receiver is busy or overwhelmed, this can lead to timeouts and a poor user experience.
- In a tightly coupled system, all connected services must scale at the same time to maintain performance, which is often impractical during sudden spikes in traffic since it takes time for knowledge of the spike to propagate and for resources to be allocated.
- If one service slows down, it can create a "domino effect" where all subsequent services that depend on it also slow down, further degrading the user experience.

<br>

The following is not a reason why synchronous communication can be a disadvantage:
- Additional security would not be a disadvantage, but synchronous communication does not inherently make a system more secure.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: fa363c40-2620-4bab-9ff7-aadcc8d672b6
* title: The Need for Event-Driven Architecture & Distributed Systems

##### !question

What is the primary benefit of "decoupling" services through Event-Driven Architecture?

##### !end-question

##### !options

a| It allows services to be written in the same programming language.
b| It ensures that all services are updated at the exact same millisecond.
c| It allows services to operate and scale independently without needing immediate responses from others.
d| It reduces the number of servers needed to run a simple website.

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

The primary benefit of "decoupling" services through Event-Driven Architecture is that it allows services to operate and scale independently without needing immediate responses from others. This means that if one service is overwhelmed or fails, it does not necessarily bring down the entire system, as other services can continue to function and process events at their own pace.

<br>

The following are not benefits of decoupling services through Event-Driven Architecture:
- Decoupling is not related to whether services are written in the same programming language.
- Decoupling does not ensure simultaneous updates across services. To the contrary, it can introduce delays in communication, which is a trade-off for increased resilience.
- Decoupling does not inherently reduce the number of servers needed. In fact, it may require more servers to handle the increased complexity of a distributed system. But it generally does so with gains in resilience and scalability. The way we prioritize design requirements can change depending on the expected traffic and user experience goals of the application.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
