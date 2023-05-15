+++
title = "Deploying Red Hat Advanced Cluster Security (RHACS) in a Private OpenShift Cluster"
date = 2023-05-14
description = '''
Learn how to deploy RHACS in a Private cluster
'''
categories = ["Technical"]
topics = ["OpenShift"]
tags = ["Technical Guide", "RHACS", "Security", "Private Network"]
author = "Nick Miethe"
+++

Today, we'll be exploring **Red Hat Advanced Cluster Security** (RHACS) and how to deploy it within a private OpenShift cluster. RHACS is an end-to-end Kubernetes-native security solution that provides deep visibility and control to minimize the risk area and provide the necessary governance and control for your OpenShift clusters.

{{< alert "circle-info" >}}
**For More Information** See my [prior write-up]({{< ref "rhacs foundations" >}}) on the basics of RHACS
{{< /alert >}}

## Why RHACS?

RHACS helps to identify, prioritize, and resolve the most critical security risks and vulnerabilities in your clusters. It offers several key features to enhance the security of your OpenShift cluster:

* Cluster-wide visibility into workloads, network traffic, and vulnerabilities
* Policy-driven security enforcement and compliance management
* Integration with CI/CD pipelines for early detection of security issues
* Centralized monitoring and management of your OpenShift cluster's security

 This is achieved through the capabilities of:

* **Risk Prioritization**: Identifies and ranks vulnerabilities to help security teams focus on the most significant issues.
* **Compliance Automation**: Simplifies compliance with automated policy enforcement and audit-ready reporting.
* **Threat Prevention**: Blocks attacks and enforces security controls in real-time.

## Deploying RHACS in OpenShift

### RHACS Offline Mode

It is possible to use Red Hat Advanced Cluster Security (RHACS) for Kubernetes for clusters that are not connected to the internet by enabling **offline mode**. Offline mode allows RHACS components to operate without connecting to public addresses or hosts.

This requires the following:

1. Retrieve RHACS images and install in cluster.
2. Enable offline mode during install (not upgrades).

{{< alert "shield" >}}
Remember to routinely update your Scanner's vulnerability list!
{{< /alert >}}

Images can be downloaded via OLM and the OperatorHub with a internet-connected workstation and then pushed to your mirror registry. You can also manually pull and retag using tools such as Skopeo, Docker, etc, described below.

The images are as follows:

* registry.redhat.io/advanced-cluster-security/rhacs-main-rhel8:4.0.0
* registry.redhat.io/advanced-cluster-security/rhacs-scanner-rhel8:4.0.0
* registry.redhat.io/advanced-cluster-security/rhacs-scanner-db-rhel8:4.0.0
* registry.redhat.io/advanced-cluster-security/rhacs-collector-rhel8:4.0.0
* registry.redhat.io/advanced-cluster-security/rhacs-collector-slim-rhel8:4.0.0

{{< alert "circle-info" >}}
You must maintain the name of the image and tag when retagging.
{{< /alert >}}

For each image, you should pull, tag, and push to your private registry:

```bash
docker login registry.redhat.io
docker pull <image>
docker tag <image> <new_image>
docker push <new_image>
```

You can also mirror entire registries, which you can read more about in my post on the [Integrated OCP Registry]({{< ref "local registry" >}}). You can also see further examples in [this post by Zhimin Wen](https://zhimin-wen.medium.com/openshift-4-10-image-mirroring-for-airgap-environment-f6bed61ea719).

### Deploying the Operator

{{< alert "redhat" >}}
See [Red Hat's docs](https://docs.openshift.com/acs/4.0/installing/installing_ocp/prerequisites-ocp.html) for prerequisite and sizing information.
{{< /alert >}}

RHACS can be deployed on OpenShift using the RHACS Operator. Here's how to get started:

* **Install the RHACS Operator**

```bash
oc new-project stackrox
oc create -f https://mirror.openshift.com/pub/rhacs/assets/latest/bin/linux/roxctl
oc apply -f operator.yaml
```

{{< alert "circle-info" >}}
You can also deploy the Operator from the OCP Web Console via the Operator Catalog.
{{< /alert >}}

* **Deploy RHACS**

Create a `RHACS` resource to deploy the Central component and its dependencies. You can customize the resource to suit your needs. Below is a basic example:

```yaml
apiVersion: stackrox.io/v1
kind: Central
metadata:
  name: central
spec:
  image: registry.redhat.io/rh-acs/central-bundle:3.0.62.0
  imagePullSecrets:
    - name: redhat-pull-secret
```

Using the Web console automates the generation and installation of this configuration.

* **Deploy Scanner and Sensor**

After deploying Central, you need to deploy the Scanner and Sensor components. Scanner is used for scanning images for vulnerabilities, and Sensor is used for securing your running workloads.

```bash
oc apply -f scanner.yaml
oc apply -f sensor.yaml
```

* **Access RHACS**

Once RHACS is up and running, you can access it through its route:

```bash
oc get route central -n stackrox
```

## Follow-up

Remember to enable offline mode during the install, as mentioned above. If you prefer using Helm or *roxctl*, you can find instructions [here](https://docs.openshift.com/acs/4.0/configuration/enable-offline-mode.html).

In order to begin monitoring a cluster, whether it be the hosting cluster or another, you must install the *SecureCluster* resource on each target cluster. Prior to that, you will need to create an init bundle. This is usually relatively easy, but can be tricky in disconnected environments. However, following [Red Hat's documentation](https://docs.openshift.com/acs/4.0/installing/installing_ocp/init-bundle-ocp.html) should resolve any issues you might have.

## Key Considerations

* **Networking**: Ensure that RHACS can communicate with your cluster nodes and that it can pull images from Red Hat's registry or your private registry.
* **Compatibility**: RHACS is compatible with OpenShift 4.6 and later. Ensure your cluster meets this requirement.
* **RBAC**: Consider who should have access to RHACS and what actions they should be able to perform. RHACS offers granular role-based access control (RBAC) to ensure that users only have access to the necessary resources and actions.
* **Storage**: RHACS requires a persistent storage backend for its components. Make sure you have enough storage available and that it's scalable.
* **Monitoring and Alerting**: Integrate RHACS with your existing monitoring and alerting solutions to receive notifications on critical security issues and policy

## Conclusion

Deploying RHACS in your private OpenShift cluster provides a significant boost to your cluster's security posture. With its risk prioritization, compliance automation, and threat prevention capabilities, RHACS offers an all-in-one solution for securing your OpenShift clusters. Although it requires some setup and configuration, the enhanced visibility and control over your cluster's security make it a worthy addition.

## References

* [Red Hat Advanced Cluster Security for Kubernetes 4.0](https://docs.openshift.com/acs/4.0/welcome/index.html) - Current version as of time of writing
* [Installing | Red Hat Advanced Cluster Security for Kubernetes 4.0](https://docs.openshift.com/acs/4.0/installing/acs-installation-platforms.html)
* [How to get started with Red Hat Advanced Cluster Security for Kubernetes | Enable Sysadmin](https://www.redhat.com/sysadmin/kubernetes-RHACS-red-hat-advanced-cluster-security)
* [ACS Workshop](https://redhat-scholars.github.io/acs-workshop/acs-workshop/01-setup.html)
