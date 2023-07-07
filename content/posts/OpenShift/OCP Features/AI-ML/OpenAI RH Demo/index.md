---
title: "Leveraging OpenAI's LLM on OpenShift for Technical Q&A: A Case Study with Red Hat Documentation"
description: "Using OpenAI's LLM on OpenShift for Technical Questions: A Case Study with Red Hat Documentation"
date: 2023-05-14
author: Nick Miethe
tags: ["OpenAI", "OpenShift", "Open Data Hub", "Kubeflow", "AI/ML", "LLM", "Red Hat", "Demo", "Seldon", "Model Training"]
categories: ["Technical", "Guide", "Demo"]
topics: ["Data Science", "OpenShift"]
series: ["OpenAI on OpenShift"]
series_order: 3
---

## Synopsis

In this follow-up post, we're going to explore how to leverage a locally hosted OpenAI Large Language Model (LLM) on an OpenShift cluster to answer specific technical questions based on a given documentation set. Basically, a locally-hosted and specially trained ["ChatGPT"](https://chat.openai.com).

We'll use Red Hat's extensive documentation as our training set. We'll also provide specific instructions and example configurations necessary to train the LLM via Kubeflow and Open Data Hub (ODH) on this set. Lastly, we'll show you how to serve the model on the cluster and how it can be accessed and utilized.

## Introduction

Large Language Models like OpenAI's GPT-4 have proven to be excellent at understanding and generating human-like text. This capability can be harnessed to create systems that can answer specific technical questions based on a provided documentation set. In this guide, we'll train such a model on Red Hat's documentation.

## Training OpenAI LLM on Red Hat Documentation

Training the LLM on Red Hat's documentation involves two main steps: data preparation and model training.

### Data Preparation

1. **Collect the Documentation Data:** Download the documentation from the Red Hat website. This documentation would be in HTML or PDF format, so you'll need to convert it into plain text for training.

2. **Preprocessing:** Clean the text data by removing unnecessary elements like footers, headers, and navigation links. Also, format the data as per the requirements of OpenAI's training process.

### Model Training using Kubeflow and ODH

We'll use Kubeflow's `TFJob` for training the model and ODH for data ingestion and storage.

1. **Create a `TFJob` for Model Training:**

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

Replace `<training-image>` with the Docker image of your OpenAI LLM training script and `<your-pvc>` with your Persistent Volume Claim for data storage.

### Serving the Model

We'll use Seldon, available in ODH, to serve our trained model.

1. **Create a SeldonDeployment:**

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
        - image: <openai/llm-trained-image>
          name: classifier
    graph:
      children: []
      endpoint:
        type

: REST
      name: classifier
      type: MODEL
    name: default
    replicas: 1
EOF
```

Replace `<openai/llm-trained-image>` with the Docker image of your trained OpenAI LLM.

## Accessing and Utilizing the Model

Once your model is served, it can be accessed through the REST API endpoint that Seldon provides. This endpoint can be used to ask technical questions based on Red Hat's documentation.

For example, you can send a POST request with a question as input to the model:

```bash
curl -X POST http://<seldon-endpoint>/api/v0.1/predictions -H 'Content-Type: application/json' -d '{"strData": "What is the command to create a new project in OpenShift?"}'
```

The model will return the answer based on its training on the Red Hat documentation.

## Conclusion

By training an OpenAI LLM on a specific documentation set like Red Hat's and serving it on an OpenShift cluster, we can create a powerful tool for answering technical questions. This approach can greatly enhance productivity by providing quick, accurate answers to technical questions based on authoritative sources.

## References

1. [OpenShift Documentation](https://docs.openshift.com/)
2. [Open Data Hub Documentation](https://opendatahub.io/docs.html)
3. [Kubeflow Documentation](https://www.kubeflow.org/docs/)
4. [Red Hat Documentation](https://access.redhat.com/documentation/)
5. [OpenAI Large Language Models](https://platform.openai.com/docs/guides/large-language-models)
