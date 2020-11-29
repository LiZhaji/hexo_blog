---
title: 「被框架玩坏的孩子们」浏览器环境下使用require
date: 2020-10-05 16:07:55
tags:
---

# 引题

常使用框架的孩子们，想要引入一个包时候，通常操作是：

- npm i xxx
- Vue/React组件里通过import/require 引入
- 可以使用了



某天，当你正在欢乐地写一个html页面时，忽然想使用一个包。于是你向往常一样

- npm init 
- npm i xxx
- 在页面里import/require引入

这时却发现报错了。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1601445014529-617ad326-654c-4914-9166-490736e944a4.png)

WHY?

首先要清楚JS有两种运行环境：浏览器环境和Node.js环境

像module/exports/require属于CommonJS语法，CommonJS是Node.js采用的模块化规范，而浏览器环境下不支持该语法，所以没有这些变量。



所以，其报错原因就是因为**浏览器下****没有这些定义好的变量。**

# 模块化

换句话说，只要浏览器环境下存在这些变量，我们就能正常使用require

下面是一个简单的例子，可以跑在浏览器环境下并正常输出。

```js
 const module = {
   exports: {}
 };
const require = (fileModule) => fileModule.exports;

// 创建一个虚拟文件
function createInventFile(module, exports) {
  exports.double = (num) => num * 2;
  return module
}
const file = createInventFile(module, module.exports);

const { double } = require(file);
console.log(double(4));
```

## 如何运行在不同环境

如何跑在浏览器环境？

- 创建一个html，在script标签里写
- 直接打开浏览器控制台，输入js代码

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1601448155778-4ef354b9-d2e1-4136-a12b-ac14f7be5f51.png)

如何跑在Node环境？

- 终端输入node，进入node环境，输入js代码
- 创建一个x.js，终端node x.js 运行



以上代码跑在Node下会报错，因为module等变量都是已定义

![image.png](https://cdn.nlark.com/yuque/0/2020/png/317801/1601448194222-f4817ef5-4ba1-499b-94d6-d923c3da844e.png)

# browserify

其实，工具[browserify](http://browserify.org/)已经帮我们实现了以上操作，可以在浏览器下愉快的使用模块化。

- npm init
- npm i -D browserify
- browserify index.js -o compiled.index.js 编译为浏览器可识别的文件
- <script src="compiled.index.js"></script> 页面引入



# npm包

- 把包下载到项目中，引入脚本
- 引入cdn



# Node模块，如child_process/fs

不存在的。

前面的browserify仅仅是将CommonJS的模块加载机制转化为浏览器可以理解的代码，所以我们能够像在Node下一样在浏览器环境下使用exports和require。

但这不意味着，我们可以任意使用Node.js模块代码。比如fs，浏览器本身是有沙箱限制的，并不允许我们任意操作本地文件，这与其安全机制背道而驰。然而chrome 83版本开始，在用户授权的情况下，允许浏览器读写用户本地文件了，有兴趣的同学参考[这篇文章](https://juejin.im/post/6844904181896052750#comment)。


怎么办？

使用node写一个服务，单独执行。与可执行文件一起打包。打开页面得时候，运行个批处理服务器。



------


欢迎关注我的其他账号～

博客：http://blog.escript.cn/

掘金：猪是倒着读着

知乎：猪是倒着读着

公众号：谷七

