# Local LLM Web UI

A web-based interface for interacting with a locally deployed language model using Kubernetes.

## Overview

This project provides a web UI for interacting with a Phi-4 language model, deployed locally using Kubernetes on Docker Desktop for Windows. It uses:
- Open WebUI for the frontend interface
- Ollama for model serving
- Kubernetes for deployment and orchestration

## Prerequisites

- Docker Desktop for Windows with Kubernetes enabled
- kubectl CLI tool
- Helm (optional, for Helm-based deployment)

## Deployment Options

### Using kubectl

1. Create the namespace:
```bash
kubectl apply -f k8s/base/namespace.yaml
```

2. Deploy the application:
```bash
kubectl apply -k k8s/base
```

### Using Helm

1. Install the chart:
```bash
helm install local-llm ./helm/local-llm
```

## Accessing the Application

The frontend service is exposed as a LoadBalancer. Once deployed:

1. Get the service IP:
```bash
kubectl get svc frontend
```

2. Access the web interface at:
```
http://<external-ip>
```

For ClusterIP deployments, use port-forwarding:
```bash
kubectl port-forward svc/frontend 80:80
```

Then access at: http://localhost:80

## Architecture

- Frontend: Open WebUI container (ghcr.io/open-webui/open-webui:main)
- Model Service: Ollama container (ollama/ollama:latest)
- Direct frontend-to-Ollama communication
- Kubernetes-based orchestration

## Configuration

The deployment can be customized through:
- Kubernetes manifests in `k8s/base/`
- Helm values in `helm/local-llm/values.yaml`

## Development

All development and testing is done directly in the Kubernetes environment, ensuring consistency across all stages of development.

## Resources

- [Open WebUI Documentation](https://github.com/open-webui/open-webui)
- [Ollama Documentation](https://github.com/ollama/ollama)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
