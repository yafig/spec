# Yet Another Free (OSS) Instagram-clone

![YAFIG Logo](logo.png)

**Status: Under heavy development**

[![Netlify Status](https://api.netlify.com/api/v1/badges/9166b573-1cee-48eb-89e4-c9b60a47c938/deploy-status)](https://app.netlify.com/sites/yafig/deploys)

Inspired by [Realword project from Thinkster](https://github.com/gothinkster/realworld), this project is aimed at the same goal:

> While most "todo" demos provide an excellent cursory glance at a framework's capabilities, they typically don't convey the knowledge & perspective required to actually build real applications with it.

**YAFIG** is aimed to build a production-ready **Instagram-clone** with minimal functionalities. This project will only implement the following features:

- Register & User login
- User can follow or block other users
- Upload, edit and delete a post
- Comment & like posts
- Search the posts

The backend of this project will be implemented in two approaches: microservice & monolithic architecture in Jamstack style.

## Table of Contents

- [But why](#But-why)
- [Detailed Specs](#Detailed-specs)
- [Implementation](#Implementation)
  - [Frontend](#Frontend)
  - [API Servers & Workers](#API-servers--workers)
    - [Monolithic](#Monolithic)
    - [Microservice](#Microservice)
  - [Backend workers](#Backend-workers)
- [Who made this](#Who-made-this)

## But why

This project is an initiative for me to learn *in public* by building a real project from scratch. I am pretty sure I have a lot of things to learn along this journey. This is my greenfield to test new technologies that interest me.

Few parts of this system is not implemented consistently. For example you will see one module is implemented in MVC architecture and another is in Clean architecture. This is intentional because I wanted to learn different approaches to do the same thing. Speed is not my priority in this project. It may takes time for me to understand many of the framework/technology in detail, so I don't mind taking a longer time to understand them in detail.

## Detailed Specs

Refer to following documents for more information:

- [Entities](entities.md)
- [Frontend](frontend.md)
- API Server
  - [API Servers Microservice (Option 1)](api-servers-microservice.md)
  - [API Servers Monolith (Option 2)](api-servers-monolith.md)
- [Backend workers](backend.md)
- [DevOps](devops.md)
- [Infrastructure](infrastructure.md)

## Implementations

### Frontend

The frontend is implemented in:

- **[VueJS + NuxtJS](https://github.com/yafig/frontend)**.

Other frameworks that I might be interested to explore next would be **Svelte** and **NextJS**.

### API Servers

The API Servers will be implemented in two approaches: Monolithic and Microservice. Both of them are doing essentially the same thing, but in different manner.

#### Monolithic

The monolithic approach will be implemented in:

- **[Python](https://github.com/yafig/api-server-monolith/tree/master/python-django-rest)** using **Django REST Framework** (DRF).

#### Microservice

The microservice approach will be implemented in programming languages:

- [Python](https://github.com/yafig/api-server-microservice/tree/master/python)
- [Upcoming] Golang

Each microservice will be implemented using either:

- Hexagonal architecture
- Model View Controller (MVC) architecture
- Flat-file architecture

### Backend workers

Backend workers will be implemented in:

- [Python](https://github.com/yafig/backend-worker/tree/master/python)
- [Upcoming] Go

The workers will consume messages from a message queue / pub-sub queue produced by API Servers and perform actions.

## Who made this

This is a solo project by [Fadhil Yaacob](http://twitter.com/sdil)
