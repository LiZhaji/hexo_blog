---
title: meta妙啊
date: 2020-10-12 00:01:03
tags:
---
## 页面自动跳转

类似ppt自动跳转的效果，除了可以用定时器实现，还有更简单的meta方法

content第一个值表示时间，单位秒，第二个值url表示跳转的页面

```html
<meta http-equiv="refresh" content="3; url=page2.html" />
```

![image.gif](http://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/41c4f61a97c146f7a769cc801b076107~tplv-k3u1fbpfcp-zoom-1.image)

## 页面刷新

页面每隔一段时间自动刷新，如看板数据监控

content为刷新时间间隔，单位秒

```html
<meta http-equiv="refresh" content="3" />
```

![image.gif](http://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9d6a14fbcbb4ec497fe2d0d0fad5895~tplv-k3u1fbpfcp-zoom-1.image)



## 伪装app

开启全屏模式时，利用`apple-mobile-web-app-capable`可以隐藏网页的状态栏工具栏，让它看起来像一个app，默认值为no，显示状态。

`apple-mobile-web-app-bar-style` 设置头部颜色，可选值为default/black/black-translucent

`apple-mobile-web-app-titles`设置主屏标题

```html
<meta name="apple-mobile-web-app-capable" content="yes" /> 
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<meta name="apple-mobile-web-app-title" content="hi">
```



先将网页添加到主屏幕，其中hi是未设置参数的，document是设置了的，打开后分别如下图所示。

第二个隐去头尾后，确实类似app。

![image.png](http:////p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5c7218c38b5a473a841d8e3524707ed1~tplv-k3u1fbpfcp-zoom-1.image)

## app广告条

使用`apple-itunes-app` 可以智能添加app广告条，如下图所示

```html
<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL">
```

![image.png](http://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/87fa7c7e74564d1e8d4018a81b156fdb~tplv-k3u1fbpfcp-zoom-1.image)
