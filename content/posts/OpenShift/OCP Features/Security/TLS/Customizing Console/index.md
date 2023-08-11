---
title: "Customizing the OpenShift Console URL with TLS"
date: "2023-08-11"
description: "This post explains how to customize the URL for the OpenShift Console by modifying the cluster Ingress configuration, including the process of creating a new TLS certificate. Tailor your OpenShift experience with this step-by-step guide!"
tags: ["Ingress", "OCP Customization", "TLS", "Console"]
topics: ["Security", "Platform Engineering", "Networking", "OpenShift"]
categories: ["Technical", "Introduction", "Guide"]
---

## Introduction

Welcome back to [MeatyBytes.io](/), your go-to destination for all things OpenShift, Platform Engineering, and more! I'm Nick Miethe, your host and resident expert in the above, along with my various hobbies like homelab tinkering, photography, and woodworking.

In a world of evolving infrastructure requirements, having control over every aspect of your environment is key. OpenShift, a popular container orchestration system, offers flexibility in many ways.

One lesser-known feature is the ability to modify the OpenShift Console (the web UI) with a number of customizations including: the logo, *Console URL*, adding custom links, new *Dynamic Plugins*, and more! These customizations provide a personalized touch and align with any branding or naming conventions you have in place, as well as custom functionality and even specific security requirements.

### Synopsis

In this comprehensive guide, we'll cover:

1. **Understanding the Ingress Configuration**: How it works and why it's critical for customizing the URL.
2. **Creating a New TLS Certificate**: A step-by-step guide to ensure secure communication with your customized Console.
3. **Implementation and Verification**: How to put it all together and ensure it's working seamlessly.

So, let's roll up our sleeves and get into the meaty details of customizing your OpenShift Console URL. Trust me; this process is easier to chew on than it sounds, especially with this guide by your side!

## Why Customize the OpenShift Console URL?

Customizing the OpenShift Console URL is more than a cosmetic tweak. It's about aligning your cluster configuration with your organization's specific needs, security policies, and branding requirements.

### Align with Corporate Branding

If you're working in an enterprise environment, aligning the OpenShift Console configuration with your organization's naming conventions creates consistency and ease of navigation. Instead of `console-openshift-console.apps.meaty.cloud`, you could instead use `openshift.meaty.cloud` via this **OpenShift Console URL** modification.

### Enhance Security

Some enterprises have strict TLS compliance policies that prohibit wildcard TLS usage. Since the default OpenShift networking configuration of the **Ingress Controller** (`ingresscontroller`) utilizes a self-signed wildcard TLS cert, such companies could not simply customize the OpenShift Ingress via `oc patch`. Instead, they would be required to set a specific TLS cert for the **OpenShift Console URL**.

## Configuring the OpenShift Console

Customizing the **OpenShift Console URL** requires modifying the cluster ingress configuration at `ingress.config.openshift.io`. This involves adding a specific ingress definition and creating a new TLS certificate (if necessary). 

While the modification is relatively straightforward, the official documentation from Red Hat can make it a bit confusing if you are editing an existing cluster.

### Ingress Configuration

In OpenShift, ingress controllers route external traffic to services within the cluster. By adding a definition to the cluster ingress config `ingress.config.openshift.io`, we're instructing OpenShift how to handle requests directed at the custom URL.

Prior to OpenShift `v4.8`, the Console URL was modified by [patching the Console URL](https://docs.openshift.com/container-platform/4.7/web_console/customizing-the-web-console.html#customizing-the-web-console-url_customizing-web-console) (`consoleURL`) directly via the Console Operator `consoles.operator.openshift.io`. However, starting in `4.8`, patching via the Console Operator was deprecated in favor of the cluster ingress config. Now, any custom route configuration in the cluster ingress config will take precedence over all defaults and other configurations within `consoles.operator.openshift.io`.

![Default Cert Flow](ingress-default-cert-workflow.png "[Default Cert Flow](https://docs.openshift.com/container-platform/4.13/security/certificate_types_descriptions/ingress-certificates.html)")

![Custom Cert Flow](ingress-custom-cert-workflow.png "[Custom Cert Flow](https://docs.openshift.com/container-platform/4.13/security/certificate_types_descriptions/ingress-certificates.html)")

### Creating a New TLS Certificate

Security is paramount, and by creating a new TLS certificate specifically for the custom URL, we are ensuring that communication is encrypted and secure. This also allows for using a different domain from your default OpenShift Networking configuration, as well as allows using TLS if wildcard certs are prohibited by your networking policies.

{{< alert "rocket" >}}
Creating and adding TLS certs is **NOT** necessary if your custom OpenShift Console URL will be using the same base domain, unless you require a *single-domain* TLS cert. IE: `console-openshift-console.apps.meaty.cloud` to `openshift.meaty.cloud` would not require adding a cert, but `openshift.meatybytes.io` would.
{{< /alert >}}

## Configure the Custom OpenShift Console URL

There are a couple options for modifying the OpenShift cluster Ingress config. The below configurations must be made as a user with administrative privileges.

For this example, we will be using the domain `openshift.meatybytes.io` for our new Console URL. Follow these steps to configure the custom OpenShift Console route:

### Step 1 (Optional): Create or Obtain a New TLS Certificate

See the [above explanation](#creating-a-new-tls-certificate) for when a new TLS cert should be used.

Either generate a new self-signed cert for your new Console URL, or obtain a new TLS cert from your Issuer of choice, either directly or via **cert-manager**.

### Step 2 (Optional): Add the Certificate to OpenShift

Add the generated certificate to a secret in the OpenShift cluster in the `openshift-config` namespace:

```bash
oc create secret tls custom-console-certificate --cert=TLS/OpenShift.Meaty.Cloud.crt --key=TLS/OpenShift.Meaty.Cloud.key -n openshift-config
```

Replace the `cert` and `key` with the local filepaths holding your own certificate chain and associated private key.

### Step 3 (Option 1): Modify the Ingress Config Directly

Edit the cluster ingress config with the following command:

```bash
oc edit ingress.config.openshift.io cluster
```

```yaml
apiVersion: config.openshift.io/v1
kind: Ingress
metadata:
  name: cluster
spec:
  componentRoutes:
    - name: console
      namespace: openshift-console
      hostname: OpenShift.Meaty.Cloud
      servingCertKeyPairSecret:
        name: custom-console-certificate
```

Note, this should be added as a new `componentRoutes` to `spec`, not `status`. Also, do not overwrite any other `componentRoutes`, unless you are replacing an old custom Console URL. Replace `hostname` & `servingCertKeyPairSecret.name` (if providing a new cert) with your own values.

### Step 3 (Option 2): Apply new Ingress Config

```yaml
cat > custom-console.yaml << EOF
apiVersion: config.openshift.io/v1
kind: Ingress
metadata:
  name: cluster
spec:
  componentRoutes:
    - name: console
      namespace: openshift-console
      hostname: OpenShift.Meaty.Cloud
      servingCertKeyPairSecret:
        name: custom-console-certificate
EOF
```

```bash
oc apply -f custom-console.yaml
```

Follow the same notes from **Option 1** above.

### Step 4: Confirm New OpenShift Console Route

After the performing the above, there are a number of steps you can take to confirm the update:

* `$ oc get route -n openshift-console` - should show your new Console URL.
* `$ oc edit console.config.openshift.io cluster` - should provide your new `consoleURL` under `status`.
* `$ oc edit ingress.config.openshift.io cluster` - should see the `spec` and `status` of your new `componentRoutes`.
* And most importantly, refresh the OpenShift Console at your new domain! You may need to log out and back in, or clear your cookies, but your Console URL should now be at your new domain, and the TLS cert should be applied.

## Troubleshooting

There are a handful of issues that can arise during the configuration and customization of your OpenShift Console Ingress.

* The new URL must be different than the default. This might seem obvious, but you can't use this method to patch a TLS cert on your default ingress config. That must be done via the `ingresscontroller` or otherwise.
* Don't create a new **Route** or **Ingress** manually. If you already have a `Route` created with your new domain name, the **Ingress Controller** will not create the correct `Route` after applying your changes to the cluster Ingress config.
* Make sure your TLS cert is configured for the correct domain, and that any you create are added to the `openshift-config` namespace.

## Alternative: Associate TLS Certificate with the Ingress Controller

If your goal is to simply change the default self-signed TLS cert from the default OpenShift network configuration, and you plan to use your own Wildcard TLS cert, it isn't necessary to change the Console URL! Instead, you can patch the `ingresscontroller` itself, applying your new Wildcard cert to the entire cluster. 

Use the following command to associate the certificate with the ingress controller:

```shell
oc patch ingresscontroller.operator default -n openshift-ingress-operator --type=merge --patch='{"spec":{"defaultCertificate": {"name": "custom-wildcard-certificate"}}}'
```

Replace `custom-wildcard-certificate` with the name of the Secret holding your new TLS Cert. After Patching, OpenShift will automatically roll out these changes. Remember not to let your new cert expire!

## Conclusion

Customizing the OpenShift Console URL empowers organizations to align with branding, enhance security, and control how external traffic is routed. Following this step-by-step guide, you can make your OpenShift environment uniquely yours.

If you liked this, make sure you check out our content on other aspects of OpenShift, such as [Routes vs Ingress](/posts/openshift/ocp-features/overview/core-foundations/networking/route-vs-ingress/), [Security](/topics/security/), and [so much more](/posts/openshift/ocp-features/)!. And stay tuned for potentially more OpenShift customization tips, as well as a deeper dive on configuring your own TLS certs in OpenShift with the **cert-manager Operator**. Until next time!

## References

1. [cert-manager Operator for Red Hat OpenShift](https://docs.openshift.com/container-platform/4.13/security/cert_manager_operator/index.html)
2. [Customizing the web console](https://docs.openshift.com/container-platform/4.13/web_console/customizing-the-web-console.html#customizing-the-console-route_customizing-web-console)
3. [How to create a TLS/SSL certificate with a Cert-Manager Operator on OpenShift | Enable Sysadmin](https://www.redhat.com/sysadmin/cert-manager-operator-openshift)
