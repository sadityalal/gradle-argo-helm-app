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
├── argocd-gradle-app.yaml
├── Dockerfile                  # Build Docker Image
├── gradle-app                  # Main application code
│   ├── build
│   ├── build.gradle
│   ├── gradle
│   ├── gradlew
│   ├── gradlew.bat
│   ├── settings.gradle
│   └── src
├── helm-chart                  # Helm Charts
│   ├── Chart.yaml
│   ├── templates
│   └── values-gradle.yaml      # Values file
└── README.md
```

## How It Works (Testing Setup)

1. The **Application Repo** builds Docker images and pushes them to a registry.
2. GitHub Actions in this **single-branch testing setup** updates the Helm chart **values file** (`values-gradle.yaml`) with the latest image tag and repository.
3. **ArgoCD** watches this Helm repository and automatically deploys updates to Kubernetes:

| Environment  | Branch | Helm Values File          | Kubernetes Namespace |
|-------------|--------|--------------------------|--------------------|
| Production  | main   | values-gradle.yaml       | backend            |

> NOTE: In production, we use **multi-branch workflows** (`develop` for dev, `main` for prod) and maintain Helm charts in a **separate repository**.

## Deployment Instructions

1. **Clone this repo**:

```bash
git clone https://github.com/sadityalal/gradle-argo-helm-app
cd gradle-argo-helm-app
```

2. Install ArgoCD on your Kubernetes cluster if not already installed.

3. Apply ArgoCD manifests:

```bash
kubectl apply -f argocd-gradle-app.yaml
```