---
author: Daniele Venzano
title: The problem with metrics
layout: post
---

Currently the Zoe deployment at Eurecom is based on a Docker-on-bare-metal configuration. We have been playing with the idea of
moving to Kubernetes on VMs, expanding our OpenStack deployment to all our servers.

The biggest issue we have is recording metrics about who used which resources, when and how. The current system is based on telegraf, kafka and postgres.
Unfortunately telegraf is unable to record VM statistics ([Bug open since 2015](https://github.com/influxdata/telegraf/issues/243)).

OpenStack comes with its own metering project, called Ceilometer. Ceilometer has recently changed its focus, dropping part of its functionality and focusing
only on gathering metrics. For storing these metrics OpenStack now proposes a new project, called Gnocchi.

So basically, to go the OpenStack way, we have to rebuild our measurement pipeline around Ceilometer and Gnocchi.

I dived in and tested these projects to understand how they work and their level of maturity. TL;DR is: not mature enough.

Currently we have 4 compute hosts with ~15 VMs. This is a very small cloud.

Ceilometer + Gnocchi managed to push the load on keystone (the authentication service) beyond horizons never seen before. A token cache can
help, but the problem is in the architecture. Each time Ceilometer wants to store some metrics, it requests a token from keystone, opens an http connection
to gnocchi and gnocchi checks the token. This is very, very wasteful. Yes, you can scale everything horizontally, but when you need to start scaling for metrics about 15 VMs, there is a problem.

With caches and some scaling I managed to bring the system up and to make it more or less performant. So I started looking into the user visualization part: storing big
amounts of metrics in Gnocchi is good, but how to access it? A Grafana plugin is provided. We already use Grafana, so this is very good.  
Unfortunately there are some issues there too. For example math between two metrics recorded at different intervals is not managed correctly, for example vm_memory_size (once per hour) divided by vm_memory_usage (once every 10 seconds) produces errors about invalid JSON.

I encountered a number of other issues in Gnocchi and Grafana that make me think that they are not really used in production by anyone and that they need more time to mature. I like Gnocchi for the following reasons:

- it uses a CEPH backend to store data: we already have CEPH and use it quite a lot, we have the tooling, the experience, etc.
- it is simple to understand, especially the archive policies let you decide the amount of data you want to store with some simple calculations on a piece of paper, so yo now since the beginning how much capacity yo need to reserve

OpenStack moves fast, so hopefully these project will mature quickly. We'll return to them in a while to check their status.

