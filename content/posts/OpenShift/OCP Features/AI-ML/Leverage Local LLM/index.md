---
title: "Demo: 3 Exciting Ways to Leverage Locally Served Large Language Models"
description: "Showcasing the Power of Locally Served Large Language Models: 3 Practical Examples"
date: 2023-05-15
author: Nick Miethe
tags: ["OpenAI", "OpenShift", "Open Data Hub", "Kubeflow", "AI/ML", "LLM", "Red Hat", "Demo", "Seldon", "Model Training"]
categories: ["Technical", "Guide", "Demo"]
topics: ["Data Science", "OpenShift"]
series: ["OpenAI on OpenShift"]
series_order: 4
---

## Synopsis

In this post, we're going to demonstrate three distinct ways to utilize a locally served OpenAI Large Language Model (LLM) on an OpenShift cluster. We will start with the model trained on Red Hat Documentation from [the previous post]({{< ref "openai rh demo" >}}). The other two examples will demonstrate technical, yet different, uses of a locally served LLM.

## Introduction

Locally served Large Language Models offer a wealth of opportunities for innovative applications. Their ability to understand and generate human-like text makes them versatile tools in various technical and creative domains. Let's explore three exciting examples.

## 1. Red Hat Documentation Q&A Bot

Using the model we trained on Red Hat's documentation, we can create a technical Q&A bot. This bot can provide quick, accurate answers to technical questions based on Red Hat's documentation.

1. **Accessing the Model:**

The model can be accessed through the REST API endpoint provided by Seldon. Send a POST request with a question as input to the model:

```bash
curl -X POST http://<seldon-endpoint>/api/v0.1/predictions -H 'Content-Type: application/json' -d '{"strData": "What is the command to create a new project in OpenShift?"}'
```

2. **Utilizing the Model:**

The model will return the answer based on its training on Red Hat documentation. This capability can be integrated into a chatbot UI, a command-line tool, or even a ticketing system for technical support.

## 2. Code Review Assistant

We can train an LLM to review code and provide useful feedback. For this example, we'll use a model trained on a large dataset of Python code and associated reviews.

1. **Accessing the Model:**

As before, the model can be accessed through the REST API endpoint provided by Seldon.

2. **Utilizing the Model:**

To get a code review, you can send the code snippet as a string input to the model. For instance:

```bash
curl -X POST http://<seldon-endpoint>/api/v0.1/predictions -H 'Content-Type: application/json' -d '{"strData": "def add(a, b):\n    return a+b"}'
```

The model will return feedback based on its training, such as suggestions to improve code readability or performance.

## 3. Natural Language Interface for Database Queries

An LLM can be trained to translate natural language queries into SQL queries. For this example, we'll use a model trained on a dataset of natural language questions paired with SQL queries.

1. **Accessing the Model:**

The model can be accessed through the Seldon REST API endpoint.

2. **Utilizing the Model:**

You can send a natural language question as an input to the model. For instance:

```bash
curl -X POST http://<seldon-endpoint>/api/v0.1/predictions -H 'Content-Type: application/json' -d '{"strData": "What is the total revenue for product X in 2023?"}'
```

The model will return a SQL query that can be executed against your database to retrieve the required data.

## Conclusion

These examples illustrate the versatility of Large Language Models served locally on an OpenShift cluster. From technical Q&A bots and code review assistants to natural language interfaces for databases, the possibilities are only limited by our imagination (or the imagination of your LLM-idea generator bot).

## References

1. [Hugging Face – The AI community building the future.](https://huggingface.co/) - The leading community for open source development around AI.
2. [openai/openai-cookbook {{< icon "github" >}}](https://github.com/openai/openai-cookbook/blob/main/apps/chatbot-kickstarter/powering_your_products_with_chatgpt_and_your_data.ipynb)
3. [Kaggle](https://www.kaggle.com/datasets) - Probably THE most popular site for free Data Science datasets.
4. [Integrating Graph Knowledge into End-to-End Task-Oriented Dialogue Systems](https://arxiv.org/abs/2010.01447)
5. [OpenShift Documentation](https://docs.openshift.com/)
