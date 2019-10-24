---
title: 闭包面试题解析
date: 2019-10-22 18:46:40
catalogies: javascript
---

# 原题
今天签约群里一位嵌入式同学问了我一个面试题，还是踩坑了，代码如下

```js
// 输出？
function f1() {
  var n = 1001
  nAdd = function() { 
    n += 2 
  }
  function f2() {
    console.log(n)
  }
  return f2
}

var cacl1 = f1()
var cacl2 = f1()

cacl1()
cacl2()

nAdd()

cacl1()
cacl2()
```
正确输出是（忽略换行）：1001 1001 1001 1003

# 解析
本道题中有几个点：
- `nAdd`前无声明关键字，为全局变量
- 闭包，作用域是定义的时候决定的


why? 
我们详解代码执行过程：

- 执行Ln13： `var cacl1 = f1()`
  - f1函数预解析阶段
  ```js
  function fn1(){
    var n = undefined
    function f2() {
      console.log(n)
    }
  }
  ```
  - f1函数执行完毕后
  ```js
  // nAdd作用域链：nAdd本身 -> 第一次执行的f1 -> window
  var nAdd = function() { 
    n += 2 
  }
  // cacl1作用域链：f2 -> 第一次执行的f1 -> window
  var cacl1 = f2(){
    console.log(n)
  }
  // 这里是两个n都是f1中的n
  ```
- Ln14： `var cacl2 = f1()`
  - f1函数预解析阶段
    同第一次
  - f1函数执行完毕后
  ```js
  // nAdd作用域链：nAdd本身 -> 第二次执行的f1 -> window
  nAdd = function() { 
    n += 2 
  }
  // cacl2作用域链：f2 -> 第二次执行的f1 -> window
  var cacl2 = f2(){
    console.log(n)
  }
  // cacl1中的n和cacl2中的n是独立的
  // nAdd被重新赋值，所以它跟cacl1的关系也没了，而是跟cacl2中的n为同一个
  ```
- Ln16： `cacl1()`
  - 也就是执行：`console.log(n)`

    它会沿着cacl1定义时创建的作用域链去寻找n，f2函数中没有，所以向外一层，在第一次执行的f1中找到了，该n为1001
- Ln17：`cacl2()`
  - 沿作用域链在第二次执行的f1中找到了，n为1001
- Ln19：`nAdd()`
  - 执行：`n += 2`
    找到了第二次执行的f1中的n，赋值为1003
- Ln21：`cacl1()`
  - 沿作用域链在第一次执行的f1中找到了，n为1001
- Ln22：`cacl2()`
  - 沿作用域链在第二次执行的f1中找到了，n为1003


# 总结
- 闭包：一个函数有权读取另一个函数的作用域下的变量。是定义的时候决定的。

- 作用域也是定义的时候决定的
- 函数中定义的全局变量，不会在预解析阶段提前（因为没有var，也不是函数声明），只有第一次执行完该函数的这一行时，才会在全局变量中出现
- 仔细/小心/谨慎




