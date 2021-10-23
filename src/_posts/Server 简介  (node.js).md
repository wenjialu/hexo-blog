---
title: node.js
date: 2020-12-09 02:30:48
tags: backend
---


# node.js （用作Server）

allow  js run everywhere, not only brewer.



## install

Node -v 检查有没有安装

没有的话去官网安装一下就好了

(我目前安装的是 v12.13.1 版本)



## 基本使用

终端 node；

就能写 js

Pocess.exit() 退出



小实验 （这次不直接在终端写了，而是用一个js文件存储js）：

创建一个script.js

Node script.js 就能运行



### globalThis

就是 window object， 不过在浏览器以外(比如终端node模式下）也能使用。





### 导包方式

**commonJs modules way:** 

const xx = require("./script.js")

Module.exports = {

xxx = xxx

}

**Es6 way **(version 12 以上)

1 命名为mjs文件

Import/ export

2 用 npm init (-y)来创建一个package.json:

里面加上 “type": "module"

Import/ export



### 一些比较常用的包

http

const xx = require("http")



Nodemon （会一直监听？server）

npm install nodemon --save-dev

* --save-dev (only use for development 部署的时候用)



## 用它来搭 server （比较老的轮子： http）

### 加 nodemon

![image-20201120224438836](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201120224438836.png)







### 用http 三行建立连接 （之后就不用http了，用express）

测试连接

![image-20201120224603712](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201120224603712.png)

ps。要打开本地浏览器 **访问localhost:3000.才会出现hear you。**

**访问一次出现一次**



### 接受与响应

![image-20201120225020575](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201120225020575.png)



### 响应返回json

传json回去

![image-20201120225338640](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201120225338640.png)



## 新轮子：express ⭐️

19年最流行的server library： express

详情可以去看官方文档

	npm install express

### 一样还是三行搭建好服务器。

	const express = require("express");
	
	const app = express();
	
	app.listen(3000)



### middleware 中间件

#### 语法

​	写在响应前

	app.use((req, res, next) => {
		console.log("hi")
		next();
	}
	) 

#### 常用中间件

##### Body parser （新版本这个中间件被写进去了，就不用了）

解析json的

	const bodyParse = require("body-parser")
	
	const app = express();
	// 中间件
	app.use(bodyParse.json());





### 返回响应

	app.get("/",(req, res) => {
		res.send("hellooo, get root")
	}
	);

对比之前的，不需要设置相应头之类的了

![image-20201120231735301](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201120231735301.png)



#### 返回 json

![image-20201120232020853](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201120232020853.png)

#### 返回静态页面 如html， css

	app.use( express.static(__dirname + "/静态文件夹"))







#### Get post delete

然后用postman 测



### 文件系统 fs

先下载 fs

fs 能读取文件

	const fs = requore("fs");
	const file = fs.readFileSync("./hello.txt");
	console.log(file.toString());



![image-20201121132027065](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201121132027065.png)

fs 能写文件

appendFile(".txt","  "   )

writeFile( " .txt", "   ")

删除

Unlink











## Postman 测试

query, params 请求可以在URL里面直接加 eg。localhost:3000/?name=xxx&age=xx.  /:id

Server 里面能 get 到 req.query



header等就要在 postman 里面模拟了

## Restful

/名词



彩蛋：

Johnny-Five （用来做robot的 js 库）❤️
