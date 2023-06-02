+++
title = "Creating a Golden Path in Backstage: Deploying Python Applications to OpenShift"
date = 2023-05-18
tags = ["Golden Paths", "Platform Engineering", "DevOps", "OpenShift", "Kubernetes", "DevOps", "IDP", "Backstage", "Python", "Janus", "Developer Hub"]
categories = ["Technical", "Guide"]
topics = ["OpenShift", "IDP"]
series = ["Golden Paths"]
series_order = 2
description = "A step-by-step guide to creating a Golden Path in Backstage for deploying Python applications to an OpenShift cluster."
author = "Nick Miethe"
draft = false
+++

## Introduction

In the previous post, we discussed the concept of Golden Paths in the context of Platform Engineering, Internal Developer Platforms (IDPs), and OpenShift/Kubernetes. Now, let's dive into the technical specifics and create a Golden Path using [Backstage](https://backstage.io/docs/overview/what-is-backstage), an open-source IDP, to deploy a Python application to an OpenShift cluster.

This post will guide you through the process of defining a Golden Path within Backstage, setting up the necessary configuration, and finally, deploying a Python application using the Golden Path.

![](backstage.png)

## Setting up Backstage

Firstly, you need to have Backstage set up in your environment. If you haven't done this yet, follow the [Backstage installation guide](https://backstage.io/docs/getting-started/).

## Defining the Golden Path

To define a Golden Path within Backstage, you need to create a Software Template. This serves as a blueprint for creating new software components. Here's how to create a Software Template for deploying a Python application to OpenShift:

1. Navigate to your Backstage repository and go to the `templates` directory.
2. Create a new directory for your template, for example, `python-openshift`.
3. In this directory, create a new `template.yaml` file.

This file should contain metadata about the template and the steps required to create a new component from the template.

### Creating the Software Template

Here's an example of a `template.yaml` file for a Python application that can be deployed to OpenShift:

```yaml
apiVersion: backstage.io/v1alpha1
kind: Template
metadata:
  name: python-openshift
  title: Python Application
  description: A simple Python application for OpenShift
spec:
  owner: team-platform
  type: service
  steps:
    - id: fetch-base
      name: Fetch Base
      action: fetch:cookiecutter
      input:
        url: https://github.com/openshift/python-ex
    - id: publish
      name: Publish to OpenShift
      action: openshift:publish
      input:
        repoUrl: '{{cookiecutter.github.repoUrl}}'
  output:
    fetchUrl: '{{cookiecutter.github.repoUrl}}'
```

This template fetches a base Python application from a GitHub repository using the `fetch:cookiecutter` action. It then publishes the application to an OpenShift cluster using the `openshift:publish` action.

## Deploying a Python Application

Once the template is in place, developers can use it to deploy a Python application to OpenShift via Backstage. They would simply:

1. Navigate to the 'Create' page in Backstage.
2. Select the 'Python Application' template.
3. Fill in the necessary details.
4. Click on 'Create'.

Backstage will then use the template to create and deploy the Python application to OpenShift.

## Conclusion

In this post, we explored the technical specifics of creating a Golden Path in Backstage for deploying Python applications to OpenShift. This process not only standardizes the deployment process but also makes it significantly easier for developers.

By integrating Backstage with OpenShift, we can leverage the power of both platforms to create a streamlined, efficient, and effective deployment pipeline.

Remember, this is just a basic example, and you can customize this process to suit your specific requirements. Stay tuned for more posts on using Backstage and OpenShift to simplify your deployment processes, especially with Red Hat's new project [Janus](https://janus-idp.io/)!

{{< alert "redhat" >}}
Red Hat just released their new [Developer Hub](https://developers.redhat.com/products/developer-hub/overview) at RH Summit '23, a supported offering, based on Janus, providing plugins for Backstage. See the developer's guide linked in the References below for more information on the initial 6 plugins!
{{< /alert >}}

## References

1. [Backstage Software Templates](https://backstage.io/docs/features/software-templates/)
2. [Backstage Installation Guide](https://backstage.io/docs/getting-started/)
3. [raffaelespazzoli/backstage-demo {{< icon "github" >}}](https://github.com/raffaelespazzoli/backstage-demo)
4. [Janus IDP Backstage Showcase](https://showcase.janus-idp.io/)
5. [Deploying Backstage onto OpenShift Using the Backstage Helm Chart | Janus](https://janus-idp.io/blog/2023/02/17/deploying-backstage-onto-openshift-using-helm/)
6. [A developer’s guide to Red Hat Developer Hub and Janus | Red Hat Developer](https://developers.redhat.com/articles/2023/05/23/developers-guide-red-hat-developer-hub-and-janus)
7. [Red Hat Unveils Red Hat Developer Hub to Help Fuel Developer Productivity](https://www.redhat.com/en/about/press-releases/red-hat-unveils-red-hat-developer-hub-help-fuel-developer-productivity)
