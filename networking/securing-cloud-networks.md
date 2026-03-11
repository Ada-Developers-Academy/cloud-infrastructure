# Securing Cloud Networks

## Goals

* Differentiate between **Stateful** and **Stateless** firewalls.
* Identify the specific roles of **Security Groups** and **Network Access Control Lists (NACLs)**.
* Evaluate whether to apply a security rule at the instance level or the subnet level.
* Design a layered security strategy using both stateful and stateless mechanisms.

## Securing the Perimeter with Network Firewalls

Building secure applications requires thinking about security at every layer of the network stack. Within our application code itself, we can implement various security measures, such as input validation, authentication, and authorization. However, if our underlying infrastructure is exposed to the internet without proper safeguards, attackers could be able to bypass our application-level defenses. 

Network firewalls act as the first line of defense, filtering traffic before it ever reaches our web servers, databases, or any other networked services. Defining strict rules about who can talk to our resources helps us ensure that our systems remain reachable to users, but secure against malicious actors.

A **firewall** is a security device or service that monitors and filters incoming and outgoing network traffic based on an established set of rules. In cloud environments, we move away from physical hardware appliances and instead use software-defined firewalls provided by our cloud platform. Firewalls allow us to create "walls" around our virtual resources. The term firewall derives from the actual concept of a physical wall built to prevent the spread of fire. In the same way, a network firewall prevents unauthorized access to or from a private network.

Firewalls protect our applications by controlling the flow of traffic based on rules we define. For example, we might have a rule that only allows inbound traffic on port 80 (HTTP) and port 443 (HTTPS) to our web servers, while blocking all other ports. This way, even if an attacker tries to connect to our server on an unexpected port, the firewall will block that connection before it can do any harm. Rules can be based on various criteria, such as IP addresses, port numbers, or even specific protocols. They can also be configured to allow or deny traffic in either direction (inbound or outbound), giving us fine-grained control over how our resources communicate with the outside world.

## Tracking Connections with Stateful Firewalls and Security Groups

When we build a web application, we expect a "conversation" to happen: a client sends a request, and our server sends a response. We know this as the Request-Response Cycle.

Even though there are standard ports for web traffic (80 for HTTP and 443 for HTTPS), the response from our server doesn't necessarily have to come back on the same port. In fact, it usually doesn't. A common pattern for web servers is to listen for incoming requests on port 80 or 443, but then send responses back to the client using a random high-numbered port. So after the initial contact, the conversation continues on a different port.

For communications that occur in both directions, we need a way to selectively allow the response traffic back to the user without opening up unnecessary ports that could be exploited by attackers. Further, we don't expect a human network operator to be watching all traffic to manually allow responses back to users. How could they be expected to keep track of every single connection and write rules for them in real time? If we had to manually write a rule for every single response we sent back to a user, our configuration would be unmanageable. This is why we use stateful firewalls.

A **Stateful Firewall** is "smart" enough to remember the state of active connections. If we allow an inbound request on port 443 (HTTPS), the firewall automatically remembers that connection and allows the outbound response to go back to the user without needing a separate outbound rule.

In most cloud environments, the primary tool for this is the **Security Group**. We associate Security Groups directly with individual resources, like a server compute instance or a database.

* **Instance-Level Protection:** Security Groups act like a digital picket fence around a specific server.
* **Default Deny:** By default, all inbound traffic is blocked. We must explicitly "allow" the traffic we want (e.g., allowing port 80 for web traffic).
* **Rule Evaluation:** Security Groups evaluate all rules before deciding whether to allow traffic, meaning there is no "order" to worry about—if any rule allows the traffic, it gets through.

![A network diagram showing a Load Balancer in a public subnet sending traffic to a fleet of Node.js microservices in a private subnet. Each microservice is part of a Security Group that only allows traffic from the Load Balancer, blocking any direct access from unauthorized sources. A user attempting to connect directly to the microservices is blocked by the Security Group, while legitimate traffic from the Load Balancer is allowed through.](assets/networking-security-group.png)  
*Fig. Security Groups are applied to instances and can prevent access from unauthorized paths.*

For example, if we are running a fleet of Node.js microservices, we might create a Security Group that only allows traffic from our Load Balancer. This ensures that even if someone finds the private IP of our server, they cannot connect to it directly; they must go through our official entry point.

## Filtering Subnet Traffic with Stateless NACLs

While Security Groups protect our individual servers, we often need a broader "gatekeeper" at the entrance of our entire subnet. This is where **Network Access Control Lists (NACLs)** come in. Unlike Security Groups, NACLs are **Stateless**. This means they have no memory of past connections. If we allow a user to send a request into our subnet, we must also explicitly write a rule to allow the response to leave the subnet.

NACLs are our "brute force" layer of security. They are particularly important for:

* **Subnet-Wide Rules:** They apply to every single resource within a specific subnet automatically.
* **Explicit Deny:** Unlike Security Groups, NACLs allow us to write "Deny" rules. If we notice a specific IP address is attempting a Denial of Service (DoS) attack on our network by flooding it with traffic, we can block that specific IP at the NACL level before it even touches our servers.
* **Ordered Rules:** NACLs process rules in a designated order. As soon as a rule matches the traffic, it is applied, and the rest are ignored.

![A network diagram showing a malicious IP address attempting to flood a subnet with traffic. The NACL at the subnet boundary has a rule that explicitly denies traffic from the malicious IP, effectively blocking the attack before it can reach any of the servers within the subnet. Legitimate traffic from other IP addresses is allowed through the NACL and can reach the servers as normal.](assets/networking-nacl.png)  
*Fig. NACLs can explicitly deny packets bound for a subnet based on the source IP address.*

By using NACLs, our defenses gain an additional layer of depth. If a port is accidentally opened on a Security Group that shouldn't be, a properly configured NACL can still block that traffic at the subnet boundary, preventing a security breach.

## Comparing Security Groups and NACLs

To build a secure architecture, we must understand how these two tools interact. We typically use Security Groups for our day-to-day "allow" logic and NACLs for broad "keep out" logic or as a backup safety net.

| Feature | Security Group | Network ACL (NACL) |
| --- | --- | --- |
| **Level** | Instance Level | Subnet Level |
| **State** | **Stateful**: Return traffic is auto-allowed | **Stateless**: Return traffic must be explicitly allowed |
| **Rules** | Supports **Allow** rules only | Supports **Allow** and **Deny** rules |
| **Evaluation** | Evaluates all rules | Processes rules in order |
| **Application** | Only applies if associated | Applies to all instances in the subnet |

## Summary

In this section, we explored how to protect our cloud infrastructure using a layered security approach. We learned that **Firewalls** are essential for controlling traffic and preventing unauthorized access to our resources, allowing us to define rules that specify who can access our resources and how they can do so. We learned that **Security Groups** provide stateful, instance-level protection by remembering connection states, making them ideal for managing application traffic. Conversely, **NACLs** provide stateless, subnet-level protection, giving us the power to explicitly deny malicious traffic before it reaches our resources. Combining the use of NACLs and Security Groups  creates a robust environment that keeps our applications secure and our data protected.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 97afee2b-24f1-4dbd-b171-ea07e3cbe810
* title: Securing Cloud Networks

##### !question

A developer is frustrated because they can send a request to their server, but the server's response is being blocked at the subnet boundary. They have already verified the Security Group allows the traffic. Which of the following is the most likely culprit?

##### !end-question

##### !options

a| The Security Group is stateful and forgot the connection.
b| The NACL is stateless and lacks an outbound rule for the response.
c| The Router is misconfigured for MAC addresses.
d| The Instance is missing a Public IP.

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

NACLs are stateless, meaning they do not remember the state of connections. If the developer can send a request to the server but the response is being blocked, it is likely because there is no outbound rule in the NACL allowing the response traffic back out of the subnet. The Security Group being stateful means it should automatically allow return traffic, so that is not the issue here. The Router and Public IP are unrelated to this specific problem of outbound traffic being blocked at the subnet level.

##### !end-explanation


### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 1da07791-aa7d-4b39-9296-77c45bbe83a5
* title: Securing Cloud Networks

##### !question

Which of the following are characteristics of a Security Group?

##### !end-question

##### !options

a| It operates at the subnet level.
b| It is stateful.
c| It supports explicit "Deny" rules.
d| It acts as a firewall for individual instances.
e| It evaluates all rules before allowing traffic.

##### !end-options

##### !answer

b|
d|
e|

##### !end-answer

##### !explanation

The incorrect responses are all characteristics of a NACL, not a Security Group. Security Groups operate at the instance level, are stateful (automatically allowing return traffic), do not support explicit "Deny" rules, and evaluate all rules before allowing traffic. NACLs, on the other hand, operate at the subnet level, are stateless (requiring explicit rules for return traffic), support both "Allow" and "Deny" rules, and process rules in order.

##### !end-explanation


### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 73c7d28b-5f9f-4a86-9b41-80ff1571ae56
* title: Securing Cloud Networks

##### !question

In what order should we place rules in a NACL to ensure a specific malicious IP `1.2.3.4` is blocked while everyone else is allowed?

##### !end-question

##### !options

a| Allow All; Deny `1.2.3.4`
b| Deny `1.2.3.4`; Allow All
c| It doesn't matter because NACLs evaluate all rules simultaneously.
d| Security Groups should be used for blocking specific IPs instead.

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

NACLs process rules in order, so we must place the "Deny" rule for the malicious IP before the "Allow All" rule. If we put the "Allow All" rule first, it would match all traffic (including from `1.2.3.4`) and allow it through before the "Deny" rule is even evaluated. By placing the "Deny" rule first, we ensure that traffic from `1.2.3.4` is blocked before any other rules are considered. Security Groups are not ideal for blocking specific IPs because they do not support explicit "Deny" rules.

##### !end-explanation


### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 28fcbdea-0298-4778-92ef-c61d5cadee14
* title: Securing Cloud Networks

##### !question

Where would a Security Group be most useful compared to an NACL?

##### !end-question

##### !options

a| When we want to block an entire country's IP range from accessing our network.
b| When we need to apply a rule to every single resource in a large subnet automatically.
c| When we want to ensure no traffic can leave the subnet on port 22.
d| When we want to allow a specific web server to talk to a specific database server.

##### !end-options

##### !answer

d|

##### !end-answer

##### !explanation

Security Groups are ideal for controlling traffic between specific instances, such as allowing a web server to communicate with a database server. The other options are better suited for NACLs, which operate at the subnet level and can apply rules to all resources within that subnet. For example, blocking an entire country's IP range or ensuring no traffic can leave the subnet on a specific port would be more effectively handled by a NACL. Security Groups do not support explicit "Deny" rules, so they are not suitable for blocking traffic based on IP ranges or ports.

##### !end-explanation


### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
