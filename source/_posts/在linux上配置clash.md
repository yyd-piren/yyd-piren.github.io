---
title: 在linux上配置clash
date: 2024-12-22 12:30:25
tags:
    - linux
categories:
    - Linux
---

[参考文章](https://wdgjx.com/article/4404.html)

1. 下载`clash-linux-amd64.tar.gz`(放到github博客仓库与文章同名文件夹下了)

2. 解压重命名为`clash`

3. 执行 `cd && mkdir clash` 在用户目录下创建 `clash` 文件夹，把上面重命名的`clash`文件放到新建的文件夹里来

4. 在终端 cd 到 Clash 二进制文件所在的目录，执行 `wget -O config.yaml 订阅的地址` 下载 Clash 配置文件（这里有时候会下载的config.yaml不对劲，换成我们自己的`config.yaml`内容就好了）

5. 执行`chmod +x clash`，给文件赋予执行权限

6. 执行 `./clash -d .`

7. 以 Ubuntu 19.04 为例，打开系统设置，选择网络，点击网络代理右边的 ⚙ 按钮，选择手动，填写 HTTP 和 HTTPS 代理为 `127.0.0.1:7890`，填写 Socks 主机为 `127.0.0.1:7891`，即可启用系统代理。

8. 打开firefox就能访问youtube.com了