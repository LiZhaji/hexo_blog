---
title: 六种继承方式总结
date: 2020-02-25 13:27:10
tags:
---
面向对象（OO）编程模式
prototype是Object的实例

# 什么是继承

子类可以使用父类的所有功能，并对这些功能进行扩展，就是从一般到特殊的过程。

# 继承方式

假设有以下两大类型，要求SubType继承SuperType
```js
function SuperType(){}
function SubType(){}
```

## 1. 原型链继承
- 🌺将子类型构造函数原型指向超类型的实例 SubType.prototype = new SuperType()
- ⚠️需重写constructor属性，否则会去原型链上找到SuperType
- ❌无法向超类型构造函数传参
- ❌所有实例共享超类型的属性和方法

## 2. 借用构造函数继承
- 🌺在子类型构造函数内调超类型构造函数，并改变this指向 Supertype.call(this, ...arguments)
- ✅可以向超类构造函传参，每个实例拥有自身属性
- ❌不能实现函数复用（无法继承超类型构造函数原型上的方法和属性）

## 3. 组合继承
- 🌺使用原型链实现超类构造函数原型对象的继承，使用借用构造函数实现实例属性的继承
- ✅
- ❌需要调用两次超类型构造函数，一次是创建子类型原型，一次是在子类型构造函数内部。
   结果：导致出现两组超类型构造函数属性，一组在实例上，一组在实例的原型对象上

## 4. 原型式继承(不需要构造函数)
- 🌺通过Object.create(obj)，将参数作为新创建对象的原型
```js
function Create(){
  function F(){}; 
  F.prototype = obj; 
  return new F();
```

## 5. 寄生式继承(不需要构造函数)
- 🌺在原型式继承上，在函数内部增强对象并返回实例。
```js
function AnotherCreate(){
  const clone = Object.create(person);
  clone.sayHi = function(){};
  retrun clone;
}
```
- ❌不能实现函数复用

## 6. 寄生组合式继承
- 🌺使用寄生式继承超类型构造函数原型，使用借用构造函数实现实例属性的继承 
```js   
Supertype.call(this, ...arguments)

function initPro(subType, superType){
  const propotype = Object.create(superType.prototype)
  propotype.constructor = subType // 增强对象
  subType.propotype = propotype
}
```
- ✅超类型构造函数只调用了一次