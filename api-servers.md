# API Servers

The main microservice architecture is based on this Microsoft article: https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices

HTTP REST API design is based on this Microsoft article: https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design#filter-and-paginate-data

## Service & Resource Definitions 
The service definitions, resources and sub-resource, its endpoints and parameters.

### User Service
Handle user registration and authentication. User service will be accessible via both HTTP REST & gRPC. The operations with HTTP REST are:
- **PUT** `/users` -> `edit_user()`
- **DELETE** `/users` -> `delete_user()`
- **POST** `/users/login` -> `login()`
- **POST** `/users/register` -> `register()`

  After creating a new user, the service will publish a message to `send_email` queue
- **GET** `/users/follow/{user_id}` -> `follow(user_id)`

  When a user follow someone, it will store the add the new relationship into the the list. The `follow count` will be a materialized view and will be updated as the function is invoked.
- **GET** `/users/block/{user_id}` -> `block(user_id)`
- **GET** `/users/{user_id}/posts` -> `get_user_posts(user_id)`

Follow and block operations will generate a materialized views in the database in the database to save the compute power during query. It will be bundled in the same database transaction.

The operations with gRPC are:
- `authenticate_user(user_id, hashed_password)`

The gRPC is mainly for the other microservices to authenticate the user & its JWT token before proceeding the operation. It will also check the status of the user.

### Post Service
Resources:
- Posts
- Comments

Comment resource is a cascade of Post resource.

Post service will be accessible via both HTTP REST & gRPC. The operations with HTTP REST are:
- **GET** `/posts` -> `feed()`
- **GET** `/posts/{id}` -> `get_post(id)`
- **POST** `/posts/{id}` -> `upload(id)`
- **PUT** `/posts/{id}` -> `edit_post(id)`
- **DELETE** `/posts/{id}` -> `delete_post(id)`

After posting a new picture, the service will publish a message to:
- `thumbnail_generator` queue
- `search_indexer` queue

The operations with gRPC are:
- `is_indexed(post_id)`
- `is_ready_thumbnail(post_id)`
- `delete_user(user_id)`

The gRPC protocol is mainly for the backend workers (`thumbnail_generator` and `search_indexer` workers) to update their progress.

#### Comment Sub-resource
Search service is exposed via HTTP REST. The HTTP REST operation is:
- **GET** `/post/{id}/comment/` -> `get_comment()`
- **POST** `/post/{id}/comment/` -> `create_comment()`
- **PUT** `/post/{id}/comment/` -> `edit_comment()`
- **DELETE** `/post/{id}/comment/` -> `delete_comment()`

### Search Service
Search will be based on picture tag. 

Search service will be designed with CQRS design pattern. Its data is derived from Post database. It has its own dedicated database tailored for searching (eg. Elasticsearch).

Search service is exposed via HTTP REST. The HTTP REST operation is:
- **GET** `/search/query?query={query}&limit={limit}&offset={offset}` -> `search(query, limit, offset)`
- **GET** `/search/autocomplete?query={query}` -> `autocomplete(query)`

# Queues
These are the queues for communications between API Servers and Backend Workers
- `search_indexer` queue: (create) post service -> queue -> search_indexer worker
- `thumbnail_generator` queue: (create) post service -> queue -> thumbnail_generator worker
- `send_email` queue: (create) user service -> queue -> send_email worker

# Implementations

The project will be implemented in both Go and Python. Both implementations are identical. It's just for me to learn both languages. Refer the API-Server repository for more information.

Across all services and both implementations, they will share the same specification:
- Logging format
- Caching using Redis
- Soft deletion
- Use ORM to ensure portability between databases
- Dependency Injection
- API Documentation must be auto-generated
