# Infrastructure

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

\* I might change another cloud provider in the future in order to experience their platform.
