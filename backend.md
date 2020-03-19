# Backend

## Backend Workers

### Thumbnail generator
It will consume message from `thumbnail queue` published by Picture API Server. 

What thumbnail generator do:
1. Thumbnail worker will resize the picture into 3 sizes: 144p, 240p and 720p
2. Upload the picture to Object Storage service
3. Trigger a gRPC call to Picture service to notify that the picture is successfully uploaded

### Search indexer
I will consume from `search indexer queue` published by Picture API Server.

What search indexer do:
- Add a new data into indexing database (Elasticsearch)
- If the data already exist, replace the data with new one

# Implementation
## Container

## Serverless