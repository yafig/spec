# Yet Another Free (OSS) Instagram-clone

Inspired by [Realword project from Thinkster](https://github.com/gothinkster/realworld), this project is aimed at the same goal:

> While most "todo" demos provide an excellent cursory glance at a framework's capabilities, they typically don't convey the knowledge & perspective required to actually build real applications with it.

**YAFIG** is aimed to build an Instagram-clone project with a loosely coupled microservice approach. This project is an initiative for me (and other contributors) to learn *in public* by building a real project from scratch. I am pretty sure I have a lot of things to learn along this journey.

## The Entity Diagram

### User
- username, unique
- email
- password
- status
- follower
- follows
- joined_at

### Relationship
- username1
- username2
- relationship: follows, block
- established_at

### Post
- post_id
- user
- tag
- status
- posted_at

### Comment
- comment_id
- post_id
- user
- comment
- commented_at
