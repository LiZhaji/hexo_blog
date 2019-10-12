---
title: hexo + github pages 搭博客使用记录
date: 2019-10-11 17:35:12
tags:
---

## 更换网址
在'_config.yml'中，有个'permalink'选项，用来定义文章的链接地址
因为不太喜欢默认值，修改为'permalink: :title'
结果出现了点击下载的问题
解决方案：
'permalink: :title/' 或者
'permalink: :title.html'

TODO:
a标签不加download，什么情况下会下载