---
title: "Top Design Patterns for the Modern Hybrid Cloud with OpenShift: Part 2"
author: "Nick Miethe"
date: "2023-06-10"
tags: ["Architecture", "Patterns", "OpenShift", "Kubernetes", "Cloud", "Design Patterns", "Hybrid Cloud", "Virtualization", "Security", "Shift Left", "XaC"]
categories: ["Technical", "Design Guide"]
topics: ["OpenShift", "Kubernetes", "Solution Architecture"]
series: ["OCP Solution Architectures"]
series_order: 3
description: "Learn about various design patterns for the modern hybrid cloud with OpenShift."
---

## Introduction

Hello everyone, this is Nick Miethe from MeatyBytes.io, and today we are going to continue our journey through the world of design patterns in a modern hybrid cloud environment using OpenShift. This is part 2 of our series, and if you haven't checked out [part 1]({{< ref "patterns pt 1" >}}), I highly recommend you do so to get up to speed.

In the previous post, we discussed the first five design patterns: the **Federated Identity Pattern**, the **Service Mesh Pattern**, the **Multi-Cluster Management Pattern**, the **GitOps Pattern**, and the **Serverless Pattern**. Today, we will be continuing the series by exploring five more design patterns that have proven to be highly effective in the hybrid cloud setting.

Let's get started!

## 6. Virtualization Pattern

The **Virtualization Pattern** is a design paradigm that addresses the need for running traditional workloads on modern container platforms like Kubernetes/OpenShift. It uses virtual machines (VMs) in conjunction with containers within the same platform, allowing organizations to bring conventional applications into a container-centric workflow.

**OpenShift Virtualization**, backed by the **KubeVirt** project, is a tool that enables this pattern by providing an API to combine hybrid workloads - VMs and containers. It allows developers and administrators to deploy and manage VMs alongside containers, serverless workloads, and others in OpenShift, all with the exact same tooling.

KubeVirt is designed to maintain a balance by providing virtualization capabilities while maintaining OpenShift's philosophy and semantics. It allows VMs to benefit from OCP features, such as multi-tenancy, RBAC, and integrated observability​, while incorporating workloads difficult or "impossible" to containerize.

This capability provides a unified development platform and simplifies resource management by bringing VMs under the same orchestration as containers.

{{< alert "lightbulb" >}}
Read more in my [Virtually Containers]({{< ref "/series/virtually-containers/" >}}) series!
{{< /alert >}}

**Example**: An organization migrating from a traditional VM-based infrastructure to containers could use **OpenShift Virtualization** to run their legacy applications within VMs on the same platform as their new, containerized applications. This allows for a gradual, low-risk migration strategy.

## 7. Pipeline Pattern

The **Pipeline Pattern** addresses the need for automated, reliable, and repeatable deployment processes. It outlines a structured way to define, execute, and automate the software delivery process: *build*, *test*, and *deploy*.

**OpenShift Pipelines**, powered by the **Tekton** project, embodies this pattern by providing Kubernetes-native CI/CD solutions. That means that pipeline components, such as `Tasks` and `Pipelines`, are Kubernetes objects, with the latter being a CRD. Developers can use Tekton to create comprehensive, cloud-native pipelines that integrate with their applications and services running on OpenShift. These pipelines are constructed out of building-blocks that can be used again and again, encouraging a culture of reusability.

There are several upcoming developments around OpenShift Pipelines which will further enhance this pattern. These enhancements include the Tekton **Pipeline Dashboard** in OpenShift, and similar *visualizer plugin* in the upcoming **RH Developer Hub**, and the **Tekton Catalog**. Additionally, a crucial implementation of the Pipeline Pattern is as an abstraction layer on top of other pipelines, using [Pipelines as Code](https://pipelinesascode.com/)(PaC). We will discuss this further in a future iteration on this post.

{{< alert "lightbulb" >}}
Read more in my introductory post on [OpenShift Pipelines]({{< ref "pipelines intro" >}})!
{{< /alert >}}

**Example**: A software team using **OpenShift Pipelines** can construct a pipeline to automatically build, test, and deploy their application whenever changes are pushed to their code repository. This approach ensures consistency in deployment processes and reduces manual intervention, thus minimizing the risk of deployment-related errors.

## 8. Observability Pattern

The **Observability Pattern** is a design approach to make applications and systems more understandable and diagnosable. It involves the gathering, visualizing, and analyzing of metrics, logs, and traces to get a comprehensive view of a system's behavior in a distributed environment.

OpenShift provides tools like **OpenShift Logging** and **Monitoring**, with **OpenShift Distributed Tracing** solution coming soon, all of which enable this pattern. Additionally, there are a number of other integrations for each pillar of Observability, providing several supported options on OpenShift. These tools provide a centralized way to collect and view logs, monitor system health, and trace transactions across microservices.

{{< alert "lightbulb" >}}
Observability is a massive topic, especially on OpenShift, so stay tuned for a post (or series) digging in deeper!
{{< /alert >}}

**Example**: A complex microservices application running on OpenShift could use these tools to trace a user's journey through the various services, monitor the performance and health of individual services, and collect and analyze logs to diagnose any issues that arise, all within a "single-pane-of-glass".

## 9. XaC Pattern

The **XaC (Everything as Code) Pattern** is a design principle in DevOps and cloud-native development for managing and provisioning computing resources where all aspects, such as infrastructure setup, policy definitions, and even security configurations, are defined as code. It allows developers and operators to define, automate, and manage all aspects of a system using code, even allowing it to be version-controlled.

**Open Policy Agent** (OPA) for Policy as Code, Terraform for Infrastructure as Code (IaC), and Ansible for configuration as code (CaC) are example tools that embody this pattern. They allow the definition and management of cloud resources and policies as code, which can then be versioned, tested, and deployed just like application code.

{{< alert "lightbulb" >}}
Read more in my introductory post on [OpenShift Pipelines]({{< ref "pipelines intro" >}})!
{{< /alert >}}

**Example**: An organization can use Terraform to define the infrastructure required for their application, Ansible to manage the configuration of this infrastructure, and OPA to enforce security and compliance policies. All these definitions can be versioned and managed in a source control system, enabling the same level of repeatability, testability, and accountability as application code.

## 10. Shift Left Pattern (SCC, RHACS, Tekton Chains, Compliance Operator)

The **Shift Left Pattern** is a design principle in software development that advocates for dealing with bugs, security issues, and other concerns as early as possible in the development cycle. This approach minimizes the cost and complexity of dealing with these issues later on. Considering OpenShift's position as a *developer's paradise*, providing the ideal development environment alongside a fully-integrated production platform, it is a natural extension to follow a *Shift Lift Pattern* with cluster and workload security.

OpenShift employs this pattern with tools like **Security Context Constraints** (SCC), **Red Hat Advanced Cluster Security** (RHACS), **Tekton Chains**, and the OpenShift **Compliance Operator**. These tools allow developers to implement security and compliance checks in the development and deployment stages, rather than leaving these considerations until after deployment. Additionally, RH recently announced their new upcoming offering RH's **Trusted Software Supply Chain**. Check out my post [here]({{< ref "rhtssc" >}})!

{{< alert "lightbulb" >}}
Read more in one of my many security-focused posts, such as this one on [RHACS Foundations]({{< ref "rhacs foundations" >}})!
{{< /alert >}}

**Example**: A development team could use Tekton Chains to automatically sign and verify their container images at build time, ensuring the integrity and authenticity of their software. They could also use the Compliance Operator to check their OCP configurations against industry standards and best practices, helping them detect and fix potential security or compliance issues before deploying their applications

## Conclusion

Design patterns provide an efficient way to tackle common problems in software and systems architecture. They enable us to leverage the collective experience of the developers who came before us, saving us from having to reinvent the wheel every time we face a common problem. When used correctly, these patterns can significantly improve the efficiency and maintainability of your cloud-native applications.

In this post, we continued our exploration of design patterns for the modern hybrid cloud with OpenShift. While these patterns can be quite complex, I hope I have been able to break them down and make them a little more understandable. I encourage you to try implementing these patterns in your own projects and see the benefits for yourself. And If you have come up with your own patterns over time, feel free to share them in the comments below!

Thank you for reading, and stay tuned for more exciting posts in the future!

## References

* [Top 10 Design Patterns for Kubernetes](https://developers.redhat.com/blog/2020/05/11/top-10-must-know-kubernetes-design-patterns)
* [Design Patterns: Elements of Reusable Object-Oriented Software](https://amzn.to/43yaATn)
* [Exploring Tekton's cloud native CI/CD primitives | Jetstack Blog](https://www.jetstack.io/blog/exploring-tekton/)
* [Multicluster DevSecOps | Hybrid Cloud Patterns](https://hybrid-cloud-patterns.io/patterns/devsecops/)
* [Architecture Patterns for Red Hat OpenShift on AWS | AWS Architecture Blog](https://aws.amazon.com/blogs/architecture/architecture-patterns-for-red-hat-openshift-on-aws/)
