# Technical Context

## Technology Stack

### Frontend
- Open WebUI for Ollama
- Pre-built container image
- Web-based interface
- Built-in streaming support
- Single user mode enabled (WEBUI_AUTH=false)
- Resource limits: 2Gi memory, 1 CPU
- Resource requests: 1Gi memory, 500m CPU

### Model Service
- Ollama for model serving
- phi4 language model
- Native API and streaming support
- Resource limits: 32Gi memory, 8 CPU
- Resource requests: 16Gi memory, 4 CPU
- Persistent storage: 20Gi

### Infrastructure
- Docker for containerization
- Kubernetes for orchestration
- Docker Desktop (Windows) as local platform
- Host system: 96 GiB RAM available

## Development Setup

### Prerequisites
- Docker Desktop for Windows with Kubernetes enabled
- Git for version control
- Minimum 32Gi RAM recommended for model serving

### Project Structure
```
/
├── k8s/               # Kubernetes manifests
├── helm/              # Helm chart
└── memory-bank/       # Project documentation
```

## Technical Constraints
1. Resource allocation optimized for 96 GiB system
2. Network communication between pods
3. Model serving requirements
4. Container image size considerations

## Dependencies
1. Docker Desktop with Kubernetes
2. Ollama requirements
3. Container registry access (local)

## Development Workflow
1. Local development in Kubernetes cluster
2. Testing in local cluster
3. Production deployment with same configuration

## Security Considerations
1. Local network security
2. API authentication/authorization (WebUI auth disabled for single user mode)
3. Resource isolation
4. Data handling
