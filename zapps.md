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

![ZApp shop at Eurecom](/img/zapp-shop.png)

