# Frontend Spec

The page will have the following features:
1. Login page at `/login` path
2. Register page at `/register` path
3. Newsfeed page at `/newsfeed` path
4. Post CRUD pages at `/p/{id}` paths
5. User CRUD pages at `/u/{id}` paths
6. Profile page at `/profile` path
7. Search page at `/search` path

How the frontend works:
- Jamstack approach where each operation must perform API requests to the API server
- The frontend must be able to be served from CDN or Object Storage.
