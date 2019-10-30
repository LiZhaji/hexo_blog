---
title: 前端项目部署到阿里云 持续运行(2)
date: 2019-10-30 11:57:16
tags:
---

之前写的前端项目部署到阿里云，有一个缺陷就是系统会自动清理进程，这时候需要自己重启服务。
## 基础使用
后来了解到，有一个管理进程的npm库: pm2，能够保持进程不被杀死
1. 进入服务器
`cmd -> ssh root@yourIp`
2. 下载pm2（全局）
`npm install pm2 -g`
3. 使用
`pm2 start 'location of serve.js'`
如果`serve`是全局安装的，命令为`pm2 start '/usr/local/lib/node_modules/serve/bin/serve.js'`

但是这样会有个局限：
只能启用一个端口，若要显示不同的网页，只能用`域名:端口/目录`的方式访问，不能做到只用`域名:端口`就可以访问
## 改进
自己写一个`serve.js`，放到了博客目录下
比如，我的博客大目录是`/root/hexo_blog` ，`/root/hexo_blog/public` 这是生成好的文章目录，所以真正要启动的是第二个，要是单独`serve .` 也是需要进入到此目录下的，否则只能通过`/目录`方式访问
所以
- 在`/root/hexo_blog`下新建`serve.js`
- 输入命令`pm2 start '/root/hexo_blog/serve.js'`
```js
// serve.js
const handler = require('serve-handler');
const http = require('http');

const server = http.createServer((request, response) => {
  // You pass two more arguments for config and middleware
  // More details here: https://github.com/zeit/serve-handler#options
  return handler(request, response, {
		public: './public' // 需要启动哪个目录作为根并对外开发
	});
})

server.listen(80, () => {
  console.log('Running at http://localhost:80');
});
```

> 参考链接：https://github.com/Unitech/pm2



推荐阅读：

[前端项目部署到阿里云 serve运行(1)](http://blog.escript.cn/deploy_project_server1/)

[前端项目部署到阿里云 lnmp运行(3)](http://blog.escript.cn/deploy_project_server3/)