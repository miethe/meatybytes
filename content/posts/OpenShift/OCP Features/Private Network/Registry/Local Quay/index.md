---

title: "Setting Up a Self-hosted Quay Registry in a Private OpenShift Cluster"
date: 2023-05-15
author: "Nick Miethe"
categories: ["Technical"]
topics: ["OpenShift"]
tags: ["Technical Guide", "Private Network", "Private Registry", "Quay"]

---

In this blog post, we'll explore how to set up a self-hosted Quay registry within a private OpenShift cluster. Quay is a powerful and flexible container registry by Red Hat that offers a number of features such as built-in image scanning, automated builds, and much more.

## Why Quay?

Quay provides a more feature-rich and flexible option compared to the integrated Docker registry in OpenShift. With Quay, you can take advantage of robust security features, a user-friendly web interface, and advanced image management capabilities, making it an ideal choice for enterprise-level applications.

## Setting Up Quay in OpenShift

Quay can be deployed on OpenShift using the Quay Operator. This Operator simplifies the deployment and management of Quay and its dependencies. Here's how to get started:

1. **Install the Quay Operator**

```bash
oc new-project quay-enterprise
oc apply -f https://raw.githubusercontent.com/quay/quay-operator/master/bundle/manifests/quay-operator.clusterserviceversion.yaml
```

2. **Deploy Quay**

Create a QuayEcosystem resource to deploy Quay and its dependencies. You can customize the resource to suit your needs. Below is a basic example:

```yaml
apiVersion: redhatcop.redhat.io/v1alpha1
kind: QuayEcosystem
metadata:
  name: quayecosystem
  namespace: quay-enterprise
spec:
  quay:
    imagePullSecretName: redhat-pull-secret
  clair:
    enabled: true
    imagePullSecretName: redhat-pull-secret
  redis:
    imagePullSecretName: redhat-pull-secret
```

3. **Access Quay**

Once Quay is up and running, you can access it through its route:

```bash
oc get route quayecosystem-quay -n quay-enterprise
```

4. **Mirror Images**

With Quay up and running, you can now mirror images from public registries to your Quay registry. Here's an example:

```bash
skopeo copy --all \
    docker://docker.io/library/busybox:latest \
    docker://quayecosystem-quay-quay-enterprise.apps.your-cluster.com/library/busybox:latest
```

## Key Considerations

- **Security**: Quay comes with built-in security features such as Clair for vulnerability scanning and the ability to create robot accounts with limited and time-bound permissions.

- **Storage**: Like any registry, Quay requires a substantial amount of storage. While it can use the cluster's storage, you might want to use external storage for scalability and performance.

- **Backup and Disaster Recovery**: Ensure you have processes in place for backing up and restoring your Quay data to ensure business continuity.

## Conclusion

Setting up a self-hosted Quay registry in your private OpenShift cluster provides a secure, reliable, and feature-rich source of container images for your applications. It does require more setup and maintenance compared to the integrated Docker registry, but the benefits make it a worthwhile investment for many organizations.

## References

- [Quay Operator](https://github.com/quay/quay-operator)
