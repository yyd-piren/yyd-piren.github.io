---
title: test
date: 2024-09-22 09:44:55
tags:
    - 测试
    - hexo
categories: 杂
---
# 测试资源文件夹功能
{% asset_img miku2.png This is an example image %}
{% asset_img "miku2.png" "This is an example image" %}
{% asset_img "miku2.png" "This is an example image" %}
## 结束
结论就是在`hexo generate`之前要每次都`hexo clean`一下，不然缓存会出问题
真是成也缓存，败也缓存