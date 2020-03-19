# Frontend Spec

The page will have the following features:
1. Main page at `/` path
2. Login page at `/login` path
3. Register page at `/register` path
4. Feed page at `/feed` path
5. Post CRUD pages at `/p/{id}` path
6. User CRUD pages at `/u/{id}` path
7. Profile page at `/me` path
8. Search page at `/search` path

How the frontend works:
- Jamstack approach where each operation must perform API requests to the API server
- The frontend must be able to be served from CDN and/or Object Storage.
