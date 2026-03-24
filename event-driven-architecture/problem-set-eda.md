# Problem Set: Event-Driven Architecture & Distributed Systems

## Scenario: The "GreenThumb" Grocery App

Imagine we are the engineering team for **GreenThumb**, a popular grocery delivery app. Originally, our app was a "monolith"—one giant program where the checkout, inventory, and notification systems were all tightly knit together. As we grew, this became a headache: if the notification system crashed, the whole checkout process would freeze. We are now transitioning to a **Distributed System** using **Event-Driven Architecture (EDA)**.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: d4694226-0dcc-4e8e-b61b-eeaff4f203a4
* title: Problem Set: Event-Driven Architecture & Distributed Systems

##### !question

We are explaining to a new teammate why we are moving away from our old centralized (monolithic) model toward a distributed system. Which of the following best describes the core mental shift we are making?

##### !end-question

##### !options

a| We are moving from many small programs to one powerful, high-capacity server to ensure data stays in one place.
b| We are replacing all human intervention with automated scripts that manage a single database.
c| We are ensuring that every part of our code is stored on every single machine in our network.
d| We are shifting from a single program to a group of independent nodes working together to achieve a shared goal.

##### !end-options

##### !answer

d|

##### !end-answer

##### !explanation

The core mental shift in moving to a distributed system is understanding that we are no longer building one giant program that runs on a single machine. Instead, we are creating a network of independent nodes (machines) that work together to achieve a shared goal. Each node can run its own part of the application, and they communicate with each other to coordinate their efforts. This allows us to scale more easily, improve reliability, and isolate faults without affecting the entire system.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 66eee1b1-f61a-4d16-b6b4-b9383e50496f
* title: Problem Set: Event-Driven Architecture & Distributed Systems

##### !question

Why are we putting in the effort to make GreenThumb distributed? Select **all** the strengths that apply to a distributed system:

##### !end-question

##### !options

a| **Fault Tolerance:** If one node (machine) fails, the others can continue to serve our customers.
b| **Simplicity:** It is much simpler to write and debug code for a distributed system than for a single-machine application.
c| **Scalability:** We can add more nodes independently as our workload increases, such as during a holiday sale.
d| **Reliability:** We can replicate data across multiple machines so it remains available even during a local outage.

##### !end-options

##### !answer

a|
c|
d|

##### !end-answer

##### !explanation

**Simplicity** is not a strength of distributed systems; in fact, they can be more complex to design and debug compared to single-machine applications due to the need to manage communication, data consistency, and fault tolerance across multiple nodes.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 3307dd01-8950-4bf8-b426-2a8d198dc514
* title: Problem Set: Event-Driven Architecture & Distributed Systems

##### !question

Our checkout process currently uses a **Request-Response** (synchronous) model. When a user clicks "Buy," the Checkout service calls the Email service and *waits* for a "Success" message before showing the user their receipt. We want to move to an **Asynchronous** (event-driven) model.

What is the primary trade-off we are making by switching to an **Event-Driven** model?

##### !end-question

##### !options

a| We gain immediate confirmation that the email was sent, but we lose the ability to scale.
b| We decouple the services so the Checkout service can finish immediately, but we must accept that the email will be sent a few moments later.
c| We make the system faster for the developer to write, but it requires more physical cables in the data center.
d| We ensure that if the Email service is down, the Checkout service will also stop working to maintain data consistency.

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

By switching to an Event-Driven model, we decouple the Checkout service from the Email service. This means that when a user clicks "Buy," the Checkout service can immediately finish processing the order and show the receipt without waiting for the Email service to confirm that the email was sent. However, this also means that the email will be sent asynchronously, and there may be a slight delay before the user receives it. This trade-off allows us to improve scalability and responsiveness while accepting that some actions will happen in the background.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: 35496124-daaa-4fec-96b3-e816ff6165f0
* title: Problem Set: Event-Driven Architecture & Distributed Systems

##### !question

We've decided to use a **Pub/Sub** (Publisher/Subscriber) model for our "Order Placed" event. When a customer finishes an order, several things need to happen: the warehouse needs to pack it, and the customer needs a text notification.

**Put the following steps in the correct order for an event traveling through our system:**

##### !end-question

##### !answer

1. The **Publisher** (Checkout Service) creates an "Order Placed" event.
1. The **Publisher** sends the event to the **Event Channel**.
1. The **Event Channel** (Event Bus) receives the "Order Placed" message and routes it.
1. A **Subscriber** (Notification Service) receives the event and sends a text message to the customer.

##### !end-answer

##### !explanation

In a Pub/Sub model, the Publisher (Checkout Service) creates an event when an order is placed and sends it to the Event Channel. The Event Channel is responsible for receiving the event and routing it to the appropriate Subscribers. In this case, the Notification Service is a Subscriber that listens for "Order Placed" events and reacts by sending a text message to the customer. This decouples the services, allowing them to operate independently while still communicating effectively through events.

<br>

Some time before the "Order Placed" event was sent, the warehouse team would have needed to configure the Notification Service to subscribe to the "Order Placed" event. This setup allows the Notification Service to react whenever an order is placed without the Checkout Service needing to know about it.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 5b33f179-4c61-4641-936d-8e3c1a788f69
* title: Problem Set: Event-Driven Architecture & Distributed Systems

##### !question

One of our goals is "loose coupling." In our new EDA setup, our Checkout service (the Producer) doesn't know that the Shipping service (the Consumer) even exists. It just sends an event to the **Event Channel**.

How does this "Event Channel" intermediary help us scale?

##### !end-question

##### !options

a| It forces all services to run on the same physical hardware.
b| It encrypts the data so that only one service can ever see it.
c| It acts as a buffer, allowing the Producer to keep working even if the Consumer is temporarily busy or offline.
d| It automatically rewrites the code of the Consumer to match the Producer.

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

The Event Channel acts as a buffer between the Producer and Consumer. This means that if the Consumer (Shipping service) is temporarily busy or offline, the Producer (Checkout service) can continue to send events without being blocked. The Event Channel will hold onto those events until the Consumer is ready to process them. This allows us to scale each service independently and ensures that temporary issues with one service do not bring down the entire system.

<br>

The following responses do not reflect the benefits of an Event Channel in an EDA setup:
- Forcing all services to run on the same physical hardware would actually reduce scalability and increase coupling, which is the opposite of what we want.
- Encrypting data so that only one service can see it would limit communication and make it difficult for multiple services to react to the same event, which is not ideal in an event-driven architecture.
- Event Channels are operational components that manage the flow of events, but they do not have the capability to rewrite code. Further, automatically rewriting the code of the Consumer to match the Producer would create tight coupling and reduce flexibility, as each service would need to be aware of the others' implementations, which goes against the principle of loose coupling.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->
<!-- Replace everything in square brackets [] and remove brackets  -->

### !challenge

* type: checkbox
* id: 4443c178-7c58-4dd6-9867-a962fbf9c4f3
* title: Problem Set: Event-Driven Architecture & Distributed Systems

##### !question

It's Black Friday, and GreenThumb is seeing a massive spike in orders. Our "Invoice Generation" service is struggling to keep up, but our "Browse Catalog" service is doing fine.

Because we chose a **Distributed, Event-Driven Architecture**, how can we handle this?

##### !end-question

##### !options

a| We can scale up *only* the Invoice service by adding more nodes to it without touching the rest of the system.
b| We must shut down the whole app to increase the capacity of the Invoice service.
c| If the Invoice service crashes due to the load, users can still browse the catalog because the services are isolated.
d| The Event Channel will hold the "Generate Invoice" events in a queue until we can add more capacity to process them.

##### !end-options

##### !answer

a|
c|
d|

##### !end-answer

##### !explanation

The following responses reflect the benefits of a distributed, event-driven architecture:
- Because we designed our system to be distributed and event-driven, we can scale up the Invoice service independently by adding more nodes to it without affecting the Browse Catalog service. This is one of the key benefits of a distributed architecture: we can target specific services for scaling based on demand.
- If the Invoice service crashes due to the load, users can still browse the catalog because the services are isolated from each other. This fault isolation is a critical feature of distributed systems, allowing parts of the application to continue functioning even if one service experiences issues.
- Additionally, the Event Channel will hold the "Generate Invoice" events in a queue until we can add more capacity to process them. This buffering capability allows us to manage spikes in demand without losing events or overwhelming the system, ensuring that all orders will eventually be processed once we have the necessary resources in place.

<br>

The following response does not reflect the benefits of a distributed, event-driven architecture:
- Shutting down the whole app to increase the capacity of the Invoice service would be counterproductive and goes against the principles of scalability and fault isolation that we gain from a distributed architecture. It would cause unnecessary downtime and disrupt the user experience for all services, including those that are not experiencing issues.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
