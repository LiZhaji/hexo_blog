---
title: 手把手教你从零上线小程序+Node.js服务端 (上)
date: 2020-09-28 23:18:21
tags:
---
# 一、引言与准备

17年微信小程序正式发布后，凭借其无需安装、触手可及、用完即走的优势爆火。至今依旧处于火热状态，不仅如此，小程序生态圈也在不断扩大，除了微信，各大主流app都安排上了自己的小程序，比如百度、支付宝、淘宝等。

据统计，至20年上半年，微信小程序数量已超320万，是所有平台中占比最高的；全网小程序累积已超500万个，并依旧呈上升趋势。

在这样的背景下，谁不想跟一波风，写几个小程序的bug呢。



本教程分为上下两章。第一章主要介绍本地开发与联调，你将了解到：

- 基本的全栈开发过程

- - 数据库设计
  - 小程序开发
  - 服务端开发
  - 本地联调

第二章主要介绍上线，你将了解到：

- 小程序与服务端的上线
- 域名与https配置



整个过程中最重要的是：

- 扩宽知识面（知道有这些工具可以用来做这些事情）
- 解决问题能力
- 阅读第一手资料的能力
- 学习总结能力



这是[小程序+服务端](https://github.com/LiZhaji/mini_demo)的代码地址。

## 准备

小程序代码跟前端类似，所以有前端基础的朋友会更容易上手和理解。

开始阅读之前，确保你本地电脑已有Node.js + Git 环境。

请使用以下方式检测，如果没有显示版本号，建议阅读[Windows-Node.js安装与配置](https://www.cnblogs.com/xinaixia/p/8279015.html) 、[Git安装与配置](https://blog.csdn.net/huangqqdy/article/details/83032408?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param)先进行安装与配置。

# ![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599380960820-807f4823-56c6-4004-816e-2b4358ce764c.png)

其次，编译器准备：

- 小程序：[微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html) （下载完就先放着，第三章会用到）
- Node.js：VS Code / WebStorm ... 



最后，先让大家有个了解，本教程将会使用的技术栈如下：

- 微信小程序原生语法 + weui-wess 
- 后端 Node.js （express）
- 数据库 MySQL
- 线上数据库 phpMyadmin



如果你纠结先写后端还是先实现页面，那就按照本文步骤来吧。再简单的全栈开发，也是需要踩坑的~



# 二、数据库设计-MySQL

为什么第一步是设计数据库呢？

因为数据库设计要求我们了解自己的产品，让我们知道在做什么。

比如页面的数据从哪儿来，到哪儿去；哪些数据需要抽象成事物存储到数据库，每个事物使用哪些信息去描述，这些都是在编码之前要设计好的东西，否则很有可能带来频繁修改代码的后果。



## 环境准备

我使用的本地数据库是[MySQL](https://dev.mysql.com/downloads/)，它有个可视化工作区叫[MySQLWorkbench](https://dev.mysql.com/downloads/workbench/)，长这样，使用起来很方便。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599491617850-803b601f-0964-452f-a3d5-509a18f51644.png?x-oss-process=image%2Fresize%2Cw_1500)

如果你没使用过MySQL，那就继续阅读，接下来将会说明如何使用它创建数据库。如果你会使用并且已经设计创建好自己的数据库了，那就开始阅读下一章吧。



如果你还没有这些环境，就进入上面的链接先去下载吧，有不清楚的自行搜索教程。



## 创建数据库

当你下载完MySQL并且出现以上界面的时候，就可以继续跟着我的步骤走了。

- \1. 新建数据库 在SCHEMAS区域右键，选择`Create Schema`
- \2. 为你的schema起一个名字
- \3. Apply 执行对应的SQL语句

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599577731728-02a5e523-656c-4c1b-9afd-50ebf897ebc7.png)

## 创建表格

### 普通表格

这时可以看到左侧的SCHEMAS中多了我们新建的数据库

- \4. 双击选择到该数据库，这一步不要漏掉，否则后面执行SQL语句会报错
- \5. Tables区域右键，选择`Create Table`

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599578525385-90832e4e-931a-49b6-b7b7-d73d9861457e.png)

此时弹出new_table的表格，你需要

- \6. 为你的表起一个名字
- \7. 为你的表添加描述信息

这里我添加了一张用户表user，用来存储用户信息，包括了id（PK表示主键primary key，NN表示不允许为空not null，AI表示自增auto increasement，其余自行搜索）、openid（openid是微信向开发人员提供的用户标识，这里知道即可）、createTime 创建时间。因为仅做demo，所以只存储2个字段了，你可以根据自己的想法进行设计。

- \8. Apply 执行创建user表

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600100418277-64e4a689-c1b3-441d-8495-1bcfb2927ac1.png?x-oss-process=image%2Fresize%2Cw_1500)

### 带有外键的表格

- \9. 创建一张带有外键的表格

与普通表一样，先创建完所有字段。这里我创建了advice的表格，包含id、content和userId三个字段，其中userId为外键，关联user表中的id字段。

- - 9.1 选择`Foreign Keys`
  - 9.2 为你的外键起一个名字
  - 9.3 选择需要外键的关联表
  - 9.4 选择需要本表中的外键字段
  - 9.5 选择关联表中的被关联的字段
  - 9.6 当被关联字段更新或删除的时候，关联表中字段如何处理。CASCADE表示级联，SET NULL表示设置为空。这里的意思就是当user表中的某个id变化的时候，关联它的外键也一起变化，id被删除的时候，关联它的外键就会变成null。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599580359673-17ff99d2-74b9-4137-ad4d-3954d70beb16.png?x-oss-process=image%2Fresize%2Cw_1500)

## 创建完毕

此时，我己经创建完了我的数据库mini_demo，有两张表格，advice和user，其中advice的userId为外键，关联user表中的id。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600098072982-206752ce-c6bc-4af7-b462-fec26aee34da.png)

如果你还没完成，就先创建完吧；如果你已经完成，开始阅读下一章吧。



# 三、**小程序注册与编码**

## 注册账号

小程序与公众号类似，需要使用账号管理。如果没有账号，先去[平台](https://mp.weixin.qq.com/wxopen/waregister?action=step1)申请一个，然后登录。

1. 需要使用邮箱登录，一个邮箱只能绑定一个小程序。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599879640542-01c69146-8a95-4271-ac93-fde6a99fe469.png?x-oss-process=image%2Fresize%2Cw_1500)

1. 登录到邮箱，点击链接以激活

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599879755547-f6b45950-2e19-4fa0-81ce-25131c875820.png?x-oss-process=image%2Fresize%2Cw_1500)

1. 填写信息，需登记本人真实信息。一个身份证号目前可以绑定五个小程序。



![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599879933965-4256a5e1-3b2a-42b4-acc9-9104e3dde0d0.png?x-oss-process=image%2Fresize%2Cw_1500)



1. 然后，很自然得跳转到这个页面。按照页面提示的流程，填写小程序信息，填写完后将显示如下界面。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599880026275-b22d3276-3391-4032-a319-c62550c2615c.png?x-oss-process=image%2Fresize%2Cw_1500)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599884341013-eaa96f50-6d5b-45f0-adc9-0cb94f563ec2.png?x-oss-process=image%2Fresize%2Cw_1500)

1. 在菜单开发-开发设置里可以看到我们的小程序id，跟身份证一样，AppID是小程序的唯一标识。之后很多地方都会用到。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599889182678-2e102c48-aa89-4eff-8a23-1983ca5b7a1e.png?x-oss-process=image%2Fresize%2Cw_1500)

## 新建项目

这时候打开前一章下载完的微信开发者工具，微信扫码登录后，就会出现此页面，让我们新建一个小程序项目吧。你需要

1. 起一个项目名字
2. 选择存放目录
3. 输入刚刚的AppID
4. 点击不使用云服务，我们使用自己搭建的后端服务。

如果仅是为了完成小程序功能，可以选择更快捷的方式。这里是为了学习如何上线后端服务及云存储。

1. 新建

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599893742214-94844d15-50b8-4116-89d9-d8b937038af6.png?x-oss-process=image%2Fresize%2Cw_1500)

稍等片刻，我们的第一个小程序便诞生了！如下图所示。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599893840434-900849a4-6ad4-425c-9cca-07d4f444414f.png?x-oss-process=image%2Fresize%2Cw_1500)

## 熟悉项目

先介绍一下常用的界面区域和按钮，见图。（不理解也没关系，这些都是熟能生巧的，现在做到心中有数就行。）

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599894330842-068e0bff-f45d-4bb6-80cb-abbd94647ede.png?x-oss-process=image%2Fresize%2Cw_1500)



相信这时你已经在模拟器上都点过一遍了，这里再给大家梳理一下。

- 初始页面，有一个按钮和欢迎语。
- 点击获取头像昵称后，跳出一个授权弹窗。
- 允许之后，初始页面的按钮变为了用户信息。
- 点击头像跳转到另一个页面，可以查看启动日志。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599895080875-8f3613ac-36c3-4898-9c9f-6843d823570c.png)![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599895262917-436a3a00-6ebf-4f82-a725-0c783e34e771.png)![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599895284116-59d6921b-f88b-4516-916c-5fed86723a47.png)![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599895299418-d1f6adad-7a38-467a-87a8-eaef3734e3f3.png)

接下来，思考这两个问题。

既然有两个页面，为什么初始页面是第一个呢，在哪控制？

每个类型文件是干嘛用的，数据如何展示？



如果你存在疑惑，请先阅读[官文-小程序代码构成](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/code.html)，开始编码前，我们务必先了解这些东西。

### 初始化页面分析

这时你对小程序代码构成有了初步了解，甚至已经动手改过代码了，很棒。

接下里分析一下两个页面的逻辑及实现。

回到刚才的问题，为什么初始页面是第一个呢。

其实是由`app.json`的`pages`决定的，第一行就是要初始化展示的界面。



小程序初始化后，会首先触发`app.js`的`onLaunch` 方法，会在小程序启动时触发

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599899947959-123383ef-0102-404d-964a-632516f5483b.png?x-oss-process=image%2Fresize%2Cw_1500)

这里面做了三步操作：

1. 记录本次启动时间，存入本地`storage`的`logs` 变量，该数据是logs页面的数据来源
2. 登录，这里需要自己补充
3. 获取用户信息。如果用户已授权过用户信息，那么这些信息可以直接通过`wx.getUserInfo`获取到，获取成功后保存到全局变量`userInfo` 中



然后，启动初始化页面，也就是`index`。 `index.js`中有个`onLoad`方法 (该方法会在页面加载时触发，详见[页面生命周期](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page-life-cycle.html)) ，主要就是判断是否已获取到用户信息，有的话就赋值给当前页面的data，没有就设置回调，等有了之后再赋值。最后一个else是没有“获取用户按钮”的兼容处理。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599900638126-5bf50cc5-103d-4aa9-8133-58d5880c5421.png?x-oss-process=image%2Fresize%2Cw_1500)

另外两个函数`bindViewTap`和`getUserInfo`是事件绑定函数，分别是点击头像和点击按钮的回调。

```js
bindViewTap` 就做了一件事，让页面跳转到`logs
```

`getUserInfo` 将获取到的用户信息存储到全局变量和当前页面data中。e是通过参数透传实现的，由小程序API返回。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599900750597-813feb76-58d3-4808-9f05-a37a141cdf27.png?x-oss-process=image%2Fresize%2Cw_1500)

`logs`页面很简单，就是把启动信息从本地storage中取出来展示。



## 编码

看到这儿，你或许对小程序代码有大概了解了，那么接下来就照葫芦画瓢，去写自己的页面吧（登录会在下一章讲，不着急实现）

编码过程中，可以预先定义好接口，变量结构和名称就根据前一章设计好的数据库来写，避免重复修改。

页面中涉及到的很多小程序API，不清楚的就去[微信小程序API文档](https://developers.weixin.qq.com/miniprogram/dev/api/)查，大家要有看第一手资料的意识和能力。



这是[我的小程序代码地址](https://github.com/LiZhaji/mini_demo/tree/master/MiniPrograms)（下面有页面截图），仅供参考。

# 四、**服务端搭建-Koa**

## 接口准备

看这节之前，你应该实现了自己的静态页面了。这是我当前的两个页面，很简单。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599910110499-d5e036f4-b6be-4849-a357-634378be2c1d.png)![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599910119264-d33638d0-f8b7-42af-b74c-3c5367473640.png)

总共涉及两个接口，登录和提交建议。

在写服务端之前，先约定好接口，如路径、请求方法、参数。

- 登录 get /login?js_code
- 提交建议 post /addAdvice body={openid, advice}



## hello world

明确接口之后，开始编写服务端，我使用的是Node.js框架Koa。

首先，创建一个空文件夹，从终端进入目录，执行下面两个命令

`npm init` 初始化npm信息，生成package.json

`npm i koa` 下载koa框架

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599912025959-e28f2775-a673-434f-bacf-73068466db01.png?x-oss-process=image%2Fresize%2Cw_1500)

这时我们的目录下有三个文件：node_modules、package.json、package-lock.json

根据[Koa文档](https://koa.bootcss.com/)，我们先写一个最简单的demo

```javascript
// index.js

const Koa = require('koa')
const app = new Koa()

app.use(async ctx => {
  ctx.body = 'hello zhaji'
})

app.listen(3000)
```

然后命令行执行这个文件`node index` 仿佛什么都没有，但实际上项目已经启动起来了，http://localhost:3000/ 就能访问到。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599961760894-145e0bfd-b42d-4040-b68d-e7f2d7865181.png)![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599961741741-2367aa9b-8f95-4c19-855d-fbc3eb8344d8.png)

所以我们可以在相应的地方加上提示。

```js
// index.js

// ...
app.listen(3000)
console.log(`项目已启动，地址为 http://127.0.0.1:3000`);
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1599962590734-5e24bd76-7d48-4c9a-8b54-145015df7468.png)

到此为止，我们已经能够启动Koa项目了，接下来就是写接口以及对接数据库。



## 提供接口

也就是上面定义的两个接口，登录和提交建议

接口需要路径，我们使用[koa-router](https://github.com/ZijianHe/koa-router)

先写一个koa-router的hello world，甚至直接从文档拷下来就行。

```js
const Koa = require("koa");
const app = new Koa();

const Router = require("koa-router");
const router = new Router();

// app.use(async ctx => {
//   ctx.body = 'hello zhaji'
// })

router.get("/", async (ctx) => {
  ctx.body = "hello zhaji";
});
app.use(router.routes()).use(router.allowedMethods());

app.listen(3000);
console.log(`项目已启动，地址为 http://127.0.0.1:3000`);
```

### get请求

接下来，实现自己的接口。先讲一下登录，也就是根据jscode获取用户唯一标识。



[官网](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/login/auth.code2Session.html)是这样描述的：

> 登录凭证校验。通过 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 接口获得临时登录凭证 code 后传到开发者服务器调用此接口完成登录流程。更多使用方法详见 [小程序登录](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/login.html)。
>
> 
>
> 请求地址 GET 
>
> https://api.weixin.qq.com/sns/jscode2session？appid=APPID&secret=SECRET&js_code=JSCODE&grant_type=authorization_code



我们要做的其实很简单，在后台向这个地址发送请求，携带三个参数，然后保存返回信息即可。

三个参数分别是appid 之前新建小程序项目也用到过的那个、secret 需要下载的密钥[下载链接](https://mp.weixin.qq.com/wxamp/devprofile/get_profile)、js_code 小程序登录接口wx.login返回信息之一

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600093719877-ce6fc6e2-c277-466f-b419-0d6ef4489f2e.png)

通过这个方法得到三个值后，我们可以先测试一下。复制**已经修改为你自己的信息的链接**到浏览器，能看到返回的信息，说明接口没问题。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600093889053-5f437436-444d-4dec-9d74-b62d7b5fe007.png?x-oss-process=image%2Fresize%2Cw_1500)

[node-fetch](https://www.npmjs.com/package/node-fetch)可以帮助我们在node.js下发起请求。为了符合关注点分离，我们将接口和数据库操作分离。

在`index.js`下新增`/login`接口

```js
// index.js

// 登录
router.get('/login', async ctx => {
  const {js_code} = ctx.query
  const res = await api.login(js_code)
  ctx.body = res
})
```

在`api/index.js`做具体实现。说明一下第6行`const { APPID, SECRET } = process.env` 这样写的目的是可以从命令行中获取项目的私密信息，避免把信息暴露在项目中。启动格式`export APPID="xxx" && export SECRET="xxx" && node index.js`

之后的数据库名和密码也可以用这样的方式启动。

```js
// api/index.js

const fetch = require("node-fetch");

function getOpenId(JSCODE ) {
  const { APPID, SECRET } = process.env;
  console.log(APPID, SECRET);
  const api = `https://api.weixin.qq.com/sns/jscode2session?appid=${APPID}&secret=${SECRET}&js_code=${JSCODE}&grant_type=authorization_code`;
  return fetch(api)
    .then((res) => res.json())
    .then((json) => json);
}

module.exports = {
  async login(JSCODE) {
    // 1. 获取openid
    const openid = await getOpenId(JSCODE)
    return openids

    // 2. 根据openid读取数据库，返回用户信息

  }
};
```

重启项目后，用浏览器测试一下接口好用不。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600096633510-3f5b16ff-ab48-4685-bc87-98c8978074e1.png?x-oss-process=image%2Fresize%2Cw_1500)

虽然返回的是错误码说明js_code无效，但也已经说明我们的login接口可以使用了。



### post请求

post请求的数据通常放在body里，但如果直接`ctx.request.body`读取其实为空，所以需要借助`koa-bodyparser`库，像这样使用。

```js
const bodyParser = require("koa-bodyparser");
app.use(bodyParser());

router.post('/addAdvice', async ctx => {
  const data = ctx.request.body
  console.log(data)
  ...
})
```

## 连接数据库

使用[sequelize](https://sequelize.org/master/)进行数据库连接。我们连接本地数据库进行调试，先确保本地数据库已启动。

然后进行数据库配置，新建`config.js`文件，配置数据库名称、用户名和密码。其余配置大同小异，基本上也不需要修改，[代码仓库](https://github.com/LiZhaji/mini_demo/blob/master/service_koa/config.js#L14)里详细说明，感兴趣同学可以看看。

```js
// config.js

module.exports = {
  // 数据库名称
  database: process.env.DATABASE,
  // 用户名
  username: process.env.USER,
  // 密码
  password: process.env.PWD,
  // 地址
  host: '127.0.0.1',
  // 使用什么数据库
  dialect: 'mysql',

  ...
  
}
```

然后新建`mysql/index.js` 主要作用就是声明表格、表格属性以及表格之前的关系。数据库有几张表格，就应该有多少个类。（应该支持导入导出操作避免手工劳动力，有兴趣的朋友的研究下。）

```js
// mysql/index.js

const Sequelize = require("sequelize");
const mysqlConfig = require("../config");

const sequelize = new Sequelize(mysqlConfig);

const Model = Sequelize.Model;

class User extends Model {}
class Advice extends Model {}

User.init(
  {
    // 属性
    id: {
      type: Sequelize.INTEGER, // 要与数据库声明的类型匹配
      autoIncrementIdentity: true, // 自增
      primaryKey: true, // 主键
    },
    openid: {
      type: Sequelize.STRING,
      allowNull: true,
    },
    createTime: {
      type: Sequelize.DATE,
      allowNull: true,
    }
  },
  {
    sequelize,
    modelName: "user",
    // 参数
  }
);
Advice.init(
  {
    id: {
      type: Sequelize.INTEGER,
      autoIncrementIdentity: true,
      primaryKey: true,
    },
    content:{
      type: Sequelize.TEXT,
      allowNull: true
    },
    userId: {
      type: Sequelize.INTEGER,
      references: { // 外键声明
        model: "user",
        key: "id",
      }
    }
  },
  {
    sequelize,
    modelName: "advice",
  }
)
module.exports = {
  User,
  Advice,
};
```

那么，如何操作放佛已经被对象化的表格呢？这就是`sequelize` 特色之处，它让开发者能够像写中文一样去操作数据库。比如，新增和查找，分别是`create`和`findOne`

```js
// api/index.js

const { User, Advice } = require("./mysql/index.js");

// 新增用户
async function addUser(openid) {
  await User.create({ openid, createTime: new Date().toLocaleString() });
}

// 查找用户
async function getUser(openid) {
  const user = await User.findOne({ where: { openid } });
  // 用户不存在，新增用户
  // ...
  return user;
}
```

### 一些报错

在执行过程中，项目总会报错的。报错不要紧，要紧的是我们解决它的能力。刚开始学的时候，项目一报错我就复制到度娘下，甚至嫌麻烦都懒得看一眼。渐渐的发现，其实很多问题自己就能看明白，不要太依赖搜索等，尽量先尝试自己解决，只有这样才能熟能生巧。没错，写代码就是一个熟能生巧的过程。

比如这里，写完增删改查后，准备启动项目，却发现报了个这样错。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600177492684-5be40728-db6f-4817-a08d-6fc81ecc9845.png?x-oss-process=image%2Fresize%2Cw_1500)

很明显引入的路径有问题，应该是`../mysql/index.js`

再启动，又报错。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600177686467-21ae7603-7c0c-4012-8407-bf1f64e652d4.png?x-oss-process=image%2Fresize%2Cw_1500)

让我们下载mysql2依赖包，那就下呗。下完之后再启动，ok了。其实这种自己慢慢把一个个bug干掉的过程，很爽的。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600177776753-74f35d19-8bcc-430f-aa53-d1d0fe360021.png?x-oss-process=image%2Fresize%2Cw_1500)

## 调试接口

到这里我们写好了接口并成功启动项目，接下来就是调试阶段，看看接口是否可用。

调试方法有很多种，最简单的可以直接用浏览器调试，get请求直接浏览器打开，post请求可以写一个xhr脚本；

也使用[postman](https://www.postman.com/)工具，专门用来测接口的。

我们选用postman测试，如果没有该工具，可以点击上面的超链接进行下载并登录。



同样，从最简单的开始。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600179278481-e14623e0-5c9f-445b-b7f7-cb9101578ca4.png?x-oss-process=image%2Fresize%2Cw_1500)

没问题后，开始测登录接口，是否与预料中一致，若存在问题，看报错解决。（前面说过，js_code在小程序项目里登录wx.login时输出res.code一下就可以得到）

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600179199539-dbd80329-a51b-402a-86ec-780dfa669a15.png?x-oss-process=image%2Fresize%2Cw_1500)

没问题后，打开数据库确认。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600179429319-9de158d6-c63b-4e93-996e-30d2c1b1d29f.png?x-oss-process=image%2Fresize%2Cw_1500)



确保每个接口都没问题后，就可以跟小程序联调了！

[这是我的服务端代码地址](https://github.com/LiZhaji/mini_demo/tree/master/service_koa)



# **五、本地联调**

## 小程序接口请求

如果之前没在小程序里写接口，那就在这一步写吧。

根据[文档](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)，我们使用`wx.request` 发送请求，下面是一个调用`login`接口的示例。

```js
 wx.request({
   url: `${this.globalData.baseUrl}/login`, // 在全局变量里定义baseUrl，方便之后修改
   method: 'GET',
   data: {
     js_code: res.code
   },
   success: res => {
     console.log(res);
   }
 })
```

如果出现了这样的报错，只需要点击右上角详情，勾选`不校验...`即可。但如果小程序发布，其规定为了安全还是需要使用https，现在取消校验是为了本地联调。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600182247061-06114177-1e34-4d3a-bcaf-f9d9f90339ca.png?x-oss-process=image%2Fresize%2Cw_1500)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600182995589-3745aa6a-a3e4-46b1-b058-a782f8d9577a.png)

## 联调

再次点击编译，接口正常运行并输出，同时可以再去数据库查看数据是否符合预期。![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1600183271993-d7b17469-5a78-4eb4-881a-369f3c933d5d.png?x-oss-process=image%2Fresize%2Cw_1500)

挨个把所有的接口都调试一遍。

## 最后

过了好一会儿，终于调试完了所有接口。恭喜你，离小程序上线又近了一大步！

为了保证服务端接口能够被非本地访问（如使用其他电脑访问），下一步我们需要把它放到云上。

欢迎关注我的其他账号～

博客：http://blog.escript.cn/

掘金：猪是倒着读着

知乎：猪是倒着读着

公众号：炸鸡呀

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1601216768372-b914f993-225e-427b-8fce-e80ec37b18cf.png?x-oss-process=image%2Fresize%2Cw_1500)