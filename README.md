# Local LLM with Ollama and Phi-4

A Kubernetes-based deployment for running Phi-4 locally using Ollama and Open WebUI, optimized for high-memory systems.

## Prerequisites

- Docker Desktop with Kubernetes enabled
- kubectl CLI tool
- Helm (optional, for Helm-based deployment)
- Windows system with at least 96GB RAM

## System Requirements

- Memory: 96GB RAM recommended
  - 32GB allocated to Ollama
  - 2GB allocated to frontend
  - Remaining for system and other processes
- Storage: At least 20GB free space for model storage and persistence
- CPU: 8+ cores recommended (8 cores allocated to Ollama)

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
1. Pull and run the Ollama server with optimized resource allocation
2. Download and serve the Phi-4 model
3. Start the Open WebUI interface in single-user mode

## Architecture

The deployment consists of:
- Ollama server for model serving
  - Memory limit: 32Gi
  - CPU limit: 8 cores
  - Persistent storage: 20Gi
- Open WebUI frontend
  - Memory limit: 2Gi
  - CPU limit: 1 core
  - Single user mode enabled
- Direct frontend-to-Ollama communication
- Persistent storage using Docker Desktop's hostpath provisioner

## Configuration

### Kubernetes (k8s/base)

The base configuration in `k8s/base/` includes:
- Ollama deployment with optimized resource allocation
- Persistent volume configuration (20Gi)
- Service definitions
- Frontend deployment with resource limits

### Helm (helm/local-llm)

The Helm chart in `helm/local-llm/` provides configuration through `values.yaml`:
- Resource allocation settings
- Storage configuration
- Service types
- Environment variables

## Ports and Access

### Frontend Service
- Exposed on port 80 via LoadBalancer
- Access the web interface at:
```
http://localhost:80
```

### Ollama Service
- Internal port: 11434
- Used for communication between frontend and Ollama
- Not exposed externally (accessed through frontend)

## Storage

Models and data are stored persistently using Docker Desktop's hostpath storage class:
- 20Gi allocated for model storage
- Persistent between pod restarts
- No re-download required after redeployment
- Efficient local storage management

## Default Configuration

- Model: Phi-4
- Authentication: Disabled (single user mode)
- Resource Limits:
  - Ollama: 32Gi memory, 8 CPU
  - Frontend: 2Gi memory, 1 CPU
- Resource Requests:
  - Ollama: 16Gi memory, 4 CPU
  - Frontend: 1Gi memory, 500m CPU

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
4. Verify memory allocation:
   ```bash
   kubectl top pods
   ```

Common issues:
- Insufficient system memory (requires 96GB RAM)
- Storage provisioning failures
- Resource limit constraints

## License

This project uses:
- Ollama (MIT License)
- Open WebUI (Apache 2.0 License)
- Phi-4 model (MIT License)
