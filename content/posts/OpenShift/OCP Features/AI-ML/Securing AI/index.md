---
title: "Securing AI Workloads on OpenShift with Native Tooling"
date: 2023-05-17
tags: ["OpenShift", "AI/ML", "Security", "ACS", "RBAC", "Network Policies", "Tekton Chains"]
author: Nick Miethe
categories: ["Technical", "Guide"]
topics: ["Data Science", "OpenShift"]
series: ["AI/ML OCP Tooling"]
series_order: 6
---

## Introduction

Ensuring the security of AI workloads is crucial in any production environment. OpenShift provides a variety of native tools that help secure your AI/ML workflows. In this post, we will discuss key security measures using Advanced Cluster Security (ACS), RBAC, OCP Policies, and Tekton Chains.

## Advanced Cluster Security (ACS)

**Red Hat Advanced Cluster Security** (ACS or RHACS - "rack-S") is an integrated security solution for OpenShift, based upon the open source tool StackRox, that provides container security, vulnerability management, and compliance features. It can secure your AI workloads by:

* **Scanning container images** for vulnerabilities during the CI/CD process
* **Enforcing security policies** to prevent deploying insecure applications
* **Monitoring running containers** for real-time threat detection

Because ACS is such a fully featured product, we will discuss it in more detail in a dedicated follow-up post, so stay tuned!

## Role-Based Access Control (RBAC)

**RBAC** is a security feature that allows you to define permissions for users and groups within an OpenShift cluster. By implementing RBAC for your AI/ML workloads, you can:

* **Restrict access** to specific projects, resources, or actions
* **Grant minimal permissions** based on the principle of least privilege
* **Segregate duties** between different roles, such as data scientists, ML engineers, and operators

To implement RBAC for your AI/ML workloads, create a `Role` and `RoleBinding`:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: ai-project
  name: ai-developer
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log", "persistentvolumeclaims"]
  verbs: ["create", "delete", "get", "list", "patch", "update", "watch"]

---

kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: ai-developer-binding
  namespace: ai-project
subjects:
- kind: User
  name: john@example.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: ai-developer
  apiGroup: rbac.authorization.k8s.io
```

This example creates a role with permissions to manage pods and PVCs in the `ai-project` namespace and assigns it to the user `john@example.com`.

## OpenShift Cluster Policies

OpenShift allows you to define cluster-wide policies that enforce certain security requirements. These policies can be used to secure AI/ML workloads by:

* **Limiting resources:** Prevent resource exhaustion by enforcing resource quotas and limits on AI/ML workloads
* **Controlling network access:** Restrict inbound and outbound network connections for AI/ML workloads using network policies

Here's an example of a **NetworkPolicy** that allows only inbound connections from other pods within the same namespace:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-same-namespace
  namespace: ai-project
spec:
  podSelector: {}
  ingress:
  - from:
    - podSelector: {}
```

## Tekton Chains

**Tekton Chains** is a project that provides supply chain security for Tekton Pipelines. It can be used to secure AI/ML workloads built using Tekton by:

* **Signing pipeline artifacts:** Generate signatures for pipeline artifacts to ensure their integrity and origin
* **Storing signed payloads:** Store these signatures and metadata in a secure, tamper-proof system

To enable Tekton Chains in your cluster, install the Tekton Chains Operator and create a `ChainConfig` custom resource:

```yaml
apiVersion: chains.tekton.dev/v1alpha1
kind: ChainConfig
metadata:
  name: chainconfig-sample
spec:
  # Add fields here
```

{{< lead >}}
For more information on Tekton, read our post on [Leveraging Tekton for AI Workloads on OpenShift]({{< ref "tekton ai" >}})
{{< /lead >}}

## Conclusion

OpenShift offers various tools and methods to secure your AI/ML workloads, ranging from ACS for container security, RBAC for access control, cluster policies for enforcing security requirements, and Tekton Chains for supply chain security. Using these tools in combination can help ensure the secure operation of your AI/ML applications.

## References

1. [Product Documentation for Red Hat Advanced Cluster Security for Kubernetes](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_security_for_kubernetes/4.0)
2. [RBAC Documentation](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
3. [How Red Hat OpenShift enables container security](https://www.redhat.com/en/technologies/cloud-computing/openshift/security)
4. [Tekton Chains Documentation](https://tekton.dev/docs/chains/)
5. [TektonCD Archives - CD Foundation](https://cd.foundation/tag/tektoncd/)
