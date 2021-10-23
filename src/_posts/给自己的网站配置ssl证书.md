---
title: 给自己的网站配置ssl证书
date: 2021-01-25
tags: Google Cloud
---

## 添加 ssl 证书



首先在平台上挑选一个免费的证书。

我尝试过 lets-encrypt， Zerossl， FreeSSL。

不过这些都比较不好，一方面是只有90天免费，另一方面是操作比较繁琐。

找来找去，最方便还好用的还是在阿里云上：有一个免费一年的证书叫 Symantec。

以下是一目了然的申请流程：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmzu7vy76jj30sx058wf7.jpg)

#### 1点[这里](https://market.aliyun.com/products/56824016/cmgj031379.html#sku=yuncode2537900001)购买免费证书

#### 2 到阿里云 管理控制台——云盾控制台——[证书服务](https://yundunnext.console.aliyun.com/?p=casnext#/overview/cn-hangzhou)

2.1 填写个人信息

2.2 生成 以下记录，复制这些记录

| Type | 主机记录: | 记录值: | T TL |
| ---- | --------- | ------- | ---- |
| TXT  | _dnsauth  | xxxxxx  | 300  |

2.3 去DNS解析记录:  hostinger - DNS

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmzu6o9ksyj307f069jrl.jpg)

#### 3 验证证书

#### 回到[阿里云证书服务平台](https://yundunnext.console.aliyun.com/?spm=a2c4g.11186623.2.6.5c803c7erq432T&p=cas#/overview/cn-hangzhou)

 DNS 解析完后，证书就能验证成功啦

#### 4 等待证书的审核

#### 5 安装证书

点开下载栏，有很多选择

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gmzvytjtkxj30gw0b3mxu.jpg)

旁边有官方帮助，可根据介绍选择适合自己的版本

我选择的是nginx版。

安装有两种方式，

方法一（推荐）：

如果你的服务器已经安装好宝塔面板的非常推荐用这个方法。

如果没有的话，可以看看我前面的[教程](https://wenjialu.github.io/hexo_blog/2021/01/23/Google-Cloud-%E9%83%A8%E7%BD%B2%E6%88%91%E7%9A%84%E7%BD%91%E7%AB%99/)，或者瞅一下方法二。

**到宝塔面板 - 网站 ；选择证书 - 其他证书**

复制前面我们所下载的证书文件中.key文件的内容粘贴到第一个文本框里，

复制另一个和key文件同名的那个.pem文件内容粘贴到第二个文本框里并保存。

就 ok 啦。



方法二：

按照官方的介绍下载。

我这里再描述下niginx 版的安装方式。

1. 在谷歌云的宝塔面板上找到自己的Nginx服务器。

2. 我的Nginx安装目录是（/www/server/nginx/conf）, 在安装目录下创建一个用于存放证书的目录（命名为cert）。

3. 使用宝塔的上传功能，将本地证书文件和密钥文件上传到Nginx服务器的证书目录（如 /www/server/nginx/conf/cert）

4. 修改 nginx.conf

   ```
   server {
       listen 443 ssl;
       #配置HTTPS的默认访问端口为443。
       #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
       #如果您使用Nginx 1.15.0及以上版本，请使用listen 443 ssl代替listen 443和ssl on。
       server_name yourdomain.com; #需要将yourdomain.com替换成证书绑定的域名。
       root html;
       index index.html index.htm;
       ssl_certificate cert/cert-file-name.pem;  #需要将cert-file-name.pem替换成已上传的证书文件的名称。
       ssl_certificate_key cert/cert-file-name.key; #需要将cert-file-name.key替换成已上传的证书密钥文件的名称。
       ssl_session_timeout 5m;
       ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
       #表示使用的加密套件的类型。
       ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #表示使用的TLS协议的类型。
       ssl_prefer_server_ciphers on;
       location / {
           root html;  #站点目录。
           index index.html index.htm;
       }
   }
   ```

   http 自动跳转到 https

   ```
   server {
       listen 80;
       server_name yourdomain.com; #需要将yourdomain.com替换成证书绑定的域名。
       rewrite ^(.*)$ https://$host$1; #将所有HTTP请求通过rewrite指令重定向到HTTPS。
       location / {
           index index.html index.htm;
       }
   }
   ```

5. 重启 nginx

```
cd /www/server/nginx/sbin  #进入Nginx服务的可执行目录。
./nginx -s reload  #重新载入配置文件。
```





#### 6 验证是否安装成功

证书安装完成后，您可通过访问证书的绑定域名验证该证书是否安装成功。

```
https://yourdomain.com   #需要将yourdomain.com替换成证书绑定的域名。
```

如果你像我一样在浏览器框能看到这个小锁标志:

![image-20210125142020794](https://tva1.sinaimg.cn/large/008eGmZEgy1gmzx498na9j306100t3yi.jpg)



那就恭喜你啦 ～～～～

我们已经成功配置好了ssl证书。