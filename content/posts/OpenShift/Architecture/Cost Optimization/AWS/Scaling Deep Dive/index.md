---
title: "Autoscaling Deep-Dive: Configuring Cluster and Machine Autoscalers on OpenShift"
date: 2023-06-14
author: "Nick Miethe"
tags: ["OpenShift", "Cloud", "AWS", "Performance", "Autoscaling"]
categories: ["OpenShift"]
description: "Master the art of autoscaling on OpenShift. Get hands-on with detailed instructions for configuring the cluster and machine autoscalers on AWS, optimizing performance and efficiency."
draft: true
---

## Introduction

Autoscaling is a crucial tool in any cloud-based deployment strategy, as it helps optimize both cost and resource efficiency. When deploying OpenShift Container Platform (OCP) on Amazon Web Services (AWS), leveraging the power of autoscaling becomes even more important. In this post, we'll take a deep dive into the world of autoscaling on OCP, focusing on configuring the cluster and machine autoscalers.

## Setting Up Cluster Autoscaling

The Cluster Autoscaler in OpenShift increases or decreases the size of the cluster based on the number of pending pods and the resource utilization of nodes. To set it up, you first need to create an AWS Autoscaling Group, and then an OpenShift MachineSet that corresponds to it.

!IMAGE-HERE! (Example image: Diagram showing the link between AWS Autoscaling Group and OpenShift MachineSet)

1. **AWS Autoscaling Group Configuration**:

- Navigate to the AWS Management Console, select "EC2," and then "Auto Scaling Groups."
- Click on "Create an Auto Scaling group" and follow the prompts, ensuring that you choose the correct instance type and VPC for your OpenShift nodes.

1. **OpenShift MachineSet Configuration**:

- Log into the OpenShift web console and navigate to "Compute" > "MachineSets."
- Click on "Create MachineSet," and then define the MachineSet parameters. Ensure that the MachineSet matches the AWS Autoscaling Group you created earlier.

1. **Cluster Autoscaler Configuration**:

- Now that the MachineSet is created, you can configure the Cluster Autoscaler. You create a `ClusterAutoscaler` object that defines the parameters for the autoscaler.

Here is a sample configuration:

```yaml
apiVersion: autoscaling.openshift.io/v1
kind: ClusterAutoscaler
metadata:
  name: default
spec:
  scaleDown:
    enabled: true
    delayAfterAdd: 30s
    delayAfterDelete: 30s
    delayAfterFailure: 30s
    maxNodesTotal: 10
```
With this configuration, the autoscaler will start downscaling nodes 30 seconds after scaling up, 30 seconds after deleting a node, and 30 seconds after a previous failed attempt to scale down.

## Configuring Machine Autoscaling

The Machine Autoscaler in OpenShift increases or decreases the number of machines in a MachineSet. It's especially useful for workloads where demand can vary, requiring the number of machines to adjust accordingly.

Here are the steps to set it up:

1. **MachineSet Selection**:

- Navigate to "Compute" > "MachineSets" in the OpenShift web console.
- Select the MachineSet that you want to autoscale.

1. **Machine Autoscaler Configuration**:

- To autoscale the selected MachineSet, create a `MachineAutoscaler` object.

Here is a sample configuration:

```yaml
apiVersion: autoscaling.openshift.io/v1beta1
kind: MachineAutoscaler
metadata:
  name: example-machineset-1
  namespace: openshift-machine-api
spec:
  minReplicas: 1
  maxReplicas: 5
  scale

TargetRef:
    apiVersion: machine.openshift.io/v1beta1
    kind: MachineSet
    name: example-machineset-1
```

With this configuration, the number of machines in `example-machineset-1` will scale between 1 and 5 based on demand.

## Conclusion

Autoscaling in OpenShift provides an effective way to manage resources and optimize costs in a dynamic environment. Proper setup and management of Cluster and Machine Autoscalers can significantly improve the performance and efficiency of your OCP deployments on AWS. Stay tuned for more deep-dives into OpenShift performance tuning and cost optimization!

## References

1. [Cluster Autoscaling on AWS](https://docs.openshift.com/container-platform/4.7/machine_management/applying-autoscaling.html)
2. [Machine Autoscaler](https://docs.openshift.com/container-platform/4.7/machine_management/applying-autoscaling.html##machine-autoscaler-cr_applying-autoscaling)
