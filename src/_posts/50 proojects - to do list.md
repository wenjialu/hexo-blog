---
title: 50 projects - to do list
date: 2021-02-03 02:35:49
tag: 50 projects 
---



![效果展示](https://raw.githubusercontent.com/wenjialu/todolist-app/main/todo-list-demo1.gif)





## to do list 

### 静态样式及代码：

<img src="https://tva1.sinaimg.cn/large/008eGmZEgy1gmzxxii2xrj30k00ck0v6.jpg" />





### 动态功能：

**知识点预览：**

 [classList](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/classList) 的方法 add， toggle 等

右键 -- 检查 -- application -- storage -- local storage

[Localstorage](https://developers.google.com/web/tools/chrome-devtools/storage/localstorage?utm_source=devtools) 的方法setItem, getItem.

Local storage.setItem(key, value)



js ：

#### **功能1: ADD**

填写todo，回车后会添加到我的待办列表里面。



**实现思路：**代办列表是个取名为 todosUL 的列表，

只有document 的 element 才能被append进去, 而 text 不能被append到  todosUL 这个列表中。

所以要创造一个 element，并赋值为 input.value。

tips: 1.要先用一个变量 todoText 存储  input.value, 对其做个简单的判断防止其为空。

2. todo.text todotext 

    todotext 不一定是当下输入的，也可能是之前输入的todo.text.



 1 what is  todo.completed?

~~2 why **if(todo && todo.completed)**{  。。 classList.add(。。）} 才能生效啊 。A：不，不是这样的~~

**答疑：**关于 classList.add("completed")。

在这个功能中不是必要的。

这只是打个标签，

后续实现保存列表内容的功能时，会依据这个标签做出相应的方法。





```
# 伪代码

listen submit? 

adddTodo(?){
  #输入的文字
  let todoText = input.value

   # 1.what is ? ?  2. what is classList.add? 3 why set innerText
   if(todoText){
          const todoEL = document.createElement("li")
          
          //if( ？ && ？.completed){
          //    todoEL.classList.add("completed")
          //}
          
           todoEL.innerText = todoText

            todosUL.appendChild(todoEL)
            console.log(todosUL)
            
            todo.value = ""

      }

}

```



#### **功能2: MARK COMPLETE & REMOVE**

MARK COMPLET: 鼠标左击，待办就用（下划线划掉的方式）标记为完成

REMOVE: 鼠标右击，待办从列表中消失

```
todoEL.addEventListener("click",()=>todoEL.classList.toggle("completed"))

todoEL.addEventListener("contextmenu",(e)=>{
	e.preventDefault()
	todoEL.remove()
})

```





####  **功能3： 刷新待办不会消失**



思路： 对于上述3种操作：增删改，每次操作都记录在 localstorage本地存储 中。

写一个 updateLS() 函数实现保存功能，并在每个方法后触发。

Q: 我已经写入到local storage了 ， 为啥还是一刷新就没了

猜想；setitem 写对，getitem写错？

```
// change string to array
const todos = JSON.parse(localStorage.getItem("todos"))

if (todos){
    todos.forEach(todo => addToDo(todo))
}


方法1/2/3{
 ...
 updateLS()
}



function updateLS(){
        todosEL = document.querySelectorAll("li")
        
        todosEL.forEach(todoEL =>{
            todos.push({
                text: todoEL.innerText,
                completed: todoEL.classList.contains("completed")
            })
        })
         // todos is an object, change it to string
        localStorage.setItem("todos",JSON.stringify(todos))
    }

```



#### 知识点总结：

Localstorage 的方法setItem, getItem.

每次增删改触发存储方法，把数据放到Localstorage；然后再从Localstorage里取出来的数据基础上，实现增删改



**Resources :**

Cdnjs ->font-awesome

Fonts.google.com

