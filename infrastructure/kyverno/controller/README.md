# Kyverno Controller

This folder contains the configuration for deploying the Kyverno controller in the cluster using Flux and Helm.

## Purpose

The Kyverno controller is required to enforce and manage Kubernetes policies. Kyverno acts as an admission controller that validates resources, performs mutations, and can generate new resources based on policies.

## What does it do?

- **Validate:** Checks whether resources meet predefined rules before they are admitted to the cluster.
- **Mutate:** Automatically modifies resources according to the policies.
- **Generate:** Can automatically create additional resources when certain conditions are met.

The controller is deployed using a [`HelmRelease`](helm-release.yaml) resource, which pulls the necessary Helm chart from the [`HelmRepository`](helm-repository.yaml).

See the [Kyverno documentation](https://kyverno.io/docs/) for more information.