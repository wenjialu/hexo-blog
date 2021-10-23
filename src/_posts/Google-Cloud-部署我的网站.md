---
title: Google Cloud 部署我的网站 (附宝塔面板安装教程)
date:
tags: Google Cloud
---



### 前言：

我曾经在github 上部署过我的个人网站，不过github上的静态网页已经越来越不能满足我对网站的需求了。

于是，我选择把整个网站迁移到自己的云服务器上。

如果你对我的网站感兴趣，可以点[这里](http://wenjialu.tech)参观。

以下是我的教程。



### 安装宝塔面板

**vm实例**右侧有个`ssh`按钮，点击就可以进入到后台。

1 sudo - i

2 脚本安装 （一整行复制哦）

```text
# My google cloud instance is Centos
Centos安装脚本 
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
 
#My google cloud instance-1 is Debian
Debian安装脚本
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh
 
Ubuntu/Deepin安装脚本 
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh
 

Fedora安装脚本
 wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
```

3 安装完成后记录下自己的宝塔地址和密码

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmyueb5t6pj30lk08nmyg.jpg)



4 浏览器登陆面板后，选择LNMP（推荐）

5 按照提示注册

安装完成～



![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmytc9yq07j310j0l4q69.jpg)

要注意将vps的临时ip设置为静态ip，不然关机重启会导致ip发生变化（设置方法：vpc网络->外部ip地址->类型）



### 添加站点

Step1:  注册域名

hostinger 上挑一个自己喜欢的域名。

我花了10块钱在hostinger 上买了一个一年的域名 wenjia.tech

有了域名相当于你生产的“宝宝”有了名字，

但是宝宝光有名字不够啊，还得去公安局登记注册身份证号，

同理，我们的域名也需要经过一个叫 DNS 的服务认证，才能建立起 域名和ip 的一对一联系。



Step2: 解析域名

 hostinger - DNS/Nameservers

| Type | Name | Points to         | Til  |
| ---- | ---- | ----------------- | ---- |
| A    | www  | 填写你的服务器 IP | 300  |



或者

打开[阿里云](https://help.aliyun.com/document_detail/29716.html)

DNS解析 https://dns.console.aliyun.com/?spm=5176.12901015.0.i12901015.7fc6525cUup6lY&accounttraceid=d17d9f090e29430f9e46fae5c55b8f00vsgi#/dns/setting/wenjialu.tech

到 hostinger - DNS/Nameservers - change nameservers

改成阿里云的 DNS 服务器

| **ns1.alidns.com** |
| ------------------ |
| **ns2.alidns.com** |



Step3: 宝塔 -- 网站 -- 添加站点

把域名填进去

保存好数据库等信息。



Step4: 添加 ssl 证书

[教程地址](https://wenjialu.github.io/hexo_blog/2021/01/25/%E7%BB%99%E8%87%AA%E5%B7%B1%E7%9A%84%E7%BD%91%E7%AB%99%E9%85%8D%E7%BD%AEssl%E8%AF%81%E4%B9%A6/)













接下来，可以根据自己的需求，选择自己所需的教程部分。

如果你以前已经有了网站，想迁移到这个新服务器上的，看上传宝塔。

如果是建站小白或者是想要一个新网站的，到安装插件部分。

### 上传宝塔

先把博客所在的文件夹 打包成**压缩包**

到网站的根目录 www - wwwroot 

依次点击：文件 - 上传

上传完成之后再解压就可以啦。





### 安装插件

软件商店--宝塔插件 -- 宝塔一键部署源码 

里面有很多中博客模版可以选择。

我推荐使用 wordpress, 口碑有保证。





















### 参考链接：

添加 ssl 证书：https://wenjialu.github.io/hexo_blog/2021/01/25/%E7%BB%99%E8%87%AA%E5%B7%B1%E7%9A%84%E7%BD%91%E7%AB%99%E9%85%8D%E7%BD%AEssl%E8%AF%81%E4%B9%A6/

阿里云ssl： https://yq.aliyun.com/articles/637307

部署网站：https://www.bilibili.com/video/av57100951/

ssh： https://www.vediotalk.com/archives/606