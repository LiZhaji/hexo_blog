---
title: 博客成长记录
date: 2019-10-11 17:35:12
categories: 实践 
---

## 初步搭建

- `hexo`
- `github pages`

花费两三个小时


## 更换文章显示网址-10.11
在`_config.yml`中，有个`permalink`选项，用来定义文章的链接地址

修改为`permalink: :title`，出现了点击后下载的问题

解决方案：`permalink: :title/` 或 `permalink: :title.html`

TODO:
a标签不加download，什么情况下会下载
能打开则打开，不能打开则下载

## 更换主题-10.21

采用了[next主题](https://github.com/iissnan/hexo-theme-next)，简约风，动画柔顺

- `serve .` 启动，加载了一个只有目录的页面
- `hexo server` 启动，加载了完整的页面
