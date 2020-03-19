# Backend

## Backend Workers

### thumbnail_generator worker
It will consume message from `thumbnail_generator queue` published by Picture API Server. 

What thumbnail generator do:
1. Thumbnail worker will resize the picture into 3 sizes: 144p, 240p and 720p
2. Upload the picture to Object Storage service
3. Trigger a gRPC call to Picture service to notify that the picture is successfully uploaded

### search_indexer worker
I will consume from `search_indexer queue` published by Picture API Server.

What search indexer do:
- Add a new data into indexing database (Elasticsearch)
- If the data already exist, replace the data with new one

### send_email worker
It will consume message from `send_email queue` published by Picture API Server.

What send email worker do:
- Send a greeting email to the new user with activation link

# Implementation
## Container

## Serverless