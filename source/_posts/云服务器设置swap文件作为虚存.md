---
title: 云服务器设置swap文件作为虚存
date: 2025-01-16 22:55:55
tags: 
	- 云服务器
	- 虚拟存储
	- swap文件
categories: 云服务器
---

前天我还想往常一样快乐地写着代码，用docker兴奋地部署我的项目，部署上去之后突然服务器上的nacos寄了，minio也时寄时不寄。

网上查原因，查到结果是内存不足把nacos挤掉了。

然后还原到nacos崩溃前的状态查看内存`free -h`：

{% asset_img jietu1.png ram1 %}

然后查怎么增加内存，看见了设置虚存，开干：

（我用的是腾讯云服务器）

参考文档：[为你的服务器增加Swap分区](https://cloud.tencent.com/developer/article/1165387)

👆真的超级详细，后面还有一些swap参数的设置，这里不列举了

    sudo fallocate -l 1G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile
    sudo swapon --show
    sudo cp /etc/fstab /etc/fstab.bak
    echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
    # 永久化swap文件↑

然后再次运行`free -h`

{% asset_img jietu2.png ram2 %}

不过说实话java微服务是真占内存