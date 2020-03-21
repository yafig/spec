# Infrastructure

# Compulsory (Interchange-able) external services:
The project will be hosted main in AWS. It will be using mainly:
- AWS ECS on Spot Instances
- AWS RDS for PostgreSQL
- AWS Lambda & SAM for non-functional function invocations
- AWS ElasticCache for caching
- AWS MQ for RabbitMQ & AWS SQS for messaging
- AWS SES for emailing
- AWS SNS for pub/sub communication
- AWS ElasticSearch for searching
- AWS Route53 for DNS routing

Other 3rd-party software/services:
- Sentry for exception logging
- LogDNA for log management

\* I might change another cloud provider in the future in order to experience their platform.

# Deployment Strategies
## Deployment 1: Gateway Routing Pattern

Main Reference: https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-routing

The services will be hosted inside a single (or multiple VMs for HA & Load balancing). The gateway routing will be handled by Nginx as the router. The microservices will be run as Linux services in the VM. **Ansible** will be used to configure the VMs.

## Deployment 2: Kubernetes Containerized Deployment

Main Reference: https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/microservices/aks

The service will be hosted in a (on-prem, because I'm cheap skate) Kubernetes cluster. The routing will be handled by Kubernetes Service & Ingress. Kubernetes manifests will be used to coordinate the deployments.

## Deployment 3: Serverless Deployment

Main Reference: https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/serverless/web-app

The service will be hosted in a Serverless environment in a cloud provider. However, it will be deployed as a serverless container (Google Cloud Run like deployment) instead of Lambda-like deployment. Routing will be handled by API Gateway.
