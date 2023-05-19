---
title: "Deploying OpenAI's Large Language Models on OpenShift"
date: 2023-05-12
author: Nick Miethe
tags: ["OpenAI", "OpenShift", "Open Data Hub", "Kubeflow", "AI/ML", "LLM", "Seldon", "TFJob"]
categories: ["Technical"]
topics: ["OpenShift"]
series: ["OpenAI on OpenShift"]
series_order: 2
---

In this guide, we'll walk you through the steps required to deploy an OpenAI Large Language Model (LLM) to an OpenShift Container Platform (OCP). We'll also delve into how tools like the Open Data Hub (ODH) and Kubeflow can be utilized for training these models. Finally, we'll discuss why hosting your own LLM on OCP is a great option and the specific software and hardware requirements you'll need.

## Introduction

Large Language Models like OpenAI's GPT-4 can be highly resource-intensive, requiring significant computational power for both training and inference. Hosting these models in-house can offer several advantages, such as increased data privacy and control, reduced latency, and the possibility of customizing the models to your specific needs.

OpenShift, a leading enterprise Kubernetes platform by Red Hat, offers a robust environment for deploying such models. Its rich ecosystem of tools and services like Open Data Hub and Kubeflow make it an excellent choice for managing and scaling machine learning workloads.

## Deploying an OpenAI LLM on OpenShift

Before we start, ensure that your OpenShift cluster is up and running. You should also have the Open Data Hub and Kubeflow installed in your environment. You can find more information on this process in the [AI/ML Tooling Series](/series/ai/ml-ocp-tooling/).

### Software and Hardware Requirements

- OpenShift Container Platform 4.6 or later
- Open Data Hub 0.8.0 or later
- Kubeflow 1.0 or later
- A machine with a minimum of 32GB RAM and 16 cores for the OpenShift cluster.
- High-performance GPU(s) for training the LLM.

### Configuration

1. **Create a new project in OpenShift:**

```bash
oc new-project openai-llm
```

1. **Deploy the OpenAI LLM:**

Here, we'll use the Open Data Hub's [Seldon](https://www.seldon.io/) component for deploying the model. Seldon provides a simple, flexible way to deploy any machine learning model on Kubernetes.

```yaml
# Create a SeldonDeployment configuration
cat <<EOF | oc apply -f -
apiVersion: machinelearning.seldon.io/v1
kind: SeldonDeployment
metadata:
  name: openai-llm
spec:
  predictors:
  - componentSpecs:
    - spec:
        containers:
        - image: <openai/llm-image>
          name: classifier
    graph:
      children: []
      endpoint:
        type: REST
      name: classifier
      type: MODEL
    name: default
    replicas: 1
EOF
```

Replace `<openai/llm-image>` with the Docker image of your OpenAI LLM.

### Training the LLM with Kubeflow

Kubeflow can be utilized to manage the LLM training process. To do this, you can create a `TFJob` or `PyTorchJob` (depending on the framework you're using) to train the model.

```yaml
apiVersion: "kubeflow.org/v1"
kind: "TFJob"
metadata:
  name: "openai-llm-training"
spec:
  tfReplicaSpecs:
    Worker:
      replicas: 2
      restartPolicy: OnFailure
      template:
        spec:
          containers:
            - name: tensorflow
              image: <training-image>
              args:
                - "--epochs=10"
                - "--data-dir=/mnt/data"
          volumes:
          - name: data
            persistentVolumeClaim:
              claimName: <your-pvc>
          volumeMounts:
          - mountPath: "/mnt/data"
            name: data
```

## Why Host Your Own LLM on OCP?

There are several reasons why hosting your own LLM on OCP is a good idea:

- **Data Privacy:** By hosting in-house, you can ensure your sensitive data never leaves your network.
- **Control:** You have complete control over the model, allowing you to tweak and adjust as necessary.
- **Low Latency:** Hosting the model in-house reduces network latency, providing faster response times.
- **Customization:** You can customize and fine-tune the model based on your specific requirements.

## Conclusion

OpenShift provides a powerful, flexible platform for hosting machine learning models like OpenAI's LLM. With the help of tools like Open Data Hub and Kubeflow, you can easily manage and scale your ML workloads. Hosting your own LLM on OCP not only offers greater control and privacy but also allows for customization and low latency.

## References

1. [OpenShift Documentation](https://docs.openshift.com/)
2. [Open Data Hub](https://opendatahub.io/)
3. [Kubeflow Documentation](https://www.kubeflow.org/docs/)
4. [Seldon Documentation](https://docs.seldon.io/projects/seldon-core/en/latest/)
5. [OpenAI Large Language Models](https://platform.openai.com/docs/guides/large-language-models)
6. [openai/openai-cookbook {{< icon "github" >}}](https://github.com/openai/openai-cookbook)
7. [willb/openshift-ml-workflows-workshop {{< icon "github" >}}](https://github.com/willb/openshift-ml-workflows-workshop) - {{< icon "redhat" >}} Summit '19
