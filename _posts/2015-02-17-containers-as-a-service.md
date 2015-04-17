---
layout: post
title: Containers as a Service
excerpt: "Containers as a Service"
modified: 2015-02-17
tags: [docker, containers, caas]
comments: true
---

With all the hype around Docker, a new layer of cloud computing is being formed. Traditionally, the cloud consists of IaaS, PaaS and SaaS offerings. IaaS is the Infrastructure-as-a-Service layer, which provides an API that is being consumed by the Platform-as-a-Service layer. This PaaS layer is an abstraction layer, that makes it easy for developers to build and deploy their applications. Popular providers are Heroku and EngineYard, but there are also private solutions like Cloud Foundry and Openshift.


But these layers are fundamentally very different. The IaaS layer provides raw building blocks like CPU, memory and storage, that allow you to compose your infrastructure in a free format. On the other hand, PaaS is pretty much locked down and enforces you to build your applications in a certain way. So what if you don't want to care about the underlying infrastructure, but you do want more freedom to build your applications? I think the container layer solves these problems.


Containers consist of the application itself and the application server responsible for running the application code. Containers are built for portability and can ideally run on every machine, locally or in the cloud. The containers need to be able to talk to eachother, and behave like one single application. But deploying and running these containers is not as easy as it seems. And this is why there is a need for a container platform. This platform should be responsible for wiring these containers together, and should have the knowledge of what container belongs to which application and where it runs.


So what role does Docker play in these layers? The Docker technology is not rocket science, it is just an abstraction layer on top of Linux containers. But the main goal of Docker is to get people agree on a standard way of packaging and deploying applications. And I hope they will succeed!