---
title: 「解放生产力」博客自动发布指南
date: 2020-10-09 21:10:03
tags:
---
不知道大家有没有过这样的情况，每次更新博客都需要反复操作：

- 本地更新文章
  - hexo new name 本地生成文章
  - hexo g 生成public静态文件
  - hexo deploy 部署到gh-pages

- 提交到GitHub
- 云服务器拉取数据
  - git pull
  - hexo g 生成public静态文件


不外乎重复这些命令，实在是浪费时间。

那可不可以解放生产力，实现一键发布呢？

趁着假期搞了一下，使用node就可以实现，但登录服务器有所限制，可以使用expect补充。

代码地址：https://github.com/LiZhaji/auto_submit



# 效果预览

以下是最终的效果截图

![image.png](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/54c94e7489ae48e783cde6300dce6342~tplv-k3u1fbpfcp-zoom-1.image)

![image.png](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b90c488b634e4008a0f95cf150a14ecd~tplv-k3u1fbpfcp-zoom-1.image)

上传成功后直接刷新博客，就能看到最近的文章了。

![image.png](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/371f90393aa14b25b6e104b6317d3417~tplv-k3u1fbpfcp-zoom-1.image)





显然，一键发布的本质是自动执行命令，也就是把上面列举出来的命令统统让代码去执行。

我们先把最重要的自动发布脚本写出来。



# *自动执行命令

我们预期在这里能够完成自动生成文章、上传并登录云服务器。

了解到Node的`child_process.exec`可以执行命令。

那就直接开始吧。

```js
// submit.js

const exec = require('child_process').exec

const blogPath = '/workspace/hexo_blog' // 输入你的博客目录地址

// 1. 本地生成文章
exec(`cd ${blogPath}`, log) // 到博客目录
exec(`npx hexo new test`, log)
console.log('ok fine')

const log = (err, stdout, stderr) => {
  console.log(err, stdout, stderr);
  
}
```

按照之前的步骤，很流畅得写下了以上代码。

然而，`node submit.js`运行的结果却事与愿违，"它安装了npx..."并没有创建好我们的test文章。

为了确保问题来源，先去终端下手动执行上面的命令，完全没问题！那很明显，就是目录问题了，说明第一个cd命令没起作用。

可以使用`pwd`命令查看当前目录。



## 如何在指定目录下执行命令

有两种方法。

- `exec(cmd, {cwd: 'path'}, (err, stdout, stderr) => {})`
- `require('shelljs') shell.cd('path')`



我们使用第一种方法。

```js
exec(`npx hexo new test`, {cwd: blogPath }, log)
console.log('ok fine')
```

这时再执行submit，文章创建成功！

![image.png](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9c93648791c4827bd8e1c81dad92c35~tplv-k3u1fbpfcp-zoom-1.image)

然而，不知道你有没有发现另一个问题。`ok fine`出现的很快，而下面的info信息过了许久才出现。

这并不是我们想要的结果。我们必须要等上一步骤执行完，再去执行下一步，不然都是白操作。

出现以上现象的原因就在于**exec是****异步**的，那我们只需要将它转为同步或等待它执行完再去执行下一个就行。

## 如何顺序执行

有两种方法

- `child_process`还提供了`execSync`方法，用于同步执行命令。但这样没有输出信息。
- 将我们后续操作放入回调里。但很容易形成回调地狱。



为了既有信息输出，又可以顺序执行，我们自己封装一个promise。

```js
// 统一命令
const myExec = (cmd, path = blogPath) => {
  return new Promise((resolve, reject) => {
    exec(cmd, { cwd: path }, (error, stdout, stderr) => {
      // 输出信息
      log(error, stdout, stderr);
      
      if (error) {
        reject(error);
      } else {
        // 打印彩色字体的日志
        console.log(`「\x1B[34m${cmd}\x1B[0m」执行完毕...`);
        resolve(stdout);
      }
    });
  });
};
```

在命令的回调中，我们才调用`resolve/reject`方法，这样就可以确保此时命令已经执行完了。如果执行有错就抛出错误，没有错就返回输出结果。

然后，就可以愉快的使用了。

```js
(async () => {
    await myExec(`npx hexo new test`);
    console.log('ok fine')
})()
```



## 写入内容

文章创建好了，我们需要给文章添加内容，内容来源于请求。

因为使用`hexo new`创建的文章开头会有一些信息，因此我们需要追加内容而不是覆盖内容。`fs`的`appendFile`方法，可以实现内容追加。

```js
// 将内容追加到生成的md文章中
const appendContent = (title, content) => {
  return new Promise((resolve, reject) => {
    fs.appendFile(`${blogPath}/source/_posts/${title}.md`, content, (err) => {
      log(err);
      if (err) {
        reject(err);
      } else {
        resolve("success");
      }
    });
  });
};

// 统一命令
const myExec = (cmd, path = blogPath) => {
  return new Promise((resolve, reject) => {
    exec(cmd, { cwd: path }, (error, stdout, stderr) => {
      log(error, stdout, stderr);
+      if (cmd.includes('hexo new')) {
+        title = stdout.split('/').pop().slice(0,-4)
+      }
      // ...
    });
  });
};
```

同样，也需要等文章内容填入后，我们再执行下一步操作，所以该方法也返回了一个promise。

其中title是全局变量，由两个地方控制。第一个是接口请求的参数之一title，第二个是`hexo new` 生成的文章，因为文章重名的话，`hexo`会自动为加上序号，因此最后要以实际生成的文件名为主，否则文章内容会加错地方。

例如，再执行`hexo new test` 实际生成的文章名为"test-1"

![image.png](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f331cf517488496cabcaf16c7be38912~tplv-k3u1fbpfcp-zoom-1.image)

继续往下走。

```js
(async () => {
  // 1. 创建文章并部署
    await myExec(`npx hexo new ${title}`);
  await appendContent(title, content);
  await myExec(`npx hexo g`);
  await myExec(`npx hexo d`);

  // 2. push
  await myExec(`git add .`);
  await myExec(`git commit -m "「auto」${title} ${new Date().toLocaleString()}"`);
  await myExec(`git push`);
  
  // 3. 云服务器
})()
```

到这里都应该没有问题了。



## 登录云服务器

最后的操作是登录云服务器然后拉取代码。

为了避免交互，设置了服务器的免密登录，然后按部就班

```js
(async () => {
  // 3. 云服务器
  await myExec(`ssh root@ip`);
  await myExec(`cd hexo_blog`);
  await myExec(`git pull`);
  await myExec(`hexo g`);
  
})()
```

但发现，运行到第三行就卡住了，并不能如期实现结果。

![image.png](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a79493733d24488a857a6f469249d93b~tplv-k3u1fbpfcp-zoom-1.image)

查阅资料发现，可以通过[expect](https://www.jianshu.com/p/2fcdf764f464)^[3]^实现服务器登录并执行（mac自带expect）

下面是expect的脚本`yun.sh` 实现了以上命令。

```sh
#!/usr/tcl/bin/expect

set timeout 30
set host "ip"
set username "root"

spawn ssh $username@$host
expect "Welcome*" 

send "cd hexo_blog\r"
expect "$"
send "git pull\r"
expect "done"
send "hexo g\r"
expect "files generated"
# interact # 不保持登录
```

使用`expect yun.sh`命令执行测试。

没什么问题后，写入之前的代码中。

```js
(async () => {
  // 3. 云服务器
  await myExec(`expect yun.sh`, `.`);

})()
```

其中第二个参数表示当前目录。

到此为止，通过一条启动命令我们已经可以如期实现“一键发布”了。

为了正常使用，我们取消立即执行，将其封装为一个函数然后暴露给外部。

```js
const autoSubmit = (initTitle, content) => {
  // 标题的空格换成下划线_
  title = initTitle.replace(/\s/g, '_') 
  
  return new Promise(async (resolve, reject) => {
    try {
      // 1. create file
      // 2. push
      // 3. 云服务器
      resolve();
    } catch (err) {
      log(err);
      reject(err);
    }
  });
};

module.exports = autoSubmit;
```

## 来点儿美好的提示

为了更好的使用体验，我们再加点儿东西。

每条命令在执行的时候都需要一个等待过程，为了让等待不那么枯燥，我们搞一个命令行的加载提示。想了解原理的同学参考[命令行中的加载动画，spinner 的艺术](https://zhuanlan.zhihu.com/p/22350732)^[2]^

```js
// spinner.js

class Spinner {
  loadingDoc = ["⠋", "⠙", "⠹", "⠸", "⠼", "⠴", "⠦", "⠧", "⠇", "⠏"];
  index = 0;
  timer = null;
  start(desc) {
    this.timer = setInterval(() => {
      process.stdout.write(
        `\r${this.loadingDoc[this.index++ % this.loadingDoc.length]} ${desc}...`
      );
    }, 300);
  }
  stop() {
    clearInterval(this.timer);
    process.stdout.write('\n')
  }
}

module.exports = Spinner;


// submit.js

const Spinner = require("./spinner");
const spinner = new Spinner();

// 统一命令
const myExec = (cmd, path = blogPath) => {
+  spinner.start(cmd);
  return new Promise((resolve, reject) => {
    exec(cmd, { cwd: path }, (error, stdout, stderr) => {
+      spinner.stop();
     
    });
  });
};
```



# 一个简单的页面

搭建一个页面，参考了[Vue的markdown实例](https://cn.vuejs.org/v2/examples/index.html)^[1]^ ，在此基础上，添加了标题输入和发布按钮。

这里贴一下发布代码，使用了xhr对象

```js
// 发布
const submit = () => {
    if (!this.title || !this.input) {
    alert('文章标题或内容不可为空')
    return
  }
  const xhr = new XMLHttpRequest();
  xhr.open("post", "http://127.0.0.1:8888", true);
  xhr.setRequestHeader("Content-Type", "text/plain; charset=utf8");

  const data = {
    title: this.title,
    content: this.input
  }
  xhr.send(JSON.stringify(data));
}
```

以上代码先检查了文章标题和内容不为空，然后创建一个xhr对象和post连接，设置了请求头类型，最后将数据以json格式发送给后台。



# 一个简单的服务

使用`http`创建一个服务，拿到数据后执行发布逻辑。

```js
const http = require("http");
const autoSubmit = require("./submit");

http
  .createServer((req, res) => {
    const arr = []
    req.on("data", function (chunk) {
      arr.push(chunk)
    });

    req.on("end", async function () {
      // 解析参数
      const body = Buffer.concat(arr).toString()
      const { title, content } = JSON.parse(body);

      // 允许跨域
      res.setHeader("Access-Control-Allow-Origin", "*");
      await autoSubmit(title, content);
      // 设置响应头部信息及编码
      res.writeHead(200, {"Content-Type": "application/json; charset=utf8"});
      res.end("success");
    });
  })
  .listen(8888);

console.log("已运行在端口8888");
```

创建服务后，监听了请求的ondata方法，将拿到的二进制数添加到数组中，请求结束时使用`Buffer`拼接，就拿到了我们请求的发来的数据。

将拿到的`title/content`传入封装好的`autoSubmit`，让该函数去执行命令。

使用`node index.js`启动服务。



查看[代码使用指南](https://github.com/LiZhaji/auto_submit/blob/master/README.md)^[4]^



参考文章及链接：

[1] Linux expect详解 https://www.jianshu.com/p/2fcdf764f464

[2] 命令行中的加载动画，spinner 的艺术 https://zhuanlan.zhihu.com/p/22350732

[3] Markdown 编辑器 Example https://cn.vuejs.org/v2/examples/index.html

[4] 代码使用指南 https://github.com/LiZhaji/auto_submit/blob/master/README.md