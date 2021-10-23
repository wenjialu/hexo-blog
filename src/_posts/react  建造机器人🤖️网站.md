---
title: 【手把手教学】用前端 react 来做一个机器人🤖️网站
date: 2021-12-13 02:35:49
tags: fontend
---

# 【手把手教学】用前端 react 来做一个机器人🤖️网站

## 先来看下最后的成果～～





## 安装

1. 先用 npm 安装 create-react-app 

   （ps. npm 是个很好用的前端包管理器。没有安装 npm 的小伙伴可以通过这个链接 https://www.npmjs.com/get-npm 下载）

2. 用 create-react-app 创建我们的机器人小项目，起名为 robofriends 。

   （这一步react会帮我们装很多东西，比如依赖包啊什么的，可能要花点时间, 请耐心等待⌛️）

# ![image-20201114143926433](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201114143926433.png)

###  装好了之后的界面是这样的 Happy hacking~

![image-20201114144237710](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201114144237710.png)

### 在终端：进入到刚刚装好的 robofriend 文件夹，可以看到相关依赖都装好啦～～



## 启动

桌面上用你喜欢的idea打开整个文件夹，我用的是vscode（打开vscode直接拖动文件夹进去就好了）。

打开pakeage.json ， 

看到 ”start“ 对应的就是“ react-scripts start” 启动脚本。

所以 ， 在终端运行 npm start 就能启动整个项目啦。

![image-20201114144822871](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201114144822871.png)

如果自动跳转到 localhost:3000 （见下图）

就是启动成功啦～

![image-20201114165308860](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201114165308860.png)



## 项目结构初探

来继续通过idea来看这个最初始的项目文件夹，

node_modules是自动帮我们下好的包，先不用去关心它。

public 里面有index.html，

src （source）文件夹就是一切 “魔法” 发生的地方啦，

我们会把最重要的js 还有 css 文件等放在这个文件夹里。

*打开里面的 index.js。*

*ReactDom 这行代码 渲染了组件或者任何你写的东东到你指定的Dom上。*

*在这里我们渲染的是id为root的dom，可以去index.html里看，这是我们主体部分唯一的dom节点。*



*![image-20201114170054769](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201114170054769.png)*



## 写模块

#### Demo -- 先做个小测试 ～

**这里先介绍个好玩的机器人api**

Robohash https://robohash.org/

在这个网站 url 中，输入任意字符，就能帮你随机生成一个小机器人，这个想法是不是超级可爱呢！！！

好啦， 我们的网站就要利用到这个功能，来生成我们自己的机器人小卡片。



首先，在 src 文件夹里新建 Card.js 文件, 把这个网站随机生成的图片 url 放到代码里。

```js
import React from 'react';

function Card() {
  return (
    <div className="Card">
        <p>Hello, I am a Robot</p>
        <img src="https://robohash.org/irina" alt="robots" /> 
    </div>
  );
}

export default Card;
```



在 index.js 中，导入 Robot.js, 并且把  <App /> 替换为 <Card />

```javascript
// 新增以及替换的代码
import Card from './Card';

ReactDOM.render(
  <React.StrictMode>
    <Card />
  </React.StrictMode>,
  document.getElementById('root')
);

```

刷新下我们的本地页面，哈哈 机器人就出现和你打招呼啦🙋。

测试成功～

![image-20201114222806928](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201114222806928.png)



#### 机器人卡片制作

1 首先，可以在 Card.js 中美化下卡片的 css 样式。 

先安装一个 依赖包

```bash
npm install tachyons
```

在 Card.js 中通过 className 调用它的样式～ 

``` javascript
import "tachyons"
<div className="bg-light-green dib br3 pa3 na2 grow bw2 shadow-5">  
```

2 然后做很多卡片：

因为  <Card /> 就是一个 component 了， 所以可以不断复用它。

直接复制就好了。

3 但是所有卡片里面的机器人名字和图像都是一模一样的， 我想让里面的信息是不一样的。

于是制作机器人对象：

用一个常量 robot 来存所有的机器人的信息。

![image-20201115022317648](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201115022317648.png)

在 index.js 中依次取出他们的信息 当作参数。

```javascript
import {robots} from './robots';
ReactDOM.render(
  <React.StrictMode>
    {/* <App /> */}
    <Card id = {robots[0].id} name={robots[0].name} email ={robots[0].email}/>
    <Card id = {robots[1].id} name={robots[1].name} email ={robots[1].email}/>
    <Card id = {robots[2].id} name={robots[2].name} email ={robots[2].email}/>
    <Card id = {robots[3].id} name={robots[3].name} email ={robots[3].email}/>
    <Card id = {robots[4].id} name={robots[4].name} email ={robots[4].email}/>
  </React.StrictMode>,
  document.getElementById('root')
);
```



在 Card.js 中传入指定好的参数

```javascript
function Card(props) {
  return (
    <div className="bg-light-green">  
    {/* dib br3 pa3 na2 grow bw2 shadow-5*/}
        <img src="https://robohash.org/irina?200*200" alt="robots" /> 
        <div>
          <h2>{props.name}</h2>
          <p>{props.email}</p>
        </div>
    </div>
  );
}
```

哈哈，别忘了把它们的样子也改一下。

```javascript
 <img src={`https://robohash.org/${props.id}?200x200`} alt="robots" /> 
```

tips: 1. ⚠️注意这里要把引号改为 ` 哦！！！

​         2. 所有的 js expression， 比如 props.name， 都要加大括号。

* Js experssion

* ```javascript
  const cardsArray = robots.map( 
          (uesr, i) => 
          {return <Card key={i} id = {robots[i].id} name={robots[i].name} email ={robots[i].email}/>
      })
  // cardsArray, i 都是 Js experssion 要加大括号。
  // 换句话说， 要想在 react 里面写 js 只要加上大括号就行了。    
      
  ```

  

  酱酱！完成后就是下面👇这个样子，是不是还蛮酷的内～

  至此，我们的机器人小网站的雏形就有啦。

  ![image-20201116001159587](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201116001159587.png)



#### 实现搜索功能

*动态可变的State  --> props*

*模块长这样：*

*app*

*cardList.   searchBox*

*Card*



##### 思路：

要让 searchBox 和 cardList 能通过父节点互动。

可以通过 React 中的 State 来实现：

State 的动态可以被当作props 传送到 searchBox 和 cardList 中。

这里可以设置 2 个state，一个是搜索关键词，起名为searchfield， 一个是被搜到的 robots，起名为 robots 。

​	

##### 实现：

searchBox 传一个参数 searchChange

```
function SearchBox(searchChange) {
    return (
     <div className="pa2">
     <input 
     className = "pa3 ba b--green bg-lightest-blue"
     type="search" placeholder="search robots"
     onChange={searchChange}/>
     </div>   
    );
  }
```



父节点 app 是传送的枢纽，用 event 来监控输入 searchBox 的文字，

```
class App extends Component {
  {/* constructor是一个mounting方法 */}
  constructor(){
    super()
    this.state = {
      robots : robots,
      searchfield: ""
    }
  }

  // 这里一定要用箭头函数，因为这个是在Searchbox被调用的，不这样写会找不到this
  onSearchChange = (event ) => {
     this.setState({ searchfield: event.target.value)   })
  }

  render(){
    const { robots, searchfield } = this.state
    return (
      <div className="tc"> 
      {/* textcenter */}
      <h1>RoboFriends</h1>
      < SearchBox searchChange={this.onSearchChange}/>
      {/* state 传到这变 props 作为{robots} 传到 CardList*/}
      <CardList robots={robots}/>
      </div>
    );
  }
}
  
```

继续改进这个传送机制，让父节点 app 传送刚刚接收到的信息到 cardList

```
 render(){
    const { robots, searchfield } = this.state
    const filteredRobots = robots.filter( robots =>{
      return robots.name.toLowerCase().includes(searchfield.toLowerCase())
   })
    return (
      <div className="tc"> 
      {/* textcenter */}
      <h1>RoboFriends</h1>
      < SearchBox searchChange={this.onSearchChange}/>
      {/* state 传到这变 props 作为{robots} 传到 CardList*/}
      <CardList robots={filteredRobots}/>
      </div>
    );
  }
```



##### 新增功能：下拉的时候搜索框也悬浮在顶端

Index.js

```
<scroll>

 {*/ scroll 内部的内容就是它的chirldren */}
 <CardList robots={filteredRobots}/>
 
</scroll>

```

Scroll.js

```
function Scroll( props ) {
    return (

     <div style ={ {overflowY:"scroll", border: "1px solid black", height: "500px"}   }>
        {props.children}
     </div>
    )
  };
```



#### 最后再美化下整个网站

##### 网站背景

Index.css

```
body {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  background: linear-gradient(to left, rgba(7, 27, 82, 1) 8%, rgba(8, 128, 128, 1) 100%);
}
```



##### 字体

Google 搜索自己想要的酷酷的字体。

下载它的 @font face (web) 版本，

 把他的css  复制到 app.css中，woff 文件则拖到我们的 src 文件夹中 。



![image-20201116165810190](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201116165810190.png)





### 动态请求

如果要在现实世界中运行我们的网站，这些机器人的信息不应该直接写在我们网站中的。

而是由真实的用户填写，浏览器发出请求而被我们所接收和响应。



现在我们可以用一个免费的API接口一个网站叫做 https://jsonplaceholder.typicode.com/

把我们的用户数据放在这个网站上，有我们来抓取模拟浏览器请求。



```
  // 1. 把robot.js 信息删掉， 导入文件的语句也要删掉
  // 2. state 中接受的robot 设为空
  // 3. 通过 fetch url 来得到数据
  constructor(){
    super()
    this.state = {
      robots : [],
      searchfield: ""
    }}

  componentDidMount(){
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(response =>{ response.json()
      .then(users => this.setState({ robots:users}));
      })
  }
```



在获取 url 数据的等待时间，页面什么也不返回。

如果数据量大，查找时间很长的话，会让用户很困惑。

所以在未获取数据的时候，可以提示正在加载 “Loading” 中。

```
render(){
    const filteredRobots = this.state.robots.filter( robots =>{
      return robots.name.toLowerCase().includes(this.state.searchfield.toLowerCase())
   })
   if (this.state.robots.length === 0){
     return <h1>Loading</h1>
   } else{
    return (
      <div className="tc"> 
      {/* textcenter */}
      <h1 className="f1">RoboFriends</h1>
      < SearchBox searchChange={this.onSearchChange}/>
      {/* state 传到这变 props 作为{robots} 传到 CardList*/}
      <CardList robots={filteredRobots}/>
      </div>
    );
   } 
  }
```



### 可以发布啦！！

step 1. npm run-script build

react 会帮自动打包出一个可被用于生产环境的最优文件夹 build。

![image-20201116174504858](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201116174504858.png)

step 2. 把他们放到 HTTP 服务器 或者 github 中。

比如 tomcat 或者 github page。

（这个还可以单开个教程，如果大家感兴趣的话可能后续也会写写233）







## ps. 

### 关于 create-react-app

本教程使用的是截止 2020.11.15 create-react-app 的最新版本。

如果是以前装了旧版本的宝宝想要更新：

1 在 package.json 中把 react-scripts 版本改到最新 或者和我一样的“4.0.0”。

2 在终端 npm install

就更新好了。

对新版本感兴趣的宝宝还可以去官网看看。

![image-20201114171737074](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201114171737074.png)



### 

