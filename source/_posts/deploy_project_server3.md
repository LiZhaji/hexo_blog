---
title: 前端项目部署到阿里云 lnmp运行(3)
date: 2019-10-30 14:38:20
tags:
---
[上一篇文章](http://blog.escript.cn/deploy_project_server2/)使用了`pm2`保持进程不被杀死，从而提供永久访问。但是此方法有个弊端就是只能开启不同的端口号进行访问，域名之后还要跟端口号，不美观

因此，这里使用`LNMP`提供多80端口访问服务。

LNMP = Linux + Nginx + MySQL + PHP

其中用到的是Nginx ，因为此方法下载比较方便，所以就下载lnmp了。
## 一、安装
进入服务器后， 在命令行输入`lnmp`
若显示`-bash: lnpm: command not found`，则需要安装，
若显示一系列使用方法Usage，直接跳到第二步。

- 进入[https://lnmp.org/nginx.html](https://lnmp.org/nginx.html)选择
新手可以参照下图，高手请绕路。MySQL和PHP必选，一些~~不了解的~~不需要用到的就暂时不安装了。
![在这里插入图片描述](https://pic1.zhimg.com/80/v2-a5ca3f9aaad7ee998e486cd71b98bc00_hd.jpg)
- 点击生成，把生成的安装命令复制到服务器命令行下（右键就是复制），回车即可。
- 安装过程非常慢，不用等，可以先看看之后的操作。

## 二、添加域名
- 登录[dnspod.cn](dnspod.cn)，添加要访问的二级域名，操作如下：
 管理控制台 -> DNS管理/域名管理 -> 我的域名 -> 点击需要的顶级域名 -> 添加记录
- 跳出信息让我们填写：
	- 主机记录就是二级域名的名字，比如为xxx，顶级域名为aaa.com，那么访问的网站就是xxx.aaa.com
	- 记录类型与记录值挂钩，类型选A表示记录值只能是ip地址，类型选CNAME表示记录值是一个域名，这样的好处是当需要更换ip地址时，只用换一个即可。
	- 别的都默认即可。可以点击取消后面的？查看帮助
- 填写完后确认

## 三、关闭之前的服务
进入服务器
- `pm2 list`
- `pm2 delete <要关闭的js>`
## 四、添加虚拟主机
进入服务器
- `lnmp vhost add`
	- `Please enter domain` 输入 刚刚创建（xxx.aaa.com）/需要访问 的域名
	- `Enter more domain name` 输入更多域名，不需要，回车就行
	- `Default directory` 输入挂钩的目录，就是前两篇文章中起serve 的目录，比如我的就是 `/root/hexo_blog/public`
	- 之后一直输入n就可以了
- 进入目标网页（xxx.aaa.com）
- 若出现403，可能原因是nginx.config的user改为和启动用户不一致
	- `vi /usr/local/nginx/conf/nginx.conf`
	- `user xxxxx` 将xxxxx改为启动用户，我的是root，所以是`user root`
	- 改完后按ese退出编辑模式，输入`:wq`回车，保存退出
	- `lnmp restart`
	- 进入目标网页
	- 若还是403，可以参考[此网页]( https://blog.csdn.net/qq_35843543/article/details/81561240)

- 这样，一个虚拟主机就添加完毕了。若要添加多个，重复二四步骤即可。