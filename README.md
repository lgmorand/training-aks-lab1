# The Azure Kubernetes Workshop - Lab n°1

Welcome to **Lab n°1** of the Azure Kubernetes Workshop. This hands-on lab guides you through the essential tasks required to deploy a containerized application to Kubernetes on [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service/).

Whether you are new to Kubernetes or looking to reinforce your skills, this lab serves as both a practical tutorial and study material.

| | |
|---|---|
| **Level** | Beginner |
| **Duration** | ~2 hours |
| **Author** | [lgmorand](https://github.com/lgmorand) |

## What you will learn

- Create and configure an **AKS cluster** and an **Azure Container Registry (ACR)**
- Import a container image and deploy it to AKS using Kubernetes manifests
- Expose your application to the internet with a **LoadBalancer** service
- Secure network traffic with **Network Policies**

## Lab content

The workshop is organized into the following sections:

### 1. Introduction

- Prerequisites (tools, Azure subscription, Cloud Shell setup)
- Container and Kubernetes basics
- Application overview

### 2. Getting up and running

| Challenge | Description | Duration |
|---|---|---|
| Deploying Kubernetes with AKS | Create an ACR, provision an AKS cluster, and connect with `kubectl` | 30 min |
| Deploying the app to AKS | Import an image into ACR and deploy it using a Kubernetes manifest | 20 min |
| Enabling public access | Expose the application externally via a LoadBalancer service | 30 min |
| Network Policy | Block and allow traffic using Kubernetes Network Policies | 10 min |

### 3. Play around

Free exploration time to dig deeper into your cluster: inspect pods in the portal, retrieve deployment YAML, check resource metrics, move workloads across namespaces, and more.

### 4. Clean up

Delete all Azure resources created during the lab.

## Prerequisites

- An **Azure subscription**
- [Azure CLI](https://github.com/Azure/azure-cli) v2.83.0+
- [kubectl](https://github.com/kubernetes/kubectl) v0.32.0+

Alternatively, you can use the [Azure Cloud Shell](https://shell.azure.com) which comes with all tools pre-installed.

## Getting started

The full workshop instructions are available on the companion website built from this repository. Clone the repo and follow the step-by-step guide.

## Sample application

The lab uses a simple **hello-world** web application (`docker.io/lgmorand/catnip`) that you will import into your own Azure Container Registry and deploy on AKS.

## Contributing

Contributions and suggestions are welcome. Please refer to the [Code of Conduct](CODE_OF_CONDUCT.md) before participating.

## License

This project is licensed under the terms of the [MIT license](LICENSE).
