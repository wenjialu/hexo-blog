---
title: SpringBoot 数据库操作指南
date: 2021-1-20
tags: SpringBoot
---



[toc]



## SpringBoot 连接数据库

### 原理

Spring中，数据从数据库到前端的流程：

**Jdbc配置（pom.xml，application.yml，cofig工程中：cof-dev.yal: spring.datasource:url?useSSL=false  usrname password ） -->   ~~model~~  HandlerMapping  --> service(interface -> impl(继承接口+@Override方法 ： 调用RoundDictExample（增删改查，验证）)) --> controller --> @responsebodey (包装响应 ResultStatus设置响应) --> [swagger(/v2/api-docs)] 怎么返回实体类？？？--> 前端**

ps. 我后期会画波脑图来更详细精确的分析。

Controller --> service --> mapper



 其中：  DispatcherServlet收到请求调用HandlerMapping处理器映射器。

 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。

```
  model （HandlerMapping ）

					 -- domain: RoundDictExample (连接sql的增删改查，验证等功能,  里面的id统统要改成dict_id 或 dict_uuid); 

          --persistence:  RoundDictMapper.xml,   RoundDictMapper (建立数据库与java代码的映射)

```



【可以对照着这张图来康】

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn0cadg3yjj30jn0a2gq6.jpg)

### Tips:

- 本地远程连接数据库
  注意: 使用远程连接时,使用的连接用户和该用户现在的ip地址应该是远程数据库中允许的用户和允许的ip,否则是不允许连接的.
  - `终端：mysql -Pxx -hxxx -uroot -p`
  - 图形化界面：Navicat / SQLyog



### 测试：

#### **原理**

理清楚从发送请求到数据被处理的流程：

发送请求 --> DispatcherServlet （--doDispatc）-----。。。controller --> **responseMessage类** (各种方法)--->

  controller --> coreController  (各种方法)

```java
... 省略部分代码....
// Actually invoke the handler.
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
... 省略部分代码
```

Spring MVC会根据请求URL的不同，配置的RequestMapping的不同，为请求匹配不同的HandlerAdapter。

比如对于请求地址：http://127.0.0.1:8888/hello/test1?id=98匹配到的HandlerAdapter是HttpRequestHandlerAdapter。

我们直接进入到HttpRequestHandlerAdapter中看下这个类的handle方法。

```java
Copy@Override
@Nullable
public ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler)
    throws Exception {
    ((HttpRequestHandler) handler).handleRequest(request, response);
    return null;
}
```

#### 工具

##### postman

##### 吹爆 Swagger 超好用

List： 返回的是分页的结果！！！！！！

action可能在父类里面，娃娃它的父类。

**getDocumentation()**方法，发现其实就是返回了一个Swagger对象（参数信息都存在Swagger对象的definitions属性里

 [org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping] 

- Mapped "{[/m/add],methods=[POST]}" onto public com.jj.api.vo.ResponseMessage<java.lang.Integer> com.jj.red.packet.user.admin.controller.RoundDictController.add(com.jj.red.packet.user.mybatis.domain.RoundDict)

- .定义统一返回数据格式

  - ```
    public class ResponseFormat {
    
        private static Map<Integer,String> messageMap = Maps.newHashMap();
        //初始化状态码与文字说明
        static {
            /* 成功状态码 */
            messageMap.put(200, "成功");
    
            /* 服务器错误 */
            messageMap.put(1000,"服务器错误");
    
            /* 参数错误：10001-19999 */
    ```



​	![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn1lh25fhlj30o10gbtb1.jpg)









####  问题：

##### 访问获得404 

* 根据原理可知：我调用的接口不存在/没找到，Spring直接将Response的errorStatus状态设置成1，将http响应码设置为500或者404。 -- 继承BasicErrorController 就可以覆盖原有的错误处理方式。

* 根据报错也可以验证：

  ```
  java.lang.IllegalArgumentException: Invalid character found in the request target. The valid characters are defined in RFC 7230 and RFC 3986
          at org.apache.coyote.http11.Http11InputBuffer.parseRequestLine(Http11InputBuffer.java:479)
          at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:684)
  ```

  可能性：

1 TestCase测试Impl类的方法没有问题，但是Swgger和前端调用接口就会报404错误：

这是因为我之前把接口的注解@RestController 改成了@Controller。

改回来就好了。

```
import org.springframework.web.bind.annotation.RestController;
```

**@RestController**  （相当于@Controller + @ResponseBody 后者是方法级的注解）

tips：1 如果使用@RestResultController，如果返回值是String类型就存在指向性问题，返回String类型，指向的地址是String字符串的地址，因此前端http访问我的接口会报404.

​          2 也可以尝试在接收参数里加上 HttpServletResponse response

2

可能是jar包的问题

Artificial -- lib

  新手在刚接触springboot的时候，可能会出现访问请求404的情况，代码怎么看都是对的，但就是404。

  在十分确定代码没问题的时候，可以看下自己的包是不是出问题了，什么意思么？

  答案：SpringBoot 注解 @SpringBootApplication 默认扫描当前类的同包以及子包下的类； 如：启动程序在包名  com.yang.test.ymkribbonconsumer下，则会查找所有 com.yang.test.ymkribbonconsumer下的文件以及 com.yang.test.ymkribbonconsumer 下的所有子包里面的文件。

* x

  * xxMapper.xml 里面应该写的表名而不是数据库名。

* ~~JDBC Connection XXX will not be managed by Spring~~和SqlSession was not registered for synchronization because synchronization is not active

  意思是相关操作没有被事务管理起来，虽不影响项目功能正常使用，可每次启动看到这堆警告，导致强迫证又犯了，有一种非除之而后快件的冲动，于是检查配置文件中事务配置的部分，又上网搜索，经过一番测试，终于消除这些警告，下面是解决的过程。

  可看 https://www.codenong.com/cs106354150/

在db.properties里配置的username和passowrd是以前的一个项目的账户，那个账户的权限只有一个sms数据库，而那个账户没有我当前测试的xzl数据库的权限。

## 

新建了一个有当前测试库权限的数据库用户，或者直接用root来测试。
链接：https://www.jianshu.com/p/259ce2ec91d3

##### 发出请求：连接数据库，响应：连接成功啦，却返回null，而没有返回实体类。

## 



* Error parsing HTTP request header

```
o.apache.coyote.http11.Http11Processor:Error parsing HTTP request header



Note: further occurrences of HTTP request parsing errors will be logged at DEBUG level.



 



java.lang.IllegalArgumentException: Invalid character found in the request target. 



The valid characters are defined in RFC 7230 and RFC 3986
```

目前，可以确定的是，该报错是由于url中有非法字符导致的，解决方法有

> - 请求url中别有奇奇怪怪的字符，老老实实不好吗 使用了不安全字符，他们直接放在Url中的时候，可能会引起解析程序的歧义。如
>
>   ```
>   { } | \ ^ [ ] ` ~ % # 等
>   ```
>
> - 使用tomcat7.0.69以前的版本，这些版本不对请求头进行检验
>
> - 不想降低tomcat版本？那就对特殊字符进行转义，嗯确实有点麻烦
>
> - if you are not accidentally requesting with HTTPS protocol instead of HTTP protocol. If you don't configure SSL on Tomcat and you send HTTPS request, it will result to this weird message..

## SpringMVC 架构

### what：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn0b9z0016j30jm0d5n1u.jpg)



**M：model**

jojo 建数据库

dao (vo value object 拆的更细的的实体类)增删改查

servive

**V：view**

**C：controller**

接收用户请求，交给模型处理，把模型处理完的数据返回给视图。

一般来说是java类





后端项目结构主要是MC，即 Model + Controller。

举例：

<img src="https://tva1.sinaimg.cn/large/008eGmZEgy1gmyudci33oj307b05dq3l.jpg"  />

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn0am3mgujj306806mmxj.jpg)

admin文件夹: 管理员用户 - VO  + -Controller

整个文件夹：-( mybatis + service ) + -Controller







### how：

Spring的核心：servlet

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn0cadg3yjj30jn0a2gq6.jpg)

只有红框部分需要自己写，其他spring都帮忙做掉了。

234 找请求url对应的类处理器

5678 找并返回了数据库的数据。

91011 拿到数据库数据，拼接url，去找对应的前端视图。

```
SpringMVC流程：
1、 用户发送请求至前端控制器 DispatcherServlet。
2、 DispatcherServlet收到请求调用HandlerMapping处理器映射器。
3、 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。
4、 DispatcherServlet调用HandlerAdapter处理器适配器。
5、 HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。
6、 Controller执行完成返回ModelAndView。
7、 HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。
8、 DispatcherServlet将ModelAndView传给ViewReslover视图解析器。
9、 ViewReslover解析后返回具体View。
10、DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。
11、 DispatcherServlet响应用户。
```







====

Selevlet

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn0be8se1mj30gd0e277r.jpg)

dispatcherservlet

![](/Users/lujiawen/Library/Application Support/typora-user-images/image-20210125223734010.png)

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn0bidh5swj30ov0c0q8d.jpg)



鼻祖：selvelet

实现 xxx（request, response)

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn0bk36cdoj30pd0bc44a.jpg)



资源：

DispatcherServlet : https://blog.csdn.net/xyx_hfut/article/details/104914200 ⭐️

Spring自动注解原理：https://juejin.cn/post/6844904009577267207

Spring后端到数据库操作：https://segmentfault.com/a/1190000010208184

Springboot集成Mybatis全流程: https://blog.csdn.net/weixin_42322648/article/details/105063601?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-2&spm=1001.2101.3001.4242

从404讲起发送请求返回请求的过程：https://www.cnblogs.com/54chensongxia/p/14007696.html ⭐️

404注解： https://blog.csdn.net/weixin_40160720/article/details/86601556

写不好就会引起 404 的注解们的剖析大全： https://juejin.cn/post/6844903893734785037 ⭐️

uuid 设为long会带来什么问题： https://www.jianshu.com/p/d7d63696eb89

Swagger: https://www.codenong.com/cs107102063/