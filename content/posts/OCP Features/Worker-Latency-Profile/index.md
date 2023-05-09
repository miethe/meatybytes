+++
title = "Worker Latency Profiles in OpenShift"
date = 2023-04-26
description = '''
Learn to optimize your OCP clusters for network latency
'''
tags = ["OpenShift", "Network", "Optimization", "MCO"]
author = "Nick Miethe"
+++

## Summary

I was recently asked about an unexpected new Machine Conf CR which updated the `nodeStatusUpdateFrequency` within a given `KubeletConfig`. This led to a discussion around the **Machine Config Operator** (MCO) and Worker Latency Profiles, and thus to this post.

In this blog post we will focus on **Worker Latency Profiles** in OpenShift, a feature that helps you optimize your OpenShift cluster for specific network latency requirements. I'll explain what Worker Latency Profiles are, why you might use them in an OpenShift cluster, and how high latency within environments can affect cluster stability. I'll also provide examples of how these profiles might be used and walk through a short demo.

## What are Worker Latency Profiles

Worker Latency Profiles are a feature in OpenShift that allows you to configure your cluster nodes to better accommodate specific network latency requirements. These profiles are useful in situations where your OpenShift nodes are spread across different geographic locations or when you need to optimize your cluster for certain latency-sensitive workloads.

## Why use Worker Latency Profiles

Using Worker Latency Profiles can help improve the performance of your OpenShift cluster in high latency environments. By properly configuring your nodes, you can ensure that the cluster remains stable and responsive even when there are significant variations in network latency.

High latency in your cluster can have negative effects on its performance and stability. For instance, it can cause increased response times, reduced throughput, and even node failures. Therefore, it's crucial to optimize your cluster for your specific latency requirements, and Worker Latency Profiles can help you achieve this.

## Examples of Worker Latency Profiles

Let's look at a few examples of how Worker Latency Profiles might be used:

1. **Distributed Data Centers**: If you have an OpenShift cluster that spans multiple data centers, you can use Worker Latency Profiles to ensure that the nodes in different locations are properly configured to handle the latency between them.

2. **Edge Computing**: In an edge computing scenario, you may have nodes located close to end-users to reduce latency. Configuring Worker Latency Profiles helps maintain optimal performance and stability in these environments.

3. **IoT Applications**: IoT applications often require low-latency communication between devices and the cloud. By configuring Worker Latency Profiles, you can ensure that your cluster nodes are optimized for these latency-sensitive workloads.

## Demo: Configuring Worker Latency Profiles

Below is an example configuration for Worker Latency Profiles in an OpenShift cluster. This isn't all inclusive, and should be closely configured with the Node Tuning Operator.

1. **Create a custom MachineConfigPool**: First, create a new MachineConfigPool (MCP) for the nodes that require a specific latency profile:

    ``` yaml
    apiVersion: machineconfiguration.openshift.io/v1
    kind: MachineConfigPool
    metadata:
        name: custom-latency-profile
    spec:
        machineConfigSelector:
            matchExpressions:
            - {
                key: custom-openshift.io/latency-profile,
                operator: In,
                values: ["high-latency"]
            }
        nodeSelector:
            matchLabels:
                node-role.kubernetes.io/worker: ""
    ```

2. **Label the nodes**: Next, label the nodes you want to include in the new MCP:

    ``` bash
    oc label node <node_name> custom-openshift.io/latency-profile=high-latency
    ```

3. **Create a custom KubeletConfig**: Now, create a KubeletConfig with the desired latency settings:

    ``` yaml
    apiVersion: machineconfiguration.openshift.io/v1
    kind: KubeletConfig
    metadata:
        name: custom-latency-profile
    spec:
        machineConfigPoolSelector:
            matchLabels:
                custom-openshift.io/latency-profile: high-latency
        kubeletConfig:
            evictionHard:
                memory.available: "500Mi"
            evictionSoft:
                memory.available: "700Mi"
    ```

## References

For more information about these topics, check out the links below:

* [Low latency tuning | OCP Docs 4.12](https://docs.openshift.com/container-platform/4.12/scalability_and_performance/cnf-low-latency-tuning.html)
* [Allocating resources for nodes | OCP Docs 4.12](https://docs.openshift.com/container-platform/4.12/nodes/nodes/nodes-nodes-resources-configuring.html)
* [Improving cluster stability in high latency environments using worker latency profiles | OCP Docs 4.12](https://docs.openshift.com/container-platform/4.12/nodes/clusters/nodes-cluster-worker-latency-profiles.html)
* [Using the Node Tuning Operator | OCP Docs 4.12](https://docs.openshift.com/container-platform/4.12/nodes/nodes/nodes-node-tuning-operator.html)
* [How to Improve Cluster Stability in Different Latency Environments Using Worker Latency Profiles for OpenShift | RH Blog](https://cloud.redhat.com/blog/how-to-improve-cluster-stability-in-different-latency-environments-using-worker-latency-profiles-for-openshift)
