+++
title = "Deploying Private ROSA OpenShift Clusters on AWS with PrivateLink"
date = 2023-05-13
description = '''
Learn how to deploy ROSA in AWS PrivateLink
'''
categories = ["Technical"]
topics = ["OpenShift"]
tags = ["Technical Guide", "ROSA", "Private Network", "GitOps", "Private Registry", "Operators"]
author = "Nick Miethe"
+++

In today's blog post, we'll delve into the process of setting up a private Red Hat OpenShift Service on AWS (ROSA) cluster using AWS PrivateLink. Our focus will be on how to install Operators, such as the GitOps Operator, within an environment that doesn't have access to the public internet. We'll also highlight key considerations for both the install process and ongoing management of the cluster. To make this process easier, we'll provide configuration examples along the way.

## What are ROSA and AWS PrivateLink?

Before we dive into the technicalities, let's start with a brief introduction to ROSA and AWS PrivateLink.

* **Red Hat OpenShift Service on AWS (ROSA)**: This is a managed service that makes it easier for users to build, scale, and deploy applications on OpenShift using AWS. 
* **AWS PrivateLink**: This service provides private connectivity between VPCs, AWS services, and on-premises applications, securely on the Amazon network.

With these services, we can create a private, secure, and scalable OpenShift environment in AWS.

## Setting Up Private ROSA Cluster

To set up a private ROSA cluster, you need to have the ROSA CLI installed and configured in your local machine. Also, you need to have an available VPC and a VPC endpoint for the ROSA service.

Here's a sample command to create a private ROSA cluster:

```bash
rosa create cluster --private-link --name=my-private-cluster --region=us-east-2
```

## Installing Operators in a Private ROSA Cluster

One of the challenges of managing a private ROSA cluster is installing and updating Operators since the cluster does not have access to the public internet. However, this can be achieved through the Operator Lifecycle Manager (OLM) and a CatalogSource, which acts as a repository of CSVs, CRDs, and packages that define an application.

Let's take the GitOps Operator as an example.

### Installing the GitOps Operator

1. **CatalogSource Setup**: You need to set up a CatalogSource that points to the GitOps Operator index image. Here is a YAML example:

```yaml
apiVersion: operators.coreos.com/v1alpha1
kind: CatalogSource
metadata:
  name: gitops-catalog
  namespace: openshift-marketplace
spec:
  sourceType: grpc
  image: '<private-registry>/redhat-openshift-gitops-catalog:<latest-tag>'
  displayName: 'Red Hat Openshift GitOps'
  publisher: RedHat
  updateStrategy:
    registryPoll:
      interval: 30m
```

Replace `<private-registry>` with the address of your private container registry. The GitOps Operator's index image must be pushed to this private registry. For more info, read my previous posts on [private registries]({{< ref "local registry" >}}) and [Quay]({{< ref "local quay" >}}).

1. **Install GitOps Operator**: Once the CatalogSource is running and the GitOps package is available, you can install the GitOps Operator. To do this, create an OperatorGroup and Subscription like the following:

```yaml
apiVersion: operators.coreos.com/v1
kind: OperatorGroup
metadata:
name: openshift-gitops-operatorgroup
namespace: openshift-operators
spec:
targetNamespaces:
    - openshift-operators
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
name: openshift-gitops-subscription
namespace: openshift-operators
spec:
channel: stable
name: openshift-gitops
source: gitops-catalog
sourceNamespace: openshift-marketplace
```

These resources will instruct OLM to install the GitOps Operator in the `openshift-operators` namespace.

## Key Considerations

When managing a private ROSA cluster, keep in mind the following:

* **Operator Updates**: Operators installed via a CatalogSource are not automatically updated. To update an Operator, you need to update the index image in your private registry and then update the CatalogSource to point to the new index image.
* **Diagnostics and Monitoring**: Since the cluster doesn't have public internet access, it won't be able to send metrics and logs to Red Hat for analysis. Ensure that you have local logging and monitoring solutions in place.
* **Access to Red Hat's container registry**: Your cluster won't be able to pull images directly from Red Hat's container registry. You need to set up a private container registry and mirror the images you need from Red Hat's registry.

## Conclusion

Setting up a private ROSA cluster on AWS using PrivateLink and managing it poses unique challenges due to the lack of public internet access. However, with the right setup and considerations, it's possible to enjoy the benefits of a private and secure OpenShift environment on AWS.

## References

* [Red Hat OpenShift Service on AWS (ROSA)](https://docs.openshift.com/rosa/welcome/index.html)
* [AWS PrivateLink](https://aws.amazon.com/privatelink/)
* [Operator Lifecycle Manager (OLM)](https://olm.operatorframework.io/)
* [GitOps Operator](https://docs.openshift.com/container-platform/4.12/cicd/gitops/understanding-openshift-gitops.html)
* [redhat-developer/gitops-operator](https://github.com/redhat-developer/gitops-operator)
* [A Guide to Going From Zero to OpenShift Cluster with GitOps](https://cloud.redhat.com/blog/a-guide-to-going-from-zero-to-openshift-cluster-with-gitops)