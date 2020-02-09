---
title: 一条龙：小程序从零至上线
date: 2020-02-09 23:03:44
tags:
categories: 小程序
---
# 编写PRD PD
决定了小程序的定位和功能后，第一步就是编写产品需求文档（PRD：Product Requirements Document)

因为是自己开发，无需太详细，列出主要的功能点，加以简单描述就好。

个人比较喜欢语雀，因此都是用语雀写的文档。
# 设计页面 UI&交互
根据PRD设计页面， 并准备好需要用到的素材，如切图、icon等。
工具：Sketch
# 数据库设计
考虑到需要上线，最终还是要用到线上数据库，因此在服务器下的mysql新建了数据库（采用了线上数据库：phpmyadmin页面）。当然为了方便，可以先在本地数据库新建，本地调试成功后再连接服务器的。
# 小程序开发
## 准备
若先前未曾开发过小程序，则在真正开发前需要一些准备工作，如小程序官网注册账号、下载小程序开发工具等，参考官网：https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/getstart.html#%E7%94%B3%E8%AF%B7%E5%B8%90%E5%8F%B7
## 文档
小程序跟前端类似，可快览官网提供的组件、基本api，知道有这些东西，等用到的时候细看即可。

阅读微信小程序官网文档基本就能有个了解，有前端基础的大概看0.5-1小时就能了解大部分，可以进行开发了。
主要有三步：
- 从官网注册小程序账号，获得AppId
- 下载小程序开发工具
- 了解项目构成，进行基本开发
## 开发
在开发过程中可以把需要用到的接口简单规定一下，避免写完服务端还要多余地修改小程序接口代码。
基本开发完后便可以着手服务端了。
## 插件使用
小程序中有图片的业务需求。一般的图片并不会直接存到服务器或数据里，而是用存储对象进行上传下载，在数据库中存的是直接可以访问到的图片地址。

阿里云的OSS存储对象要付费，七牛云有免费额度可以使用。因此使用了七牛云。

七牛已经有了上传图片的小程序插件，有基本信息和使用教程：https://mp.weixin.qq.com/wxopen/pluginbasicprofile?action=intro&appid=wx00caa212d6710dcb&token=607354097&lang=zh_CN

小程序使用插件：
- 登录小程序平台 -> 设置 -> 第三方设置 -> 插件管理 -> 添加插件
- 搜索该插件，点击添加
- 在小程序开发工具里 app.json的plugins下添加代码
"qiniuUploader": {
"version": "1.3.0",
"provider": "wx00caa212d6710dcb"
}
⚠️json文件下不能有注释。
- 按照该插件教程使用该插件。
# Koa服务端开发与联调
## 文档
官网：https://koa.bootcss.com/
详细文档：https://chenshenhai.github.io/koa2-note/
## 开发
边看文档边开发，通过demo一个基本的get请求就出来了。
开发过程中做了些记录，包括热更新、请求数据获取等：http://blog.escript.cn/Koa/#more
## 联调
可使用浏览器或postman测试接口，没问题后再进行联调。

联调先与本地联调，没问题后再上传到服务器。

❗️使用本地调试，localhost一般为http服务，可以先在小程序开发者工具中取消校验。

❗️但是上线的小程序只允许使用https，最终还是需要配置。
# 配置https

在本地/服务端调试通过后，在微信开发者工具中点击右上角的上传按钮，便可以在公众平台 -> 版本管理 -> 开发者版本中看到了，可以将该版本勾选为体验版。

在成员管理 -> 体验成员 中添加成员，该成员即可享受体验版。
体验版与线上版本一样，也需要https，若之前未曾配置，体验版就拿不到数据。

- 在配置前，首先需要ssl证书，阿里云上可以免费买。买完后绑定域名，验证通过后就可以下载使用了。
- 若该项目要上传到服务器，则下载的证书文件最好不要直接暴露在项目中。
- 直接下载到服务器：wget url下载链接
- 若url失效，则先下载到本地，使用scp xxx.zip root@ip:/root/https ，使用unzip xxx.zip 命令解压
- 在Koa项目下下载https，用来启动https服务
```js
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