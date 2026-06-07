# GitOps Kubernetes Manifests - Jenkins Deployment & Service

Kubernetes manifests (Deployment + Service) for GitOps workflows. This repository is used alongside Jenkins pipelines to enable declarative, automated deployments on Kubernetes clusters.

![Kubernetes](https://img.shields.io/badge/Kubernetes-%23326CE5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-%232C5263.svg?style=for-the-badge&logo=jenkins&logoColor=white)
![GitOps](https://img.shields.io/badge/GitOps-%231A1A1A.svg?style=for-the-badge&logo=argo&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

## Overview

This is a **GitOps configuration repository** that stores Kubernetes YAML manifests. A Jenkins pipeline updates the container image tag in the Deployment and commits the change back to the repository, allowing tools like **ArgoCD** to automatically sync the desired state to the cluster.

It works as the **CD (Continuous Deployment)** part of a larger E2E CI/CD pipeline.

## Repository Contents

- **`deployment.yaml`** — Kubernetes Deployment with readiness/liveness probes, resource limits, and environment settings
- **`service.yaml`** — LoadBalancer Service exposing the application
- **`Jenkinsfile`** — Pipeline that updates the image tag and pushes changes to trigger GitOps sync

## Key Features

- **Declarative GitOps** approach using raw Kubernetes manifests
- **Automated image updates** via Jenkins
- **Production-ready Deployment** with health checks and resource requests/limits
- **LoadBalancer Service** for external access
- Designed to pair with the [Production E2E CI/CD Pipeline](https://github.com/Joseph-peemi/Production-E2E-CI-CD-Pipeline-Jenkins-Maven-Docker-SonarQube)

## Tech Stack

| Component       | Technology              |
|-----------------|-------------------------|
| Orchestration   | Kubernetes              |
| GitOps          | ArgoCD (recommended)    |
| CI/CD           | Jenkins                 |
| Application     | Java/Spring Boot        |

## How It Works

1. Jenkins CI pipeline builds and pushes a new Docker image
2. This GitOps repo's Jenkins pipeline is triggered with the new `IMAGE_TAG`
3. The pipeline updates `deployment.yaml` with the latest image
4. Changes are committed and pushed
5. ArgoCD (or similar GitOps tool) detects the change and applies it to the cluster

## Deployment Manifest Highlights

- **Replicas**: 1 (easily scalable)
- **Probes**: Readiness and Liveness configured
- **Resources**: Sensible requests and limits
- **Image**: Updated dynamically via Jenkins

## Usage

### Triggering a Deployment

Trigger the Jenkins job with the parameter:
- `IMAGE_TAG`: e.g., `abuchijoe/register-app-pipeline:1.2.3`

### Applying Manually 

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

Prerequisites

Kubernetes cluster (EKS, Minikube, etc.)
Jenkins with Git credentials configured
(Recommended) ArgoCD or Flux for GitOps sync


