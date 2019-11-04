---
title: 引入Spring boot项目失败问题
date: 2019-11-04 17:26:40
catalogies: java
---

想要打开别人的Spring boot项目，import的包总是报错，运行不了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105164918778.png)
可能有如下原因。
## 一、配置问题
- 打开首选项Preferences
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105165054296.png)
- 搜索 maven，查看最后两行是否是别人的配置路径。如果是的话，勾选Override，默认配置目录即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105165253410.png)
- 之后重新下载包即可。
- 一般IDEA会自动下载，如果没有下载，点击右侧的maven，双击clean
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105170139152.png)
- 如果还是报错，可以看到包下载的具体失败原因了

## 二、版本问题
- 选择pom.xml
- 搜索`spring-boot-starter-parent`，找到`<version>xxx.RELEASE</version>`(xxx表示版本号)
- 更改版本号尝试