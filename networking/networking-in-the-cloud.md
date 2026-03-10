# Networking in the Cloud

## Goals

By the end of this lesson, you will be able to:

* Define a **Virtual Private Cloud (VPC)** and its role in resource isolation.
* Distinguish between **Public and Private Subnets** and justify the use cases for each.
* Calculate network segments using **CIDR blocks** to organize IP address space.
* Explain how **Internet Gateways**, **NAT Gateways**, and **Route Tables** direct traffic flow.
* Identify how **Load Balancers** improve application availability and scale.

## Cloud-scale Applications require Cloud-scale Networking

The architecture of the projects  we've built so far have been relatively simple. We had a frontend server that managed the user experience, a backend API that handled business logic, and a database that stored our data. Since they were relatively small scale applications in terms of traffic and expected response times, it didn't really matter where each part was hosted. Often our decisions were driven more by convenience than by best practices, with the capabilities of free tiers being more of a constraint than the needs of our applications. 

However, as we move toward deploying professional applications, the "where" of our applications becomes just as critical as "what" and "how" they accomplish their business goals. We may intentionally spread our frontend, backend, and database across different regions of the world to ensure low latency for users. We may need to connect our application to third-party services that are only accessible from certain locations. We may also need to design our network in a way that protects sensitive data from unauthorized access while still allowing legitimate users to interact with it seamlessly.

Cloud networking is the discipline of defining the digital boundaries and communication paths for our software. By applying cloud networking concepts, we ensure our applications are both reachable by users and shielded from unauthorized access.

## Isolated Environments Provide Security and Control

When services are deployed in the cloud, they are hosted on shared infrastructure. This means that without proper isolation, our database or backend server could be exposed to the internet, making it vulnerable to attacks. In the physical world, a company might own a private data center with locked doors. In the cloud, we use a **Virtual Private Cloud (VPC)** to achieve the same result. A Virtual Private Cloud is a private, isolated section of a cloud provider’s network where we launch resources in a virtual network that we define.

Without a VPC, our database or backend server would potentially be visible to anyone with an internet connection. Wrapping our infrastructure in a VPC creates a digital perimeter that ensures only the traffic we explicitly allow can reach our services.

Using a VPC is essential because it gives us complete control over our IP address range, subnets, and security settings.

## Subnets and Availability Zones: Providing Performance and Resilience

Once a VPC is established, we must decide how to organize the space inside it. We do this by carving the VPC into **Subnets** (sub-networks). Just as a large office building is divided into floors or departments, subnets allow us to group related resources.

There are two main types of subnets:

* **Public Subnets** have a direct path to the internet. "Front-facing" resources that need to be reachable by users or external services are placed here. This includes components like web servers or load balancers.
* **Private Subnets** have no direct path from the internet. Sensitive resources are placed here, such as production databases or internal API services, ensuring they are unreachable by external bad actors.

To ensure our application doesn't go offline if a single data center has a power failure or other outage, cloud providers offer **Availability Zones (AZs)** (also called *Availability Domains*). These are physically separate data centers within a single region. When planning the deployment requirements for a production system, making use of Availability Zones is crucial for high availability.

![An example cloud network. A VPC is hosted in a region with a gateway providing external access. The VPC is divided into two AZs. AZ1 hosts a public subnet with a load balancer. It is connected to AZ2, which hosts private web servers that the load balancer sends traffic to.](assets/networking-azs.png)  
*Fig. Cloud networking often involves splitting up services into public and private subnets for security, and multiple AZs for resiliency.*

### Public and Private Addresses for Public and Private Subnets

Beyond placing private services in private subnets, we also can restrict access to them by using **Private IP Addresses**. Private IP addresses are not routable on the public internet. This means that even if someone discovers the IP address of a resource in a private subnet, they won't be able to access it directly from the internet. Private IP addresses look just like regular IP addresses, but they fall within specific ranges reserved for private use.

Range | Number of addresses | Description
--- | --- | ---
`10.0.0.0` to <span style="white-space:nowrap;">`10.255.255.255`</span> | 16,777,216 | Commonly used for large private networks. Provided a traditional Class A block, but can be subnetted further.
`172.16.0.0` to <span style="white-space:nowrap;">`172.31.255.255`</span> | 1,048,576 | Often used for medium-sized private networks. Provided 16 traditional Class B blocks, but can be subnetted further.
`192.168.0.0` to <span style="white-space:nowrap;">`192.168.255.255`</span> | 65,536 | Commonly used for home and small business networks. Provided 256 traditional Class C blocks, but can be subnetted further.

There are 3 different private IP address ranges because traditionally, IP addresses were divided into classes (A, B, and C) based on the size of the network.

### Historical IP Address Classes and Networks

In the previous topic, we briefly mentioned Routers and how they direct traffic between different networks, but we didn't look at how networks are defined. The concept of IP address classes was an early way of determining how many IP addresses were available in a given network and how those addresses were grouped into networks. While the class-based system is mostly historical, it still influences how we think about network structures.

Essentially, different IP address ranges belonged to different "classes" of networks. The class of a network determined how many IP addresses it could have and how it was structured.

- **Class A networks**
  - Designed for very large networks
  - Used the `1.X.X.X` to `126.X.X.X` range
  - Provided 16 million addresses per network (2^24 addresses)
  - Any two addresses with the same first octet (the first number in the IP address) were considered to be on the same network
- **Class B networks**
  - Designed for medium-sized networks
  - Used the `128.X.X.X` to `191.X.X.X` range
  - Provided 65,536 addresses per network (2^16 addresses)
  - Any two addresses with the same first two octets were considered to be on the same network
- **Class C networks**
  - Designed for small networks
  - Used the `192.X.X.X` to `223.X.X.X` range
  - Provided 256 addresses per network (2^8 addresses)
  - Any two addresses with the same first three octets were considered to be on the same network

When it became clear that the original IPv4 addressing scheme would become exhausted, sub-ranges of the Class A, B, and C address spaces were reserved for private use. This is why we have three different private IP address ranges today, even though the concept of classes is no longer used in modern networking.

IPv4 is still faced with the problem of address exhaustion, so we still see the use of these historically defined ranges in private networks, but the fixed notion of a "Class A" or "Class B" network is no longer relevant. For a period of time, the concept of **netmasks** was used to identify how many bits of an IP address were used for the network portion versus the host portion. In modern networking, however, we use **CIDR notation** to achieve the same goal.

Feel free to expand the following section for a brief look at netmasks, but it's not necessary for understanding modern cloud networking.

<br>

<details>
<summary>Defining Networks with Netmasks</summary>

A netmask is a 32-bit number that defines which portion of an IP address is the network portion and which part is the host portion.

Though we typically write an IPv4 address as 4 octets, fundamentally, the entire address is a single 32-bit number. For example, the IP address `192.168.0.1` can be represented in binary as `11000000 10101000 00000000 00000001`. A netmask works by "masking" the IP address to determine which part is the network and which part is the host.

We "mask" the network portion by placing a binary `1` in the netmask for each bit that belongs to the network portion, and a binary `0` for each bit that belongs to the host portion. For example, a netmask of `11111111 11111111 11111111 00000000` (which is `255.255.255.0` when written as octets) indicates that the first 24 bits of the IP address are the network portion, and the last 8 bits are the host portion. This means that all IP addresses that share the same first three octets (e.g., `192.168.0.X`) belong to the same network, while the last octet can vary to represent different hosts within that network.

From the network sizes we discussed earlier, we can derive the corresponding netmasks:

Network Size | Netmask (Binary) | Netmask (Octets)
--- | --- | ---
Class A (16 million addresses) | `11111111 00000000 00000000 00000000` | `255.0.0.0`
Class B (65,536 addresses) | `11111111 11111111 00000000 00000000` | `255.255.0.0`
Class C (256 addresses) | `11111111 11111111 11111111 00000000` | `255.255.255.0`

While these were the most common netmasks used in the classful network design, they could also be customized to further divide an address space, especially for carving out subnets within a larger network.

For example, consider a Class B network range, which is typically a network with 65,536 addresses. Let's assume that we needed to make networks that each have around 1,000 addresses. We could use a netmask of `255.255.252.0`, which in binary is `11111111 11111111 11111100 00000000`. This netmask indicates that the first 22 bits are used for the network portion, and the last 10 bits are used for the host portion. With this netmask, we can create multiple subnets within the Class B network, each with around 1,000 addresses (2^10 = 1024 addresses per subnet). This allows us to efficiently organize our network while still providing enough addresses for our resources.

Notice that while netmasks most commonly started with a contiguous string of `1`s followed by a contiguous string of `0`s, this was not a requirement. For example, a netmask of `11111111 11111111 00000000 11111111` (which is `255.255.0.255` in decimal) would indicate that the first 16 bits and the last 8 bits are used for the network portion, while the middle 8 bits are used for the host portion. This would create a network where all IP addresses that share the same first two octets and the same last octet belong to the same network, while the middle octet can vary to represent different hosts within that network. However, this type of netmask is less common and can lead to confusion, which is why CIDR notation is generally preferred in modern networking.

</details>

## CIDR Blocks Define the Boundaries of Your Network

Resources in your VPC require a unique address in order to communicate with each other, and need to know whether they are part of the same network or if they need to send data through a router to reach another network. While we no longer rely on the fixed network sizes defined by the historical class-based system, we still need a way to define the size and structure of our network. The modern way to do this is through CIDR (Classless Inter-Domain Routing) notation.

**CIDR (Classless Inter-Domain Routing)** notation (read the same as the beverage cider) defines the range of IP addresses available to our VPC and its subnets. Effectively applying CIDR prevents address exhaustion, and ensures our network doesn't "overlap" with other networks we might need to connect to later.

A CIDR block looks like this: `10.0.0.0/16`.

* The first part (`10.0.0.0`) is the starting IP address.
* The second part (`/16`) is the **Subnet Mask**, which tells us how many bits are "locked" for the network ID.

A smaller number after the slash means a larger network. For example, a `/16` provides 65,536 addresses (ideal for a whole VPC), while a `/24` provides 256 addresses (perfect for a specific subnet).

![3 addresses evaluated under different CIDR blocks. The subnet 10.0.0.1/16 is defined, with an example host address of 10.0.0.1. The first 16 bits (10.0) are the network, and the second 16 are the host (0.1). The subnet 10.0.0.1/24 is defined, with an example host address of 10.0.0.1. The first 24 bits (10.0.0) are the network, and the next 8 are the host (1). A third address 10.0.1.1 is presented, with no information about the network or host.](assets/networking-cidr.png)  
*Fig. Address c) would be part of the same network as address a), but not address b) even though the addresses look the same in isolation.*

In order to determine whether two addresses are part of the same network, we need to know the CIDR block that defines the network. We can then compare the network portion of the addresses (the bits defined by the CIDR block) to see if they match. If they do, then the addresses are part of the same network and can communicate directly with each other. If they don't match, then the addresses are part of different networks and would need to communicate through a router or gateway to reach each other.

In the diagram above, address a) `10.0.0.1` is part of the network defined by `10.0.0.0/16`, because the first 16 bits (10.0) match the network portion of the CIDR block. In a different organization, the same address could instead be considered part of the network defined by `10.0.0.0/24`, because the first 24 bits (10.0.0) also match the network portion of that CIDR block. This situation is shown as address b). Address c) `10.0.1.1` would be part of the network as shown for address a) because the first 16 bits (10.0) match, but it would not be part of the network as shown for address b) because the first 24 bits (10.0.1) do not match the network portion of that CIDR block.

## Routing and Gateways for Public and Private Subnets

Now that we have an idea about how to define our network and organize it into subnets, we need to understand how data moves within that network and how it reaches the outside world. If we we're building our own physical data center, we would have switches and routers to direct traffic between different parts of the network. In the cloud, we have virtualized versions of these components that perform the same functions.

In order for data to move within the VPC, we need to define **Route Tables**. A route table contains a set of rules, called routes, that determine where network traffic from your subnet or gateway is directed. They virtualize the function of a switch in that it provides connections within a subnet, as well as the function of a router in that it provides connections between different subnets and even to the outside world.

The route table associated with a subnet determines how traffic from that subnet is directed. For a private subnet, the route table will have a local route that directs all traffic that matches the CIDR block of the VPC to stay within the VPC. For a public subnet, in addition to the local route, the route table will have a route that directs all traffic that does not match the CIDR block of the VPC to an **Internet Gateway (IGW)**. This allows resources in the public subnet to communicate with the internet.

Sometimes, resources in a private subnet need to access the internet for things like software updates or connecting to external APIs. However, we don't want to expose these resources directly to the internet. To solve this problem, we use a **NAT (Network Address Translation) Gateway**. A NAT Gateway allows instances in a private subnet to connect to the internet or other cloud services, but prevents the internet from initiating a connection with those instances.

To do so, the NAT Gateway is placed in a public subnet and has a route in the private subnet's route table that directs internet-bound traffic to the NAT Gateway. When an instance in the private subnet sends a request to the internet, the request goes to the NAT Gateway, which then forwards it to the Internet Gateway. The NAT Gateway also translates the private IP address of the instance to its own public IP address, allowing the response from the internet to be routed back through the NAT Gateway to the instance in the private subnet. This way, the instances in the private subnet can access the internet without being directly exposed to it.

## Load Balancers Distribute Traffic for Stability

As an application grows, a single web server will eventually struggle to handle thousands of simultaneous users. To build a system that is both efficient and highly available, we use **Load Balancers**.

A Load Balancer sits in front of your servers, directing network traffic. It receives incoming requests from the internet and distributes them across a "pool" of servers in different Availability Zones. This is important for two reasons:

1. **Scalability:** You can add more servers to the pool as traffic increases.
2. **Reliability:** If one server crashes, the Load Balancer detects the failure and stops sending traffic to it, routing users to the healthy servers instead. This ensures your users never see a "500 Internal Server Error" just because one instance failed.

![An example load balanced network configuration. A VPC is hosted in a region with a gateway providing external access. The VPC is divided into two AZs. AZ1 hosts a public subnet with a public-facing services. It is connected to AZ2, which hosts an internal service, via a router.](assets/networking-load-balancer.png)  
*Fig. A Load Balancer is configured in a public subnet and typically distributes traffic among servers in a private subnet.*

Typically, all traffic from the internet will be constantly passing through the Load Balancer, which then routes it to the appropriate backend servers. This means that the Load Balancer itself must be in a public subnet, while the backend servers can be in private subnets for added security. The Load Balancer focuses on managing the flow of traffic, while the backend servers focus on processing requests and generating responses. This separation of concerns allows for better performance and security. The Load Balancer additionally provides features like SSL termination for HTTPS connections, which offloads the work of encrypting and decrypting traffic from the backend servers, further improving performance.

## Summary

Cloud networking is the foundation of secure, professional application deployment. By using a **VPC**, we create an isolated environment for our services. Within that VPC, **Subnets** and **Availability Zones** provide organization and fault tolerance. We manage the available "real estate" of IP addresses using **CIDR blocks**. Finally, we control the flow and safety of data using **Route Tables**, **Internet Gateways**, **NAT Gateways**, and **Load Balancers**. Together, these tools allow us to build systems that are accessible to users while remaining secure and resilient against failures.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 8d101fda-b9e5-4680-ae52-d21d6eb743c7
* title: Networking in the Cloud

##### !question

You are deploying a three-tier application (Frontend, Backend API, and Database). Which of these resources should be placed in a Private Subnet?

##### !end-question

##### !options

a| Load Balancer receiving user traffic
b| PostgreSQL Database
c| Backend API processing business logic
d| Internet Gateway

##### !end-options

##### !answer

b|
c|

##### !end-answer

##### !explanation

- The Load Balancer receiving user traffic should be placed in a Public Subnet, as it needs to be accessible from the internet.
- The PostgreSQL Database and Backend API processing business logic should be placed in a Private Subnet to protect them from direct exposure to the internet, enhancing security.
- The Internet Gateway is not a resource that is placed in a subnet. it is a component that allows communication between the VPC and the internet.
- 
##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: ded99509-02c9-4f80-9d5d-4f306263f241
* title: Networking in the Cloud

##### !question

Which CIDR block provides the largest number of available IP addresses?**

##### !end-question

##### !options

a| `10.0.1.0/24`
b| `10.0.0.0/16`
c| `10.0.0.0/28`
d| `192.168.1.0/24`

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

The CIDR block `10.0.0.0/16` provides the largest number of available IP addresses. A `/16` CIDR block allows for 65,536 addresses (2^(32-16)), while a `/24` allows for 256 addresses (2^(32-24)) and a `/28` allows for only 16 addresses (2^(32-28)). The value of the CIDR block indicates how many bits are used for the network portion of the address, with a smaller number indicating a larger network and more available IP addresses. The IP address portion of the CIDR block does not affect the number of available addresses, as it is the subnet mask (the number after the slash) that determines the size of the network and the number of available IP addresses.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: 2fe1f474-d743-4aae-99d2-4766fe5400cb
* title: Networking in the Cloud

##### !question

Arrange the following steps in the correct order for a packet traveling from a server in a private subnet to a public website to download an update:

##### !end-question

##### !answer

1. The server checks its Route Table and sends the packet toward the NAT Gateway.
2. The packet reaches the NAT Gateway in the public subnet.
3. The packet is sent to the Internet Gateway.
4. The packet exits to the public internet.

##### !end-answer

##### !explanation

When a server in a private subnet needs to access the internet, it first checks its Route Table to determine how to route the packet. Since it is in a private subnet, it will have a route that directs traffic to the NAT Gateway. The packet is then sent to the NAT Gateway, which translates the private IP address of the server to a public IP address. The NAT Gateway then sends the packet to the Internet Gateway, which allows it to exit to the public internet and reach the destination website for downloading the update.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 7b03ec4a-6850-4b22-9138-94b12b387e64
* title: Networking in the Cloud

##### !question

Which of the following would be a valid IP address for a resource inside a subnet with the CIDR block `10.0.1.0/24`?

##### !end-question

##### !options

a| `10.0.2.1`
b| `10.1.1.5`
c| `10.0.1.50`
d| `172.16.0.1`

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

The IP address `10.0.1.50` is a valid IP address for a resource inside a subnet with the CIDR block `10.0.1.0/24`. The CIDR block `10.0.1.0/24` indicates that the first 24 bits of the IP address are used for the network portion, which means that any IP address that starts with `10.0.1` and has a valid host portion (the last octet) can be part of that subnet. The valid range of IP addresses for this subnet would be from `10.0.1.0` to `10.0.1.255`.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
