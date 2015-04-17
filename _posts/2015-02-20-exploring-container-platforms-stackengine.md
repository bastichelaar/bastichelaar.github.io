---
layout: post
title: "Exploring container platforms: StackEngine"
excerpt: "Docker has been around for more than a year already, and there are a lot of container platforms popping up. In this series of blogposts I will explore these platforms and share some insights. This blogpost is about StackEngine."
modified: 2015-02-20
tags: [docker, stackengine, container platform, containers]
comments: true
---
Docker has been around for more than a year already, and there are a lot of container platforms popping up. In this series of blogposts I will explore these platforms and share some insights. This blogpost is about [StackEngine](http://www.stackengine.com/ "StackEngine").

*TL;DR: StackEngine is (for now) just a nice frontend to the Docker binary. Nothing more.*

Requirements
---------------------

First, let's define some requirements. The things I really need:

* A group of containers as first class citizens: almost every application will consist of more than one container. So I need a way to deploy multiple containers belonging together
* An overview of what runs where: I want to know which container will end up on which host. When debugging containers, often you have to debug from the host where it is running.
* Smart scheduler: I don't want to decide where to run what container, but leave that up to the container platform.
* Integrated logging: I need to see the logs of a running container. If this is not possible, I need at least to be able to connect an external logging solution.
* Easy to install, easy to use: installing or using a container platform should not be rocket science.
* Resilient: I want to be able to tear down a node without the cluster falling apart. The cluster should restore itself, or at least report what's wrong and how to fix it. And any containers running on that node should be spawned elsewhere.
* Pluggable: I want to be able to connect additional software to the platform, like Logstash for logs, or a different scheduler.

So let's get started with StackEngine. According to their website, StackEngine is an *Enterprise-grade management for Container-centric Architectures*. It has a nice webinterface, that allows you to deploy and manage your Docker containers. The architecture uses a Raft consensus protocol, so the cluster should be resilient and scalable.

Installation
----------------

The [documentation](http://docs.stackengine.com/ "documentation") is pretty straightforward. For a minimal cluster, you need at least 4 hosts:

* 1 Master
* 2 Followers
* 1 Client

You can install the needed daemons on the host itself, or you can run them in Docker containers. I went for the last option, and had a cluster up and running within half an hour. When entering the address of the webinterface in my browser, I had a nice looking page with spinning gears.

<figure>
  <img src="/images/stackengine.png" alt="">
</figure>

Using StackEngine
---------------------------
Ok, the webinterface works, the Docker logs don't give any errors, so I should be able to deploy an application now, right? So I searched for the "Create container" button or something similar, but I couldn't find anything. But I did see a red exclamation mark in the top-right corner. When clicking the link, I saw a page where I could enter a license. Guess what: no license was installed. So... maybe that was the reason why I couldn't do anything with the webinterface. I searched in my mailbox for the license, and found a link to the Documentation. And indeed, after putting in the generated license keys, the gears transformed into numbers, and some buttons appeared. Ready to go!

First deployment
-------------------------
Let's first try to deploy a simple Nginx container. I went to the Containers page and searched for a button like "Create container". But it wasn't there. Maybe I should search for a Docker image? But when entering nginx in the search box, it didn't return any results. Well, let's take another look at the documentation. Apparently you need to create a new Docker Component (confusing name). So I entered the component name, and looked for a way to search the Docker hub for Nginx containers. Unfortunately, there is no search function. I had to manually enter the full Docker image name. This was a disappointment: how difficult is it to connect this to the Docker Hub?

Anyway, I entered the Nginx image name, and created the component. Next, I clicked **run**, and another window popped up, asking me where to run the container. Imagine I have 100's of hosts, I want the container to end up wherever there is enough room. So I left the Hosts field empty. I added port 80 to map to the host itself, and clicked **Go**. But it didn't work...

<figure>
  <img src="/images/stackengine2.png" alt="">
</figure>

Ok, so I have to schedule the containers myself apparently. This was a major drawback. After selecting a node, I finally was able to deploy a Docker container. So let's see if I can see some output or logs.

Managing a running container
--------------------------------------------

When clicking on the Containers tab, I could see my nginx container running. After clicking on it, I saw a lot of details, but I couldn't find a link to logs or container output. Guess what: it isn't integrated. Even a one-off command's output doesn't get displayed.

What about scaling containers? Disappointing again: there is no way to scale up a running container. The only working way is to specify the number of containers when creating a new one.

Resiliency test
---------------------
Let's shaken things up a bit and tear down one of the nodes. Ideally, the container running on that node should be spawned on a different node. But guess what: StackEngine didn't notice at first. After a minute or so, it displayed that the node was Inactive. But the containers running on were still in Running state. Magic...!


Pluggability
-----------------
It is probably possible to extend the platform, but there is no documentation at all. The related documentation page says:

*The API exists and can be gleaned through developer tools to see requests and headers.*

Yeah, right...

Conclusion
----------------
StackEngine is nothing more than just a fancy frontend on top of Docker (for now at least). The good things are:

* Everything runs in Docker containers
* Installation is pretty straightforward
* The platform is lightweight

But that's about it. The bad things are:

* No resiliency: the cluster is resilient, but it doesn't do anything with running containers
* Lack of insight of running containers: no output, no logging
* Lack of developer documentation
* Expensive for a simple frontend

So there is still a lot of work to be done to meet my requirements. Too bad...

Scoring table
-------------

| Requirement                               | StackEngine |
|:------------------------------------------|:-----------:|
| Group of containers as 1st class citizen  |      1      |
| What runs where?                          |      3      |
| Smart scheduler                           |      1      |
| Integrated logging                        |      1      |
| Easy to install                           |      3      |
| Resilient                                 |      2      |
| Pluggable                                 |      1      |
|===========================================|=============|
| Total                                     |     12      |

(1=bad, 5=good)

*Update: Today, February 27th, I had a chat with Eric Anderson, CTO of StackEngine. I provided some more feedback, and he shared their vision and roadmap. The points mentioned above are being addressed. I will write another blogpost in a few months.*

**Next contender: [Rancher](http://rancher.io/ "Rancher")!**
