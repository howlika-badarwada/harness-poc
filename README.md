# Harness.io Proof of Concept

A simple Node.js application demonstrating deployment via Harness.io to Kubernetes.

## Project Structure

- `server.js` - Node.js HTTP server that runs on port 3000
- `Dockerfile` - Docker image build configuration (Node.js 18-Alpine)
- `k8s.yaml` - Kubernetes deployment and service manifests
- `docker-compose.yml` - Local Drone CI/CD setup (optional, for local testing)

## Prerequisites

- Docker
- Kubernetes cluster (Minikube, K3s, EKS, GKE, etc.)
- kubectl CLI configured
- Harness.io account

## Quick Start - Local Docker

```bash
# Build the Docker image
docker build -t harness-demo:latest .

# Run the container
docker run -p 3000:3000 harness-demo:latest

# Test the application
curl http://localhost:3000
```

## Kubernetes Deployment

### Manual Deployment

```bash
# Build and load image into Kubernetes cluster
docker build -t harness-demo:latest .

# For Minikube, load the image
minikube image load harness-demo:latest

# Apply Kubernetes manifests
kubectl apply -f k8s.yaml

# Check deployment status
kubectl get deployments
kubectl get pods

# Port-forward to test locally
kubectl port-forward svc/harness-demo-service 3000:3000

# Test the application
curl http://localhost:3000
```

### Harness.io Deployment

1. Create a new Harness Pipeline
2. Configure the artifact source (Docker registry or local image)
3. Add a Deployment step with the `k8s.yaml` manifest
4. Set deployment target to your Kubernetes cluster
5. Execute the pipeline

## Configuration

### Environment Variables

For the docker-compose setup, configure GitHub OAuth credentials:
```bash
export DRONE_GITHUB_CLIENT_ID=<your-client-id>
export DRONE_GITHUB_CLIENT_SECRET=<your-client-secret>
```

## Cleanup

```bash
# Delete Kubernetes resources
kubectl delete -f k8s.yaml
```

## Notes

- The application returns a simple text response on port 3000
- Uses Node.js 18-Alpine for a lightweight image (~150MB)
- Configured for easy deployment to any Kubernetes environment