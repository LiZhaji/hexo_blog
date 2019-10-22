---
title: js引用类型：对象赋值失败，取到的总是最后一项
date: 2019-10-11 15:15:03
categories: javascript
---

## 需求
现有对象数组`list`后，要取出每一项有用的属性，形成新数组
例如：
```js
// ... 已获得的数组 list ，假设为以下值
let list = [
  {a: 1, b: 2, c: 3 }, 
  {a: 54, b: 89, c: 49}
];
// 期望输出值
result = [
  {a: 1, c: 3 }, 
  {a: 54, c: 49}
];
```
## 问题描述
使用如下代码：
```js
let result = [], item = {};
list.forEach( el => {
  item.a = el.a;
  item.c = el.c;
  result.push(item);
})
console.log(result); // [ {a: 54, c: 49 }, {a: 54, c: 49}];
```
得到的总是最后一组的对象的值。
## 问题来源
一句话 ：因为`item`是指向对象的指针。
每次`push`到`result `之后，保存的都是这个引用，循环遍历完数组后，每个item指向的都是同一个对象。图解如下：
![](https://img-blog.csdnimg.cn/20190628144300147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjEyODEw,size_16,color_FFFFFF,t_70)
## 解决方案
### 1）把值push进数组，而不是指针
Ⅰ.既然问题来源是指针，那直接不出现指针了
```js
list.forEach( el => {
  result.push({a: el.a, b: el.b});
})
console.log(result); // [ {a: 1 c: 3 }, {a: 54, c: 49}];
```
### 2）保证每次push的是独立对象
Ⅰ.
```js
list.forEach( el => {
  let item = {};
  // 或者 var item = {};
  item.a = el.a;
  item.b = el.b;
  result.push(item);
})
console.log(result); // [ {a: 1 c: 3 }, {a: 54, c: 49}];
```
Ⅱ. 
```js
let item = {};
list.forEach( el => {
  item.a = el.a;
  item.b = el.b;
  result.push(Object.assign({}, item));
})
console.log(result); // [ {a: 1 c: 3 }, {a: 54, c: 49}];
```
上面两种行为的原理是一样的，每次遍历都重新创建了一个对象，上一个也不会被改变了

### 3）切断指针的联系
```js
let item = {};
list.forEach( el => {
  item = {};
  item.a = el.a;
  item.b = el.b;
  result.push(item);
})
console.log(result); // [ {a: 1 c: 3 }, {a: 54, c: 49}];
```
