# Introduction to Containers

## Learning Goals
- Explain what a container is.
- Describe the problem containers solve, specifically the problem of environment inconsistency.
- Identify the key components of a container: the image, the container runtime, and the registry.
- Explain how containers relate to the compute models.

### What is a Container?

When a developer writes code on their laptop, that code runs inside a specific environment that includes the operating system, the programming language runtime, installed libraries, and configuration settings. The code works because all of those pieces are present and compatible with each other.

When that same code needs to run somewhere else, the environment is rarely identical. A teammate might be running a different version of Python. A staging server, which is a pre-production environment used to test changes before they reach real users, may be running a different operating system. A cloud instance might be missing a library the application depends on. These environment mismatches are one of the most common sources of bugs in production software, and they are the problem containers were designed to solve.

A **container** is a lightweight, portable unit of software that packages an application together with everything it needs to run: the code, runtime, dependencies, libraries, and configuration. Since all of these dependencies are bundled together inside the container, the application behaves consistently regardless of where it is deployed. A container that runs on a developer's laptop will run the same way on a staging server, a cloud instance, or a teammate's machine.

Containers achieve this portability through isolation. **Isolation** means that each container runs as an independent process on the host machine with its own bundled dependencies, separated from other containers and from the host environment itself. This means multiple containers can run side by side on the same machine without interfering with each other, even if they depend on different versions of the same library.

![A diagram showing a host machine containing a container runtime layer labeled "Container runtime and three side-by-side containers labeled Container A, Container B, and Container C. Each container lists four components: app code, a runtime version, libraries, and configuration. Container A uses runtime version 3.9, Container B uses version 3.11, and Container C uses version 2.7. Dashed vertical lines separate the three containers to indicate isolation.](assets/containers.png)
*Fig. A host machine runs a container runtime, which manages multiple containers simultaneously. Each container bundles its own application code, runtime version, libraries, and configuration, keeping them isolated from one another.*

### !callout-info

## Containers Are Not Virtual Machines

If you have heard of virtual machines (VMs), you might be wondering how containers differ. Both technologies create isolated environments for running software, but they do so at different layers of the system. We will explore this distinction in detail in the next lesson, The Compute Spectrum. For now, the key thing to know is that containers are significantly lighter and faster to start than virtual machines, which makes them well-suited for the kinds of dynamic, scalable cloud deployments we will discuss throughout this curriculum.

### !end-callout

The industry term for a container that has been started and is actively running is a running container instance. The distinction between a container and a running container instance will become clearer in the next section when we look at container images.

### Container Components: Image, Runtime, and Registry

Before a container can run, it has to:
1. Be built from a blueprint
2. Be executed by a piece of software
3. Be stored somewhere it can be retrieved and deployed. 

These three responsibilities map to the **image**, **runtime**, and **registry**.

#### The Image

A container **image** is a read-only blueprint that defines everything a container needs to run: the application code, the runtime, the libraries, and the configuration. When a container is started, it is created from an image. The image is the static definition that does not change and the running container is the live instance produced from it.

The image itself never changes. It is the static definition, and the running container is the live instance produced from it. This is the same relationship as a class and an object in object-oriented programming: the class defines the blueprint, and the object is what gets created when that blueprint is executed. Just as multiple objects can be instantiated from the same class, multiple containers can be created from the same image.

An image is defined by a file called a Dockerfile. A Dockerfile is a plain text file that contains a set of instructions for assembling the image, such as which base operating system to use, which dependencies to install, and which command to run when the container starts. When those instructions are executed, the result is a built image that can be run anywhere the container runtime is available.

#### The Container Runtime

The container **runtime** is the software responsible for building images and starting, stopping, and managing containers on a host machine. When a developer runs a container, it is the runtime that reads the image, sets up the isolated environment, and starts the process inside it. Without a runtime, an image is just a file that cannot execute on its own.

Docker is the most widely used container runtime . However, the container ecosystem has matured and other runtimes such as containerd and Podman are also common in production environments. The important concept is not the specific tool but the role it plays: the runtime is the layer that sits between the image and the running container.

#### The Registry

A container **registry** is a storage and distribution system for container images. Once an image has been built, it needs to be stored somewhere so that it can be retrieved and deployed, whether by a teammate, a deployment pipeline, or a cloud provider's platform. A registry serves this purpose. The most widely known public registry is Docker Hub, where developers can publish and download images. Major cloud providers also offer their own managed registries for teams that prefer to keep their images private and close to their deployment infrastructure.

The workflow that connects these three components follows this pattern: 
1. A developer writes a Dockerfile and builds an image
2. They push that image to a registry
3. A deployment system pulls the image from the registry and hands it to the runtime to create a running container. 

This push and pull model might feel familiar because it mirrors how teams use version control: code is pushed to a remote repository and pulled down where it is needed.

### Where Containers Fit in the Cloud Ecosystem

Containers are a foundational building block of modern cloud infrastructure. When applications are deployed to the cloud, they are very often packaged and shipped as container images. This is true whether a team is running a single small application or a large system made up of many independent services.

Since containers are portable and self-contained, they can run consistently across different environments, from a developer's laptop to a remote server managed by a cloud provider. This portability is a large part of why containers have become the standard unit of deployment in the industry.

As the number of containers in a system grows, managing them manually becomes impractical. Container orchestration is the automated management of containers across multiple machines, including starting and stopping containers, scaling them up or down based on demand, and recovering from failures. Kubernetes is one of the most widely adopted orchestration platform in the industry. Container orchestration is out of scope for this lesson, but feel free to explore your curiosity.

## Summary

A container is a lightweight, portable unit of software that packages an application together with everything it needs to run. By bundling the application code, runtime, libraries, and configuration into a single self-contained unit, containers solve the environment inconsistency problem that arises when code moves from development to production.

Three components work together to make containers function in practice. The image is the static blueprint that defines the container, built from a set of instructions in a Dockerfile. The runtime is the software that takes that image and creates a running container instance from it. The registry is the storage and distribution system where images are pushed after they are built and pulled from when they are deployed. Together, these three components form the foundation of how containerized applications are built, stored, and delivered in modern software development.

## Check for Understanding

### !challenge
<!-- Question 1 -->
* type: multiple-choice
* id: 03bef6eb-d549-4554-958e-c8b9e4cc1274
* title: Introduction to Containers

##### !question
A developer builds a web application on their laptop running macOS. A teammate pulls the same code and tries to run it on a Windows machine with a different version of Node.js installed. The app crashes. How would containerizing the application prevent this problem?
##### !end-question

##### !options
a| It would install the correct version of Node.js on the teammate's Windows machine automatically.
b| It would package the application together with the correct version of Node.js so the app runs in a consistent environment regardless of what is installed on the host machine.
c| It would notify the developer that the teammate is running an incompatible version of Node.js.
d| It would convert the application code so it is compatible with any version of Node.js.
##### !end-options

##### !answer
b|
##### !end-answer

#### !explanation 
A container bundles the application together with everything it needs to run, including the runtime. This means the application always runs in the same environment regardless of what is installed on the host machine.
#### !end-explanation 
### !end-challenge

<!-- Question 2 -->
### !challenge
* type: ordering
* id: 716299ac-4c7a-4c97-acbe-bf7f36404c16
* title: Introduction to Containers

##### !question
Your team has finished building a container image for your API and needs to make it available for deployment. Arrange the steps below in the correct order to so that an image moves from your local machine to a running container in production.
##### !end-question

##### !answer
1. Write a Dockerfile
1. Build the image
1. Push the image to a registry
1. Pull the image and run it with a container runtime
##### !end-answer

#### !explanation 
The three components of a container work together in a consistent sequence. The Dockerfile defines the image, the image is built and pushed to a registry for storage and distribution, and the runtime pulls that image and creates a running container from it.
#### !end-explanation 
### !end-challenge