---
title: 【教程】Mac 怎么启动第一个 SpringBoot项目
date: 2021-1-22 02:35:49
tags: SpringBoot
---







### 背景知识：

###  Spring -Maven - SpringBoot

一句话简单讲述三者关系： " The quickest way to create a Spring application is to use a [Spring Boot](https://spring.io/projects/spring-boot) Maven project."

--- 官方教程指路： https://www.jetbrains.com/help/idea/2020.3/your-first-spring-application.html

#### Maven

What：包管理器

**tips: maven 配置里面有个奇天大坑**，不是为了下载依赖快一点改成国内的镜像了吗，结果老是出错也不知道是为啥。

折腾好几天才想起来，我一直是翻着墙的。。。 干啥子要用国外的站点下国内的资源嘛！ 要么改回原来镜像，要么关掉梯子。

果然，

变得又快又好了。

【第一次打开一个项目的时候，要下很多依赖会有点慢。但是之后打开就很快了。】

#### Spring 

What: 万能框架


官方文档（https://spring.io/projects/spring-boot）

**原则：约定大于配置**

比如：

文件摆放位置；

application.yaml (必须是这个名字) 配置可以放在一下位置，
执行优先级依次是：
项目路径./ config文件夹/application.yaml
项目路径./application.yaml
classpath :/config文件夹/application.yaml
classpath :/application.yaml

场外提示： classpath =类路径=src-main-java/resource 下面都算





### 新建项目

IDEA -- spring initializr

### 三种启动方法

**在这三种启动方法前，如果你同时导入的是多个项目，并且maven没有出现的话。需找到每个项目的pom.xml，右键，add as a maven project.**

https://blog.csdn.net/weixin_34296641/article/details/94171666?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control
前两种方法供本地运行，第三种是线上部署使用
**方法1: 直接点绿色箭头运行**

最关键就是要先 mvn package， 再点运行

**方法2**: 应用的根目录下运行

```java
mvn -U clean spring-boot:run
```

(没有自动创建也可以用这个手动创建)

**方法3: 使用mvn install 生成jar后运行**
先到项目根目录

```
mvn compile
mvn package
java -jar target/xxx.jar
https://spring.io/guides/gs/maven/#initial
```



**1 mvn  clean install**
在有的pom路径下
mvn  clean install
（This will clean whatever created from the previous build, then build your project and add the jars to your local maven repository ）
就会帮忙安装pom里的依赖

Maven projects are defined with an XML file named *pom.xml*. Among other things, this file gives the project’s name, version, and dependencies that it has on external libraries.

**2 cd target**
**3 java -jar   xxxx.jar**
tips: 使用maven工具打包（jar包），打包时要注意，由于模块与模块之间的依赖关系，所以打包是要有顺序的，需要先打被依赖的模块；你也可以在父类模块上打一个包。

其他配置 (可选)
perference -- maven -importing -勾选 自动下载 sources

### 启动别人的项目
https://blog.csdn.net/gao_xiao_qi/article/details/99707592
https://www.pianshen.com/article/16541114992/
要**import** 而不是 open：
file-new-from existing project-maven

**配置：**
**Crtl+Alt+s 快捷键打开idea设置，并搜索maven。**

**里面的setting 还有仓库要改成自己的位置啊！**





**贴心避坑指南总结：**

1.找不到classpath下的resources ：

首先，需要把 java 文件夹右键 - 标成蓝色，resources - 标成绿色。 （或者在 file -- project structure 改动）

其次，每个pom.xml 都要右键 - **add as a maven project.** 不过要是右键没有的话也不用担心，这说明已经是maven项目了。

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn2eza21efj30ad0gzq4v.jpg)



2  启动顺序很重要 ！！！！！

如果一个项目中，有多个子项目，那么要注意启动顺序。

因为有的项目的依赖需要在其他项目编译完之后才能拥有。

一般启动顺序如下：

1注册中心eureka  2网关 gateway

3配置中心config   4 api  5服务中心 user

依次启动 round-table-eureka-server 、round-table-config-server、round-table-user-server

![image-20210119235350856](/Users/lujiawen/Library/Application Support/typora-user-images/image-20210119235350856.png)







3 现在和大家提醒一个很大的坑。我这次所有的子项目都有一个父项目spring-boot-starter-parent。

明明pom中导入了依赖，本地也有这个包，但是为什么会提示找不到呢。

注意：仔细看下pom中，包的地址是相对路径！！！！

不懂 relativePath 指的是哪个仓库的话，在idea中点击进去看源码。

可知：它的默认值是.../pom/xml

```
<parent>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-parent</artifactId>
   <version>2.0.4.RELEASE</version>
   <relativePath/> <!-- lookup parent from repository -->
</parent>
```

在resources下面新建lib文件夹，并把jar包文件放到这个目录下 .${project.basedir}只是一个系统自己的常量

<systemPath>${project.basedir}/src/main/resources/lib/alicom-mns-receive-sdk-1.0.0.jar</systemPath>  



**4 导包失败：**

有了maven后 导包快捷键： alt+回车，

如果本地仓库没有的话，去maven repository 上搜索，然后在pom.xml中引用。



https://wangao.info/2020/04/13/spring-boot-00/





5 **找不到主类**/没有主清单属性

https://www.cnblogs.com/thinking-better/p/7827368.html

```
<!--自动生成主清单属性-->
    <build>
    <plugins>
    <plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
           <!--设置自己想要的主类-->
            <mainClass>com.jj.api.rest.common.MapToolUtil</mainClass>
        </configuration>
    </plugin>
    </plugins>
    </build>
```



6 user子程序启动问题

**Could not locate PropertySource: I/O error on GET request**

在usr -dev -   .xml中 关闭 spring.cloud.config 的配置

```shell
spring.cloud.config= false
```



7 找不到符号

把.idea删除，再重新打开ide

```
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>

所以我就把他注释了再编译，结果就没问题了，所以我觉得应该是这个插件导致
```



8 plugins 报红色波浪线

- 到本地仓库下找到未下载成功的插件及对应版本
- 删掉后缀为`lastUpdated`的文件
- 再到idea上 maven刷新按钮重新下载即可成功



9 程序包不存在-导入maven项目时程序包不存在

出错原因：project structure -- Libraries -- 第三栏 sources, javaDocs 包 没找到或者安装错误。

去仓库找到相对应版本的包安装位置，**如果文件是以 .lastUpdated 结尾的，那这就是下载失败的jar，当这个下载失败的文件存在的时候，reimport 我们的pom.xml文件，它是不会重新下载这个失败的jar的。**

解决方案： 

Step 1: 重新导入前的准备。

首先尝试了以下几种方式，但是没能解决问题（注意是无效的）：**

- 修改maven的 `settings.xml` 增加或修改`mirror`或其他配置（并非中央仓库问题）；
- 删除`.lastUpdated`文件 (删除后会复活);

* 通过**在项目路径下使用`mvn -U compile`命令！**

* 在maven 的settings里面加入：

```
<profiles>
<profile>
    <id>downloadSources</id>
    <properties>
        <downloadSources>true</downloadSources>
        <downloadJavadocs>true</downloadJavadocs>           
    </properties>
</profile>
</profiles>

<activeProfiles>
  <activeProfile>downloadSources</activeProfile>
</activeProfiles>
```

Step 2: 重新导入

Step 3: 点第三列左上角+号，选择并添加正确的lib文件路径。

- /Users/lujiawen/Desktop/工作台/my_packs_env_dont_change_name_position/apache-maven-3.6.3/repository/antlr/antlr/2.7.7/antlr-2.7.7-sources.jar

未解决



10 java.lang.IllegalStateException: Service id not legal hostname (${feign.serviceId.user})

user 中 resources 文件夹下 取名为 application.yml 的配置文件， 最好不要随便改名。



11 Could not autowire. No beans of 'xxxx' type found

（这个错误提示并不会产生影响。控制台也不报错，可以正常运行。）

。如果有强迫症的童鞋可以尝试：

1降低Autowired检测的级别，将Severity的级别由之前的error改成warning或其它可以忽略的级别。

2确认导包是否正确：import org.springframework.stereotype.Service;

![](https://tva1.sinaimg.cn/large/008eGmZEly1gnpv14blw2j31010o50xl.jpg)



参考资料：

https://blog.csdn.net/sunnyzyq/article/details/86711708 ⭐️

自己创建：https://blog.csdn.net/typa01_kk/article/details/76696618

配置gitlab： https://blog.csdn.net/u010454030/article/details/80653660