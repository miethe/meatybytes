+++
title = "Perspective on the Evolution of Architectural Frameworks"
description = '''
Learn everything about Architectural Frameworks for systems and applications, and how they've developed into modern cloud native architectures.
'''
Date = 2023-04-10
author = "Nick Miethe"
topics = ["OpenShift", "Kubernetes", "Solution Architecture", "Cloud Native"]
categories = ["Technical", "Design Guide", "Architecture"]
tags = ["SOA", "Microservices", "Architecture", "EDA", "Layered Architectures", "Segmented Architectures", "MSA", "Application Architecture", "System Architecture"]
series = ["App and System Architectures"]
series_order = 1
+++

## What is an Architectural Framework

An Architectural Framework is a set of best practices and guidelines for designing and operating reliable, secure, efficient, and cost-effective systems or applications. It is designed to help architects and developers make informed decisions about their architectures and identify potential issues before they become problems. Each framework provides a set of principles, questions, and best practices for designing, operating, and maintaining systems and apps in a variety of environments or deployment models.

## Popular Frameworks: General Applications

There have been numerous developments around architecture standards as the industry has evolved, starting with Single-tier Architecture for Cobol and other 3G languages, to N-Tier and Microservices with containerized workloads.

In this context, app *tiers* are the separation of components of an application. For example, the move from **Single-** to **Two-Tier Architectures** was the separation of data from the UI and Logic, with the introduction of DBs. This introduced the role of DBAs. **Three-Tier** was again the separation of the UI and Logic, eventually evolving further into *MVC*, with a *Controller* between the Logic (*Model*) and UI (*View*). This introduced still popular app/dev frameworks like .Net and Java Beans.

![](app-arch-timeline.png)

Below is a list of some of the common architectures currently used in modern systems:

* Model View Controller (MVC)
* N-Tier Architecture
* Service-Oriented Architecture (SOA)
  * Event-Driven Architecture (EDA)
  * Web-Oriented Architecture (WOA)
* Microservice Architecture (MSA)

## Layered Architecture (Centralized)

The above architectures were traditionally considered **Layered Architectures**, with each layer responsible for a specific set of functions or services. Layers depend only on the layer below, and provide services to the layer above. While this approach works well for well-defined systems, it is inflexible and rigid, non-conducive to modern distributed systems, or where components bridge various technologies.

### Layers in Modern Technologies and Practices

While new developments in the industry aren't often focused on **Layered Architectures**, many of tech's strongest foundations are textbook examples of **Layers**.

#### OSI Model

The **Open Systems Interconnection (OSI)** model is a layered architecture used to standardize the communication functions of computer networks. It consists of seven layers, with each layer responsible for a different aspect of network communication, such as physical transmission, error detection, routing, and application-level services.

#### MVC Architecture

The **Model-View-Controller (MVC) architecture** is a popular design pattern used in web applications. It consists of three layers - the model layer, which handles data and business logic, the view layer, which handles user interface components, and the controller layer, which manages user input and interaction.

#### TCP/IP Protocol Stack

The **Transmission Control Protocol/Internet Protocol (TCP/IP)** protocol stack is a layered architecture used to standardize the communication functions of the Internet. It consists of four layers - the application layer, the transport layer, the network layer, and the physical layer.

## Segmented Architecture (Centralized)

This need introduced **Segmented Architectures**. This approach separates systems into modules or *segments* (also called pods), each of which has its own set of functions or services. There are various methods of segmentation being utilized today, including:

* Runtime Partitioning
* Multi-tenancy
* Platform of Platforms

### Segmentation in Modern Technologies and Practices

These methods are all supported by various modern technologies, such as Microservices or Containerization, and are even reflected in modern team organization and best practices.

#### Service Meshes

A **Service Mesh** is a dedicated infrastructure layer for managing service-to-service communication within a microservices architecture. Service meshes can help to achieve segmentation by providing a centralized and standardized way to manage and secure communication between different services, without requiring changes to the application code. Some popular service meshes include [Istio](https://istio.io/), [Linkerd](https://linkerd.io/), and [Consul](https://developer.hashicorp.com/consul/docs/connect).

#### Infrastructure as Code (IaC)

**Infrastructure as Code** involves using code to automate the deployment and management of infrastructure resources, such as servers, networks, and databases. **IaC** can help to achieve segmentation by providing a consistent and repeatable approach to infrastructure deployment, which can help to ensure that resources are provisioned and configured correctly across different environments. [Terraform](https://www.terraform.io/) and [Ansible](https://www.ansible.com/) are very popular tools used for IaC.

#### DevOps

**DevOps** practices can help to achieve segmentation by encouraging collaboration and communication between different teams involved in software development and operations. By breaking down traditional silos between development, testing, and operations, organizations can better align their efforts around shared goals, and reduce the risk of conflicts or miscommunications.

## Layered and Segmented - Applied Examples

Many modern Architecture frameworks, such as *MSA*, can be structured as **Layered** (ie LXC) or **Segmented** (ie Docker).

In a **Layered Microservice Architecture**, each layer of a service is responsible for a specific aspect of the application. For example, the presentation layer might handle user interface components, the business logic layer might handle data processing and manipulation, and the data storage layer might handle data storage and retrieval. Each layer would consist of one or more microservices that are responsible for implementing the specific functionality of that layer.

{{< alert icon="docker" >}}
LXC can be used as an example of a Layered Microservice Architecture, as it provides a lightweight virtualization solution that can be used to create isolated environments for each layer of the architecture.
{{< /alert >}}

In a **Segmented Microservice Architecture**, each segment is responsible for a specific business capability or service. For example, one segment might handle customer management, another might handle order processing, and another might handle payment processing. Each segment would consist of one or more microservices that are responsible for implementing the specific functionality of that segment.

{{< alert icon="docker" >}}
Docker containers can be used as an example of a **Segmented Microservice Architecture**, as they provide a lightweight and portable way to package and deploy individual microservices, which can then be orchestrated and managed using container orchestration tools like Kubernetes.
{{< /alert >}}

In fact, many **Microservice Architectures** use a combination of both approaches, with logical layers used to organize services within each segment or domain. Additionally, there are other architectural patterns and approaches that can be used in conjunction with **Microservice Architectures**, such as event-driven architectures, reactive architectures, and service-oriented architectures.

For more information, I recommend the paper from **WSO2** from which many of these fundamentals were defined - [Layered and Segmented Architectures](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-layered-segmented.md).

## Distributed Architectures

**Layered** and **Segmented Architectures** are generally considered **Centralized Architectures**, and are highly focused around APIs in different forms. Because **Distributed Architectures** and their associated systems often follow such different frameworks, they would generally be defined separately.

That said, many of the above architectures can also be considered *distributed*, depending on the design of the system. For example, *Microservices* might be distributed across many systems, or deployed to a single server or localized cluster of servers.

### Cloud Native Architectures

*Cloud native* and *Kubernetes-based* systems are not necessarily inherently *distributed*, as it depends on how the system is designed and deployed.

In a **Centralized Architecture**, all components of the system are hosted on a single server or cluster of servers, with no redundancy or failover mechanisms in place. This type of architecture can be appropriate for simple, low-traffic applications, but it can also be a single point of failure and limit scalability.

In a **Distributed Architecture**, the application is broken down into smaller, independent components that can be deployed and scaled independently. These components can be hosted on multiple servers, potentially in different data centers or cloud regions, and communicate with each other over a network. This type of architecture can help to improve scalability, availability, and fault tolerance.

*Cloud native* and *Kubernetes-based* systems are often designed to be *distributed*, as they are built on a **Microservices Architecture**, which involves breaking down applications into smaller, independently deployable components. These components can be deployed and managed using containerization technologies like **Docker** or **Podman**, and orchestrated using **Kubernetes**. However, it's possible to build a *cloud native* or *Kubernetes-based* system that is *centralized*, by hosting all components on a single server or cluster of servers.

### Popular Frameworks: Cloud & Distributed Systems

While *Cloud-native* and *Kubernetes-based* architectures are not inherently distributed, the methodologies of their design often cross-paths.

* Cloud Native Architecture
* [Cell-based Architecture (CBA)](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md)
* Well-Architected Framework
* Reactive Architecture

Other Popular Models/Frameworks:

* Service Mesh Interface (SMI)
* [Open Application Model](https://oam.dev/) (OAM)
* [12 Factor App](https://12factor.net/)

## References

* [Reference Methodology for an Agile Digital Business](https://github.com/wso2/reference-methodology/blob/master/reference-methodology.md)
* [Emerging Reference Architectural Patterns](https://github.com/wso2/reference-architecture)
* [Reference Architecture for a Cloud Native Digital Enterprise](https://github.com/wso2/reference-architecture/blob/master/reference-cloud-native-architecture-digital-enterprise.md)
* [The Kubernetes Effect: Igniting Transformation in Your Team | IBM](https://www.ibm.com/cloud/blog/the-kubernetes-effect)

### Kubernetes References

* [An illustrated guide to 12 Factor Apps](https://www.redhat.com/architect/12-factor-app)
* [12 Factor App meets Kubernetes: Benefits for cloud-native apps](https://www.redhat.com/architect/12-factor-app-containers)
* [Top 10 must-know Kubernetes design patterns](https://developers.redhat.com/blog/2020/05/11/top-10-must-know-kubernetes-design-patterns)

## Examples

* [Clean Architecture Example (Java)](https://github.com/mattia-battiston/clean-architecture-example)
