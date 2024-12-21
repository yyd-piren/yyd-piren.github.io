---
title: nacos配置问题
date: 2024-12-21 14:50:21
tags:
    - nacos
    - springboot
    - springcloud
categories:
    - Java
---
## nacos注册问题

今天springboot项目启动报错如下:

连不上远程服务器的nacos

第一次报错：
```java
NacosException: failed to req API:/nacos/v1/ns/instance after all servers([localhost:8848]
```

按csdn上说的“添加nacos作为配置中心的依赖就好了”

添加了如下依赖：

```java
<!--引入nacos config配置依赖-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>
```

然后报错就变成下面这个了🥲

```java
2022-02-24 14:50:26.189 ERROR 13070 --- [           main] c.a.n.c.config.http.ServerHttpAgent      : [NACOS ConnectException httpGet] currentServerAddr:http://localhost:8848, err : Connection refused (Connection refused)
2022-02-24 14:50:26.378 ERROR 13070 --- [           main] c.a.n.c.config.http.ServerHttpAgent      : [NACOS ConnectException httpGet] currentServerAddr:http://localhost:8848, err : Connection refused (Connection refused)
2022-02-24 14:50:26.578 ERROR 13070 --- [           main] c.a.n.c.config.http.ServerHttpAgent      : [NACOS ConnectException httpGet] currentServerAddr:http://localhost:8848, err : Connection refused (Connection refused)
2022-02-24 14:50:26.778 ERROR 13070 --- [           main] c.a.n.c.config.http.ServerHttpAgent      : [NACOS ConnectException httpGet] currentServerAddr:http://localhost:8848, err : Connection refused (Connection refused)

```

接着查，然后看到一个blog写了“不配置nacos配置的地址，它就默认为localhost:8848这个服务”，原因是因为没找到我在application.yml中配置的nacos

又看到一个博客

{% asset_img nacos.png springboot配置加载顺序 %}

所以我就猜是因为我没写bootstrap.properties文件所以在加载时就按它默认的本地nacos去加载了

然后我创建了bootstrap.properties，加上了：

```java
# nacos服务器地址
spring.cloud.nacos.discovery.server-addr=111.229.119.208:8848
spring.cloud.nacos.config.server-addr=111.229.119.208:8848
spring.cloud.nacos.config.enabled=true
```

顺便把application.yml中的👇删除

```java
cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
      config:
        server-addr: 127.0.0.1:8848
        enabled: true
```

然后再运行就成功啦🙌

改bug出来的东西真得记录一下，不然很容易就忘了。