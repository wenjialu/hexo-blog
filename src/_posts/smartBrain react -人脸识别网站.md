---
title: SmartBrain -  用 React 做一个人脸识别网站
date: 2021-12-22 02:35:48
tags: react
---

[TOC]

# 用 React 做一个人脸识别网站

## 引言

React 可以说是近些年最热门的前端框架啦， 并且非常非常的好用 ~

很多的国外公司比如 [Netflix](https://zh.wikipedia.org/wiki/Netflix)、[Feedly](https://zh.wikipedia.org/wiki/Feedly)、[Airbnb](https://zh.wikipedia.org/wiki/Airbnb) 都是用它来实现的网站主页。



那么 React 到底是个什么呢？它其实是一个为数据提供渲染为[HTML](https://zh.wikipedia.org/wiki/HTML)视图的[开源](https://zh.wikipedia.org/wiki/开源软件)[JavaScript 库](https://zh.wikipedia.org/wiki/JavaScript函式库)。传说最早起源于Facebook 的内部项目，由于太好用了，就开源给大家使用了。



现在国内的很多公司都在招聘会 React 的前端工程师。说明如果会这项技术还是会很吃香哒。

那么我们应该怎么学习它呢？ 

[官方文档](https://zh-hans.reactjs.org/docs/hello-world.html)当然是一个选择 ～

里面有官方人员给出的最严谨的定义和最全的方法解释，一步一步跟着学的话应该也能学会。

不过，稍显有那么点点枯燥啦。

**想要更好玩的学习方法，不妨找个小项目来上手吧。**

因为根据认知心理学的理论，学习的最好方法之一就是应用 ～

在“做中学” 会比单纯的 “看概念” 更记忆深刻哦 ～





我之前在初步学习 react 的时候，有小试牛刀，

做了一个机器人🤖️网站，网址在这里：https://rocky-hamlet-79784.herokuapp.com/  。教程在此：



今天想教大家一个升级版！！！一个可以有用户的人脸识别网站～

## 效果展示 

https://wenjia-smartbrain.herokuapp.com/ （还没完全完成好）

由于教程篇幅过长，将会分成四个部分：前端篇、后端篇、数据库篇 和 部署篇。

跟着我一起学完，不仅能掌握 react 框架的使用，还能拥有一个炫酷的网站哦～～





## 源码 （hide）

Front end: https://github.com/aneagoie/smart-brain

Back end: https://github.com/aneagoie/smart-brain-api

```bash
Final project for ZTM course

    Clone this repo
    Run npm install
    Run npm start
    You must add your own API key in the src/App.js file to connect to Clarifai.

You can grab Clarifai API key here
```



## 前端

前端的页面想设计成这样：



![未命名文件 (2)](/Users/lujiawen/Desktop/未命名文件 (2).png)

首页可以拆分成3个元素， 分别是登录、注册、导航栏。

那就每个元素做成一个组件。



登录后的页面

![未命名文件 (1)](/Users/lujiawen/Desktop/未命名文件 (1).png)



可以拆分成4 个模块。除去导航栏，还有logo、搜索框、面部识别功能区。

设计component，并放在app.js中 。

tips：app.js中的多个 components 要放在 <div></div> 里面哦。

```

function App() {
  return (
    <div>
       <Navigation />
       <Logo /> 
        <ImageLinkForm/>
        <FaceRecognition />

    </div>

  );
}

```







### 注册, 登录功能

#### css

Google 搜索 tachyons + forms,  tachyons + card 加个边框

页面预览和代码都有啦～～

```jsx
const Signin = () => {
    return(
        // 这里放html代码, 
       //不过记得把input 像这样 /> close 掉. for 要改成htmlFor, class 改成 className
      <article class="center br3 ba b--black-10 mv4 w-100 w-50-m w-25-l shadow-5 mw6 center">
        ...
      <article />  
    )
}

export default Signin;
```

自行删删改改



#### 页面

App.js

功能： 没 signin 之前是signin 页面， signin 之后是rank+上传image 页面。

实现： constructor()

tips:  jsx语法： html 里加了{  } 就可以写 js 了。

```js
constructor(){
    super()
    this.state = {
      input: "",
      route: "signin"
    }
  }


render() {
    return (
      <div className="App">
         <Navigation />
         { this.state.route === "signin"
          ?  <div>< Signin /></div>
          :  <div> 
            <Logo /> 
            <Rank />
            <ImageLinkForm onInputChange={this.onInputChange} onButtonSubmit={this.onButtonSubmit}/>
            <FaceRecognition />
            <Particles className="particles"
                  params={particleOptions}
                /></div>
         }   
      </div>
  
    );
    }
  }
   

```

同理，新建 register 页面。

```
// 新增代码
 <div className="mt3">
                <label className="db fw6 lh-copy f6" htmlFor="name">Name</label>
                <input
                  className="pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100"
                  type="text"
                  name="name"
                  id="name"
                  onChange={this.onNameChange}
                />
              </div>
```





#### 页面跳转

设置

```
constructor(){
    super()
    this.state = {
      input: "",
      route: route
      // route: "signin"
    }
  }
  
  
  onRouteChange = (route) => {
    this.setState({route: route});
  }

render() {
    return (
      <div className="App">
         <Navigation onRouteChange = {this.onRouteChange}/>
         { this.state.route === "signin"
          ?  <div>
              <Signin />
              <Particles className="particles"
                    params={particleOptions} />
              </div>
          :  <div> 
              <Logo /> 
              <Rank />
              <ImageLinkForm onInputChange={this.onInputChange} onButtonSubmit={this.onButtonSubmit}/>
              <FaceRecognition />
              <Particles className="particles"
                    params={particleOptions}
                /></div>
         }   
      </div>
  
    );
    }
```







添加：思路；需要跳转到什么页面， onRouteChange(param)}， param就写什么参数。

tips： 千万别忘了在函数头传参{onRouteChange} ！！！！！

Navigation.js  (signout button)

```
onClick={({onRouteChange}) => onRouteChange("signin")}
```



Signin.js 

```
// 千万别忘了传参{onRouteChange} ！！！！！
// 箭头函数：render的时候不调用这个函数，只有当onClick的时候才调用
//            onClick = {() => onRouteChange("rankpage")}
            
     <div class="">
            <input 
            // 箭头函数：render的时候不调用这个函数，只有当onClick的时候才调用
            onClick = {() => onRouteChange("rankpage")}
            className="b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6 dib" 
            type="submit" 
            value="Sign in" />
            </div>
            <div class="lh-copy mt3">
            <a onClick = {() => onRouteChange("register")} href="#0" className="f6 link dim black db">Register</a>
                        </div>       
            
```



Register

```
<input 
  // 箭头函数：render的时候不调用这个函数，只有当onClick的时候才调用
  onClick = {() => onRouteChange("signin")}
  className="b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6 dib" 
  type="submit" 
  value="Register" />
  </div>
```

### 

### 上传照片并识别

1. 识别输入框的内容（url）： 设置state：input

2. Detect 按钮识别输入框的图片url：imagelinkform component(oninputchange + onbuttonsubmit)。
3. 在输入框下方展示url中的照片：FaceRecognition component.
4. 识别照片中的人脸：clarifai
5. 标示人脸：查看 clarifai 的 response -->写入到 FaceRecognition component.



#### Step 1. 输入框，按钮框对于输入的识别

App.js 设置 state,并渲染

```
constructor(){
    super()
    this.state = {
      input: ""
    }
  }

  onInputChange = (event) => {
    console.log(event.target.value);
  }

  onButtonSubmit = () => {
    console.log("click");
  }
  
render() {
    return (
      <div className="App">
  
          <ImageLinkForm onInputChange={this.onInputChange} onButtonSubmit={this.onButtonSubmit}/>
         
      </div>
  
    );
    }  
```





ImageLinkForm.js

传参，用参

```
 <input className="f4 pa2 w-70 center" type="text" onChange={onInputChange}/>
  <button className="w-30 grow f4 link ph3 pv2 dib white bg-light-purple" 
                    onClick = {onButtonSubmit}>Detect</button>
            
```



#### step 2. 能上传图片

调用人家已经做好的api

clarifai （https://www.clarifai.com/）

注册一个账户就能免费用了

#### step 3. 展示图片

```
const FaceRecognition = ({ imageUrl, box }) => {
    return (
      <div className='center ma'>
  
      <div className='absolute mt2'>
        {/* 下面这行是关键的代码，通过src展示url对应的图片 */} {/* width， height固定图片大小 */}
        <img id='inputimage' alt='' src={imageUrl} width='500px' heigh='auto'/>
        {/* 标示人脸 */}
        <div className='bounding-box' style={{top: box.topRow, right: box.rightCol, bottom: box.bottomRow, left: box.leftCol}}></div>
      </div>
    </div>
    );
  }
```



#### step4能识别图片

调用人家已经做好的api, 注册一个账户就能免费用了

##### 查看文档

clarifai （https://www.clarifai.com/）

Model->face dectection = https://www.clarifai.com/models/face-detection

```
https://clarifai.com/developer/
docs.clarifai.com/api-guide/predict/images
```

Google：npm clarifai -> GitHub 源码 -> 里面有所有的模型

##### 下载

npm install clarifai

##### 使用

```
const Clarifai = require('clarifai');

//You must add your own API key here from Clarifai. 

const app = new Clarifai.App({

 apiKey: '注册后就有了'

});

...除了人脸识别，还可以使用clarifai上不同的模型做不同的事情（识别色彩，理解图片，识别视频。。。）

```



### 

### css 部分

因为要花好多时间调试好看的样式，而样式会根据项目的不同而进行很大的调整。

所以这里具体样式就直接给大家了，就不用花太多时间纠结样式了。

#### Magic 1. tachyons

##### 安装

```
npm install tachyons
npm tachyons

```

##### Index.js 导入

```
import "tachyons";
```

##### 使用：

###### Naviation.js

Q: 是怎么知道里面的样式呢。

A:

1. https://tachyons.io/#style style guide

```
			<nav style = {{display: "flex", justifyContent: "flex-end"}}>
        <p className="f3 link dim black underline pa3 pointer">Sign Out</p>
      </nav>
```

长这样：

![image-20201120171507674](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201120171507674.png)

2. Google 搜索 tachyons + 关键词

   eg. tachyons  form

   页面预览和代码都有啦～～

```jsx
const Signin = () => {
    return(
        // 这里放html代码, 不过记得把input 像这样 /> close 掉. for 要改成htmlFor
        ...
    )
}

export default Signin;
```


   ![image-20201202211043579](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201202211043579.png)



Ps. 关于className.

自己在app.css logo.css 里面定义的class 也可以用className 引用

```css
.center{
  display: flex;
  justify-content: center;
}
```





#### Magic 2. React-tilt （for Logo）

React-tilt library 可以有炫酷的交互。

去它的网站上看安装和使用方式，直接复制就能用了

```
npm install --save react-tilt
```

好可爱，喜欢

```
import Tilt from 'react-tilt'
 
<Tilt className="Tilt" options={{ max : 25 }} style={{ height: 250, width: 250 }} >
 <div className="Tilt-inner"> 👽 </div>
</Tilt>
```

可以加一些css样式

然后把👽换成自己的logo （没有的话去Google一个免费的）

```
import brain from "./xx.png";

<img alt="logo" src={brain} />
```



#### Magic 3. linear-gradient (for background)

自己去Google一个好看的linear-gradient。

Index.css (index的样式会影响整个项目)

```
body {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  background: linear-gradient(89deg, #e66465 0% , #9198e5 100%);
}
/* linear-gradient(89deg, #FF5EDF 0%, #04C8DE 100%); */

/* 手指 */
button{
  cursor: pointer;

}
```



#### Magic 4. Particles.js  (for background)

超炫酷！！！！动态背景

react 版本： 搜索  react Particles npm

```
npm install react-particles-js
```

app.css 

```

.particles{
  position: fixed; 
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  /* 这样就能不覆盖其他层了 */
  z-index: -1;

}
```

app.js 

```
const particleOptions = {
  particles: {
    number:{
      value: 200,
      density: {
        enable: true,
        value_are: 800
      }
    }
    // ,
      // shape: {
      //     type: 'images',
      //     image: [
      //         {src: 'path/to/first/image.svg', height: 20, width: 20},
      //         {src: 'path/to/second/image.jpg', height: 20, width: 20},
      //     ]
      // }
  }
}


在render里面第一个使用这个模块。
```



参数再研究下让他能互动！





#### Magic 5. css3 partern gallery

http://projects.verou.me/css3patterns/



\#\#\## 



ps. 关于 `<React.StrictMode></React.StrictMode>`

In index.js change `<React.StrictMode><App /></React.StrictMode>` to `<App />` and you will not see this warning. Please note that strict mode helps you with

- Identifying components with unsafe lifecycles
- Warning about legacy string ref API usage
- Warning about deprecated findDOMNode usage
- Detecting unexpected side effects
- Detecting legacy context API

Please refer to https://reactjs.org/docs/strict-mode.html before removing it.







### 状态转化总结 ⭐️⭐️⭐️

(可以放到 react 教程中)

App.js

1 设置 constructor: 创建state, 初始值可以随便设不影响（因为目的是创建出state，初始值不是很重要），比如设置空“”

tips：js里面布尔值false才是F， 字符串“false”是T。所以不能加引号哦。

```
# 需要时要传 props 
constructor(){
    super()
    this.state = {
      input: "",
      route: "swf",
      isSignedIn:false
    }
  }
```



2 设置事件函数：

函数功能：setState

```
 onRouteChange = (route) => {
    if (route === 'signout') {
      this.setState({isSignedIn: false})
    } else if (route === 'rankpage') {
      this.setState({isSignedIn: true})
    }

    this.setState({route: route});
  }
```



函数功能：联系后端

ps. This.props 表示全局都能用啦～

```

    // 箭头函数：render的时候不调用这个函数，只有当onClick的时候才调用
    onSubmitSignIn = () => {
        fetch("http://localhost:3000/signin",
        {
            method:"post",
            headers:{"Content-Type":"application/json"},
            body:JSON.stringify({
                name: this.state.name,
                email: this.state.email,
                password: this.state.password
            })
                .then(response => response.json())
                // 传参，传user
                .then(user =>{
                    if (user.id){
                        this.props.loadUser(user)
                        this.props.onRouteChange("rankpage")
                    }
                })
        })
        console.log(this.state);
    }

```







3 渲染 

Tips: onRouteChange 是函数，调用的时候要this.onRouteChange

isSignedIn 是状态，调用的时候要this.state.isSignedIn

```
<Navigation isSignedIn = {this.state.isSignedIn} onRouteChange = {this.onRouteChange}/>
```



Component 

根据具体情况调用函数传参，以供渲染

```js
function Navigation({onRouteChange, isSignedIn}) {
      if (isSignedIn){
        return (
          <nav style = {{display: "flex", justifyContent: "flex-end"}}>
          <p onClick={() => onRouteChange("signout")} className="f3 link dim black underline pa3 pointer">Sign Out</p>
          </nav>)
      } else {
        return (
            <nav style = {{display: "flex", justifyContent: "flex-end"}}>
              <p onClick={() => onRouteChange("signin")} className="f3 link dim black underline pa3 pointer">Sign in</p>  
              <p onClick={() => onRouteChange("register")} className="f3 link dim black underline pa3 pointer">Register</p>
           
          </nav>);         
      }
  }
```





## 后端

### 开始搭建服务器

有了服务器就能有真的用户注册，登出功能啦～

#### 新建一个 smartbrain-api 文件夹

1. npm init

安装一些依赖，如express， nodemon 等 （npm install xxx）

![image-20201120220035314](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201120220035314.png)

Nodemon: automatically restarts node when file changes are detected in a directory. It replaces the word node when executing a script.
npm install --save-dev nodemon


2. 在文件夹里面新建一个 server.js

```js
const express = require("express");

const app = express();

app.listen(3000, () => {

    console.log("3000")
}
)
```


   在脚本里面设置每次 npm start 自动启动server.js

```
  "scripts": {
    "start": "nodemon server.js"
  }
```


服务器就搭好了。



### 确定后端 api 元素 - 通过看一下前端页面

前端register页面有3个元素

login 里面有名字，rank，上传的图片数



#### 初步的设计

// register --\> req: POST  res: user 

// signin --\> req: POST  res: success/fail 

// profile userId --\> GET res: user 

// image --\> PUT (更新) res: user count 



### 实现 api 元素

既然已经确定了前端页面元素，现在不用管前端了。用postman来模拟前端request ，并接受后端response



##### register

```js
app.post("/register", (req, res) => {
    const {email, name, password} = req.body
    database.users.push({
        id: "125",
        name: name,
        email: email,
        password: password,
        entries: 0,
        joined: new Date()
    })
    res.json(database.users[database.users.length - 1])
    
})
```

![image-20201202165908426](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201202165908426.png)





##### signin

```js
// 如果用户psot的在数据库里面，就返回
app.post("/signin", (req, res) => {
    if (req.body.email === database.users[1].email &&
        req.password === database.users[1].password){
            res.json("success")
        } else{ 
            res.status(400).json("fail")
        }
})
```



##### profile

```js
// 好像有点问题，只能获取到第一个 123
app.get("/profile/:id", (req, res) =>{
    const {id} = req.params;
    let found = false
    database.users.forEach(user =>{
        if (user.id === id){
            found = true;
            return res.json(user);
        } 
        if (!found){
            res.status(400).json("not found")      
        }     
    })
})
```



Image  for rank

```js

app.put("/image", (req, res) => {
    const {id} = req.params;
    let found = false
    database.users.forEach(user =>{
        if (user.id === id){
            user.entries++;
            return res.json(user.entries);
        } 
        if (!found){
            res.status(400).json("not found")      
        }     
})
```













### 用户密码安全🔐

这个项目里用 bcrypt-nodejs    实际用bcrypt.js 或者 bcrypt

通过hash 对用户密码加密

```
We prefer bcrypt for three reasons:  1. bcrypt is 15 years old and has been vetted by the crypto community. Although argon2 won last year’s password hashing competition, it is still fairly new and we would like to see it have a longer lifespan in the crypto community.  2. scrypt is 7 years old and also a good choice, but bcrypt is a bit easier to implement in a node.js server with simpler documentation as you will see below in the example.  3. The bcrypt npm package is better over other bcrypt implementations available on npm since it is native, highly popular, and vetted by the community without trying to reinvent the wheel.

Now you might be asking yourself, why not just use a hashing function like SHA256, add a salt (randomly generated bytes to place in front of the password) for each user, and store those in a database? The problem with computing power increasing is that attackers can now use GPUs to try out passwords at over 100 million per second and see if they get a hit. That’s why you want to use hash functions that were specifically designed to be slow. Although it is fast enough so the user won’t notice (about 100ms), it is long enough to make it infeasible for an attacker to try out a long list of passwords. bcrypt allows you to add a saltRound (10 is the recommended value) which iterates 2^10, or 1024 times over the password in a process called key stretching. Finally, bcrypt implementation is also safe from timing attacks. 

---- udemy
```



### 连接上前端

前后端在现实中应该在不同的电脑上。

后端 端口：3000

前端 端口：3001

先启动后端，再启动前端。会提示我port被占用要不要换，按y就会换到新端口了。



#### 测试

在前端 fetch 后端根路径/的响应，并在渲染的时候打印出来。

根路径在后端设置返回的是用户数据。

可以在浏览器里面看到打印出了用户数据，获取成功～

![image-20201204234706807](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201204234706807.png)





#### 前端代码改写

Signin.js

Const 改为 class， 目的是能够加上监听输入框状态的方法。

因为这些是只和 signin 有关的智能状态，可以写在 component-signin.js 里面。不用写在 app.js 里面让他变得好臃肿。

```
#变为class的操作
1. class Signin extends React.Component {
 render(){
 	return()
 }
2. const {onRouteChange} = this.props;

# 状态
1 constructor(props){
       super(props);
       this.state =  {
           signInEmail:"",
           signInPassword:"" 
       }
    }

 2   onEmailChange = (event) => {
        this.setState({signInEmail: event.target.value })
    }

    onPasswordChange = (event) => {
        this.setState({signInPassword: event.target.value })
    }

    // 箭头函数：render的时候不调用这个函数，只有当onClick的时候才调用
    onSubmitSignIn = () => {
        console.log(this.state);
        this.props.onRouteChange("rankpage")
    }
3 onChange={this.onEmailChange}
 onClick = {this.onSubmitSignIn}
```

Q:  onSubmitSignIn doesn't work.

#### 传送数据到服务器

由前端向服务器发送请求

signin.js

```
// 箭头函数：render的时候不调用这个函数，只有当onClick的时候才调用
    onSubmitSignIn = () => {
        fetch("http://localhost:3000/signin",
        {
            method:"post",
            headers:{"Content-Type":"application/json"},
            body:JSON.stringify({
                email: this.state.signInEmail,
                password: this.state.signInPassword
            })

        })
        console.log(this.state);
        this.props.onRouteChange("rankpage")
    }
```

Fetch 失败，返回404 not found

检查网络： 发现我明明写的方法是post，为啥变成了get

我用post测后端signin接口是正常的呀。

![image-20201205002832834](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205002832834.png)









#### 允许跨域访问

##### 概念

by supporting CORS requests, superawesome.com can allow bob.com to access the data.

Read about it more at [MDN's great resource here][1]!

##### 操作

Tips: 先 ctrl c 断开 nodemon 连接

npm install cors

const cors = require("cors");

app.use(cors());



#### 常见失败的解决方案

在后端加入 打印error的代码，就能查看到失败的原因。

如果是 type error：fail to fetch

就可能是响应头没有允许同源， 即发送请求的地址 和 响应请求的地址 不一样时，fetch 就会失败。

问题原因：The issue could be with the response you are receiving from back-end. If it was working fine on the server then the problem could be with the response headers. Check the *Access-Control-Allow-Origin* (ACAO) in the response headers. Usually react's fetch API will throw fail to fetch even after receiving response when the response headers' ACAO and the origin of request won't match. 

解决方案：  If the API is using `express` for node you can use the simple `cors` package. If you want to make your site properly secure, consider using a whitelist for the `Access-Control-Allow-Origin` header.

```
// express 版本

//设置跨域访问
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});

```







* 忘记 npm install clarifai



* App.js:140 SyntaxError: Unexpected token < in JSON at position 0

  说明返回的是html 页面。可以用postman 试一下，可以看到通常是因为某变量没有被声明。

* DevTools failed to load SourceMap: Could not load content for chrome-extension://xxxxxxxxx/writer/js/angular-ui-router.js.map: HTTP error: status code 404, net::ERR_UNKNOWN_URL_SCHEME

  Chrome 上的拓展程序加载错误。

  右上角三点 - 更多工具 - 拓展程序。

  找到 id 是 xxxxx 的程序，比如我出错的是grammar for ginger 程序。

  原来我每次访问后端 localhost:3000 的时候都跑到这来了！！ 怪不得页面啥反应也没有，控制台返回 DevTools failed to load SourceMap， 真是哭笑不得。。。

  那就先把它卸载了吧。

  ![image-20201216190243433](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201216190243433.png)

![image-20201216190432723](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201216190432723.png)

## 数据库

#### 安装

![image-20201205005239165](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205005239165.png)

brew update

brew install postgresql

安装成功之后，安装路径为：

/usr/local/var/postgres

可以 pull 最新版

```
brew postgresql-upgrade-database
```



#### 启动

brew services start postgresql

![image-20201205005554035](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205005554035.png)



createdb "smartbrain"



#### 连接

下载可视化图形操作界面 Psequel。

用 Psequel 连接数据库。

如果在连接本地的时候出问题，按以下步骤操作。

```text
# 1 找到它的地址
sudo find / -name "postgresql.conf"
/usr/local/var/postgres/postgresql.conf
# 2
vim postgresql.conf
把
listen_addresses = 'localhost'
替换成
listen_addresses = '*'
# 3
重启服务器
brew services restart postgresql
# 查看
netstat -nlt
“Local Address” for port 5432 变成了 0.0.0.0.
```



创建表

```
CREATE TABLE users(
    id serial PRIMARY KEY,
    name VARCHAR(100),
    email text UNIQUE NOT NULL,
    entries BIGINT DEFAULT 0,
    joined TIMESTAMP NOT NULL
);

CREATE TABLE login(
    id serial PRIMARY KEY,
    hash VARCHAR(100),
    email text UNIQUE NOT NULL
    );
```



这有详细的 postpresql 语法教程 https://www.postgresqltutorial.com/







#### knex

Js 操作数据库

```
Knex.js (pronounced /kəˈnɛks/) is a "batteries included" SQL query builder for Postgres, MSSQL, MySQL, MariaDB, SQLite3, Oracle, and Amazon Redshift designed to be flexible, portable, and fun to use. It features both traditional node style callbacks as well as a promise interface for cleaner async flow control, a stream interface, full featured query and schema builders, transaction support (with savepoints), connection pooling and standardized responses between different query clients and dialects.
```



Npm install knex

npm install pg

具体语法在官网里有。











## 部署

这样全世界都能访问啦

#### 工具选择

可以有以下选择，在此使用heroku。

![image-20201205152950920](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205152950920.png)

heroku 部分如果有啥不懂的，直接看官方文档 https://devcenter.heroku.com/articles/git

https://devcenter.heroku.com/articles/heroku-cli

【心得: heroku 官网写的还不错的， 能看一手的资料，就不要看 二三手经过加工的资料💾。】

### 部署后端

#### heroku 创建app

```term
brew tap heroku/brew && brew install heroku
```

Heroku login -i

到我的后端文件夹（smart-brain-api) 

Heroku create

```
Creating app... done, ⬢ damp-eyrie-02199
https://damp-eyrie-02199.herokuapp.com/ | https://git.heroku.com/damp-eyrie-02199.git
```

![image-20201205164210960](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205164210960.png)



Tips:

建议新建好就改成自己想要的名字。

不然的话，建立好的连接又要删掉重新建立。也不是不行，就是怕好不容易搞起来的连接，万一又出什么幺蛾子连不上了。

改名方法1:

```
1.打开终端，登录heroku，切换到要修该的app下
2.执行heroku apps:rename xxx (xxx是你的app的新名称）
3.更新remote，
执行git remote rm heroku
执行heroku git:remote -a xxx
```

改名方法2:

```
1.打开heroku的网址，并登录
2.找到你要修改的app的名字，点击进入
3.点击setting
4.修改名字（只支持小写字母、数字和破折号）
5.更新remote
（1）打开终端iterm
（2）输入heroku login, 输入email和password进行登录
（3）执行git remote rm heroku
git remote -v
git remote add heroku https://git.heroku.com/XXXXX.git（XXXXX请换成修改后的heroku名字）
git push heroku 你的最新分支:master
```







Heroku open -a app名字，如damp-eyrie-02199

![image-20201205172459961](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205172459961.png)

成功打开～



#### 与 GitHub 建立连接

在github 新建一个仓库 smart-brain

还是在我的后端文件夹（smart-brain-api) 

```
git init
git add .
git commit -m "smart"

git remote add origin https://github.com/wenjialu/smart-brain.git
git push --set-upstream origin master
```

GitHub 联系上heroku？

```
heroku git:remote -a 刚刚创的app名字，如damp-eyrie-02199
```

##### 连接成功～

git remote -v 查看下，如果heroku 和 github 的仓库都对了就是成功啦～

![image-20201205174240762](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205174240762.png)





我之前用过heroku了，现在要重命名



```term
 heroku git:remote -a infinite-hamlet-23870
 git remote rename heroku heroku-staging
```



#### 上传到heroku

上传到heroku 啦，见下：

##### [Deploying code][2]

To deploy your app to Heroku, you typically use the `git push` command to push the code from your local repository’s [master or main branch][3] to your `heroku` remote, like so:

```term
# mastet 或者 main 要根据实际情况
$ git push heroku master
Initializing repository, done.
updating 'refs/heads/master'
...
```



#### 查看成功了没

```term
heroku open
```

检查出错原因

```
heroku logs --tail
```

通过日志我发现 heroku会为我的app设置动态端口（Heroku dynamically assigns your app a port），我要把写死的3000 端口改掉。

Heroku adds the port to the env, so you can pull it from there. Switch my listen to this:

```js
.listen(process.env.PORT || 3000)
```

在后端文件夹路径下，终端输入

```
PORT=5000 node server.js
```

可以看到返回5000 啦



修改完了别忘记上传到git 和 heroku

![image-20201205182211270](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205182211270.png)

哈哈哈哈～ 成功了哦～



### 部署前端

用同样的方法部署前端。

![image-20201205192330279](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201205192330279.png)

**修复bug**

提示1: node 和npm version 需要指定

```
node --version
v12.13.1
npm -v
```

在 package.json里面添加

```
"engines": {
    "node": "12.13.1",
    "npm": "6.14.8"
  },
```



提示2: 依赖中缺少模块

Tachyon 模块缺少。

打开它给出的官方网址，找到了解决方案

https://devcenter.heroku.com/articles/troubleshooting-node-deploys

https://help.heroku.com/n/13/general-platform-features/builds

```
# 重要！！
In order create a smaller slug size for apps, the buildpack will prune out the devDependencies from the package.json at the end of the build, so that the slug will only include the dependencies that are listed at runtime. If there is a dependency that is in the devDependencies that is needed after the prune occurs, 

move the dependency to dependencies, so it is not removed.
or 
heroku config:set NPM_CONFIG_PRODUCTION=false
```



不知道为啥 Tachyon 模块不见了。

⚠️ 我打开本地 package.json 好多模块竟然都显示没有安装，非常奇怪。

只能全部重新安装

```
npm install tachyon 

npm shrinkwrap
产生了一个新的lockfile, 然后再提交。 (亲测了两个项目都不用lock反而成功了，所以把这个 lockfile 删光光，重新提交git，就成功啦)
```



装好后在dependencies 里面可以看到它又出现啦。

![image-20201206001834886](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201206001834886.png)





```
npm install package-lock.json

heroku buildpacks:set heroku/nodejs

npm shrinkwrap
```



​	

修复 npm 和 yarn 的冲突。

```
git rm yarn.lock
git commit -m "Remove yarn lock file"
git push --set-upstream origin master
```

如果 git 分支出了问题，见我 git 篇更新部分 https://zhuanlan.zhihu.com/p/290333697

```
# 注意！ 根据项目的实际情况选择 main，master
git push heroku main
```

```
remote:  ! We have detected that you have triggered a build from source code with version ed530c8d0de5c0f450d117595c65a054dfc9cbe8
remote:  ! at least twice. One common cause of this behavior is attempting to deploy code from a different branch.
remote:  !
remote:  ! If you are developing on a branch and deploying via git you must run:
remote:  !
remote:  !     git push heroku <branchname>:main
remote:  !
remote:  ! This article goes into details on the behavior:
remote:  !   https://devcenter.heroku.com/articles/duplicate-build-version
```

```
git push heroku main:main
```





### 部署数据库








[1]:	https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
[2]:	https://devcenter.heroku.com/articles/git#deploying-code
[3]:	https://devcenter.heroku.com/articles/git-branches
