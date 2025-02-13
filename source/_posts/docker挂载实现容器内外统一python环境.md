---
title: docker挂载实现容器内外统一python环境
date: 2025-02-13 13:06:26
tags:
	- docker
	- python
categories:
    - Docker
---
## docker挂载实现容器内外统一python环境
DockerFile:
```dockerfile
FROM openjdk:14
LABEL authors="sunyushuo"
WORKDIR /app
COPY target/HIT-File-0.0.1-SNAPSHOT.jar /app/app.jar
EXPOSE 8005
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```
挂载命令：
```shell
docker run -d -p 8005:8005 --name hitfile -v /usr/bin/python3:/usr/bin/python3 -v /usr/bin/python3.11:/usr/bin/python3.11 -v /usr/local/lib/python3.11:/usr/local/lib/python3.11 -v /usr/local/lib/python3.11/site-packages:/usr/local/lib/python3.11/site-packages  -v /root/pythoncode:/root/pythoncode/ -v /usr/lib64:/usr/lib64 -v /usr/local/lib64:/usr/local/lib64 -v /usr/lib/python3.11/:/usr/lib/python3.11/ -e PYTHONPATH=/usr/local/lib/python3.11/site-packages efed80bbb5cb
```
如果还显示缺库的话就在容器外面执行`pip show [ModuleName]`，然后加到挂载路径里就行了。