---
title: React 简易教程
date: 2020-12-09 02:35:48
tags: react
---
先做 react 介绍

亮点 和 局限。
优化：
PureComponent 是优化 React 应用程序最重要的方法之一，易于实施，只要把继承类从 Component 换成 PureComponent 即可，可以减少不必要的 render 操作的次数，从而提高性能，而且可以少写 shouldComponentUpdate 函数，节省了点代码。

和 vue区别



然后案例 smartbrain

# react 简易教程

Component : 不仅包含html css 还有功能，可以之后被复用

就像搭乐高一样搭 Component

自上而下渲染，一个Component 节点修改，只有他孩子节点知道，父母节点不会知道。



具体啥prop在主js里设定，

组件js 的函数里面，prop只是个穿传去的参数 （所以具体prop是啥不知道，他只是个参数啊。具体的在主js里面定义），里面调用 this.prop.prop名



export

但凡要被import的文件 都要export



## Lifecycle hooks 

https://reactjs.org/docs/react-component.html

component 

every time  auto triggered

### mounting

- [**constructor()**](https://reactjs.org/docs/react-component.html#constructor)
- [`static getDerivedStateFromProps()`](https://reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
- [**render()**](https://reactjs.org/docs/react-component.html#render)
- [**componentDidMount()**](https://reactjs.org/docs/react-component.html#componentdidmount)
- render()

### updating

### Unmounting

## Chirdren

<scroll>

 内部的内容就是它的chirldren

</scroll>

```
function Scroll( props ) {
    return (
     <div style ={ {overflowY:"scroll", border: "1px solid black", height: "500px"}   }>
        {props.children}
     </div>
    )
  };
```



## 代码框架

```javascript
class App extends Component{
		// 设置状态初始值（创参）
     constructor (){
				super();
    		this.state = initialStatexxx;
     }

    // 给状态赋值的函数 (形参)
    // 都是函数, 设定完了之后在组件里面使用 （用参），最后在 render 里面被调用 （实参）
    onRouteChange = () =>{
      this.setState({route: route});
    }

    onXXX = () =>{
      
    }
    
render(){
  const {route, ...} = this.state;
  return(
  )
}
  
}

export default App;
```



