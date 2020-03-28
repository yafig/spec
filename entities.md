
# The Entity Diagram

## User
- username, unique
- email
- password
- password_salt
- status
- follower_count
- following_count
- joined_at

## Relationship
- user1
- user2
- relationship: follows, block
- established_at

## Post
- id
- user
- tag
- status
- posted_at

## Comment
- comment_id
- post_id
- user
- comment
- commented_at
