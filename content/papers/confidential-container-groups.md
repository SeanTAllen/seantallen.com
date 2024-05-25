---
title: "Confidential Container Groups: Implementing confidential computing on Azure container instances"
slug: "confidential-container-groups"
date: 2024-05-25T00:00:00-04:00
draft: false
tags: [Confidential Computing,Containers,Cryptography,Security]
description: "A paper about the technology behind Confidential Azure Container Instances."
---
## Abstract

The experiments presented here demonstrate that Parma, the architecture that drives confidential containers on Azure container instances, adds less than one percent additional performance overhead beyond that added by the underlying TEE. Importantly, Parma ensures a security invariant over all reachable states of the container group rooted in the attestation report. This allows external third parties to communicate securely with containers, enabling a wide range of containerized workflows that require confidential access to secure data. Companies obtain the advantages of running their most confidential workflows in the cloud without having to compromise on their security requirements. Tenants gain flexibility, efficiency, and reliability; CSPs get more business; and users can trust that their data is private, confidential, and secure.

## Versions of this paper

- [ACM Queue](https://dl.acm.org/doi/10.1145/3664293)
