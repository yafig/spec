# Infrastructure

## Compulsory external services

Third-party services:

- Sentry for exception logging
- LogDNA for log aggregation

## Deployment Strategies

### Deployment 1: Gateway Routing Pattern

Main Reference: https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-routing

The services will be hosted inside a single VM (or multiple VMs for HA & Load balancing) on any cloud provider. The gateway routing will be handled by Caddy/Nginx as the web server or router. **Ansible** will be used to configure the VMs.

### Deployment 2: Kubernetes Containerized Deployment

Main Reference: https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/microservices/aks

The service will be hosted in any Kubernetes cluster. The routing will be handled by Kubernetes Service & Ingress. Kubernetes manifests will be used to coordinate the deployments.

### Deployment 3: Serverless Deployment

Main Reference: https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/serverless/web-app

The service will be hosted in a Serverless serverless container service (Google Cloud Run-like deployment).
