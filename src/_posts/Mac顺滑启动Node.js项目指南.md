---
title: Mac顺滑启动Node.js项目指南
---

**可视化**

建议可以用一个仪表盘来可视化管理vue项目

1 启动

```
vue ui
```

tip：如果遇到报错：getaddrinfo ENOTFOUND localhost错误。

则：

sudo vim /etc/hosts , 并在 host里面加上 127.0.0.1 localhost

2 导入/新建项目 

如果项目已经有雏形的话，可以直接导入。然后点击右边的“在编辑器中打开”按钮，就会自动在 vscode 中打开你的项目代码，非常的方便好用。

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn9qohguo4j30yo09xaay.jpg)

3 管理项目

使用仪表盘，可以非常直观的管理项目的依赖、插件、配置等等。

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn9qshw2l0j311z0juwh8.jpg)

4 编译/发布项目

点开任务栏，甚至能帮助直接编译启动你的项目。

启动成功后，还会有详细的数据分析，如速度统计，资源占比分配是否合理等等，并给出具体的建议。

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn9qvx87smj31240j9adn.jpg)



原理：

-

api.js

Crud.js

index.vue



-

Router -- index.vue

**配置：**

需要解决2个问题

1 Proxy问题：

**process.env**

> Nodejs 提供了 process.env API，它返回一个包含用户环境信息的对象。当我们给 Nodejs 设置一个环境变量，并且把它挂载在 process.env 返回的对象上，便可以在代码中进行相应的环境判断。

windows下你要这样：

```
#node中常用的到的环境变量是NODE_ENV，首先查看是否存在 
set NODE_ENV 
#如果不存在则添加环境变量 
set NODE_ENV=production 
#环境变量追加值 set 变量名=%变量名%;变量内容 
set path=%path%;C:\web;C:\Tools 
#某些时候需要删除环境变量 
set NODE_ENV=
```

**永久配置**
右键(此电脑) -> 属性(R) -> 高级系统设置 -> 环境变量(N)...

mac下你要这个：

```
#node中常用的到的环境变量是NODE_ENV，首先查看是否存在
echo $NODE_ENV
#如果不存在则添加环境变量
export NODE_ENV=production
#环境变量追加值
export path=$path:/home/download:/usr/local/
#某些时候需要删除环境变量
unset NODE_ENV
#某些时候需要显示所有的环境变量
env
```





**Mac必须要写 http://127.0.0.1 而不是 localhost ！！！**

即使host文件里面配置过 DNS 的关系也得这样写。



For windows:

```js
"proxy": {
  "/auth/google": {
    "target": "localhost:8830/m"
  }
}
```

For Mac:

```js
"proxy": {
  "/auth/google": {
    "target": "http://127.0.0.1:8830/m"
  }
}
```





2 请求超时：

请求超时，首先判断接口服务器是否启动，再去判断接口地址、端口、路径是否正确

我这些都没问题。

然后查看 axios：

scr - utils - request.js的文件

打开文件，在里面如下图所示更改一个地方即可，即把timeout的时间改长一些（比如 5000 改 90000），再重新打开项目获取数据会发现成功获取并且不报超时～



3页面直接显示空白
修改vue.config.js中publicPath为**' ./ '**即可注意这里的publicPath 为build下



4 表单不显示

当后台服务器启动，其他表单能显示，唯独我自己写的表单加载不出来。

当后端服务器没有开启的时候，所有表单都是加载不出来的，因为请求找不到相应资源。

由此推断，是没有找到相应的路径，问题就出现在url上！

解决方案：

1 把后端Controller的访问url改成Round_Dict

 api 和 user 间有循环应用，在pom中注释掉其中一个依赖就行了。

Q: SpringCloud启动出现 Could not locate PropertySource: I/O error on GET request for 解决](https://my.oschina.net/PTOldDriver/blog/3016037)

1.检测配置文件里是否有如果没有请加上

```\
spring:
  cloud:
    config:
      enabled: false
```

2.如果是application.yml(application.properties)改为 bootstrap.yml（bootstrap.properties）即可.

Q

```
java.lang.IllegalStateException: Service id not legal hostname (poi_portrait_service)
```

解决办法：

application.yml文件中的

```
spring:
  application:
    name: poi_portrait_service
```











资料：

环境变量process.env： https://github.com/frmachao/blog/issues/4

element_ui: https://cloud.tencent.com/developer/section/1489884