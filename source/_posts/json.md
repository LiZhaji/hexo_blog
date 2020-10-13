JSON表示一种数据格式，可以用来表示普通类型（数值字/符串/布尔值）、对象、数组、null，与XML相比更简洁便利。



我们通常使用`JSON.stringify`将对象序列化为json字符串，用`JSON.parse`将json数据解析为js对象，如下是一个简单的demo。

```js
const zhaji = {
  name: "zhaji",
  age: "22",
  family: ["mom", "dad"]
};

const zhajistr = JSON.stringify(zhaji); 
// "{"name":"zhaji","age":"22","family":["mom","dad"]}"

const zhajicopy = JSON.parse(zhajistr); 
// 类似zhaji对象
```



### JSON.stringify(val, ?replacer, ?space)

实际上，`JSON.stringify`可以接收三个参数，分别是`value(js对象), ?replacer(过滤器), ?space(缩进表示)`。（?表示可选）

**replacer****过滤器可以是数组或者函数形式。**

- 数组项表示要返回的key值



```js
JSON.stringify(zhaji, ["name", "family"]);
// "{"name":"zhaji","family":["mom","dad"]}"
```



- 函数接收两个参数，key value，返回值就是相应key值，默认返回value



```js
JSON.stringify(zhaji, (key, val) => {
  switch(key){
    case "name": return val;
    case "age": return 23;
    case "family": {
      // 注意 这样的写法会影响原对象zhaji的family，因为指向同一个地址
      val.push("sister"); 
      return val;  
    }
    default: return val;
  }
})
// "{"name":"zhaji","age":23,"family":["mom","dad","sister"]}"
```



**space缩进****可以是数字或字符**

- 数字 表示缩进多少个空格

```js
JSON.stringify(zhaji, '', 4)
/* 
"{
    "name": "zhaji",
    "age": "22",
    "family": [
        "mom",
        "dad"
    ]
}"
*/
```

- 字符 表示用此字符表示缩进

```js
JSON.stringify(zhaji, '', '--')
/*
"{
--"name": "zhaji",
--"age": "22",
--"family": [
----"mom",
----"dad"
--]
}"
*/
```

#### toJSON

`toJSON`方法允许开发者自定义序列化的行为。可以作为过滤器的补充。

```js
const zhaji = {
  name: "zhaji",
  age: "22",
  family: ["mom", "dad"],
  toJSON(){
    return "hi zhaji!"
  }
};

JSON.stringify(zhaji)
// ""hi zhaji!""
```

序列化对象的**顺序**如下^[1]^

1. 若存在`toJSON`方法，则调用并返回该方法的返回值，否则返回对象本身
2. 如果存在过滤器，则调用，传入值为第一步的返回值
3. 对第二步每个值进行序列化
4. 如果存在相应对象，执行相应的格式化



### JSON.parse(value, ?reviver)

同样，`JSON.parse`也接收两个参数，`value(json字符串), reviver(还原函数)`。这个还原函数和上面的过滤器一样，也接受两个参数key/value，只是在不同的环境下，所以换了个叫法。

```js
JSON.parse(zhajistr, (key, val) => {
      switch(key){
        case "name": return `${val}ya!`;
        case "age": return 18;
        case "family": {
          val.push("sister"); 
          return val;  
        }
        default: return val;
    }
})
/* 
{
  name: "zhajiya!",
  age: 18,
  family: (3) ["mom", "dad", "sister"]
}
/*
```

### 不会被序列化的值

并不是所有的值都会被序列化，以下情况就会忽略掉键值对

- value值为`undefined`
- value值不可枚举

```js
const noappear = {
    hi1: undefined
}
Object.defineProperties(noappear,{
  hi2: {
    value: "hi2",
    enumerable: false
  },
  hi3: {
    value: "hi3",
    enumerable: true
  }
})

JSON.stringify(noappear)
// "{"hi3":"hi3"}"
```



参考资料：

[1] 《JavaScript高级程序设计第3版》P569

[2] JSON | MDN https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/JSON

------------------------------------------------

欢迎关注我的其他账号～

- 博客：http://blog.escript.cn/

- 掘金：[猪是倒着读着](https://juejin.im/user/1697301686129127) 

- 知乎：[猪是倒着读着](https://www.zhihu.com/people/zhu-shi-dao-zhao-du-de)

- 公众号：炸鸡呀（更多内容）

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5379eb07cc84cfd850afb4e41f41acc~tplv-k3u1fbpfcp-zoom-1.image?imageslim)


