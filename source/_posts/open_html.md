---
title: 如何打开html页面
date: 2020-01-09 20:10:28
tags: 实践
---
# open in default browser
之前启动html都是采用右键 open in default browser，这样子打开的文件是file协议。
file://文件路径 ，用于访问本地计算机中的文件。
![](https://p4.ssl.qhimg.com/t01d7fb7e981c4f52e1.png)
一般通过浏览器访问的页面属于http协议
file协议相对于http协议，有很多不支持的api，比如history.pushState
# serve
通过serve服务可以使用http协议启动一个页面，默认显示的文件目录是./index.html，若没有此目录，则以固定的形式显示该文件下所有的目录。
![](https://p3.ssl.qhimg.com/t016010b3316753c207.png)