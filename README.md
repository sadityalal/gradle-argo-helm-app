# Gradle Project Helm Charts

This repository contains the **Helm charts** required to deploy the **Gradle Project Application**.  
It works in tandem with the [Application Repository](https://github.com/sadityalal/gradle-project-app) and is designed to integrate with **ArgoCD** for automated deployments.

> **Note:** This setup is intended for **testing/demo purposes only**. In production, we use a **multi-branch Git setup** and keep Helm charts in a **separate repository**. Production code and setup can be checked at:  
> [Application Repo](https://github.com/sadityalal/gradle-project-app)  
> [Helm Repo](https://github.com/sadityalal/gradle-project-helm)  

---

## Purpose

- Provide Helm charts for deploying the application in **development**.  
- Enable **CI/CD automation** where Docker images from the application repository are deployed automatically.  
- Ensure **environment-specific configurations** through separate values files.  
- For testing/demo, this workflow **uses a single branch (`main`)** to update Helm charts directly.  

---

## Repository Structure

```bash
.
├── helm-chart/
│   ├── Chart.yaml           # Helm chart metadata
│   ├── templates/           # Kubernetes templates (Deployment, Service, etc.)
│   │   └── deployment.yaml
│   ├── values-gradle.yaml   # Production environment values (used for testing/demo)
│   └── values-gradle-dev.yaml # Development environment values (used for testing/demo)
├── argo-app-dev.yaml        # ArgoCD Application manifest for dev
├── argo-app-prod.yaml       # ArgoCD Application manifest for prod
└── README.md
```

## How It Works (Testing Setup)

1. The **Application Repo** builds Docker images and pushes them to a registry.
2. GitHub Actions in this **single-branch testing setup** updates the Helm chart **values files** (`values-gradle.yaml` and `values-gradle-dev.yaml`) with the latest image tag and repository.
3. **ArgoCD** watches this Helm repository and automatically deploys updates to Kubernetes:

| Environment  | Branch | Helm Values File          | Kubernetes Namespace |
|-------------|--------|--------------------------|--------------------|
| Development | main   | values-gradle-dev.yaml   | backend-dev        |
| Production  | main   | values-gradle.yaml       | backend            |

> In production, we use **multi-branch workflows** (`develop` for dev, `main` for prod) and maintain Helm charts in a **separate repository**.

## Deployment Instructions

1. **Clone this repo**:

```bash
git clone https://github.com/sadityalal/gradle-project-helm.git
cd gradle-project-helm
```

2. Install ArgoCD on your Kubernetes cluster if not already installed.

3. Apply ArgoCD manifests:

```bash
kubectl apply -f argocd-gradle-app.yaml
```
Ensure your application repository is configured to update image tags in this Helm repo via CI/CD (single-branch testing workflow).