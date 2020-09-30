---
title: 手把手教你从零上线小程序+Node.js服务端 (下)
date: 2020-09-29 21:52:16
tags:
---
# 六、搭建线上数据库

也就是把我们的本地数据库到服务器上照搬一套。

第一步，下载`mysql`

第二步，利用`phpMyAdmin` 实现网页可视化数据库，方便查看和管理

## LNMP

有一个[LNMP一键安装包](https://lnmp.org/auto.html)可以帮助我们快速在服务器上搭建好Nignx、MySQL和PHP，省去自己单个下载的麻烦。



- 进入页面后，新手可以参照下图选择。MySQL和PHP必选，一些不了解的不需要用到的暂时不安装了。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600353730552-9880ec55-9197-4595-a55f-2aff4b6f4a6f.png?x-oss-process=image%2Fresize%2Cw_1500)

- 点击生成，把生成的安装命令复制到服务器命令行下（右键就是复制），回车即可。
- 安装过程非常慢，不用等，可以先看看之后的操作。



安装完成后，输入`lnmp`看看是否出现相关信息，若出现了表示安装成功了。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600355017953-ceb73acb-e488-4f98-9417-ec901e702d57.png)



`lnmp mysql start` 启动数据库。

输入网址`yourIp/phpmyadmin`即可看到线上数据库，需要输入账户和密码。



## 导出.sql

将本地电脑上创建好的数据库导出为`.sql` （创建该数据库的命令sql集合）避免在云服务器上重复创建了。

1. 选择Adminitration
2. Data Export 数据导出
3. 选择想要导出的数据库
4. 导出表格还是导出数据还是都导出，这里只导出表格
5. 导出为一个.sql文件，选择导出目录
6. 包括数据库创建
7. 开始导出

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600356857489-680afc1f-6301-4bbe-abec-0a0362e6d2c6.png?x-oss-process=image%2Fresize%2Cw_1500)

然后，我们拿到了`mini_demo.sql`

## 导入.sql

来到phpMyAdmin页面，登录之后选择主页，点击导入，选择刚刚生成的sql文件，滚动到最下面点击执行。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600355905276-5ec1cee9-f48a-4e77-ad5d-72a38c418586.png?x-oss-process=image%2Fresize%2Cw_1500)

最后执行结果应该为这样。如果导入过程出现报错，发现原因后可以打开sql文件手动修改。比如B表有个外键引自

A表，那必须要先创建A表，否则就会报错，这时候把两个表的创建命令换一下位置就行。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600356675908-07e068bf-bad9-4bbe-95a6-80b039b81720.png?x-oss-process=image%2Fresize%2Cw_1500)

检查一下，没错。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600356986311-5ff0a9f6-40e2-4b29-8d13-e6361162af9e.png)



恭喜你，线上数据库搭建完了。（lnmp可能还在下载，先看看下一节）



# 七、部署到云服务器并启动

上线其实很简单，无非就是让我们的后端代码在云服务器上跑起来。

如果你还不知道云服务器是什么，可以去[阿里云](https://www.aliyun.com/?utm_content=se_1000301881)点点看看。如果你是个学生并且对开发有兴趣，（请赶快买吧！）学生价还是很香的哟。



到此为止，假设我们已经拥有了自己的服务器。



## 登录服务器

- 打开终端
- `ssh root@yourip`
- 输入密码
- 登录成功

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600445754554-2dd73dfc-ad4e-4ee9-952f-8dcdb82030bd.png)

## 拉取代码

如果你的本地代码还没上传到GitHub，赶紧的先上传。步骤就不再赘述，一搜便是。

- git 可用 

`git --version` 查看git是否可用，若出现版本号，直接开始拉取代码。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600186697645-9ab45cb4-5b86-42ac-9e75-83bc1958400d.png)

若报错，可以使用`yum install git` 下载git（云服务器的操作系统通常会自带yum）

下载完后，添加用户名和邮箱

```
git config --global user.name 'yourName'
git config --global user.email 'your Email'
```



- clone 项目

进入到目录后，使用`git clone <项目地址>` 拉取代码到服务器上。

若使用ssh地址并且服务器上的git密钥没有添加到你的GitHub帐号上，会报无权限错误，有两种解决方法（推荐2）

1. 改用https地址clone
2. `ssh-keygen -t rsa -C 'yourEmaill'` 生成密钥并添加到GitHub帐号上。



重新clone

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600188009893-ee1ad4d0-6a51-4b54-90ad-2dcd7e66c1fd.png?x-oss-process=image%2Fresize%2Cw_1500)



## 启动项目

- 进入项目目录 cd xxx （按下tab键会有文件夹名称提示）

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600188254981-1165fbec-17b5-49b5-a15f-3dd12c6dae1f.png)

- 下载依赖包 `npm install` ，可简写为`npm i`

若command not found ，说明没有npm环境，同时检查一下是否有node，不存在可以参考[此文章](https://blog.csdn.net/abcdefg2343/article/details/81355002)进行下载，两种方式选择一种就行。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600188506480-57eb3416-f6b5-4d7c-aeb1-2a0333a54419.png)

- 按照本地同样的方式启动 `export XXX="xxx" ... && node index.js`

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600189142510-060ee57f-c805-4b84-9378-c7149bdac2bb.png)

- 若出现以上信息，说明云服务器本地已经成功启动服务端了，外界访问它的地址是 `yourIP:端口号`

例如，我的是这个[地址](http://blog.escript.cn:3000/)（域名通过DNS解析最后访问的也是ip地址）



### 持久化

普通的node 启动项目，当退出终端之后，服务端也就停止运行了，不能实现随时随地访问。

我们使用npm包 `pm2` 工具持久化运行。

`npm install pm2 -g`下载

`pm2 -v`检测



然后使用下面的命令启动项目，请先填入自己的环境变量信息。

```
pm2 start index.js --env={"DATABASE": "","USER": "","PWD":"","APPID": "","SECRET": ""}
```

但是通过这种方式启动起来的项目不会输出信息，所以先确保使用`node`启动没问题后，再使用该方法启动，否则一旦出错，都不知道问题在哪里。



## 线上联调

这时把小程序的`baseUrl` 替换为我们的服务器启动该项目的地址，然后开始调试吧。



过程中总会出现问题的，遇到问题不要心急，要从可能的错误原因一个个排查。

分享一个我的错误吧，login登录的时候报的错。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600359008625-a30e8bbb-d83a-43c9-8eaa-b4e03038676c.png?x-oss-process=image%2Fresize%2Cw_1500)

访问数据库被拒绝，需要密码。第一反应是数据库的问题，`mysql -u root -p` 输入密码后就进入数据库了，那说明数据库本身没问题，可能的原因就是环境变量的用户名和密码写错了，检查一下输出，果然密码的引号是中文格式的，所以报错了。

修改完后重启，就没什么问题了。



# 八、配置https

小程序发布和联调，服务端地址都需要使用https协议，否则啥也访问不到。



https证书申请通常是和域名绑定的，所以我们先需要一个域名。在[阿里云域名](https://dc.console.aliyun.com/next/index?spm=5176.12818093.0.ddomain.5aa416d0snfnXD#/domain/list/all-domain)或者[腾讯云域名](https://console.dnspod.cn/)下购买一个域名。



## ip地址改域名

个人购买的绝大部分是二级域名，也就是a.b形式的。拿到域名后，去[域名解析页](https://console.dnspod.cn/dns/list)添加域名，然后添加记录（三级域名）。

什么是二三级域名？例如[tieba.baudu.com](http://tieba.baudu.com)，其中`.com`是一级（顶级）域名，`baidu`是二级域名，`tieba`是三级域名。二级域名可以衍生出很多三级域名。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600433291993-979374af-8a21-4fb3-b149-aa93b7dd7a58.png)



这种不对外开放的后端地址，我们添加一个三级域名映射即可。

主机记录就是三级域名名字

记录类型为`CNAME` 表示映射到记录值对应的ip地址；如果为`A` 表示映射到ip。这样的好处就是如果ip有变动，只修改一次ip即可

记录值就是ip地址，可以为ip或者域名

最后点击确认即可



![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600434252410-baa61e4d-32cc-4746-a1ca-cfc7f4384b61.png?x-oss-process=image%2Fresize%2Cw_1500)

我最后要访问的服务器地址就是[mini_demo.escript.cn](http://mini_demo.escript.cn)，进入之后的页面与直接访问自己的ip地址一样，说明域名解析成功了。





## 申请SSl证书

[阿里云](https://yundun.console.aliyun.com/)或[腾讯云](https://console.cloud.tencent.com/ssl)上可以免费购买ssl证书，前提需要实名认证。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600434157877-0b38dd35-871a-4006-acc5-e29d8ad7fc94.png)

买完后绑定上一步生成的域名，验证通过后就可以下载使用了。

这是阿里云的免费证书，点击下载，选择其中一个服务器，这样下载到的是本地电脑上。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600435517047-389c6d14-2cb4-4dfb-957e-4d3b47648cc2.png)

如果项目在自己电脑上运行，我们可能直接会把证书文件放到项目文件夹里，但如果该项目要上传到服务器，则下载的证书文件最好不要直接暴露在项目中，可以选择下载到服务器别的文件夹里。

有两种方法

- 直接下载到服务器：wget url下载链接
- 若url失效，则先下载到本地，使用`scp xxx.zip root@ip:/root/https` ，使用`unzip xxx.zip` 命令解压



## 启动https服务

需要最后一次修改服务端代码。

下载https`npm i https`，用来启动https服务。

```
// index.js

const https = require("https")

// SSL options
// 若读取不到文件，则启动http服务
try {
  const options = {
    key: fs.readFileSync("/root/https/xxx.key"), //ssl文件路径  下载下来的证书文件
    cert: fs.readFileSync("/root/https/xxx.pem") //ssl文件路径  下载下来的证书文件
  };
  // 创建https 服务
  const httpsServer = https.createServer(options, app.callback());
  httpsServer.listen(443); // 默认监听443
  console.log("已启动https服务");

} catch (error) {
  app.listen(3000);
  console.log("无证书文件！已启动http服务，端口3000");
}
```

这些也都先在本地电脑上编码测试，通过之后再上传到服务器。

这个过程应该没什么问题。完成后，与之前一样自测一遍https接口，通过之后把小程序的`baseUrl`改成最新的https地址。



过五关斩六将，终于来到了激动人心的时刻。

确保你的小程序本地编译都通过后，来，向前一步，让梦想照进现实。



# 九、审核发布

## 上传

进入微信开发者工具，点击右上角的上传按钮，跳出一个提示弹窗，点击确定。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600438579899-d56f72c5-cc6c-4fd8-8ffc-735ef55f77bc.png?x-oss-process=image%2Fresize%2Cw_1500)



然后输入版本号和备注。

如果是正式版，可以发布为v1.0.0；试用版v0.0.x

版本号不是随意取的。虽然输入什么都可以上传成功，但作为一名（专业的）开发人员最好还是遵守一下[约定](https://www.jianshu.com/p/6df3b8dcbe17)，以后工作中都用得上。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600438465930-bca4c37d-df6d-4f38-812e-abad57de60a3.png)![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600438579791-268edebe-6ff2-4aae-b727-0b7fbc2ab636.png)

## 体验版

上传成功之后，登录[小程序](https://mp.weixin.qq.com/wxamp/wacodepage/getcodepage?)，进入版本管理。

最下面的开发版本就是刚刚上传的版本，可以将其选为体验版，页面路径就是进入小程序的首页。

扫描二维码进入体验版，但只有管理员和体验者有权访问，在[成员管理](https://mp.weixin.qq.com/wxamp/user/manage)可以添加体验者。



## 审核版

开发版需要先提交审核，点击一些下一步/确定的按钮们后，再填写一些信息，其中版本描述是用户能够看到的小程序信息之一，所以不要乱写，别的信息都是选填，不填问题也不大。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600440543085-25f6f84e-b8b0-4d73-936c-1492f38e18ff.png)

提交审核之后，该版本就变成了审核版本。

审核版本先是机器审核，然后人工审核。主要看小程序对用户的权限申请是否规范以及本身是否有问题，现在已经越来越严格了。（有一次我连续审核了六次都没过，最后提出异议，就过了。。



## 线上版

审核通过之后，审核版本发布，管理员扫码确认，就发布成功了。这个版本会变成线上版本

紧接着，就能在微信上搜到自己的小程序了。



# 总结

到底为此，小程序的第一版就over了，我们对如何开发、上线一个以Node.js为服务端的小程序，也应该有了大致了解。接下来就是版本迭代与维护，实现方法大同小异，相信这时候的你已经心里有底了。



PS：在整个阅读/实践过程中，有任何的问题和建议，都欢迎与我联系。

欢迎关注我的其他账号～

掘金：猪是倒着读着

知乎：猪是倒着读着

公众号：炸鸡呀

<img src="https://cdn.nlark.com/yuque/0/2020/png/317801/1601216768372-b914f993-225e-427b-8fce-e80ec37b18cf.png?x-oss-process=image%2Fresize%2Cw_1500" alt="image.png" style="zoom:25%;" />