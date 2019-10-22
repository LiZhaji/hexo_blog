---
title: 前端项目部署到阿里云
date: 2019-10-19 19:31:43
categories: 实践
---
## 进入阿里云服务器
- 打开cmd命令窗口，键入 ssh root@服务器ip地址 
如果忘了ip地址，打开阿里云服务器登录，点击左上角-轻量应用服务器，点击进入就能看见
![](https://cdn.nlark.com/yuque/0/2019/png/317801/1571445527183-26d2608b-0db4-4f72-afd1-fd8a390b9479.png?x-oss-process=image/resize,w_1440)

![](https://cdn.nlark.com/yuque/0/2019/png/317801/1571445219114-1c690aa7-0e2c-4cf1-8d22-dc195feed2e8.png)
- 键入密码
## 新建目录
- 第三步的clone会自建目录，若不需要，跳到第三
- 新建文件夹 `mkdir <folderName>`
- 进入文件夹 `cd <folderName>` 
- 返回上级目录 `cd ..`
- 查看子文件 `ls`  
- 查看具体文件 `cat <foldName>`
## clone项目
- 下载 `git yum install git`
- 克隆项目 `git clone <仓库地址>`
- 出现报错：
![](https://cdn.nlark.com/yuque/0/2019/png/317801/1571447959761-b30bdb0a-8058-4826-9ee3-2936309e6db7.png?x-oss-process=image/resize,w_1060)
- 解决错误：生成该服务器的ssh密钥 `ssh-keygen -t rsa -C "yourEmaill@163.com"` 
  拿到密钥 `cat <存储目录>`
  添加到github账号上
![](https://cdn.nlark.com/yuque/0/2019/png/317801/1571449117820-69323ce5-1cd6-4fc8-8224-e492666e9733.png)
- 重新clone项目
## 运行
- 进入到需要部署的项目的文件夹中，执行 `serve .`
- 如果报错提示： `-bash: serve: command not found`  执行 `npm i serve -g` 下载
- 进入阿里云-轻量应用服务器管理控制台-防火墙-添加规则。将5000端口号新增进去
![](https://cdn.nlark.com/yuque/0/2019/png/317801/1571490245509-1a97e4fa-307d-4631-a102-b0b855067697.png?x-oss-process=image/resize,w_1492)
- 重新 serve .
![](https://cdn.nlark.com/yuque/0/2019/png/317801/1571483903740-e79125aa-2c4c-4631-88e7-1e025322820d.png)
- 要注意以下几点：
1. 要访问的话，用该服务器ip地址 + 启动的端口号
2. serve是用来启动服务的， . 表示当前目录，打开网址默认显示 index.html  ，要是index不存在，就显示空白页面。
3. 如果该服务关闭，则页面也就无法访问了。