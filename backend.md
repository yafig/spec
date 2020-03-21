# Backend Workers

Generally, backend workers and the workers that consume Message Queue (MQ) message and perform tasks based on it. This will allow asynchronous communication between API Server microservices and Backend workers. The design is based on this Publisher-Subscriber pattern from Microsoft article: https://docs.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber

List of workers
- [thumbnail_generator worker](#thumbnail_generator-worker)
- [send_email worker](#send_email-worker)
- [search_index_builder worker](#search_index_builder-worker)

## List of Backend Workers

###thumbnail_generator worker
It will consume message from `thumbnail_generator queue` published by Picture API Server. 

What thumbnail generator do:
1. Thumbnail worker will resize the picture into 3 sizes: 144p, 240p and 720p
2. Upload the picture to Object Storage service
3. Trigger a gRPC call to Picture service to notify that the picture is successfully uploaded

### send_email worker
It will consume message from `send_email queue` published by Picture API Server.

What send email worker do:
- Send a greeting email to the new user with activation link

### search_indexer worker
I will consume from `search_indexer queue` published by Picture API Server.

What search indexer do:
- Add a new data into indexing database (Elasticsearch)
- If the data already exist, replace the data with new one

### search_index_builder worker
This worker will destroy the current

What search index builder do:
- Destroy the current index
- Repopulate index derived from Post database

# Implementation
## Container

## Serverless