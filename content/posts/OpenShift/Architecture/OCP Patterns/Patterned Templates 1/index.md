---
title: "Design Pattern Templates for OpenShift: Part 1"
date: 2023-07-26
description: "Delve into the specific details of OpenShift's top platform patterns: Serverless, Virtualization, and DevSecOps. Understand the key components and integrations, and explore how to optimize your platform through optional integrations and complementary patterns."
tags: ["OpenShift", "Serverless", "Virtualization", "DevSecOps", "Shift-Left", "Patterns"]
categories: ["Technical", "Design Guide"]
topics: ["OpenShift", "Kubernetes", "Solution Architecture", "Hybrid Cloud", "Platform Engineering"]
series: ["OCP Solution Architectures"]
series_order: 4
---

## Introduction

In previous entries of this series, we explored the **Top Design Patterns for the Modern Hybrid Cloud with OpenShift**. Building on that foundation, today we delve deeper into three specific platform patterns: **Serverless**, **Virtualization**, and **DevSecOps** (or Shift-Left). The mastery of these patterns will allow you to build and manage OpenShift clusters that are robust, resilient, and perfectly tailored to your needs.

Each of these patterns comes with its set of components and integrations. But worry not, we will dissect each one to understand what they do and how they contribute to the larger pattern. Further, we will discuss optional integrations and complementary patterns for added functionality and capabilities.

So, buckle up and prepare for an immersive journey into the world of OpenShift clusters.

## Serverless Pattern

A **Serverless** pattern provides the ability to run applications without having to manage underlying servers. It automatically scales from zero to potentially thousands of instances based on demand, only charging for the actual compute consumption.

Despite what one might think based on the name, Serverless workloads still rely on underlying servers for compute; however, workloads and users aren't involved in the management of the hardware.

Key components include:

* [{{< icon "knative" >}}](knative.svg) **OpenShift Serverless**: Based on [Knative](https://knative.dev/docs/), a Kubernetes platform, to deploy and manage modern serverless workloads. It enables auto-scaling, routing, eventing, and many more for a seamless serverless experience. Think AWS Lambda, but on OpenShift instead of AWS.
* [{{< icon "tekton" >}}](tekton-icon-color.svg) **OpenShift Pipelines**: [Tekton](https://tekton.dev/) provides Kubernetes resources for declaring CI/CD-style pipelines, allowing for serverless workload deployment infrastructure to run alongside the final product.

Optional integrations and Patterns might include:

* [{{< icon "kafka" >}}](apache_kafka_wordtype.svg) **Event Streams**: Kafka/[OpenShift Streams](https://www.redhat.com/en/blog/introducing-red-hat-openshift-streams-apache-kafka) and other Event-streaming patterns are often used alongside Serverless, as the events can trigger serverless workloads.
* [{{< icon "quarkiverse" >}}](quarkus-logo.svg) **Quarkus**: [Quarkus](https://quarkus.io/) is a Java Stack that dramatically reduces start-up times for applications, and thus is well suited for Serverless patterns.
* [{{< icon "rhacm" >}}](rhacm.svg) **ACM**: [Advanced Cluster Management](https://www.redhat.com/en/technologies/management/advanced-cluster-management) (*ACM*) provides multi-cluster and Hybrid Cloud capabilities, bringing Serverless workloads to any environment, including the Edge.

Complementary patterns could be **Event-Driven Architecture**, **Edge**, and **Hybrid Cloud**.

{{< alert icon="knative" >}}
Check out my [series on Serverless](/series/serverless-servers/) for more deep dives on the topic!
{{< /alert >}}

## Virtualization Pattern

The **Virtualization** pattern is perfect for managing both virtualized workloads alongside containerized applications in a hybrid cloud environment. A bare-metal deployment is recommended for this pattern to offer the best performance and features.

{{< lead >}}
[Kubevirt v1.0](https://kubevirt.io/2023/KubeVirt-v1-has-landed.html) was recently released in July 2023, which is an important event in the move to Kubernetes-based Virtualization!
{{< /lead >}}

Key components and integrations include:

* [{{< icon "ocp-virt" >}}](ocp-virt-red-circle.png) **OpenShift Virtualization**: Based on [Kubevirt](https://kubevirt.io/), OCP Virt allows you to run and manage virtual machines alongside containerized applications.
* [{{< icon "tekton" >}}](tekton-icon-color.svg) [{{< icon "argo" >}}](argo.svg) **OpenShift Pipelines & GitOps**: Based on [Tekton](https://tekton.dev/) and [ArgoCD](https://argo-cd.readthedocs.io/en/stable/), respectively, these OpenShift integrations provide full CI/CD capabilities within the cluster, allowing both the VMs and any relevant workloads to be deployed and managed from one location.
* **Containerized Data Importer (CDI)**: CDI simplifies the process of moving and managing data between platforms and virtual machines.
* **Local Storage Operator**: Helps to manage local volume provision, creating a persistent volume on individual nodes.
* **Observability Capabilities**: Various tools provide monitoring, alerting, and tracing to track application and system health.

Optional integrations might be:

* [{{< icon "odf" >}}](odf.svg) **OpenShift Data Foundations**: Based on technologies such as [Ceph](https://ceph.io/en/), [Rook](https://rook.io/), and [Noobaa](https://www.noobaa.io/), [ODF](https://www.redhat.com/en/technologies/cloud-computing/openshift-data-foundation) provides clustered storage capabilities, enhancing Disaster Recovery and storage performance for all attached workloads.
* **OpenShift Migration Toolkit for Containers (MTC)**: Can ease the transition from older systems to your modern hybrid cloud platform.

Complementary patterns could be **Microservices** and **Edge Computing**.

{{< alert icon="ocp-virt" >}}
Check out my [series on KubeVirt](/series/virtually-containers/) for more deep dives on the topic!
{{< /alert >}}

## DevSecOps (Shift-Left) Pattern

DevSecOps, or **Shift-Left**, integrates security into the DevOps pipeline instead of treating it as an afterthought. This pattern encourages security measures to be implemented from the get-go, resulting in more secure applications.

Key components and integrations include:

* [{{< icon "tekton" >}}](tekton-icon-color.svg) [{{< icon "argo" >}}](argo.svg) **OpenShift Pipelines & GitOps**: Specifically for Security, OpenShift Pipelines plays a crucial role with the Tekton Chains functionality.
* [{{< icon "rhacs" >}}](rhacs.svg) **Red Hat Advanced Cluster Security**: [Advanced Cluster Security](https://www.redhat.com/en/technologies/cloud-computing/openshift/advanced-cluster-security-kubernetes) (ACS), formerly **Stackrox**, provides a K8s-native container security platform. Particularly when paired with the new **Trusted Software Supply Chain**, this integration is crucial for DevSecOps.
* **OpenShift Security (Kyverno, Gatekeeper)**: Foundational security concepts in OpenShift that provide inherent security.

Optional integrations could be:

* **Vulnerability Management Tools**: Tools such as **Quay Security Operator** provide image scanning capabilities to ensure secure application delivery.

Complementary patterns might be **Continuous Deployment** and **Policy-Driven Deployment**.

{{< alert icon="opa" >}}
Check out my [series on Software Supply Chain Security](/series/securing-your-supply-chain/) for more deep dives on the topic!
{{< /alert >}}

## Conclusion

OpenShift patterns offer powerful ways to tailor your clusters to your specific needs. By understanding the components and integrations of each pattern, you can create a robust and secure infrastructure that aligns with your organization's goals. Remember, OpenShift's strength lies in its flexibility, and the Serverless, Virtualization, and DevSecOps patterns represent just a fraction of what's possible with this potent platform.

## Additional Reading

* [Building a DevSecOps culture and shifting security left](https://www.redhat.com/en/blog/building-devsecops-culture-and-shifting-security-left)
* [9 Tips on Shifting Left for Security and DevSecOps | GitLab](https://about.gitlab.com/blog/2020/06/23/efficient-devsecops-nine-tips-shift-left/)
* [Portfolio Architecture | Red Hat](https://www.redhat.com/architect/portfolio/)
* [Enable Architect](https://www.redhat.com/architect/)
