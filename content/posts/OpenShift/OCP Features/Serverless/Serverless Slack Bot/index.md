---
title: "Real-Time Data Processing with OpenShift Serverless: Deploying a Python Slack Bot"
author: "Nick Miethe"
date: "2023-05-16"
tags: ["OpenShift", "Knative", "Serverless", "Python", "Slack Bot", "GitHub"]
categories: ["Technical", "Guide"]
topics: ["OpenShift"]
series: ["Serverless Servers"]
series_order: 3
---

## Introduction

Welcome back to MeatyBytes.io! In our previous posts, we've explored the power of [OpenShift Serverless and Knative]({{< ref "knative" >}}), and provided practical examples of how to use them to build [event-driven applications]({{< ref "eda serverless" >}}). Today, we're going to take it a step further and demonstrate how to use OpenShift Serverless for real-time data processing.

For our example, we'll deploy a Python Slack bot called `github-user-adder.py`. This bot will listen for API calls, run some logic, and then make API calls to GitHub. The serverless deployment will trigger when an API event exists, and scale up or down based on the amount of traffic.

Let's get started!

## Creating the Python Slack Bot

First, we need to create our Python Slack bot. This bot will listen for API calls, run some logic (in this case, adding a user to a GitHub repository), and then make API calls to GitHub.

Here's a basic example of what the `github-user-adder.py` script might look like:

```python
import os
import slack
from github import Github

## Initialize Slack client
slack_token = os.getenv("SLACK_TOKEN")
client = slack.WebClient(token=slack_token)

## Initialize GitHub client
github_token = os.getenv("GITHUB_TOKEN")
g = Github(github_token)

## Listen for API calls
@client.on("message")
def handle_message(data):
    text = data.get("text")
    if "add user" in text:
        user, repo = text.split(" ")[2:]
        add_user_to_repo(user, repo)

## Add user to GitHub repo
def add_user_to_repo(user, repo):
    repo = g.get_repo(repo)
    repo.add_to_collaborators(user)

## Start the bot
client.run(os.getenv("SLACK_SIGNING_SECRET"))
```

This script uses the `slack` and `github` Python libraries to interact with the Slack and GitHub APIs. It listens for messages that contain the phrase "add user", and then adds the specified user to the specified GitHub repository.

## Deploying the Slack Bot with OpenShift Serverless

Next, we need to deploy our Slack bot with OpenShift Serverless. This involves creating a new Knative Service with a corresponding container image.

Here's a sample YAML configuration for our service:

```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: github-user-adder
  namespace: default
spec:
  template:
    spec:
      containers:
      - image: quay.io/meatybytes/github-user-adder:latest
        env:
        - name: SLACK_TOKEN
          valueFrom:
            secretKeyRef:
              name: slack-secrets
              key: token
        - name: GITHUB_TOKEN
          valueFrom:
            secretKeyRef:
              name: github-secrets
              key: token
        - name: SLACK_SIGNING_SECRET
          valueFrom:
            secretKeyRef:
              name: slack-secrets
              key: signing-secret
```

This configuration creates a new Knative Service named `github-user-adder` that runs a container based on the `quay.io/meatybytes/github-user-adder:latest` image. This image should contain our `github-user-adder.py` script. The Slack and GitHub tokens are provided as environment variables from Kubernetes Secrets.

## Conclusion

And there you have it! We've created a Python Slack bot that listens for API calls, runs some logic, and then makes API calls to GitHub. The bot is deployed as a serverless application on OpenShift, allowing it to scale up and down based on the amount of traffic.

This is just one example of how OpenShift Serverless can be used for real-time data processing. With its ability to automatically scale in response to demand, OpenShift Serverless is a powerful tool for building responsive, efficient applications.

As always, if you have any questions or run into any issues, don't hesitate to reach out. Happy coding!

## References

For additional reading, check out the following resources:

1. [Knative Documentation](https://knative.dev/docs/)
2. [OpenShift Serverless Documentation](https://docs.openshift.com/container-platform/4.13/serverless/about/about-serverless.html)
3. [Python Slack SDK Documentation](https://slack.dev/python-slack-sdk/)
4. [PyGithub Documentation](https://pygithub.readthedocs.io/en/latest/)
