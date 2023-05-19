---
title: "Scaling AI Workloads on OpenShift: Techniques and Best Practices"
date: 2023-05-17
author: Nick Miethe
tags: ["OpenShift", "Open Data Hub", "Kubeflow", "AI/ML", "LLM", "HPA", "VPA", "TensorFlow", "Seldon"]
categories: ["Technical", "Guide"]
topics: ["OpenShift"]
series: ["AI/ML OCP Tooling"]
series_order: 2
---

## Synopsis

OpenShift, built on Kubernetes, offers robust capabilities for scaling AI workloads, ensuring efficient resource utilization and consistent performance. In this post, we'll explore various OpenShift tools and techniques for scaling AI workloads, including Kubernetes features and AI/ML tools like Open Data Hub (ODH) and Kubeflow. We'll provide detailed technical configurations to help you optimize your AI workloads on OpenShift.

{{< lead >}}
Read our post on [ODH and Kubeflow]({{< ref "ai-ml tools" >}}) for more information.
{{< /lead >}}

## Introduction

Scaling AI workloads on OpenShift requires a combination of Kubernetes-native capabilities and specialized tools for AI/ML workloads. Let's discuss several examples and the technical configurations required for each.

## 1. Horizontal Pod Autoscaling (HPA)

HPA allows you to automatically scale your application based on CPU or memory utilization, which is especially useful for AI workloads that might have fluctuating resource requirements.

To enable HPA, create a `HorizontalPodAutoscaler` object for your AI workload:

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: ai-workload-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: ai-workload
  minReplicas: 1
  maxReplicas: 5
  targetCPUUtilizationPercentage: 80
```

This configuration sets the target CPU utilization to 80%, with a minimum of one replica and a maximum of five replicas. Kubernetes will automatically scale the number of replicas based on the CPU utilization.

## 2. Vertical Pod Autoscaling (VPA)

VPA adjusts the CPU and memory resources allocated to individual pods based on their actual usage, which is beneficial for AI workloads with changing resource requirements over time.

To enable VPA, first, install the Vertical Pod Autoscaler Operator from the OperatorHub. Then, create a `VerticalPodAutoscaler` object for your AI workload:

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: ai-workload-vpa
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: ai-workload
  updatePolicy:
    updateMode: Auto
```

This configuration will automatically adjust the CPU and memory resources of your AI workload's pods based on their usage.

## 3. Open Data Hub (ODH) and Kubeflow

ODH and Kubeflow offer various capabilities to help you scale AI workloads on OpenShift. Here are a couple of examples:

### a. Model Training with Distributed TensorFlow Jobs

Kubeflow allows you to run distributed TensorFlow training jobs using its `TFJob` custom resource. A distributed training job can scale horizontally to utilize multiple nodes or GPUs, significantly reducing training time.

```yaml
apiVersion: "kubeflow.org/v1"
kind: "TFJob"
metadata:
  name: "distributed-tensorflow-job"
spec:
  tfReplicaSpecs:
    Worker:
      replicas: 4
      restartPolicy: OnFailure
      template:
        spec:
          containers:
            - name: tensorflow
              image: <your-tensorflow-image>
              resources:
                limits:
                  nvidia.com/gpu: 1
```

This configuration

 creates a distributed TensorFlow training job with four workers, each with one GPU.

### b. Serving Models with Seldon

Seldon helps you serve your trained models and automatically scales the model serving pods based on the incoming request load. Here's an example configuration:

```yaml
apiVersion: machinelearning.seldon.io/v1
kind: SeldonDeployment
metadata:
  name: seldon-model
spec:
  predictors:
    - componentSpecs:
      - spec:
          containers:
            - image: <your-model-image>
              name: model
              resources:
                requests:
                  cpu: "0.5"
      graph:
        children: []
        endpoint:
          type: REST
        name: model
        type: MODEL
      name: default
      replicas: 1
      hpaSpec:
        minReplicas: 1
        maxReplicas: 5
        targetCPUUtilizationPercentage: 80
```

This configuration serves your model with Seldon, and uses HPA to automatically scale the serving pods based on CPU utilization.

## Conclusion

OpenShift offers a plethora of options to scale your AI workloads efficiently. From Kubernetes-native capabilities like HPA and VPA to AI/ML-specific tools like ODH and Kubeflow, you can optimize resource utilization and performance for your AI workloads on OpenShift.

## References

1. [OpenShift Documentation](https://docs.openshift.com/)
2. [Kubernetes Autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
3. [Open Data Hub Documentation](https://opendatahub.io/docs.html)
4. [Kubeflow Documentation](https://www.kubeflow.org/docs/)
5. [Seldon Documentation](https://docs.seldon.io/projects/seldon-core/en/latest/)
