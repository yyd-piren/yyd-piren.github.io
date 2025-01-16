---
title: HikariPool报错解决小记
date: 2025-01-12 16:50:38
tags: HikariPool
categories : Java
---

启动时一开始可以响应，过一会儿就不行了并报错如下：

HikariPool-1 - Failed to validate connection com.mysql.cj.jdbc.ConnectionImpl@18e7d21c (No operations allowed after connection closed.). Possibly consider using a shorter maxLifetime value.

解决参考：

1. [HikariPool-1 - Failed to validate connection com.mysql.cj.jdbc....Possibly consider using a shorter](https://blog.csdn.net/cobracanary/article/details/105257594)

2. [SpringCloud 中使用HikariPool 报错Possibly consider using a shorter maxLifetime value](https://blog.csdn.net/xgw1010/article/details/113604080)

总结：

问题原因：用springboot连接数据库的时候，会建立一个和数据库的连接，这个连接保存在数据库连接池中，现在这个连接已经time out已经不能用了，但是这个连接还是保存在数据库连接池中，springboot仍然使用这个连接去连接数据库，所以就会报错。

解决方法：修改或设置spring.datasource.hikari.max-lifetime=120000

或者直接用这个现成的：
```java
spring:
  datasource:
    hikari:
  		connection-timeout: 10000
 		validation-timeout: 3000
 		idle-timeout: 60000
 		login-timeout: 5	
 		max-lifetime: 60000
  		maximum-pool-size: 10
  		minimum-idle: 5
  		read-only: false
```