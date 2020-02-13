---
title: 不同环境下的JavaScript
date: 2020-02-13 22:26:01
tags:
categories: javascript
---
ECMAScript和JavaScript准确来说不一样。ECMAScript是由Ecma国际通过ECMA-266标准化的脚本程序设计语言，JavaScript是它的一种实现。
# 浏览器
![](https://p3.ssl.qhimg.com/t01c6427c9bd87bb918.png)

浏览器中的JavaScript 是由 ECMAScript 和 BOM（浏览器对象模型）以及 DOM（文档对象模型）组成的，Web前端开发者会很熟悉这两个对象模型，它使得开发者可以去操作浏览器的一些表现，比如修改URL、修改页面呈现、记录数据等等。

# Node
![](https://p5.ssl.qhimg.com/t01a95b56790bbd229b.png)

NodeJS中的JavaScript 是由 ECMAScript 和 NPM以及Native模块组成，NodeJS的开发者会非常熟悉 NPM 的包管理系统，通过各种拓展包来快速的实现一些功能，同时通过使用一些原生的模块例如 FS、HTTP、OS等等来拥有一些语言本身所不具有的能力。
# 小程序
![](https://p0.ssl.qhimg.com/t01b18204f6c88f3afc.png)

小程序中的 JavaScript 是由ECMAScript 以及小程序框架和小程序 API 来实现的。同浏览器中的JavaScript 相比没有 BOM 以及 DOM 对象，所以类似 JQuery、Zepto这种浏览器类库是无法在小程序中运行起来的，同样的缺少 Native 模块和NPM包管理的机制，小程序中无法加载原生库，也无法直接使用大部分的 NPM 包。