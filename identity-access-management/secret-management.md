# Secret Management

## Learning Goals

- Explain the difference between data at rest and data in transit, and describe why each requires protection.
- Identify common types of secrets used in software systems and describe the contexts in which each is used.
- Explain why secrets management is a critical security practice and describe the consequences of poor secrets management.
- Explain how cloud-based secrets management tools are used to store and control access to secrets securely.
- Describe what automated secret rotation is and why it is an important security practice.
- Explain how services authenticate to each other and describe the role secrets play in service-to-service authentication.
- Identify common secrets management mistakes made during development and describe practices for avoiding them.

### What are Secrets?

In the context of computing and cloud environments, a **secret** is any piece of sensitive information that is used to authenticate, authorize, or encrypt access to a resource. Secrets are the credentials that systems, services, and users rely on to prove identity and establish trust. They are called secrets because their value depends entirely on remaining confidential. A secret that is known to an unauthorized party is no longer a secret, and the protection it was intended to provide is compromised.

Secrets are not limited to passwords. In modern software systems, secrets take many forms, including API keys that authorize access to external services, cryptographic keys that encrypt sensitive data, certificates that establish secure communication channels, and tokens that grant temporary access to resources. Each of these will be covered in detail later in this lesson.

Secrets function as a trust mechanism. When a service presents an API key to an external system, that service treats the key as proof that the requester is authorized to access its resources. When a database connection string includes a password, the database treats that password as proof that the connecting application is permitted to read or write data. The system receiving the secret has no way to distinguish between a legitimate principal presenting a valid secret and an attacker who has obtained that same secret through other means. If a secret is compromised, the system it protects has no way of knowing that the entity presenting it is not the intended principal. This is what makes secrets fundamentally different from other security controls such as biometric authentication, and it is what makes their protection so important.

### !callout-warning

### Consequences of Compromised Secrets

The consequences of a compromised secret depend on the scope of access that secret controls, but they can be severe. A compromised secret can result in:
- **Unauthorized access to sensitive data**: An attacker who obtains a database credential or an API key may be able to read, modify, or delete data they were never intended to access.
- **Service disruption**: An attacker with access to infrastructure credentials can deprovision resources, modify configurations, or interfere with the operation of a system.
- **Lateral movement**: In systems where secrets are shared across services, a single compromised secret can provide an attacker with a foothold to move through a system and escalate their access to additional resources.
- **Reputational and financial damage**: Data breaches resulting from compromised secrets can have significant consequences for an organization, including regulatory penalties, legal liability, and loss of customer trust.

It is worth noting that many high-profile security breaches are not caused by sophisticated attacks on encryption or authentication protocols. They are caused by secrets that were hardcoded in source code, stored in plaintext configuration files, accidentally committed to a public code repository, or left in place long after they should have been rotated or revoked. Secrets management is as much about operational discipline as it is about technical controls.

### !end-callout

### Understanding the States of Data

Before diving into the specific types of secrets used in software systems and how they are managed, it is important to understand the foundations of secrets management: the distinction between **data at rest** and **data in transit**. These two terms describe the two states in which data exists in a system, and each state presents distinct security considerations.

#### Data at Rest

**Data at rest** refers to data that is stored in a persistent location and is not actively moving between systems or components. This includes data stored in databases, file systems, object storage, backup archives, or any other storage medium. Secrets are a category of data that must be protected when at rest. For example, a database credential stored in a configuration file, an API key stored in a secrets management tool, and an encryption key stored in a key management service are all secrets that exist in a state of rest until they are retrieved and used. 

Data at rest is primarily vulnerable to threats that target the storage layer directly. If an attacker gains access to the storage medium where data is held, whether through unauthorized access to a database, theft of a physical storage device, or exploitation of a misconfigured storage bucket, they may be able to read the data directly. This is why data at rest should be encrypted: even if an attacker gains access to the underlying storage, encrypted data is unreadable without the corresponding decryption key.

#### Data in Transit

**Data in transit** refers to data that is actively moving between two points, such as between a client and a server, between two microservices, or between a application and a database. Secrets that are in transit include an API key being sent in an HTTP request header, a database password being passed in a connection string, or an access token being transmitted between services.

Data in transit is primarily vulnerable to threats that target the network layer, where data can be intercepted as it travels between two points. An attacker who is able to intercept network traffic, a technique known as a man-in-the-middle attack, may be able to read or modify data as it travels between two points. This is why data in transit should be encrypted using a secure transport protocol. Transport Layer Security (TLS) is the standard protocol used to encrypt data in transit in modern software systems. When data is transmitted over TLS, it is encrypted before it leaves the sender and decrypted only when it reaches the intended recipient, making it unreadable to anyone who intercepts it along the way.

A common mistake is to focus on protecting data in only one of these states. A complete security strategy requires protecting data in both states. This is particularly relevant for secrets. A secret that is encrypted at rest but transmitted in plaintext over an unencrypted connection can be intercepted and compromised just as easily as one that is stored without encryption. Secrets must be protected throughout their entire lifecycle, from the moment they are created and stored, through every transmission, to the moment they are rotated or revoked. When evaluating a secrets management tool or designing a system that handles secrets, two of the most fundamental questions to ask: how does this system protect secrets at rest, and how does it protect secrets in transit?

### Types of Secrets

Secrets take many forms in modern computing environments. Understanding the different types of secrets, what they are used for, and where they typically appear in a system is a prerequisite for managing them effectively. The following are some common types of secrets that developers will encounter in professional software development. This section is meant to be an introduction to different types of secrets so we will not review each in great detail. Feel free to explore your curiosity about secrets!

| Secret Type | What It Is | Where It Is Commonly Used | Primary Security Consideration|
| ----- | ----- | ----- | ----- |
| Passwords and Passphrases | A string of characters used to authenticate a user or service to a system. | User accounts, database connections, administrative interfaces. | Frequently hardcoded or stored in plaintext, increasing the risk of exposure. |
| API Keys | A token issued by a service that grants the holder access to that service's API. | Application integrations with external services and third-party APIs. | Anyone who obtains the key can use it to make requests on behalf of the key's owner. |
| Encryption Keys | A piece of data used by an encryption algorithm to protect data at rest and in transit. | Encrypting databases, storage, and network communications. | If compromised, any data protected by that key may be exposed. |
| SSH Keys | A pair of cryptographic access credentials used in the Secure Shell (SSH) protocol to provide a more secure and convenient method for authenticating and establishing encrypted connections between servers. | Developer access to servers, CI/CD pipelines, automated processes. | A compromised private key grants access to any system that trusts the corresponding public key. |
| TLS/SSL Certificates | A digital certificate that authenticates a server's identity and enables encrypted communication. | HTTPS connections between clients and servers. | The certificate itself is public, but the associated private key must be protected carefully. |
| Database Credentials | The username and password used to authenticate a connection to a database. | Application connections to databases. | Often grant direct access to sensitive data, bypassing application-level access controls. |
| Access Tokens | A credential issued after authentication that represents a principal's session and permissions. | OAuth 2.0 and OIDC authentication flows in modern applications. | Typically short-lived by design, limiting the window of exploitation if compromised. |

#### Passwords and Passphrases

A **password** is a string of characters used to authenticate a human user or a service to a system. Passwords are one of the oldest and most widely used forms of authentication and appear across virtually every type of system, from user accounts to database connections to administrative interfaces.

A **passphrase** is a longer, more complex form of password that typically consists of a sequence of words or a sentence. Passphrases are generally considered more secure than traditional passwords because their length makes them significantly harder to compromise through brute force attacks, while also being easier for humans to remember.

In the context of secrets management, passwords and passphrases are among the most commonly mishandled secrets. They are frequently hardcoded in source code, stored in plaintext configuration files, or shared across multiple systems and team members, all of which are practices that significantly increase the risk of exposure.

#### API Keys

An **API key** is a secret token issued by a service that grants the holder access to that service's API. API keys are widely used because they are simple to generate and implement, but they carry significant risk if not managed carefully. Unlike some other authentication mechanisms, an API key typically grants access to a service without requiring any additional verification, meaning that anyone who obtains the key can use it to make requests on behalf of the key's owner.

Since the receiving service has no way to distinguish between the legitimate owner presenting the key and an attacker who has obtained it through other means, an attacker can silently impersonate the key's owner, consuming resources, accessing data, and performing actions that will be attributed to the legitimate owner rather than the attacker. This makes detecting and investigating unauthorized use significantly harder, since the activity appears in audit logs as legitimate requests from a known principal.

#### Encryption Keys

An **encryption key** is a piece of data used by an encryption algorithm to transform plaintext into ciphertext, and ciphertext back into plaintext. Encryption keys are among the most sensitive secrets in a system because if a key is compromised, any data protected by that key may be exposed. Encryption keys are covered in depth in the [Introduction to Encryption](./intro-to-encryption.md) lesson.

#### SSH Keys

**Secure Shell (SSH) keys** are cryptographic key pairs used to authenticate access to remote systems and servers. An SSH key pair consists of a public key and a private key. During the setup of a remote system, an administrator configures that system to trust a specific public key. The corresponding private key is kept secret on the machine of the user or service that will be connecting. When an SSH connection is initiated, the remote system issues a cryptographic challenge that can only be answered correctly by whoever holds the matching private key. This allows the remote system to verify the identity of the connecting party without the private key ever being transmitted over the network.

SSH keys are commonly used by developers to access remote servers, by CI/CD pipelines to deploy code to production environments, and by automated processes that need to interact with remote systems. The private key in an SSH key pair is a secret that must be protected carefully. If a private key is compromised, an attacker can use it to authenticate to any system that trusts the corresponding public key.

#### TLS/SSL Certificates

**Transport Layer Security (TLS) certificates**, sometimes still referred to by the name of their predecessor Secure Sockets Layer (SSL), are digital certificates that serve two purposes: they authenticate the identity of a server to a client, and they enable encrypted communication between the two parties. When a client connects to a server over HTTPS, the server presents its TLS certificate to prove its identity. The client verifies the certificate against a trusted certificate authority before establishing an encrypted connection.

TLS certificates are not secrets in the same sense as passwords or API keys. The certificate itself is public and is presented openly to any client that connects to the server. However, the private key associated with a TLS certificate is a secret that must be protected carefully. If the private key is compromised, an attacker can impersonate the server or decrypt intercepted traffic.

#### Database Credentials

**Database credentials** are the username and password, or equivalent authentication information, used to authenticate a connection to a database. In most software systems, applications connect to databases programmatically using credentials that are configured at deployment time. These credentials control what the connecting application is permitted to do within the database, such as reading data, writing data, or modifying the database schema.

Database credentials are among the most sensitive secrets in a system because they often grant direct access to the data that the application is built to protect. A compromised set of database credentials can give an attacker direct read and write access to sensitive data, bypassing the application layer and any access controls it enforces entirely.

#### Access Tokens

An **access token** is a credential issued by an authentication system that represents a principal's authenticated session and their associated permissions. Rather than requiring a principal to present their primary credentials, such as a username and password, with every request, the authentication system issues a token after the initial authentication event. The principal then presents that token with subsequent requests, and the receiving system validates the token to determine whether the request should be permitted.

Access tokens are widely used in modern authentication protocols including OAuth 2.0 and OpenID Connect. They are typically short-lived by design: a token that expires quickly limits the window of time during which it can be exploited if it is compromised. This is one of the key advantages of token-based authentication over long-lived credentials such as API keys.

### Service-to-Service Authentication

Modern software systems are rarely monolithic. They are composed of multiple services that communicate with each other to fulfill requests: a web application might call an authentication service to verify a user's identity, a payment service to process a transaction, and a notification service to send a confirmation email. Each of these interactions requires the calling service to prove to the receiving service that it is authorized to make that request. This is the problem that service-to-service authentication solves.

When a human user authenticates to a system, they typically do so interactively by presenting credentials such as a username and password or a second factor. Services cannot authenticate interactively. They must authenticate programmatically, without human intervention, often at high frequency and across many simultaneous connections. This requires a different approach to authentication than the mechanisms used for human users. Service-to-service authentication ensures that every request between services is verified, regardless of where it originates. Services authenticate to each other by presenting a secret that the receiving service can verify. The type of secret used depends on the architecture of the system and the authentication mechanism in place.

#### Short-Lived Credentials versus Long-Lived Credentials

One of the most important design decisions in service-to-service authentication is whether to use short-lived or long-lived credentials. This distinction has significant security implications. **Long-lived credentials**, such as API keys or passwords that do not expire, are straightforward to implement but carry persistent risk. If a long-lived credential is compromised, it remains valid until it is explicitly rotated or revoked. In a large system with many services, tracking and rotating long-lived credentials manually is operationally complex and easy to neglect, which is why stale credentials are a common source of security vulnerabilities.

**Short-lived credentials**, such as access tokens with a short expiration time, significantly reduce this risk. If a short-lived credential is compromised, it becomes invalid after a brief window of time, limiting the damage an attacker can do with it. The tradeoff is that short-lived credentials require a mechanism for services to obtain new credentials regularly, which adds architectural complexity. In practice, this is handled by a central authentication service that issues tokens on demand, allowing services to request fresh credentials as needed without human intervention.

The general principle is that credentials should be as short-lived as the system's architecture can reasonably support. This is a direct application of the principle of least privilege introduced in the previous lesson: just as permissions should be scoped as narrowly as possible, the window of time during which a credential is valid should be as short as possible.

### Managing Secrets

Having established what secrets are, the states in which they exist, the forms they take, and how they are used in service-to-service communication, the next question is how they should be managed. **Secrets management** is the set of practices and tools used to store, control access to, and maintain the lifecycle of secrets in a software system. Done well, secrets management ensures that secrets are available to the principals and services that need them while remaining inaccessible to those that do not.

Poor secrets management is one of the most common and preventable sources of security vulnerabilities in software systems. The most frequently encountered poor practices include:
- **Hardcoded secrets**: Embedding secrets directly in source code means that anyone with access to the codebase, including anyone who can view a public code repository, has access to those secrets. Secrets committed to version control are particularly difficult to remediate because they persist in the commit history even after they are removed from the current codebase.
- **Plaintext storage**: Storing secrets in plaintext configuration files, environment variable files checked into version control, or unencrypted databases means that anyone who gains access to those files has immediate access to the secrets they contain.
- **Overly broad access**: Granting access to secrets beyond what is necessary violates PoLP and increases the number of principals that could expose a secret.
- **Stale secrets**: Secrets that are never rotated or revoked accumulate risk over time. A secret that has been in use for years has had more opportunities to be exposed than one that is rotated regularly.
- **Shared secrets**: Using the same secret across multiple services or principals makes it impossible to revoke access for one without affecting all others, and makes it difficult to attribute access events to a specific principal in an audit.

A secrets management tool is a dedicated system for storing, retrieving, and managing secrets securely. Rather than storing secrets in configuration files or environment variables, a secrets management tool provides a centralized, controlled interface through which secrets can be accessed programmatically by authorized principals. At a minimum, a secrets management tool provides:
- **Encrypted storage**: Secrets are encrypted at rest so that they cannot be read directly from the underlying storage medium.
- **Access control**: Access to secrets is governed by policies that determine which principals are permitted to retrieve which secrets.
- **Audit logging**: Every access to a secret is recorded, providing a clear audit trail of which principal retrieved which secret and when. This is critical for incident response and compliance requirements.
- **Programmatic access**: Secrets can be retrieved by applications and services at runtime through an API, eliminating the need to store secrets in configuration files or pass them through environment variables.

Cloud platforms provide dedicated secrets management services that implement the capabilities described above at scale. While the specific implementation details vary across platforms, the underlying model is consistent: secrets are stored in an encrypted vault, access is controlled through the platform's IAM system, and all access events are logged.

A typical workflow using a cloud-based secrets management tool looks like this: 
1. A secret, such as a database credential, is stored in the secrets management service at deployment time. 
2. The application that needs the credential is assigned an IAM role with a policy that grants it permission to retrieve that specific secret. 
3. At runtime, the application authenticates to the cloud platform using its assigned role and requests the secret from the secrets management service. 
4. The service verifies that the application's role has permission to access the requested secret and, if so, returns it over an encrypted connection. 
5. The application uses the secret for the duration of its need and does not store it persistently.  

This workflow ensures that the secret is never stored in the application's configuration files or source code, that access to the secret is governed by explicit IAM policies, and that every retrieval is logged for audit purposes.

### Automated Secret Rotation

Even when secrets are stored securely and access is properly controlled, secrets that remain unchanged indefinitely represent a growing security risk. The longer a secret is in use, the more opportunities there have been for it to be exposed, whether through a data breach, an insider threat, a misconfigured system, or simply the accumulation of principals who have had legitimate access to it over time. **Secret rotation** is the practice of periodically replacing a secret with a new one, invalidating the old secret so that it can no longer be used.

Rotating a secret means generating a new secret, updating all systems and services that use it to use the new value, and invalidating the old secret. The goal is to limit the useful lifespan of any given secret, so that even if a secret has been compromised without the organization's knowledge, it will eventually become invalid. Secret rotation matters for several reasons:
- **It limits the damage of an undetected compromise**. If a secret has been obtained by an attacker but the compromise has not been detected, rotating that secret invalidates the attacker's access even though the organization does not know that a compromise occurred.
- **It reduces the risk of stale credentials**. Secrets that are rotated regularly are less likely to be forgotten or left in place after they are no longer needed.
- **It supports compliance requirements**. Many security frameworks and regulatory standards require secrets and credentials to be rotated on a defined schedule.

Manual secret rotation is operationally complex and error-prone. In a system with many secrets across many services, tracking which secrets need to be rotated, coordinating the update across all systems that use each secret, and doing so on a regular schedule is difficult to manage reliably without automation. This is where automated rotation becomes important. A secrets management tool that supports automated rotation handles the rotation process programmatically. At a defined interval, or in response to a trigger such as a detected security event, the tool generates a new secret, updates the systems and services that use it, and invalidates the old secret. This process happens without human intervention, which reduces the operational burden of rotation and eliminating the risk of rotation being skipped or delayed due to manual oversight.

For automated rotation to work reliably, services must be designed to retrieve secrets dynamically at runtime rather than loading them once at startup and caching them. A service that caches a secret at startup will continue using the old value after rotation until it is restarted, which can cause authentication failures and service disruptions. Designing services to retrieve secrets fresh from the secrets management tool each time they are needed is what allows automated rotation to propagate to all dependent services without manual intervention. To avoid disrupting services that may be mid-request when a secret is rotated, some systems maintain two valid versions of a secret simultaneously during the rotation window. The old secret remains valid for a brief period after the new one is introduced, giving all services time to retrieve and begin using the new value before the old one is invalidated.

Organizations that do not rotate secrets regularly accumulate risk in ways that are not always immediately visible. A secret that has been in use for months or years may have been accessed by principals who have since left the organization, stored in systems that have since been decommissioned, or transmitted over connections that were later found to be insecure. None of these exposures may be known, but all of them represent potential attack vectors. The risk is compounded by the fact that compromised secrets often go undetected for extended periods. Without rotation, an attacker who has obtained a valid secret can maintain persistent access to a system indefinitely, or until the compromise is discovered through other means. Regular rotation limits this window of persistent access even in the absence of detection.

### Secrets Hygiene in Development

The most sophisticated secrets management infrastructure provides limited protection if developers introduce vulnerabilities during the development process itself. Many of the most common and damaging secrets-related security incidents are not the result of failures in production systems but of poor practices during development. Secrets hygiene refers to the set of practices that developers follow to ensure that secrets are handled safely throughout the development workflow.

#### Hardcoded Secrets Become a Persistent Risk

Hardcoding a secret means embedding it directly in source code as a literal value. This is one of the most common and persistent secrets management mistakes made by developers, and it is particularly dangerous because source code is frequently shared, copied, and stored in ways that extend well beyond the immediate development environment. A hardcoded secret in a private repository is already a risk: anyone with read access to that repository has access to the secret. If that repository is ever made public, or if a developer accidentally pushes code to a public repository, the secret is immediately exposed to anyone who can view it. Even after the secret is removed from the current codebase, it persists in the repository's commit history and can be retrieved by anyone with access to that history. Automated tools exist to scan repositories for hardcoded secrets, and many code hosting platforms run these scans automatically. However, detection after the fact does not eliminate the risk: a secret that has been exposed in a public repository, even briefly, must be treated as compromised and rotated immediately.

#### Using Environment Variables to Manage Secrets Locally

A common alternative to hardcoding secrets is to store them in environment variables, which are key-value pairs that are set in the operating system or runtime environment and can be accessed by an application at runtime. This keeps secrets out of source code and allows different values to be used in different environments, such as development, staging, and production, without changing the application code. Environment variables are a reasonable approach for local development, but they have limitations that make them unsuitable as a long-term secrets management strategy in production environments. Environment variables can be accidentally logged, exposed through process inspection, or leaked through error messages. They also do not provide the access control, audit logging, or rotation capabilities that a dedicated secrets management tool provides. A common and practical pattern for local development is to store environment variables in a local file, conventionally named `.env`, that is explicitly excluded from version control by adding it to the repository's `.gitignore` file. This keeps secrets accessible to the application during local development without committing them to the repository.

#### Keeping Secrets Out of Version Control

Version control systems such as Git are designed to retain a complete history of every change made to a codebase. This makes them particularly unforgiving when secrets are accidentally committed: simply deleting the secret in a subsequent commit does not remove it from the repository's history. Recovering from an accidental secret commit requires rewriting the repository's history, which is a disruptive and error-prone process, particularly in repositories with multiple contributors. The most effective way to keep secrets out of version control is to prevent them from being committed in the first place. Practical measures include:
- **Using a .gitignore file** to exclude files that contain secrets, such as `.env` files, from being tracked by version control.
- **Using pre-commit hooks** to scan staged changes for potential secrets before a commit is made. Tools such as git-secrets and detect-secrets can be configured to run automatically before each commit and block commits that contain patterns matching known secret formats.
- **Conducting code reviews with secrets exposure in mind**. Code reviewers should be alert to hardcoded credentials, API keys, and other sensitive values in submitted code.

## Summary

Secrets are any piece of sensitive information used to authenticate, authorize, or encrypt access to a resource. Their value depends entirely on remaining confidential, and the consequences of a compromised secret can range from unauthorized access to sensitive data to significant reputational and financial damage.

**Data at rest** and **data in transit** describe the two states in which secrets, and all data, exist in a system. Each state presents distinct security threats, and a complete secrets management strategy must account for both. The types of secrets used in modern software systems, including **passwords**, **API keys**, **encryption keys**, **SSH keys**, **TLS certificates**, **database credentials**, and **access tokens**, each carry their own security considerations and are used in different contexts. In systems composed of multiple services, secrets are the primary mechanism through which services authenticate to each other, and the choice between **long-lived** and **short-lived credentials** has significant security implications.

Secrets management tools provide the infrastructure for storing secrets securely, controlling access through IAM policies, logging all access events, and **rotating secrets** automatically. Automated rotation limits the useful lifespan of any given secret, reducing the risk of persistent access by an attacker who has obtained a valid credential without the organization's knowledge. Finally, the practices that developers follow during development are as important as the infrastructure used to manage secrets in production. Avoiding hardcoded secrets, handling environment variables carefully, and keeping secrets out of version control are foundational habits that prevent vulnerabilities from being introduced before code ever reaches a production environment.

## Check for Understanding

### !challenge
<!-- Question 1 -->
* type: multiple-choice
* id: dd331929-8f6d-4974-89a0-71b515d4c0d5
* title: Secret Management

##### !question
A developer is building a web application that connects to a database. During local development, they hardcode the database password directly in the application's source code and commit it to a private GitHub repository. Which of the following best describes the security risk this introduces?
##### !end-question

##### !options
a| There is no immediate risk because the repository is private and only accessible to the development team.
b| The password is exposed to anyone with read access to the repository, persists in the commit history even if removed, and could be exposed if the repository is ever made public.
c| The risk is limited because database passwords are rotated automatically by most secrets management tools.
d| The risk is limited to the local development environment and does not affect the production system.

##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
Hardcoding a secret in source code is a persistent risk regardless of whether the repository is private. Anyone with read access to the repository has access to the secret, and removing it in a subsequent commit does not eliminate it from the repository's history. If the repository is ever made public, the secret is immediately exposed.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: multiple-choice
* id: 5e1954ff-4dbe-4d41-b759-1c34e6457b53
* title: Secret Management

##### !question
A microservices-based application has a service that needs to authenticate to a database every time it processes a request. The team is deciding between issuing the service a long-lived API key that does not expire, or configuring the service to request a short-lived access token from a central authentication service before each database connection. Which approach is more consistent with the principle of least privilege, and why?
##### !end-question

##### !options
a| The long-lived API key, because it is simpler to implement and reduces the number of authentication requests made to the central authentication service.
b| The long-lived API key, because it ensures the service always has access to the database without the risk of an expired token causing a service disruption.
c| The short-lived access token, because it limits the window of time during which the credential is valid, reducing the potential damage if the credential is compromised.
d| The short-lived access token, because access tokens are always more secure than API keys regardless of their expiration time.
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
Short-lived credentials directly apply the principle of least privilege to the time dimension of access: a credential should be valid for only as long as it is needed. If a short-lived token is compromised, it becomes invalid after a brief window, limiting the damage an attacker can do with it.
#### !end-explanation 
### !end-challenge

<!-- Question # 3 -->
### !challenge
* type: multiple-choice
* id: 918e4f08-c3ac-4bd7-90cd-ea3e51d5c60d
* title: Secret Management

##### !question
An application stores its configuration in a cloud-based secrets management tool. The secrets management tool encrypts all secrets at rest and transmits them over TLS. A developer notices that the application retrieves its database credential once at startup and caches it in memory for the duration of the application's runtime. The organization has recently enabled automated secret rotation with a 24-hour rotation interval. Which of the following best describes the risk introduced by the application's caching behavior?
##### !end-question

##### !options
a| There is no risk because the secret is encrypted at rest and transmitted over TLS, which protects it in both states.
b| There is no risk because the secrets management tool will notify the application when the secret has been rotated.
c| After the secret is rotated, the application will continue using the old cached value until it is restarted, which may cause authentication failures and means the application is not benefiting from rotation.
d| The application should cache the secret at startup because retrieving it dynamically on every request would create excessive load on the secrets management tool.
##### !end-options

##### !answer
c|
##### !end-answer

#### !explanation 
Automated rotation only provides its intended security benefit if services are designed to retrieve secrets dynamically at runtime rather than caching them. An application that caches a secret at startup will continue using the old value after rotation, which defeats the purpose of rotation and can cause authentication failures when the old secret is invalidated. 
#### !end-explanation 
### !end-challenge