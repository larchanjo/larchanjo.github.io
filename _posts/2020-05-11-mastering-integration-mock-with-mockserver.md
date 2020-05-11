---
title: "Mastering integration mock with Mockserver"
summary: "..."
tags:
    - " #java"
    - " #api"
    - " #mockserver"
---

# Introduction

Hi, how are you? I hope everything is fine!

Today I would like to talk about integration and mock. In the last few years, the way that we build software is changing 
constantly and one way to start a new project is by using the API-first approach.

Summarizing, An API-first approach means that for any given development project, your APIs are treated as “first-class 
citizens.” That everything about a project revolves around the idea that the end product will be consumed by mobile 
devices, and that APIs will be consumed by client applications.

So as your APIs are treated as "first-class citizens", your first task as architecture is to define and provide them
 as soon as possible. But Luram, my backend it is not ready.

That is the main advantage related to the API-first approach, you enable parallel development and to do it, you need 
to mock your APIs and when your backend is ready, it is time to turn off the mock and start the integration-test phase.

That sounds a good idea, right?

Yes, it is a good idea, but sometimes the API has many flows and became very complex to mock and maintain it.

On my previous project, I had this scenery so I had thought,  I need to keep it simple to maintain and evolve. Then 
I have found the MockServer project.

# What is MockServer?

[Mockserver](https://github.com/mock-server/mockserver) is an open-source project that enables easy mocking of any 
system you integrate with via HTTP or HTTPS with clients written in Java, JavaScript, and Ruby. MockServer also includes 
a proxy that introspects all proxied traffic including encrypted SSL traffic and supports Port Forwarding, Web Proxying
, etc.

# Why use MockServer
  
MockServer allows you to mock any server or service via HTTP or HTTPS, such as a REST or RPC service.

# Getting Started

Well, We have seen all the required concepts, so it is time to get your hands dirty.

## Start MockServer

To do that, first, we need to run the Mockserver, there are many [ways](https://www.mock-server.com/mock_server/getting_started.html#start_mockserver)
, but today, we are going to use [Docker](https://www.docker.com/), so run the command bellow

```
docker run --rm --net=host mockserver/mockserver -serverPort 1080 -logLevel INFO
```