# API Servers

The main microservice architecture is based on this Microsoft article: https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices

## Service Definitions
The service definition and structs.

### User
Handle user registration and authentication. User service will be accessible via both HTTP REST & gRPC. The operations with HTTP REST are:
- login
- register
- follow
- block

Follow and block operations will generate a materialized views in the database in the database to save the compute power during query. It will be bundled in the same database transaction.

The operations with gRPC are:
- authenticate user

The gRPC is mainly for the other microservices to authenticate the token before proceeding the operation. It will also check the status of the user.

### Post
Handle post operations: upload, edit and delete pictures.

After posting a new picture, the service will publish a message to:
- Thumbnail generator queue
- Search indexer queue

Post service will be accessible via both HTTP REST & gRPC. The operations with HTTP REST are:
- upload
- edit
- delete

The operations with gRPC are:
- is_indexed
- is_ready_thumbnail

The gRPC is mainly for the backend workers (thumbnail generator and search indexer) to update their progress.

### Comment
Handle picture comment operations: post, update, delete comments.

### Search
Handle picture search. Search will be based on picture tag.

Search service will be designed with CQRS design pattern. Its data is derived from Post database. It has its own dedicated database tailored for searching (eg. Elasticsearch).

Search service is exposed via HTTP REST. The HTTP REST operation is:
- search

## Design

# Implementations

The project will be implemented in both Go and Python. Both implementations are identical. It's just for me to learn both languages. Refer the API-Server repository for more information.

Across all services and both implementations, they will share the same specification:
- Logging format
- Caching using Redis
- Soft deletion
- Use ORM to ensure portability between databases
- Dependency Injection 