---
title: "Securing Your Software Supply Chain: Tracing it Back to the Start"
author: "Nick Miethe"
date: 2023-05-26
tags: ["Software Supply Chain", "SLSA", "SBOM", "Image Signing", "Provenance", "SPIFFE/SPIRE", "OpenShift", "Red Hat"]
series: ["Securing Your Supply Chain"]
series_order: 2
draft: true
---

## Introduction

As software permeates every aspect of our lives, the need for robust software security measures has become paramount. One area that has gained substantial attention over the past few years is software supply chain security. The increased reliance on software and cloud-based services has exposed organizations to new vulnerabilities, with software supply chain attacks becoming a prevalent concern. This post aims to shed light on the concept of software supply chain security, discussing its history, the industry's response, and explaining key terminology and concepts.

## The Rise of Software Supply Chain Security

!IMAGE-HERE! *Example Image Description: A timeline showing significant events in the history of software supply chain security.*

Software supply chain security has been an under-the-radar concern for many organizations until high-profile attacks made it impossible to ignore. The 2020 SolarWinds attack, which compromised the software maker SolarWinds’ Orion software, among other targets, led to an urgent reassessment of software supply chain risk. This attack, coupled with the recent Log4j vulnerability and a series of incidents in 2023, spotlighted the need for enhanced software supply chain security measures.

## Industry's Response to Software Supply Chain Attacks

The industry has responded to these threats by investing in software supply chain defenses. For instance, platform operators like GitHub have introduced automated vulnerability scanning for code and made secrets scanning and detection features available for free to public repositories. They've also tightened account security and integrated AI technologies like OpenAI's ChatGPT to help developers avoid common coding mistakes. Similarly, the Federal Government has been working to regulate software quality, pushing software publishers to improve the security of their wares through mandates and regulations.

Additionally, development teams are becoming more aware of the risks lurking in their code and the broader software supply chain. They're bolstering their defenses and skills to counter threats from sophisticated cyber-criminals and nation-state groups.

## Key Terminology: SLSA and SBOMs

!IMAGE-HERE! *Example Image Description: An infographic explaining SLSA and SBOMs.*

**Software Supply Chain Levels for Software Artifacts (SLSA)** is a framework aimed at improving the security of software supply chains. It provides a measurable and scalable path towards improving the integrity of software artifacts.

**Software Bill of Materials (SBOMs)**, on the other hand, are detailed lists of components in a piece of software. They're essential tools for understanding what's in your software, enabling better management and security of your software supply chain. The Cybersecurity and Infrastructure Security Agency (CISA) has been actively working on common guidance and structure for the global SBOM community.

## Image Signing, Provenance, and More

Image signing refers to the practice of adding a digital signature to a software image, providing a means to verify its authenticity and integrity. Provenance, in the context of software, refers to the origin of each piece of code and the path it took from development to deployment. These concepts, along with others like reproducibility, are integral to the security of software supply chains.

## Tools and Technologies: SPIFFE/SPIRE

!IMAGE-HERE! *Example Image Description: An illustration showing how SPIFFE/SPIRE works.*

The **Secure Production Identity Framework For Everyone** (SPIFFE) and the **SPIFFE Runtime Environment** (SPIRE) are open-source projects that provide a secure identity framework for production workloads. They're useful tools for enhancing software supply chain security.

## OpenShift and Red Hat's Trusted Software Supply Chain

OpenShift and Red Hat's Trusted Software Supply Chain are powerful tools that can help organizations secure their software supply chains. OpenShift is a container application platform that brings Docker and Kubernetes to the enterprise, while Red Hat's Trusted Software Supply Chain provides a comprehensive set of security features designed to protect the entire software supply chain.

## Conclusion

Software supply chain security is a complex but increasingly important area of focus. The landscape is evolving rapidly, and organizations need to stay informed and proactive to protect their software supply chains. Understanding key concepts, adopting relevant tools and technologies, and investing in robust security measures are crucial steps towards achieving a secure software supply chain.

## References

[1] Peričin, T. (2023). Software Supply Chain Security is Going Mainstream in 2023. Solutions Review.
[2] CISA. (2023). CISA Releases Community-Drafted Documents on Software Bill of Materials (SBOM). CISA.
