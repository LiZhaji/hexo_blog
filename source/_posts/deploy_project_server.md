---
title: 前端项目部署到阿里云
date: 2019-10-19 19:31:43
categories: 实践
---
## 一、进入阿里云服务器
- 打开cmd命令窗口，键入 ssh root@服务器ip地址 
如果忘了ip地址，打开阿里云服务器登录，点击左上角-轻量应用服务器，点击进入就能看见
![](https://pic4.zhimg.com/80/v2-a19a129277ba6526ae73daca709b3817_hd.jpg)

![](https://pic3.zhimg.com/80/v2-93be89ea241f641b26435e1b98e8cd32_hd.png)
- 键入密码
## 二、新建目录
- 第三步的clone会自建目录，若不需要，跳到第三
- 新建文件夹 `mkdir <folderName>`
- 进入文件夹 `cd <folderName>` 
- 返回上级目录 `cd ..`
- 查看子文件 `ls`  
- 查看具体文件 `cat <foldName>`
## 三、clone项目
- 下载 `git yum install git`
- 克隆项目 `git clone <仓库地址>`
- 出现报错：
![](https://pic2.zhimg.com/80/v2-88f0b462a90b4a0ad26d855ef74caad5_hd.jpg)
- 解决错误：生成该服务器的ssh密钥 `ssh-keygen -t rsa -C "yourEmaill@163.com"` 
  拿到密钥 `cat <存储目录>`
  添加到github账号上
![](https://pic2.zhimg.com/80/v2-b4e9c79dac0e38a0b8df659918e35b99_hd.jpg)
- 重新clone项目
## 四、运行
- 进入到需要部署的项目的文件夹中，执行 `serve .`
- 如果报错提示： `-bash: serve: command not found`  执行 `npm i serve -g` 下载
- 进入阿里云-轻量应用服务器管理控制台-防火墙-添加规则。将5000端口号新增进去
![](https://pic4.zhimg.com/80/v2-71df4229830f70e04760daf7f47f6c3f_hd.jpg)
- 重新 serve .
![](https://pic1.zhimg.com/80/v2-76735e6bc0773a693ff23e6ab0f684ec_hd.jpg)
- 要注意以下几点：
1. 要访问的话，用该服务器ip地址 + 启动的端口号
2. serve是用来启动服务的， . 表示当前目录，打开网址默认显示 index.html  ，要是index不存在，就显示空白页面。
3. 如果该服务关闭，则页面也就无法访问了。