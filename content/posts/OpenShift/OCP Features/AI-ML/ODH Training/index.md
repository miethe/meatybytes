---
title: "Simplifying AI/ML Pipelines with Open Data Hub and Kubeflow on OpenShift"
date: 2023-05-18
author: Nick Miethe
tags: ["OpenShift", "Open Data Hub", "Kubeflow", "AI/ML", "TFJob", "Model Training", "Model Testing", "ODF"]
categories: ["Technical", "Introduction"]
topics: ["Data Science", "OpenShift"]
---

## Introduction

Hello, OpenShift (and AI/ML) enthusiasts! Today, we're going to delve into the world of AI/ML pipelines, specifically focusing on how to use **Open Data Hub** (ODH) and **Kubeflow** on Red Hat OpenShift.

This post is intended to simplify the process for those of you who are less experienced with OpenShift and AI/ML tooling, and are interested in training your own ML Models. Therefore, this will be fairly introductory and brief. With that said, let's get started!

{{< alert "lightbulb" >}}
For more information on these topics, read the [original Red Hat post](https://developers.redhat.com/blog/2019/12/16/ai-ml-pipelines-using-open-data-hub-and-kubeflow-on-red-hat-openshift) that inspired this summary.

Also, check out our various series around AI/ML for (much) deeper dives:
[OpenAI on OpenShift](/series/openai-on-openshift/) and [AI/ML OCP Tooling](/series/ai/ml-ocp-tooling/).
{{< /alert >}}

## AI/ML Pipelines: A Brief Overview

AI/ML pipelines are integral to optimizing a production-level artificial intelligence/machine learning process. They create workflows that are repeatable, automated, customizable, and intelligent. These pipelines automate functionalities such as data extract, transform, and load (ETL), model training, model evaluation, and model serving.

## Training a New Model with ODH: A Step-by-Step Guide

Training a new model with ODH involves several steps. Let's break them down:

### Step 1: Storing Training Data for Ingest

The training data is typically stored in a distributed file system that is accessible by all the nodes in your OpenShift cluster. This could be a cloud-based storage system like Amazon S3, or a distributed file system like Ceph.

{{< lead >}}
See our [recent post]({{< ref "odh training" >}}) on OpenShift Data Foundations for AI/ML Workloads for an OCP-native S3 solution.
{{< /lead >}}

### Step 2: Preparing Your Data

The data should be in a format that your chosen ML framework can ingest. For TensorFlow, this is typically in the form of TFRecords, but other formats like CSV or JSON can also be used. You can create your own datasets or find publicly available ones online.

### Step 3: Collecting Evaluation Metrics

Evaluation metrics are crucial for understanding the performance of your model. These can be collected using Kubeflow's metrics collector, which automatically scrapes metrics from your TensorFlow logs and stores them for later analysis.

### Step 4: Testing the Model

Automated testing of the model can be done using Kubeflow's pipeline system. This allows you to define a series of steps, including model training, evaluation, and serving, and then run these steps automatically whenever new data is available.

{{< lead >}}
Check out this post for considerably more information - [Testing Machine Learning Models](https://serokell.io/blog/machine-learning-testing).
{{< /lead >}}

## Conclusion

AI/ML pipelines using Open Data Hub and Kubeflow on Red Hat OpenShift can simplify and streamline your machine learning workflows. While there is a learning curve involved, understanding these tools and processes can greatly enhance your productivity and effectiveness as an OpenShift engineer.

## References

1. [AI/ML pipelines using Open Data Hub and Kubeflow on Red Hat OpenShift | Red Hat Developer](https://developers.redhat.com/blog/2019/12/16/ai-ml-pipelines-using-open-data-hub-and-kubeflow-on-red-hat-openshift)
2. [TensorFlow TFJob | Kubeflow](https://www.kubeflow.org/docs/components/training/tftraining/)
3. [How to Test Machine Learning Models](https://deepchecks.com/how-to-test-machine-learning-models/)
4. [Open Data Hub | Red Hat](https://opendatahub.io/)
5. [opendatahub-io/kubeflow {{< icon "github" >}}](https://github.com/opendatahub-io/kubeflow/tree/master)
