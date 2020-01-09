---
title: 前端路由实现 hash和history
date: 2019-01-09 17:59:20
categories: javascript
---

单页应用(SPA)最大的特点是通过改变URL，可以在不重新请求页面的情况下更新页面。（只请求一次html）
因此，要实现spa需要解决两个问题：
- 用什么方式可以实现改变url后保持页面不刷新
- 如何监听url的变化

实现技术是前端路由系统，路由可以根据不同的url显示不同的内容，目前主要有两种方式实现：
- hash值，location.hash 和 window.onhashchange
- history，history.pushState 和 window.onpopstate，H5新增
## hash
hashchange事件，当路由当hash值发生改变时调用，包括#及其后面的片段标识符
- 改变url
- location.hash = xx

### 思路
- 监听dom元素变化
  - 通过hash更新url
  - 更新视图（hashchange）
- 监听hash变化（监听url变化）
  - 更新视图
### 简单的js实现
```html
<p><a href="#/home">Home</a></p>
<p><a href="#/about">About</a></p>
<p><a href="#/result">Result</a></p>

<div>content:</div>
<div id="content"></div>
```
```js
<script>
  // 注册路由
  const routes = {
    '/home': 'home',
    '/about': 'about',
    '/result': 'result',
  }
  // 监听hash变化
  window.addEventListener("hashchange", e => {
    const curHash = location.hash.slice(1)
    changeView(routes[curHash]);
  });
  // 改变视图
  const contentDom = document.querySelector("#content");
  const changeView = content => {
    contentDom.innerHTML = content;
  };
  // 页面刷新时，内容不丢失
  window.addEventListener("load", e => {
    const text = location.hash.slice(2);
    changeView(text);
  });
</script>
```
### class实现
html代码同上，实着是大同小异的。借此熟悉es6语法吧。
```js
<script>
  const contentDom = document.querySelector("#content");
  class HashRoute {
    routes = {};
    init() {
      // 绑定函数时注意this是window还是HashRoute
      window.addEventListener("load", () => {
        this.changeView();
      });
      window.addEventListener("hashchange", () => {
        this.changeView();
      });
    }
    changeView() {
      const curHash = location.hash.slice(1) || "/";
      contentDom.innerHTML = this.routes[curHash] || "";
    }
    add(path, content) {
      this.routes[path] = content;
    }
  }

  const Route = new HashRoute();
  Route.init();
  Route.add("/home", "home");
  Route.add("/about", "about");
  Route.add("/result", "result");
</script>
```
## history
popstate事件，历史栈的state状态发生变化时调用
- 前进、后退浏览器 back() forward() go()
- 地址栏hash改变 
- ⚠️pushState和replaceState不会触发popstate事件
- 🙅刷新页面会出现404，因此需要后端配合进行重定向
### 思路
主要实现两个监听事件，并执行对应的逻辑
- 监听dom点击事件
  - 使用pushState更新至对应的url
  - 更新视图（手动调用）
- 监听url变化
  - 更新视图
### 简单的js实现
方便起见，直接把click的事件监听写到html中了。
```html
<p><a onclick="changeState('/home')">Home</a></p>
<p><a onclick="changeState('/about')">About</a></p>
<p><a onclick="changeState('/result')">Result</a></p>
<div>content:</div>
<div id="content"></div>
```
```js
<script>
  // 注册路由
  const routes = {
    "/home": "home",
    "/about": "about",
    "/result": "result"
  };
  // 处理监听事件
  const handlePopstate = e => {
    const url = location.pathname
    changeContent(routes[url] || '');
  };

  window.addEventListener("popstate", handlePopstate);

  const changeState = url => {
    history.pushState({ url }, "", url);
    // pushState不会出发popstate事件，所以需手动触发
    handlePopstate(); 
  };

  // 改变视图
  const contentDom = document.querySelector("#content");
  const changeContent = content => {
    contentDom.innerHTML = content;
  };
</script>
```
## 总结
hash和history都可以实现更新url但不重新请求页面，但各有千秋。
- hash缺点
  - 服务端无法捕获准确的路由信息（会影响重定向等操作）
  - 若有锚点需求，会发生冲突
  - url中带一些人不喜欢的#
  - 搜索引擎无法读取#后面的内容
- history缺点
  - 兼容性不如hash，到ie10，hash到ie8
  - pushState不会触发popstate事件，还需要另外执行更新视图逻辑
  - 页面刷新后通常会出现404，需要后端配合进行重定向，hash则没有这些负担。

因此，要是我用，我选择hash。