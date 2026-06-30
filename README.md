# ArgoCD repo

This repository is a GitOps-style Argo CD configuration repository for deploying the IMDG chatbot platform and the supporting Kubernetes infrastructure. It contains Kubernetes manifests, Helm values, and Argo CD application definitions that describe the desired state of the environment.

## Source code

- The application source of the IMDG chatbot is at: https://github.com/kelvin124/imdg-chatbot-argocd

## What this repository contains

- Application definitions for the chatbot stack under [apps/imdg-chatbot](apps/imdg-chatbot)
  - Server deployment configuration
  - UI deployment configuration
  - Helm values for each component
- Infrastructure definitions under [infrastructure](infrastructure)
  - Argo CD itself
  - Cert Manager
  - Grafana
  - Prometheus
  - Storage resources for local environments such as Minikube
- Installation manifests under [install](install)
  - AppProject definitions
  - Root applications for Argo CD
  - Environment-specific install manifests

## What it does

This repository is used to automate the deployment and ongoing synchronization of the chatbot platform on Kubernetes. Argo CD reads these manifests and ensures the cluster matches the configuration stored in Git. In practice, this means:

- Deploying the chatbot server and UI
- Provisioning shared platform services such as monitoring and certificates
- Keeping applications consistent across environments
- Making it easier to manage changes through Git-based workflows

## Installation

1. Apply the AppProject manifests from [install/project](install/project)
2. Apply the environment-specific install manifest such as [install/install-minikube.yaml](install/install-minikube.yaml)
3. Ensure Argo CD is configured to watch this repository and sync the applications

### Profile: Minikube
- Use [install/install-minikube.yaml](install/install-minikube.yaml) for a Minikube deployment

### Profile: Oracle Cloud (install-oci.yaml)
- Status: IN PROGRESS
- For bootstrapping the applications and platform

#### Completed
- Private registry (container + Helm)
- K3s

#### Next
- Ingress: Expose services via Load Balancer
- TLS with cert-manager for cert renewal, cert issuance automation
- Secrets: Integrate Vault (External Secrets Operator) to eliminate plain-text secrets in values.yaml

## Repository structure
```text
.
├── apps/
│   └── imdg-chatbot/
│       ├── server/
│       │   ├── application.yaml
│       │   └── values.yaml
│       └── ui/
│           ├── application.yaml
│           └── values.yaml
├── infrastructure/
│   ├── apps/
│   │   ├── argocd/
│   │   ├── cert-manager/
│   │   ├── grafana/
│   │   └── prometheus/
│   └── storage/
│       └── minikube/
│           ├── grafana/
│           └── prometheus/
├── install/
│   ├── install-minikube.yaml
│   ├── project/
│   └── root-app/
├── README.md
```