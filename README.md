# Local LLM with Ollama and Phi-4

A Kubernetes-based deployment for running Phi-4 locally using Ollama and Open WebUI.

## Prerequisites

- Docker Desktop with Kubernetes enabled
- kubectl CLI tool
- Helm (optional, for Helm-based deployment)

## System Requirements

- Memory: At least 16GB RAM (8GB for Ollama)
- Storage: At least 10GB free space for model storage
- CPU: 4+ cores recommended

## Quick Start

### Using kubectl

```bash
# Create namespace (if not exists)
kubectl apply -f k8s/base/namespace.yaml

# Deploy the application
kubectl apply -f k8s/base/
```

### Using Helm

```bash
# Install the chart
helm install local-llm helm/local-llm
```

The deployment will automatically:
1. Pull and run the Ollama server
2. Download the Phi-4 model (approximately 9.1GB)
3. Start the Open WebUI interface

## Architecture

The deployment consists of:
- Ollama server for model serving
- Open WebUI for the web interface
- Persistent storage for model data using Docker Desktop's hostpath provisioner

## Configuration

### Kubernetes (k8s/base)

The base configuration is in `k8s/base/` and includes:
- Ollama deployment with Phi-4 model
- Persistent storage configuration
- Service definitions

### Helm (helm/local-llm)

The Helm chart in `helm/local-llm/` provides additional configuration options through `values.yaml`:
- Model selection
- Resource limits
- Storage configuration
- Service types

## Accessing the Interface

The Open WebUI interface is exposed as a LoadBalancer service. Access it at:
```
http://localhost:8080
```

## Storage

Models are stored persistently using Docker Desktop's hostpath storage class. This ensures:
- Models persist between pod restarts
- No need to re-download models after redeployment
- Efficient local storage management

## Default Model

Phi-4 is configured as the default model and will be automatically pulled on startup. The model:
- Size: ~9.1GB
- Optimized for efficiency
- Suitable for various NLP tasks

## Troubleshooting

If the pod fails to start:
1. Check resource availability:
   ```bash
   kubectl describe node
   ```
2. Verify storage provisioning:
   ```bash
   kubectl get pvc
   ```
3. Check pod logs:
   ```bash
   kubectl logs -f deployment/ollama
   ```

## License

This project uses:
- Ollama (MIT License)
- Open WebUI (Apache 2.0 License)
- Phi-4 model (MIT License)
