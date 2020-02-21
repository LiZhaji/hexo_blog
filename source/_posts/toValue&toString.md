---
title: toValue & toString 一览
date: 2020-02-21 20:51:21
categories: javascript
---

# 0. 小结

`valueOf()`是获取对象的原始值，其类型总是该对象的类型

`toString()`是把对象转换成字符串，其类型就是字符串

在利用`==`比较时，如果有一个是对象，另一个是字符串 数值或布尔值，js引擎会先优先调用内置对象的`valueOf`方法，`Date`比较特殊直接调用`toString`方法
# 1. 字符串-值类型
```js
'abc'.valueOf() // "abc"
'abc'.toString() // "abc"
'123'.valueOf() // "123"
'123'.toString() // "123"
typeof '123'.toString() // "string"
typeof '123'.valueOf() // "string"
```
# 2. 数值-值类型
```js
var n = 123
n.valueOf() // 123
n.toString() // "123"
typeof n.valueOf() // "number"
typeof n.toString() // "string"
```
# 3. 布尔值-值类型
```js
true.valueOf() // true
true.toString() // "true"
typeof true.valueOf() // "boolean"
typeof true.toString() //"string"
```
# 4. sybmol-值类型
```js
var sm = Symbol('test')
sm.valueOf() // Symbol(test)
sm.toString() // "Symbol(test)"
typeof sm.valueOf() // "symbol"
typeof sm.toString() // "string"
```
# 5. 数组-引用类型
```js
var arr = [1,2,'a',true]
arr.valueOf() // [1, 2, "a", true]
arr.toString() // "1,2,a,true"
typeof arr.valueOf() // "object"
typeof arr.toString() //"string"
arr.valueOf() instanceof Array // true

[].valueOf() // []
[].toString() // ""
```
# 6. 函数-引用类型
```js
var fn = function(){console.log('in fn'); return 1;}
fn.valueOf() // ƒ (){console.log('in fn'); return 1;}
fn.toString() // "function(){console.log('in fn'); return 1;}"
typeof fn.valueOf() // "function"
typeof fn.toString() // "string"
```
# 7. 对象-引用类型
```js
var obj = {name: 'zhaji', age: 21}
obj.valueOf() // {name: "zhaji", age: 21}
obj.toString() // "[object Object]"
typeof obj.valueOf() // "object"
typeof obj.toString() // "string"
```
# 8. Date
```js
var d = new Date()
d // Tue Jul 23 2019 13:42:23 GMT+0800 (中国标准时间)
d.valueOf() // 1563860543000
d.toString() // "Tue Jul 23 2019 13:42:23 GMT+0800 (中国标准时间)"
typeof d.valueOf() // "number"
typeof d.toString() // "string"
```