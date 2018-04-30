---
title: Zapps
layout: docs
permalink: /zapps/
---

Zoe applications, ZApps, are packaged descriptions of analytics applications. An analytics application is a composition of one or more front-ends, frameworks, libraries, like Jupyter notebooks, Spark clusters, Tensorflow fo PyTorch libraries, etc. We call these analytics services.

A ZApp is composed of:

 * One or more docker images (we use Docker for convenience, changing engine and image format is quite easy)
 * A JSON description that tells Zoe how to start the services and in which order
 * A manifest that helps the Zoe web interface to render the ZApp shop

Below there is a screenshot from the ZApp shop in the Eurecom production deployment.

![ZApp shop at Eurecom]({{ site.url }}/img/zapp-shop.png)

## Classification

Zoe runs processes inside containers and the Zoe application description is very generic, allowing any kind of application to be described in Zoe and submitted for execution. While the main focus of Zoe are so-called "analytic applications", there are many other tools that can be run on the same cluster, for monitoring, storage, log management, history servers, etc. These applications can be described in Zoe and executed, but they have quite different scheduling constraints. Zoe is not a generic application deployment solution and lacks, by design, features like automatic migration, rolling upgrades, etc.

Please note that in this context an "elastic" service is a service that "can be automatically killed or started".

- Long running: potentially will never terminate

  - Non elastic

    - Storage: need to have access to non-container storage (volumes or disk partitions)

      - HDFS
      - Cassandra
      - ElasticSearch

    - Interactive: need to expose web interfaces to the end user

      - Jupyter
      - Spark, Hadoop, Tensorflow, etc history servers
      - Kibana
      - Graylog (web interface only)

    - Streaming:

      - Logstash

    - User access

      - Proxies and SSH gateways

  - Elastic (can be automatically resized)

    - Streaming:

      - Spark streaming user jobs
      - Storm
      - Flink streaming
      - Kafka

- Ephemeral: will eventually finish by themselves

  - Elastic:

    - Spark classic batch jobs
    - Hadoop MapReduce
    - Flink

  - Non elastic:

    - MPI
    - Tensorflow

In general, Zoe is not adapted to deploy long-running storage services.

All the applications in the **long-running** category need to be deployed, managed, upgraded and monitored since they are part of the cluster infrastructure. The Jupyter notebook at first glance may seem out of place, but in fact it is an interface to access different computing systems and languages, sometimes integrated in Jupyter itself, but also distributed in other nodes, with Spark or Tensorflow backends. As an interface the user may expect for it to be always there, making it part of the infrastructure.

The **elastic, long-running** applications have a degree more of flexibility, that can be taken into account by Zoe. They all have the same needs as the non-elastic applications, but they can also be scaled according to many criteria (priority, latency, data volume).

The applications in the **ephemeral** category, instead, will eventually terminate by themselves: a batch job is a good example of such applications.

