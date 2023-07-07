---
title: "Exploring AI/ML Tools on OpenShift"
date: 2023-05-11
author: Nick Miethe
tags: ["OpenShift", "Open Data Hub", "Kubeflow", "AI/ML", "Operators"]
categories: ["Technical", "Guide", "Introduction"]
topics: ["Data Science", "OpenShift"]
series: ["AI/ML OCP Tooling"]
series_order: 1
---

## Synopsis

OpenShift, a powerful enterprise Kubernetes platform, offers an array of tools for Artificial Intelligence (AI) and Machine Learning (ML) workloads. In this follow-up post, we'll delve into the roles of key OpenShift AI/ML tools such as Kubeflow and Open Data Hub (ODH). We will also discuss how to deploy these tools on an OpenShift Container Platform (OCP) cluster, including hardware requirements and important considerations.

## Introduction

OpenShift provides a robust platform for developing, deploying, and scaling AI/ML workloads. Two key tools in this context are Kubeflow and Open Data Hub, each playing a vital role in managing and orchestrating AI/ML tasks.

## Open Data Hub

Open Data Hub (ODH) is a blueprint for building an AI-as-a-Service (AIaaS) platform on OpenShift. It provides a rich catalog of AI/ML components that you can install and integrate as per your needs.

ODH includes tools for:

- **Data ingestion and preparation:** Apache Kafka, Hue, etc.
- **Data storage:** Ceph, etc.
- **Data exploration and AI modeling:** JupyterHub, TensorFlow, PyTorch, etc.
- **Model serving and monitoring:** Seldon, Prometheus, Grafana, etc.

### Deploying ODH on OpenShift

To deploy ODH on OpenShift, you need an OpenShift 4.x cluster with at least 32GB of RAM and 6 vCPU. You can use the ODH Operator from the OperatorHub in OpenShift.

```bash
oc new-project odh
```

1. Install the ODH Operator:

In the OpenShift console, go to **OperatorHub**, search for "Open Data Hub" and click on **Install**. Follow the prompts to complete the installation.

1. Deploy ODH:

In the OpenShift console, go to **Installed Operators**, click on **Open Data Hub Operator**, and then **Create Instance**.

## Kubeflow

Kubeflow is an open-source machine learning toolkit for Kubernetes. It aims to make deployments of ML workflows on Kubernetes simple, portable, and scalable. Kubeflow includes services for experiment tracking (Katib), model training (TFJob, PyTorchJob), model serving (KFServing), and more.

### Deploying Kubeflow on OpenShift

To deploy Kubeflow on OpenShift, you need an OpenShift 4.x cluster with at least 32GB of RAM and 6 vCPU.

```bash
oc new-project kubeflow
```

1. Install the Kubeflow Operator:

In the OpenShift console, go to **OperatorHub**, search for "Kubeflow" and click on **Install**. Follow the prompts to complete the installation.

1. Deploy Kubeflow:

In the OpenShift console, go to **Installed Operators**, click on **Kubeflow Operator**, and then **Create Kubeflow**.

## Conclusion

With tools like Open Data Hub and Kubeflow, OpenShift offers a comprehensive ecosystem for managing AI/ML workloads. These tools provide a wide array of capabilities from data ingestion and preparation, model training, serving, to monitoring. By leveraging these tools, organizations can accelerate their AI/ML journey while ensuring scalability and flexibility.

## References

1. [Open Data Hub Documentation](https://opendatahub.io/docs.html)
2. [Kubeflow Documentation](https://www.kubeflow.org/docs/)
3. [Open Data Hub on OperatorHub](https://operatorhub.io/operator/opendatahub)
4. [Kubeflow on OperatorHub](https://operatorhub.io/operator/kubeflow)
5. [Operationalizing AI/ML for the enterprise with cnvrg.io and Red Hat OpenShift MLOps solution](https://cloud.redhat.com/blog/operationalizing-aiml-for-the-enterprise-with-cnvrg.io-and-red-hat-openshift-mlops-solution)
