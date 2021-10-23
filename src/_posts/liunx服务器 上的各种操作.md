---
title: liunx服务器 上的各种操作
time: 2020.12.30
tag: 服务器
---







liunx 命令大全： https://www.runoob.com/linux/linux-command-manual.html



1 安装 jdk 包

第一种：已经安装好了，那么解压就行了

比如 Jdk包是rmp格式的，怎么解压呢。

要先转换成cpio格式：

rpm2cpio  xxxx.rpm | cpio -idv

第二种：去oracle官网下载

选择合适的liunx版本jdk8

那么怎么知道合不合适呢，

查看getconf LONG_BIT

如果显示32，则是32位的Linux系统，如果显示64，则是64位的Linux系统。这里是64位的，所以下载**Linux x64**，如下图：

![image-20210116181806625](/Users/lujiawen/Library/Application Support/typora-user-images/image-20210116181806625.png)

详见 https://zhuanlan.zhihu.com/p/94010951



ok, jdk装好了，

接下来配置环境变量。



2  配置环境变量

2.1 找到相应的文件

在.bashrc 或者 .bash_profile 里面配置，由于不知道具体叫啥名字

cd ~

ls -a显示全部文件，包括隐藏文件

2.2 添加环境变量

注意啦，这里 open 是打不开的，要用vim编辑器

Vim .bashrc

输入以下内容（ps：JAVA_HOME 路径写你自己的java安装路径 ）

```java
#set java environment
JAVA_HOME=/root/usr/java/jdk1.8.0_131
CLASS_PATH=.:$JAVA_HOME/lib/
PATH=$PATH:$JAVA_HOME/bin
export JAVA_HOME CLASS_PATH PATH
```

```java
  export JAVA_HOME=/root/usr/java/jdk1.8.0_131
  export JRE_HOME=${JAVA_HOME}/jre  
  export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
  export  PATH=${JAVA_HOME}/bin:$PATH
```



![image-20210116175250183](/Users/lujiawen/Library/Application Support/typora-user-images/image-20210116175250183.png)

关于vim的使用：打开后按i就可以进入编辑模式，在文件最后加上环境变量配置内容按exit退出编辑模式返回命令模式输入:wq保存并退出vim

2.3 刷新一下使它生效

source ~/.bashrc



## 打开 jar 包

假设Linux服务上已经有了打好的jar包，下面介绍几种常用的部署方式：

**1、java -jar启动方式。**

```
java -jar *.jar
复制代码
```

此中方式只会运行在当前窗口，当关闭窗口或断开连接，jar程序就会结束。

![image-20210116182734012](/Users/lujiawen/Library/Application Support/typora-user-images/image-20210116182734012.png)

**2、nohup启动方式。（推荐）**

```
# nohup: 不挂断的运行命令
# &：后台运行
# >: 日志重定向输出到
nohup java -jar *.jar >jarLog.txt &
复制代码
```

**3、注册为Linux服务（推荐）**

- 首先需要现修改pom中spring-boot-maven-plugin配置，其实spring boot 打成jar包以后，是可以直接像shell脚本一样直接运行的，要实现这样可以直接运行，pom.xml 的build节点需要增加这样的配置：

```

```

作者：BothEyes1993
链接：https://juejin.cn/post/6844904052153647111
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。







## 问题：

I am trying to run java command in linux server when I tried to run java I got some error-

> Error occurred during initialization of VM
>
> Could not reserve enough space for object heap
>
> Could not create the Java virtual machine.

memory space is -

```
free -m
```



Increase the amount of system resources in /etc/security/limits.conf, and then issue the “ulimit” command to check that the new resource limit has been changed to “unlimited”.



