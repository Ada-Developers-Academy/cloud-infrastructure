# Problem Set: Networking

## Scenario 1: Scaling the Microservices Architecture

We are part of an engineering team building a new e-market application. Our architecture consists of a **public-facing React frontend**, a **Node.js API**, and a **database** containing sensitive customer information. We have decided to use a **Virtual Private Cloud (VPC)** to isolate our resources.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 662405fe-87ad-4707-81f4-6078af4f4845
* title: Problem Set: Networking

##### !question

To ensure the highest level of security for our customer data, where should we place our database and our web-facing frontend? (Select the best approach)

##### !end-question

##### !options

a| Place both the database and the frontend in a public subnet so they can easily talk to each other.
b| Place both the database and the frontend in a private subnet and use a VPN for all customer traffic.
c| Place the frontend in a public subnet and the database in a private subnet.
d| Place the database in a public subnet for easier maintenance and the frontend in a private subnet.

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

The best approach is to place the frontend in a public subnet and the database in a private subnet. This way, the frontend can receive traffic from the internet, while the database remains isolated and protected from direct access. The frontend can communicate with the database securely within the VPC, while external users cannot directly access the database, reducing the risk of unauthorized access to sensitive customer information.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: cd3ea165-68f4-42ce-b9e6-92eed239f25b
* title: Problem Set: Networking

##### !question

Our API (located in a private subnet) needs to download security patches from the open internet, but we must ensure that no unauthorized users from the internet can initiate a connection directly to our API servers. Which component should we implement to allow this one-way communication?

##### !end-question

##### !options

a| Internet Gateway
b| Router
c| MAC Address Switch
d| NAT Gateway

##### !end-options

##### !answer

d|

##### !end-answer

##### !explanation

A NAT Gateway allows instances in a private subnet to connect to the internet for outbound traffic (like downloading security patches) while preventing any inbound traffic initiated from the internet. This ensures that our API servers can access necessary updates without exposing them to potential threats from unauthorized users.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: ffcafae8-6007-44fc-a4ac-48433a08b3af
* title: Problem Set: Networking

##### !question

We are configuring our servers to ensure traffic reaches the correct service. Match the following protocols to their standard **Port** numbers:

1. **HTTPS** (Secure Web Traffic)
2. **SSH** (Secure Shell for Remote Management)
3. **PostgreSQL** (Database Traffic)

##### !end-question

##### !answer

1. 443
1. 22
1. 5432

##### !end-answer

##### !explanation

HTTPS typically uses port 443 to secure web traffic, SSH uses port 22 for secure remote management, and PostgreSQL uses port 5432 for database communication. The ports for HTTPS and SSH are well-known and standardized, while PostgreSQL's preferred port lands in the "registered" range, which companies can use for their applications. While it may not be an "official" standard, it's common enough that it may as well be.

<br>

Learning the ports of common services, especially those we use in our applications, is often helpful for troubleshooting and configuring our network security rules.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

## Scenario 2: Defense in Depth

Our team is auditing our network security. We want to ensure that even if one layer of our defense is bypassed, our infrastructure remains protected. We are looking at how we filter traffic entering our subnets versus traffic reaching our individual compute instances.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 44574f8c-d20d-4c99-a46d-728fb107abbe
* title: Problem Set: Networking

##### !question

We need to apply a security rule that **automatically allows return traffic**. For example, if our server sends a request out to a service, the response should be allowed back in without us writing a specific "Inbound" rule for it. Which tool should we use?

##### !end-question

##### !options

a| A Stateful Firewall (such as a Security Group)
b| A Stateless Firewall (such as a Network ACL)
c| A Hub
d| An IP Route Table

##### !end-options

##### !answer

a|

##### !end-answer

##### !explanation

A Stateful Firewall, like a Security Group, automatically allows return traffic for any outbound request. This means that if our server initiates a connection to an external service, the response will be allowed back in without needing an explicit rule.

<br>

In contrast, a Stateless Firewall (like a Network ACL) requires us to write separate rules for inbound and outbound traffic, which can be more complex to manage and may lead to unintended blocks if not configured correctly. Hubs and IP Route Tables do not provide this functionality, as they are not designed for traffic filtering or stateful inspection.

### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 768245e3-ce11-4e2d-b476-8d65a4cb28e6
* title: Problem Set: Networking

##### !question

Which of the following statements accurately describe the differences between **Security Groups** and **Network ACLs (NACLs)**?

##### !end-question

##### !options

a| Security Groups operate at the instance level, while NACLs operate at the subnet level.
b| NACLs support "Deny" rules, whereas Security Groups only support "Allow" rules.
c| Security Groups evaluate all rules before allowing traffic, while NACLs process rules in numbered order.
d| NACLs are stateful, meaning they remember the state of a connection.

##### !end-options

##### !answer

a|
b|
c|

##### !end-answer

##### !explanation

Security Groups operate at the instance level, allowing us to define rules that apply to specific compute resources. NACLs operate at the subnet level, providing a broader layer of security for all resources within that subnet. NACLs support both "Allow" and "Deny" rules, giving us more granular control over traffic filtering, while Security Groups only allow us to specify what traffic is permitted. Additionally, Security Groups are stateful, meaning they automatically allow return traffic for outbound requests, while NACLs are stateless and require explicit rules for both inbound and outbound traffic.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

## Scenario 3: The Hybrid Office

Our company maintains a physical office with an on-site data center, but we are moving our main application to the cloud. We need our office employees to be able to access the private cloud resources securely as if they were on the same local network.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: d3d63c5a-39bc-44e5-bfbf-50a66de8265a
* title: Problem Set: Networking

##### !question

We want to create a secure "tunnel" over the existing public internet to connect our office network to our VPC. What is the most common, cost-effective way to achieve this without laying physical fiber-optic cables?

##### !end-question

##### !options

a| Direct Connect
b| Public Subnet with an Internet Gateway
c| CIDR Block Segmentation
d| Virtual Private Network (VPN)

##### !end-options

##### !answer

d|

##### !end-answer

##### !explanation

A Virtual Private Network (VPN) is the most common and cost-effective way to create a secure tunnel over the public internet. It allows our office employees to access cloud resources securely as if they were on the same local network, without the need for expensive physical infrastructure like Direct Connect.

<br>

A public subnet with an Internet Gateway would expose our resources to the internet, which is not secure. CIDR Block Segmentation is a method of dividing IP address space and does not provide secure connectivity on its own.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: 06e84a64-f1d8-445e-b08e-fce51268416d
* title: Problem Set: Networking

##### !question

When data travels from our office to the cloud, it moves through the **OSI Model**. Place these layers in the correct order from the **Physical hardware** up to the **Application software**:

##### !end-question

##### !answer

1. Physical
2. Data Link
3. Network
4. Transport
5. Application

##### !end-answer

##### !explanation

The OSI Model is a conceptual framework that describes how data travels through a network. The correct order from the physical hardware to the application software is: Physical (where bits are transmitted over cables), Data Link (where frames are created and MAC addresses are used), Network (where IP packets are routed), Transport (where TCP/UDP protocols manage data flow), and Application (where protocols like HTTP and DNS operate). This layered model allows us to think about equivalent abstractions communicating directly with each other, so knowing the general responsibilities of each layer can help us troubleshoot network issues.

<br>

This version of the OSI Model is the simplified 5-layer version, which combines the Session and Presentation layers into the Application layer for easier understanding.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
