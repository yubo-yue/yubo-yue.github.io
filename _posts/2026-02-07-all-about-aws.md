---
layout: post
title:  "Setup env for linux kernel 2.6.14 source code"
date: 2026-02-07 01:12:13
categories: linux kernel
---

从0开始搭建环境，可以学习linux kernel。
注意，本文基于的kernel版本为2.6.14. 

# Docker 环境

Example commands:

```bash
# Build the image
docker build -t kernel-2.6.14 .

# Run the container
docker run --rm -it kernel-2.6.14
```

Example image:

![Docker setup diagram]({{ site.baseurl }}/assets/images/docker-conf-1.jpg){: width="80%" }
