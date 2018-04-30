---
layout: docs
title: About
permalink: /about/
---

## The project

Zoe was born in 2015 as a solution to the growing need in Eurecom to have a flexible and reproducible environment to do research on data intensive frameworks.
Virtual machines, managed via OpenStack, were cumbersome and users, once finished with their work, would not terminate them, wasting resources in our computing cluster.

Since then Zoe has grown a web interface and several enterprise-friendly features, like LDAP login. Several enterprises use Zoe in production and participate in development activities with bug reports and feature requests. Notable examples include Air France KLM and KPMG.

The project is released with an open source license. The source code is available in two repositories:

 * [Eurecom's GitLab](https://gitlab.eurecom.fr/zoe): day-to-day development, use this to develop Zoe
 * [GitHub](https://github.com/DistributedSystemsGroup/zoe): code is published only when all tests are passed, use this to deploy Zoe

### The team

 * [Pietro Michiardi](http://www.eurecom.fr/~michiard/): Professor and data science department head
 * [Daniele Venzano](https://brownhat.org/): Research engineer, Zoe creator and main developer
 

### The data science department at EURECOM

For more information about the Data Science department at Eurecom, please visit the [institutional website](http://www.eurecom.fr/en/research/department-data-science).

### Focus

Zoe focus is data analytics. This focus helps defining a clear set of objectives and priorities for the project and avoid the risk of competing directly with generic infrastructure managers like Kubernetes or Swarm. Zoe instead sits on top of these "cloud managers" to provide a simpler interface to end users who have no interest in the intricacies of container infrastructures.

Data analytic applications do not work in isolation. They need data, that may be stored or streamed, and they generate logs, that may have to be analyzed for debugging or stored for auditing. Data layers, in turn, need health monitoring. All these tools, frameworks, distributed filesystems, object stores, form a galaxy that revolves around analytic applications. For simplicity we will call these "support applications".

Zoe does not focus on support applications. Managing a stable and fault tolerant HDFS cluster, for example, is a task for tools like Puppet, Chef or Ansible and is done by system administrators. Zoe, instead, targets data scientists, that need to use a cluster infrastructure, but do not usually have sysadmin skills.

### Motivations

The fundamental idea of Zoe is that a user who wants run data analytics applications should not be bothered by systems details, such as how to configure the amount of RAM a Spark Executor should use, how many cores are available in the system or even how many worker nodes should be used to meet an execution deadline.

Moreover final users require a lot of flexibility, they want ot test new analytics systems and algorithms as soon as possible, without having to wait for some approval procedure to go through the IT department. Zoe proposes a flexible model for applications descriptions: their management can be left entirely to the final user, but they can also prepared very quickly in all or in part by an IT department, who sometimes is more knowledgeable of resource limits and environment variables. We also plan to offer a number of building blocks (Zoe Frameworks) that can be composed to make Zoe Applications.

Finally we feel that there is a lack of solutions in the field of private clouds, where resources are not infinite and data layers (data-sets) may be shared between different users. All the current Open Source solutions we are aware of target the public cloud use case and try, more or less, to mimic what Amazon and other big names are doing in their data-centers.

Zoe strives to satisfy the following requirements:

 * easy to use for the end-user
 * easy to manage for the system administrator, easy to integrate in existing data-centers/clouds/VM deployments
 * short (a few seconds) reaction times to user requests or other system events
 * smart queuing and scheduling of applications when resources are critical

Kubernetes, OpenStack Sahara, Mesos and YARN are the projects that, each in its own way, come near Zoe, without solving the distributed analytics problem.

Kubernetes
----------
Kubernetes is a very complex system, both to deploy and to use. It takes some of the architectural principles from Google Borg and targets data centers with vast amounts of resources. We feel that while Kubernetes can certainly run analytic services in containers, it does so at a very high complexity cost for smaller setups. Moreover, certain scheduler choices in how preemption is managed do not apply well to environments with a limited set of users and compute resources, causing a less than optimal resource usage.

OpenStack Sahara
----------------
We know well [OpenStack Sahara](https://wiki.openstack.org/wiki/Sahara), as we have been using it since 2013 and we contributed the Spark plugin. We feel that Sahara has limitations in:

 * software support: Sahara plugins support a limited set of data-intensive frameworks, adding a new one means writing a new Sahara plugin and even adding support for a new version requires going through a one-two week (on average) review process.
 * lack of scheduling: Sahara makes the assumption that you have infinite resources. When you try to launch a new cluster and there are not enough resources available, the request fails and the user is left doing application and resources scheduling by hand.
 * usability: setting up everything that is needed to run an EDP job is cumbersome and error-prone. The user has to provide too many details in too many different places.

Moreover changes to Sahara needs to go through a lengthy review process, that on one side tries to ensure high quality, but on the other side slows down development, especially of major architectural changes, like the ones needed to address the concerns listed above.

Mesos
-----

Mesos is marketing itself as a data-center operating system. Zoe has no such high profile objective: while Zoe schedules distributed applications, it has no knowledge of the applications it is scheduling and, even more importantly, does not require any change in the applications themselves to be run in Zoe.

Mesos requires that each application provides two Mesos-specific components: a scheduler and an executor. Zoe has no such requirements and runs applications unmodified.

YARN
----

YARN, from our point of view, has many similarities with Mesos. It requires application support. Moreover it is integrated in the Hadoop distribution and, while recent efforts are pushing toward making YARN stand up on its own, it is currently tailored for Hadoop applications.

