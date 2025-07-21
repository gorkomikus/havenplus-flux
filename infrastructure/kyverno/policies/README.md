# Kyverno Policies

This folder contains the Kyverno policies that are applied to the Kubernetes cluster.

## Why are Kyverno policies necessary?

Kyverno policies ensure that the cluster complies with security and compliance requirements by enforcing rules on resources. They help standardize configurations, enforce best practices, and prevent unwanted changes.fdwingen van best practices en het voorkomen van ongewenste wijzigingen.

## What do the policies do?

- **Validate:** Check whether resources meet specified requirements (such as labels, annotations, image registries).
- **Mutate:** Automatically adjust resources to meet the desired standards.
- **Generate:** Create additional resources if needed (such as default network policies or configmaps).

These policies are managed as code and automatically deployed via GitOps using Flux and Kyverno.