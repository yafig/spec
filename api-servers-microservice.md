# API Servers (Microservice)

The main microservice architecture is based on this Microsoft article: https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices . However, there is a slight modification to the convention:

- `user_id` is replaced with `username` for identifier. For example, the get_user URL is `/users/john_doe` instead of `/users/1`.

HTTP REST API design is based on this Microsoft article: https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design#filter-and-paginate-data

# Service & Resource Definitions 
The service definitions, resources and sub-resource, its endpoints and parameters.

There will be 3 main services in the system:
- [User Service](#User-Service)
- [Post Service](#Post-Service)
  - [Comment Subresource](#Comment-Sub-resource)
- [Search Service](#Search-Service)

## User Service
Handle user registration and authentication. User service will be accessible via both HTTP REST & gRPC. The operations with HTTP REST are:
- **PUT** `/users/{username}` -> `get_user(username)`
- **PUT** `/users/{username}` -> `edit_user(username)`
- **DELETE** `/users/{username}` -> `delete_user(username)`
- **POST** `/users/login` -> `login()`

  Once a user logged in via frontend website, we will return the JWT token for the Javascript to set into the local storage in the browser.
- **POST** `/users/register` -> `register()`

  After creating a new user, the service will publish a message to `send_email` queue
- **GET** `/users/follow/{username}` -> `follow(username)`

  When a user follow someone, it will store the add the new relationship into the the list. The `follow count` will be a materialized view and will be updated as the function is invoked.
- **GET** `/users/block/{username}` -> `block(username)`
- **GET** `/users/{username}/posts` -> `get_user_posts(username)`

  This function will invoke `get_post_by_user` from  Post microservice via gRPC to obtain the user's posts.

Follow and block operations will generate a materialized views in the database in the database to save the compute power during query. It will be bundled in the same database transaction.

The operations with gRPC are:
- `authenticate_user(user_id, hashed_password)`

The gRPC is mainly for the other microservices to authenticate the user & its JWT token before proceeding the operation. It will also check the status of the user.

## Post Service
Posts resource has a cascade subresource of Comments.

Post service will be accessible via both HTTP REST & gRPC. The operations with HTTP REST are:
- **GET** `/posts` -> `feed()`
- **GET** `/posts/{id}` -> `get_post(id)`
- **POST** `/posts/{id}` -> `upload(id)`

  After posting a new picture, the service will publish a message to: `thumbnail_generator` queue and `search_indexer` queue. The backend workers will perform tasks based on the message received
- **PUT** `/posts/{id}` -> `edit_post(id)`
- **DELETE** `/posts/{id}` -> `delete_post(id)`

The operations with gRPC are:
- `is_indexed(post_id)`
- `is_ready_thumbnail(post_id)`
- `delete_user(user_id)`
- `get_post_by_user(user_id)`

The gRPC protocol is mainly for the backend workers (`thumbnail_generator` and `search_indexer` workers) to update their progress.

### Comment Sub-resource
Search service is exposed via HTTP REST. The HTTP REST operation is:
- **GET** `/posts/{post_id}/comments/` -> `get_comment(post_id)`
- **POST** `/posts/{post_id}/comments/` -> `create_comment(post_id)`
- **PUT** `/posts/{post_id}/comments/{comment_id}` -> `edit_comment(post_id)`
- **DELETE** `/posts/{post_id}/comments/` -> `delete_comment(post_id)`
- **DELETE** `/posts/{post_id}/comments/{comment_id}` -> `delete_comment(post_id, comment_id)`

## Search Service
Search will be based on picture tag. 

Search service will be designed with **CQRS** design pattern. Its data is derived from Post database. It has its own dedicated database tailored for searching (eg. Elasticsearch). The database will be generated by [`search_index_builder` worker](https://github.com/yafig/spec/blob/master/backend.md#search_index_builder-worker).

Search service is exposed via HTTP REST. The HTTP REST operation is:
- **GET** `/search/query?query={query}&limit={limit}&offset={offset}` -> `search(query, limit, offset)`

  The `limit` and `offset` parameters are for the search result pagination.
- **GET** `/search/autocomplete?query={query}` -> `autocomplete(query)`

# Queues
These are the queues for communications between API Servers and Backend Workers.
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
- API Documentation must be auto-generated
- Each endpoints will designed best to be idempotent

The implementation of the microservice will be based on either of these architecture:
- [Flat-file](https://www.calhoun.io/flat-application-structure/)
- [Hexagonal / Port & Adapter Design](https://www.calhoun.io/moving-towards-domain-driven-design-in-go/)
- [Model-View-Controller (MVC)](https://www.calhoun.io/using-mvc-to-structure-go-web-applications/)

*\* Other useful resource to structure the microservice: https://www.youtube.com/watch?v=1rxDzs0zgcE*