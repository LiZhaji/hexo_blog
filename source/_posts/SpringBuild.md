---
title: IDEA SpringMVC与Springboot项目的搭建记录
date: 2019-11-05 16:43:40
catalogies: java
---


spring mvc和spring boot，后者相对来说更大一些，比如包含了tomcat，本质·上都是java + java包，只是不同的方式导入不同的包罢了。
它们的快速构建方式也有所不同，在此做个记录。

## 如果是未装环境的新电脑Mac，先安装JDK环境。
- [官网](https://www.oracle.com/technetwork/java/javase/downloads/index.html)下载JDK
	- 目前有13、11、8三个版本，选择其中一个点击download
	- 选择dmg版本，下载完后双击运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110516255111.png)
	- 完成之后打开terminal，输入 `java -version`，如果出现版本号，表示下载成功（mac自动配置好了。）
## SpringMVC
- File -> New -> Project
- Maven 共四步，见下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105155041261.png)
- GroupId表示组织名，一般为com.xxx，lyj是笔者名字缩写
	ArtifactId表示项目名


![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105155434525.png)
- 灰色表示默认的maven路径，也可以Override选择自己下载好的maven路径
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105155850351.png)
- 确认/重写项目名和项目保存的位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110516003446.png)
- Finish
- 需要等待一会儿，让其下载相关的包
- 下载完毕后，打开即可。
## Spring boot
- File -> New -> Project
- Spring Initializr 默认即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105160428174.png)
- 填写包名、项目名等信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110516085396.png)

- 选择想要安装的依赖。如果不清楚要装什么直接next
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105161125876.png)
- 确认/重写项目名和项目存储位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105161158368.png)
- Finish 
- 打开新建的项目后会跳出框，选择Enable Auto-Import让其导入依赖
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105163914298.png)
- 导入完成后，默认目录如下。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191105164209965.png)
- 至此，一个Spring boot项目搭建完毕。
