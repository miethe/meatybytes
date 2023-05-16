---

title: "Setting Up a Private Registry in a Private OpenShift Cluster"
date: 2023-04-14
author: "Nick Miethe"
categories: ["Technical"]
topics: ["OpenShift"]
tags: ["Technical Guide", "Private Network", "Private Registry"]

---

In this blog post, we'll walk through setting up a private container registry within a private OpenShift cluster. This guide assumes you have already set up a private OpenShift cluster and have the necessary permissions to create and manage resources within it.

## Why a Private Registry?

In a private OpenShift cluster, direct access to public container registries is typically restricted. A private registry within your cluster can serve as a mirror for images from public registries, making them accessible to your applications.

## Setting Up the Private Registry

OpenShift includes an integrated container registry that you can deploy in your cluster. Here's a step-by-step guide on how to do it:

### Deploy the Registry

```bash
oc new-project my-registry
oc new-app -n my-registry docker-registry
```

### Expose the Registry

To allow access to the registry from outside the cluster, you need to expose it as a service.

```bash
oc expose svc/docker-registry -n my-registry
```

### Secure the Registry

By default, the registry is accessible without any authentication. To secure it, you need to set up authentication and, ideally, enable TLS.

```bash
oc create secret generic registry-credentials \
    --from-literal=username=admin \
    --from-literal=password=secret

oc set env dc/docker-registry \
    REGISTRY_AUTH=htpasswd \
    REGISTRY_AUTH_HTPASSWD_REALM="Registry Realm" \
    REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
    REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
    REGISTRY_HTTP_TLS_KEY=/certs/domain.key
```

### Mirror Images

You can now mirror images from public registries to your private registry. Here's an example:

```bash
skopeo copy --all \
    docker://docker.io/library/busybox:latest \
    docker://my-registry.example.com/library/busybox:latest
```

## Key Considerations

* **Storage**: The integrated container registry stores images on the cluster's storage. Make sure you have enough storage available and that it's scalable.
* **Permissions**: Consider who should have access to your registry and what actions they should be able to perform. OpenShift uses its built-in RBAC for this.
* **Quotas and Limits**: To prevent the registry from consuming too many resources, consider setting up quotas and limits.

## Conclusion

By setting up a private container registry in your private OpenShift cluster, you can overcome the challenges associated with restricted access to public registries. With the right configuration and management, a private registry can provide a secure and reliable source of container images for your applications.

## References

* [OpenShift Integrated Container Registry](https://docs.openshift.com/container-platform/4.12/registry/index.html)
* [Securing the Registry](https://docs.openshift.com/container-platform/4.12/registry/securing-exposing-registry.html)
* [Mirroring Images](https://docs.openshift.com/container-platform/4.12/openshift_images/image-configuration.html#images-configuration-registry-mirror)
