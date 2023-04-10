---
title: "Parma: Confidential Containers via Attested Execution Policies"
slug: "parma-confidential-containers-via-attested-execution-policies"
date: 2022-04-10T00:00:00-04:00
draft: false
tags: [Confidential Computing,Containers,Cryptography,Security]
description: "A paper about the technology behind Confidential Azure Container Instances."
---
## Abstract

Container-based technologies empower cloud tenants to develop highly portable software and deploy services in the cloud at a rapid pace. Cloud privacy, meanwhile, is important as a large number of container deployments operate on privacy-sensitive data, but challenging due to the increasing frequency and sophistication of attacks. State-of-the-art confidential container-based designs leverage process-based trusted execution environments (TEEs), but face security and compatibility issues that limits their practical deployment.

We propose Parma, an architecture that provides lift-and-shift deployment of unmodified containers while providing strong security protection against a powerful attacker who controls the untrusted host and hypervisor. Parma leverages VM-level isolation to execute a container group within a unique VM-based TEE. Besides container integrity and user data confidentiality and integrity, Parma also offers container attestation and execution integrity based on an attested execution policy. Parma execution policies provide an inductive proof over all future states of the container group. This proof, which is established during initialization, forms a root of trust that can be used for secure operations within the container group without requiring any modifications of the containerized workflow itself (aside from the inclusion of the execution policy.)

We evaluate Parma on AMD SEV-SNP processors by running a diverse set of workloads demonstrating that workflows exhibit 0-26% additional overhead in performance over running outside the enclave, with a mean 13% overhead on SPEC2017, while requiring no modifications to their program code. Adding execution policies introduces less than 1% additional overhead. Furthermore, we have deployed Parma as the underlying technology driving Confidential Containers on Azure Container Instances.

## Versions of this paper

- [arVix](https://arxiv.org/abs/2302.03976)
