# Exam App - Full CI/CD Pipeline

A Flask-based application with complete CI/CD automation using GitHub Actions and Helm charts for Kubernetes deployment. This project demonstrates modern DevOps practices including containerization, infrastructure as code, and GitOps workflows.

## Overview


- **Flask Web Application** with comprehensive Prometheus metrics
- **Docker Containerization** for consistent deployments
- **Helm Chart** for Kubernetes deployments
- **CI/CD Pipeline** with GitHub Actions 
- **GitOps Ready** for ArgoCD or Flux deployments

## Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│  GitHub Repo    │───▶│  GitHub Actions  │───▶│  Docker Hub     │
│  (Code Push)    │    │  (Build & Test)  │    │  (Image Push)   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                        │
                                                        ▼
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│  Kubernetes     │◀───│ Apply Helm      │◀───│  GitOps        │
│  Cluster        │    │ Template (Deploy)│    │  (ArgoCD)      │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```


### CI/CD Features
- **Automated Builds**: Docker images built on every push
- **Automated Testing**: linting and vulnerability scans
- **Image Push**: Automatic push to Docker Hub
- **Helm Deployment**: Automated chart updates and deployments
- **GitOps Compatible**: Works with ArgoCD/Flux for continuous deployment



## CI/CD Pipeline

### GitHub Actions Workflow

The CI/CD pipeline is automated through GitHub Actions. The workflow typically includes:

1. **Build Stage**
   - Checkout code
   - Build Docker image
   - Run tests

2. **Push Stage**
   - Push image to Docker Hub
   - Update Helm chart values

3. **Deploy Stage**
   - Deploy to Kubernetes using Helm
   - Verify deployment health


### Setting Up GitHub Actions

1. **Create GitHub Secrets**:
   - `DOCKERHUB_USERNAME`: Your Docker Hub username
   - `DOCKERHUB_TOKEN`: Your Docker Hub access token
   - `KUBE_CONFIG`: Kubernetes kubeconfig for deployment

2. **Push to repo, to start Workflow** (`.github/workflows/ci-cd.yml`):   
```bash
git add . && git commit -m "app code update" && git push
```   
a new helm template manifest will be pushed into argocd repo

## Kubernetes Deployment

### Using Helm

```bash
# Navigate to helm chart directory
cd exam-app/helm-charts

# Dry run (test the deployment)
helm install exam-app . --dry-run --debug

# Install the chart
helm install exam-app .

# Upgrade with custom values
helm upgrade exam-app . --set replicas=3 --set service.type=LoadBalancer

# Uninstall
helm uninstall exam-app
```

### Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicas` | Number of pod replicas | `1` |
| `image.repository` | Docker image repository | `kfire312/exam-app` |
| `image.tag` | Docker image tag | `latest` |
| `image.policy` | Image pull policy | `IfNotPresent` |
| `service.type` | Kubernetes service type | `ClusterIP` |
| `port` | Service port | `8000` |
| `target_port` | Container port | `8000` |

### Helm Chart Structure

```
helm-charts/
├── Chart.yaml          # Chart metadata (name: eyal, version: 0.1.0)
├── values.yaml         # Default configuration values
├── .helmignore         # Files to ignore during packaging
└── templates/
    ├── _helpers.tpl    # Template helpers and labels
    ├── deployment.yaml # Kubernetes deployment
    ├── service.yaml    # Kubernetes service
    └── ingress.yaml    # Kubernetes ingress (optional)
```

## Project Structure

```
exam-app/
├── app.py                # Flask application with Prometheus metrics
├── Dockerfile            # Container image definition
├── README.md             # This file
├── helm-charts/          # Kubernetes Helm chart
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│       ├── _helpers.tpl
│       ├── deployment.yaml
│       ├── service.yaml
│       └── ingress.yaml
└── ignore/               # Reference files
    ├── Jenkinsfile.Declerative  # Jenkins pipeline example
    └── index.html
```

## Use Cases

- **CI/CD Demonstrations**: Complete pipeline from code to deployment
- **Kubernetes Training**: Helm chart creation and management
- **GitOps Workflows**: ArgoCD continuous deployment patterns
- **DevOps Best Practices**: Containerization, automation, and infrastructure as code

## Troubleshooting

### Common Issues

1. **Image Pull Errors**: Ensure Docker Hub credentials are correct
2. **Helm Deploy Failures**: Check Kubernetes cluster connectivity
3. **Pipeline Failures**: Check GitHub secrets and permissions

