---
title: dockerfile构建小问题
date: 2024-12-22 12:26:29
tags:
    - docker
    - dockerfile
categories:
    - Docker
---
## 在dockerfile中写FROM openjdk:11会拉不下来

解决方法就是先在本地`docker pull openjdk:11`下来再运行dockerfile