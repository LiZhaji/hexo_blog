---
title: Koa
date: 2020-02-08 19:50:31
categories: Koa
---
Koa是Web服务框架。
# 搭建
- 文件夹下 -> npm init
- -> npm i koa
- 创建app.js
```js
const Koa = require('koa')
const app = new Koa()
app.use(async ctx => {
    ctx.body = 'hello world'
})
app.listen(3000)
```
- -> node app.js
- 本地端口号3000查看
若不设置ctx.body，则会显示404 not found
# 热更新
默认的Koa非热更新，修改app文件后，只有重启执行node app.js才能更新页面内容。
方法：使用nodemon插件
- -> npm i nodemon
- -> nodemon app.js
# 中间件
中间件middleware的作用是丰富一个对象。
app.use(async (ctx, next) => {}) 每个callback就是一个中间件
- ctx是整个koa应用程序的上下文context。
- await next() 执行下一个中间件。若是最后一个中间件，则该回调的next相当于无用。
- 中间件函数按照栈的顺序执行。
```js
app.use(async (ctx, next) => {
  console.log(1);
  await next()
  console.log(2);
});
app.use(async (ctx, next) => {
  console.log(3);
  await next()
  console.log(4);
});
app.use(async (ctx, next) => {
  console.log(6);
});
// 结果： 13642
```
# 路由
- 基础写法
```js
const url = ctx.request.url
if(url === '/'){}else if (url === '/test'){}
```
- koa-router
  - get请求获取参数
    - '/test/:id' ，通过ctx.params.id获取
    - params参数?&，通过ctx.query获取

```js
const Router = require('koa-router')
const router = new Router()
// get请求
router.get('/', async (ctx, next) => {})
// requestfor参数
router.get('/test/:id', async (ctx, next) => {
    const id = ctx.params.id
})
// params参数 /test?a=1&b=2
router.get('/test', async (ctx, next) => {
  // 第一种
    const {a, b} = ctx.query
  const parmas = ctx.querystring
  
  // 第二种
  const {a, b} = ctx.request.query
  const parmas = ctx.request.querystring
})
app.use(router.routes())
```

  - post请求获取参数
    - 使用中间件bodyParser，限制为json格式，通过
ctx.request.body获取

```js
const bodyParser = require('koa-bodyParser')
const Router = require('koa-router')
const router = new Router()
app.use(bodyParser())
// post请求
router.post('/add', async (ctx, next) => {
    const postdata = ctx.request.body
})
```
# 数据库
使用了ORM框架sequelize连接和操作mysql数据库。

config.js 用来配置数据库，需要填写数据库基本信息。

❗️一般来说，数据库、用户名和密码不可暴露。因此可以使用临时变量，在启动服务的时候配置。
```js
// config.js
module.exports = {
  // 数据库名称
  database: process.env.WHERE_DATABASE,
  // 用户名
  username: process.env.WHERE_USER,
  // 密码
  password: process.env.WHERE_PWD,
  // 地址
  host: '127.0.0.1',
  dialect: 'mysql',
  // 连接池
  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000
  },
  define:{
    //是否冻结表名,最好设置为true，要不sequelize会自动给表名加上复数s造成查询数据失败。
    //mongoose也有这样的问题...
    freezeTableName:true,
    // 是否为表添加 createdAt 和 updatedAt 字段
    // createdAt 记录表的创建时间
    // updatedAt 记录字段更新时间
    timestamps:false,
    // 是否为表添加 deletedAt 字段
    // 在日常开发中删除数据记录是一大禁忌，因此我们删除数据并不会真正删除，而是为他添加
    // deletedAt字段
    paranoid:false,
    operatorsAliases: false
  },
  // 时区
  timezone: '+08:00'
}
```