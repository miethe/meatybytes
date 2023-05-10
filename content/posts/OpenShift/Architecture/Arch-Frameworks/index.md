+++
title = "Architectural Frameworks"
description = '''
Learn everything about Architectural Frameworks
'''
Date = 2023-04-10
author = "Nick Miethe"
categories = ["Technical"]
topics = ["OpenShift"]
tags = ["SOA", "Microservices", "Architecture", "EDA"]
+++

## What is an Architectural Framework

## Popular Frameworks: General Applications

There have been numerous developments around architecture standards as the industry has evolved, starting with Single-tier Architecture for Cobol and other 3G languages, to N-Tier and Microservices with containerized workloads.

In this context, app *tiers* are the separation of components an application. For example, the move from Single- to Two-Tier Architectures was the separation of data from the UI and Logic, with the introduction of DBs. This introduced the role of DBAs. Three-Tier was again the separation of the UI and Logic, eventually evolving further into MVC, with a *Controller* between the Logic (Model) and UI (View). This introduced still popular app/dev frameworks like .Net and Java Beans.

![](/images/app-arch-timeline.png)

Below is a list of some of the common architectures currently used in modern systems:

* Model View Controller (MVC)
* N-Tier Architecture
* Service-Oriented Architecture (SOA)
  * Event-Driven Architecture (EDA)
  * Web-Oriented Architecture (WOA)
* Microservice Architecture (MSA)

### Layered and Segmented Architecture (Centralized)

The above architectures were traditionally considered **Layered Architectures**, with each layer responsible for a specific set of functions or services. Layers depend only on the layer below, and provide services to the layer above. While this approach works well for well-defined systems, it is inflexible and rigid, non-conducive to modern distributed systems, or where components bridge various technologies.

This need introduced **Segmented Architectures**. This approach separates systems into modules or *segments* (also called pods), each of which has its own set of functions or services. There are various methods of segmentation being utilized today, including:

* Runtime Partitioning
* Multi-tenancy
* Platform of Platforms

These methods are all supported by various modern technologies, including:

* Microservices and Containerization
* Service Meshes
* IaC, CI/CD, and GitOps

### Layered and Segmented - Examples

Many modern Architecture frameworks, such as *MSA*, can be structured as **Layered** (ie LXC) or **Segmented** (ie Docker).

In a **Layered Microservice Architecture**, each layer of a service is responsible for a specific aspect of the application. For example, the presentation layer might handle user interface components, the business logic layer might handle data processing and manipulation, and the data storage layer might handle data storage and retrieval. Each layer would consist of one or more microservices that are responsible for implementing the specific functionality of that layer.

{{< alert icon="docker" >}}
LXC can be used as an example of a Layered Microservice Architecture, as it provides a lightweight virtualization solution that can be used to create isolated environments for each layer of the architecture.
{{< /alert >}}

In a **Segmented Microservice Architecture**, each segment is responsible for a specific business capability or service. For example, one segment might handle customer management, another might handle order processing, and another might handle payment processing. Each segment would consist of one or more microservices that are responsible for implementing the specific functionality of that segment.

{{< alert icon="docker" >}}
Docker containers can be used as an example of a Segmented Microservice Architecture, as they provide a lightweight and portable way to package and deploy individual microservices, which can then be orchestrated and managed using container orchestration tools like Kubernetes.
{{< /alert >}}

In fact, many Microservice Architectures use a combination of both approaches, with logical layers used to organize services within each segment or domain. Additionally, there are other architectural patterns and approaches that can be used in conjunction with Microservice Architectures, such as event-driven architectures, reactive architectures, and service-oriented architectures.

For more information, I recommend the paper from WSO2 on [Layered and Segmented Architectures](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-layered-segmented.md).

### Distributed Architectures

Layered and Segmented Architectures are generally considered Centralized Architectures, and are highly focused around APIs in different forms. Because Distributed Architectures and their associated systems often follow such different frameworks, they will largely be covered in the next section.

Also, many of the above architectures can also be considered Distributed, depending on the design of the system. For example, Microservices might be distributed across many systems, or deployed to a single server or localized cluster of servers.

## Popular Frameworks: Cloud & Distributed Systems

While Cloud-native and Kubernetes-based architectures are not inherently distributed, the methodologies of their design often cross-paths.

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

### Kubernetes References

* [An illustrated guide to 12 Factor Apps](https://www.redhat.com/architect/12-factor-app)
* [12 Factor App meets Kubernetes: Benefits for cloud-native apps](https://www.redhat.com/architect/12-factor-app-containers)
* [Top 10 must-know Kubernetes design patterns](https://developers.redhat.com/blog/2020/05/11/top-10-must-know-kubernetes-design-patterns)

## Examples

* [Clean Architecture Example (Java)](https://github.com/mattia-battiston/clean-architecture-example)
