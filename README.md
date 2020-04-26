# Yet Another Free (OSS) Instagram-clone

![YAFIG Logo](logo.png)

**Status: Under heavy development**

[![Netlify Status](https://api.netlify.com/api/v1/badges/9166b573-1cee-48eb-89e4-c9b60a47c938/deploy-status)](https://app.netlify.com/sites/yafig/deploys)

Inspired by [Realword project from Thinkster](https://github.com/gothinkster/realworld), this project is aimed at the same goal:

> While most "todo" demos provide an excellent cursory glance at a framework's capabilities, they typically don't convey the knowledge & perspective required to actually build real applications with it.

**YAFIG** is aimed to build an **Instagram-clone** project with a loosely coupled microservice approach. This project will only implement the basic features of Instagram:

- Register & User login
- Upload, edit and delete a post
- Comment on posts
- Search the posts

## Table of Contents

- [But why?](#But-why)
- [Detailed Specs](#Detailed-specs)
- [Implementation](#Implementation)
  - [Frontend](#Frontend)
  - [API Servers & Workers](#API-servers--workers)
    - [Monolithic](#Monolithic)
    - [Microservice](#Microservice)
    - [Backend workers](#Backend-workers)
- [Who made this](#Who-made-this)

## But why

This project is an initiative for me (and other contributors) to learn *in public* by building a real project from scratch. I am pretty sure I have a lot of things to learn along this journey. This is my greenfield to test new technologies that interest me.

## Detailed Specs

Refer to following documents for more information:

- [Entities](entities.md)
- [Frontend](frontend.md)
- [API Servers Microservice (Option 1)](api-servers-microservice.md)
- [API Servers Monolith (Option 2)](api-servers-monolith.md)
- [Backend workers](backend.md)
- [Devops](devops.md)
- [Infrastructure](infrastructure.md)

## Implementation

### Frontend

The frontend is implemented in **[VueJS + NuxtJS](https://github.com/yafig/frontend)**. Other frameworks that I might be interested to explore next would be **Svelte** and **NextJS**.

### API Servers & Workers

The API Servers will be implemented in two approaches: Monolithic and Microservice.

#### Monolithic

The monolithic approach will be implemented in **[Python](https://github.com/yafig/api-server-monolith)** using **Django REST Framework** (DRF).

#### Microservice

The microservice approach will be implemented in 2 programming languages: **[Python](https://github.com/yafig/api-server-microservice/tree/master/python) and [Go](https://github.com/yafig/api-server-microservice/tree/master/go)**. Each microservice will be implemented using either:

- Hexagonal architecture
- Model View Controller (MVC) architecture
- Flat-file architecture

#### Backend workers

Backend workers will be implemented in 2 programming languages: **[Python](https://github.com/yafig/backend-worker/tree/master/python) and [Go](https://github.com/yafig/backend-worker/tree/master/go)**. The workers will consume messages from a message queue / pub-sub queue produced by API Servers and perform actions.

## Who made this

This is solo project by [Fadhil Yaacob](http://twitter.com/sdil)
