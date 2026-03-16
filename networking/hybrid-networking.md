# Hybrid Networking

## Learning Goals

* Explain the role of hybrid networking in connecting on-premises infrastructure to cloud environments.
* Describe how Virtual Private Networks (VPNs) create secure tunnels using various authentication methods.
* Compare VPNs with high-performance alternatives like dedicated interconnects and transit gateways.
* Identify appropriate use cases for private resource mapping to minimize network exposure.

## Strategic Integration of Local and Cloud Resources

Up to this point, we've focused on building isolated environments within a single cloud provider. However, many organizations do not live entirely in the cloud. They often have "on-premises" (on-prem) data centers. These are physical buildings filled with servers, storage devices, and networking equipment that the company owns and operates. These resources might host legacy applications, sensitive data, or specialized hardware that cannot be easily moved to the cloud. We use hybrid networking to bridge the gap between these physical locations and our virtual private clouds (VPCs).

**Hybrid networking** is the practice of connecting these on-prem resources to our cloud environments, allowing them to communicate as if they were part of the same network.

Hybrid networking can enable a variety of possible scenarios, such as:

* **Database Migration:** We might keep our sensitive data on-prem while our web servers run in the cloud, requiring a constant, secure link between them.
* **Disaster Recovery:** We can use the cloud as a backup site. If our physical building loses power, our cloud resources can take over.
* **Cloud Bursting:** During peak traffic, like a massive holiday sale, we might "burst" our workload into the cloud to handle the extra load while keeping our baseline operations on our local servers.

When we have local hardware and cloud resources that need to work together, we can use hybrid networking to create a seamless connection between them.

## Secure Communication Through Virtual Private Networks

When two networks are colocated, they can connect through a simple physical cable. But what happens when our office is in Seattle and our cloud resources are in a data center in Oregon? That would be a long cable! Instead, we use the public internet to connect these two locations. However, the internet is a shared medium, and we need to ensure that our data remains private and secure as it travels across it.

**Virtual Private Network (VPN)** create encrypted "tunnels" that we can use to securely connect two networks, even if they are on opposite sides of the world. Between the two networks, the data still travels over the public internet, but it is wrapped in an encryption layer that makes it unreadable to anyone else. So between the two network endpoints, it's like there's a private network cable that only our data can use. but it's not a physical cable, instead, it's a virtual one created by software. Hence the name "Virtual Private Network."

To build this connection, we rely on specific **VPN Infrastructure**:

* **Gateway Devices:** These are specialized routers or firewalls at the edge of our local network that "speak" the VPN protocol to the cloud.
* **Software Clients:** In some cases, individual developer laptops use software to connect directly to the VPC.
* **Virtual Gateways:** On the cloud side, we configure a virtual endpoint to receive and decrypt the incoming traffic.

![A local office connects to a cloud VPC through a VPN tunnel. The local office has a VPN gateway device that initiates the connection to the cloud's virtual gateway. The data travels securely through the encrypted tunnel over the public internet, allowing the local office to access resources in the cloud as if they were on the same network. A machine with a software client connects directly to the cloud, also through the VPN tunnel, allowing it to access cloud resources securely from anywhere.](assets/networking-vpn.png)  
*Fig. A VPN creates a secure, encrypted tunnel between two networks over the public internet. With appropriate software, it can also allow individual devices to connect directly to the cloud.*

In addition to providing a secure connection, VPNs also enable us to designate who can access the tunnel by authenticating users and devices connecting to it. In some ways, this can mean that the VPN is more secure than a physical cable, where often it's assumed that any device plugged into it is trustworthy. With a VPN, we can require users to prove their identity before they can access the tunnel, and we can enforce strict policies about which devices are allowed to connect.

The method of authentication can vary. Consumer VPNs often use simple username and password combinations, but in a corporate environment, we typically require stronger methods. For example, we might use **digital certificates**, which are like electronic "passports" that verify the identity of a device or user. Alternatively, we might implement **Multi-Factor Authentication (MFA)**, which requires users to provide two or more verification factors to gain access. This could be something they know (like a password), something they have (like a smartphone app that generates a one-time code), or something they are (biometrics like a fingerprint).

While it's not necessary to have a deep understanding of the underlying protocols (like IPsec or SSL/TLS) that VPNs use to create these tunnels, it's useful to have a general understanding of how a connection is established. Try to identify each component in the following steps in the previous diagram.

When a VPN connection is initiated:
1. The local VPN gateway and the cloud virtual gateway perform a handshake to establish the tunnel.
2. They negotiate encryption keys to secure the communication
3. Then they authenticate each other using the chosen method (like digital certificates or MFA).
4. Once the tunnel is established, data can flow securely between the two networks.

## Physical and Logical Alternatives for High-Demand Systems

VPNs are cost-effective and easy to set up, but they rely on the public internet for connectivity, which can be (relatively) unpredictable. The routed nature of internet traffic means that our data might take a longer path than expected, and the shared nature of the medium can lead to congestion and variable latency. For many applications, this is perfectly fine. But for high-demand systems that require consistent performance, we might need to look beyond VPNs.

**Dedicated Connections** can provide consistent speed with high security. Instead of a virtual tunnel, we work with telecommunications providers to lay a physical fiber-optic cable between our data center and the cloud provider. This removes the public internet from the equation, providing low latency and massive bandwidth. However, this option is significantly more expensive, takes much longer to set up than a VPN, and can lead to vendor lock-in since the physical connection will be tied to a specific cloud provider. As a result, this is typically reserved for large enterprises with critical workloads that justify the cost and lock-in risks.

| Concept | AWS | Azure | GCP | OCI |
| --- | --- | --- | --- | --- |
| **Dedicated Connection** | Direct Connect | ExpressRoute | Interconnect | FastConnect |
| **Network Hub** | Transit Gateway | Virtual WAN | Connectivity Center | Dynamic Routing Gateway |
| **Private Mapping** | PrivateLink | Private Link | Private Service Connect | Private Endpoint |

As our systems grow, we might find ourselves managing dozens of VPCs across different regions. Connecting every single VPC to our on-prem office individually would create a "spaghetti" of connections. To solve this, we use **Network Hub or Transit Gateway** services. These act as a central distributor for our data. We connect our office to the hub once, and the hub directs traffic to any VPC in our fleet, greatly simplifying our architecture.

![Two network diagrams side by side. The first diagram shows a local office with multiple direct VPN connections to several VPCs, creating a complex web of connections. The second diagram shows the same local office connected to a central transit gateway, which then connects to all the VPCs, illustrating a much simpler and more organized architecture.](assets/networking-transit-gateway.png)  
*Fig. In networks with many VPCs, a transit gateway acts as a central hub that connects the local office to all VPCs through a single connection, simplifying the architecture and improving manageability.*

Finally, we sometimes need to access a specific cloud service (like a managed database or a storage bucket) but establishing a full VPN connection might be more than we need. **Private Resource Mapping** allows us to map a specific service to a private IP address inside our own subnet. This way, our traffic never leaves the provider's internal backbone, and we don't have to manage a complex, wide-reaching VPN just to reach a single tool.

## Summary

Hybrid networking allows us to combine the stability of on-premises hardware with the flexibility of the cloud. We use VPNs to create secure, encrypted tunnels for standard communication, backed by strong authentication like MFA. For systems requiring higher performance or simpler management at scale, we graduate to dedicated physical connections and centralized transit hubs. By choosing the right connection method, we balance cost, security, and speed to meet the needs of our specific application.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 45a0af4b-9778-4f92-8df6-18141650078a
* title: Hybrid Networking

##### !question

Which of the following are essential characteristics of a VPN used for hybrid networking?

##### !end-question

##### !options

a| It uses physical fiber-optic cables dedicated solely to one company.
b| It creates an encrypted tunnel over the public internet.
c| It allows a remote network to appear as if it is part of the local network.
d| It replaces the need for an Internet Gateway entirely.

##### !end-options

##### !answer

b|
c|

##### !end-answer

##### !explanation

VPNs create encrypted tunnels over the public internet, allowing secure communication between on-premises and cloud networks. They also enable remote networks to appear as part of the local network, facilitating seamless integration. However, VPNs do not use physical fiber-optic cables dedicated solely to one company (that's a characteristic of dedicated connections), nor do they replace the need for an Internet Gateway entirely, as they still rely on the public internet for connectivity.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: 5ec4676e-b5c9-424c-a2e4-8e3abe15a0d8
* title: Hybrid Networking

##### !question

Place the following steps in the correct order for establishing a secure VPN connection from a local office to the cloud:

##### !end-question

##### !answer

1. The local VPN appliance initiates a connection request to the cloud endpoint.
2. Both sides negotiate encryption keys to secure the "tunnel."
3. The cloud gateway validates the digital certificate or MFA token.
4. Encrypted data begins flowing through the tunnel.

##### !end-answer

##### !explanation

A simple VPN connection process is discussed in the reading material. First, the local VPN appliance initiates the connection to the cloud endpoint. Then, both sides negotiate encryption keys to secure the tunnel. After that, the cloud gateway validates the authentication method (like a digital certificate or MFA token). Finally, once the tunnel is established and authenticated, encrypted data can begin flowing securely between the two networks.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: f9d8b997-f8f6-4f78-9e6c-4a3f10a83def
* title: Hybrid Networking

##### !question

In which scenario would a Dedicated Interconnect (like AWS Direct Connect) be preferred over a standard VPN?

##### !end-question

##### !options

a| A financial institution needs to move terabytes of data daily with guaranteed low latency.
b| A small startup needs to quickly connect one developer's laptop to a test environment.
c| A team wants to connect two VPCs in the same cloud region.
d| An app needs to access a public API like the Google Maps API.

##### !end-options

##### !answer

a|

##### !end-answer

##### !explanation

A Dedicated Interconnect provides a physical, high-bandwidth connection that offers consistent performance and low latency, making it ideal for scenarios like a financial institution moving large volumes of data daily. In contrast, a standard VPN would be more suitable for quick, temporary connections (like a developer's laptop) or connecting resources within the same cloud region. Accessing public APIs typically does not require a dedicated connection and can be done over the public internet.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
