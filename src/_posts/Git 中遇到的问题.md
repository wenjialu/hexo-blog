---
title: git 疑难杂症大全 （更新中）
date: 2020-12-09 02:30:48
tags: Writing
---

# Git 令人吐血使人窒息，看我如何与它斗智斗勇！



最新更新：

花了 68 元 买了 daisy

把磁盘空间的问题解决之后。

终端的pull 和 push 操作没有啥问题了， 操作相当的丝滑。

But 我终于发现了真正的问题所在，

在于 github page 构建不成功诶。

疑难杂症检查：

https://docs.github.com/cn/free-pro-team@latest/github/working-with-github-pages/troubleshooting-jekyll-build-errors-for-github-pages-sites

* 可能是我的仓库太大了。没有啊，才300m，没有超过1g。







## 问题初显

事情的起因是这样的，我不是自己建了好几个网站嘛。

有的是为了好玩，比如前文提到的hexo； 还有的我拿来当电子简历了（嘻嘻，简历网站就没有公开了啦）。

我在面试某东的时候，面试官竟然对我的简历版的网站很有兴趣，并且对着我网站上列举出的技术栈进行了狂轰滥炸式的考察。

面试最后还不忘夸了夸我的网站设计。 呜！受宠若惊～

但其实我还有点不满意的点，于是在面试结束后就马不停蹄的去修改了。



当我提交完，兴奋的想去看网站效果的时候。 

咦，怎么毫无变化？ 

我反复刷新刷新页面，最后又回到github上看。

嘶 !!! 竟然。。。

## github上提交的修改commit都没成功！

![image-20201113031438774](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113031438774.png)

纳尼，咋都没提交上呢？？？  



原来前一天终端的git commit 也没有成功，但是终端这个铁憨憨好像没有发现，还提示 Everything up-to-date (主人，一切都更新好了哦！), 仿佛一切安好的样子。

![image-20201113000622941](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113000622941.png)



## 解决问题

工欲善其事， 必先利其器。

但是好像我的解决问题工具都有些问题，哭哭。

#### 工具 1 github desktop ![image-20201113032007246](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113032007246.png)

曾经是个非常好用的一个应用端可视化管理git的平台。

直到。。。我建了那个hexo_blog加了个ssh密钥之后。

**总是有问题，不是ssh 连接失败，就是commit提交error -1.或者狂按fetch也没有反应。**

哎，先不用它了。

#### 工具 2 终端

 终端是个天真的铁憨憨，始终觉得自己同步完了，git树很干净。

。。。 

这究竟是为啥？

嗷呜，先不去打扰它对这个世界的美好幻想了。

![image-20201113023439128](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113023439128.png)

#### 工具 3 我自己的手👋--手动同步 

我的设想是这样，既然两个仓库冲突了，那我删掉其中一个。那不就铁定不会冲突了吗。

呵呵，理想很美好。现实总是。。。啪啪啪打脸呢。

我已经把本地仓库删光光，然后复制了远程的到本地，重新建立了他们的连接。竟然还是不行。

到底是哪里出问题了呢？？

难道是我本地空间不足，一些文件上传了云，所以本地没有，导致有冲突吗？

下面是疯狂提示：

![image-20201113021509915](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113021509915.png)

![image-20201113021554349](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113021554349.png)![image-20201113021611872](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113021611872.png)

不不不，应该不会。。。。吧



### 猜想 -- 为啥会出现这些问题呢？

应该就是远程仓库和本地仓库同步有问题，导致版本冲突。那么为啥同步会有问题呢？

#### 猜测 1：密钥🔑不对，远程链接不上

#### 猜测 2:  本地仓库和远程仓库之间没同步好

#### 猜测 3:  难道是我本地空间不够？



### 验证猜想 -- 来嘛，咱一个一个来

问题总是一个一个一个一个一个一个一个...一个一个一个一被解决的嘛。

别怕，先来验证第一个猜想。



#### 是不是因为公钥的问题，去查查看

- ```bash
  1.ssh-keygen -t rsa -C “youremail@example.com” # 创建SSH-Key
  将本地用户目录（用户目录/.ssh/id_rsa.pub）的ssh公钥添加到GitHub中
  2.git bash中输入
  cat ~/.ssh/id_rsa.pub
  复制好出现的内容
  3.打开github，在头像下面点击settings，再点击SSH and GPG keys，新建一个SSH，名字随便。
  把刚刚复制的内容全部黏贴到框中，点击确定保存。
  
  # 以上步骤我以前已经做过了。也就是说我已经在github添加了我的ssh。我现在只要验证下是不是还连接成功。
  （这个ssh链接是和我的整个github的）
  4输入ssh -T git@github.com，（ssh -T git@code.aliyun.com ssh -T git@https://github.com/wenjialu/wenjialu.github.io）
  如果如下图所示，出现我的用户名，那就成功了。
  也可以直接进行项目的克隆或提交进行测试。
  
  我的建议是，在配置完成后，对各个平台都进行一次链接测试。
  ```

- 可以看到，我已经与我的github建立了远程联系了：![image-20201113010754206](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113010754206.png)

- 就是好像和仓库的ssh连接有些问题，但是git里面又已经加了这个仓库了。

- ![image-20201113011658853](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113011658853.png)

- ![image-20201113012049666](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113012049666.png)

暂且认为这个是没问题的吧。继续验证下一个猜想。



#### 本地仓库和远程仓库之间没同步好？

**那我就重新建立一次远程连接嘛**

```zsh
git init # 初始化本地仓库
git add ./ # 将文件添加到待提交区域
git commit -m “提交信息” # 提交
使用GitHub创建仓库（new Repository）
ssh-keygen -t rsa -C “youremail@example.com” # 创建SSH-Key
将本地用户目录（用户目录/.ssh/id_rsa.pub）的ssh公钥添加到GitHub中
git remote add origin git@github.com:XXXX.git # 将远程仓库与本地仓库建立连接
git pull origin master –allow-unrelated-histories # 首先将远程仓库的文件pull下来
#如果执行这句命令时，出现了“ couldn’t find remote ref –allow-unrelated-histories”的错误，输入如下命令即可解决：
​```git pull --rebase origin master
​```git push origin master
git push origin master # 将本地提交同步到远程仓库
```

注意那个绿色的代码，重大突破来咯！！！

当我用那行绿色代码解决error的时候，这个解决error的语句也出现了error。。

![image-20201113020540424](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113020540424.png)

当我搜索解决方案的时候，stackoverflow[https://stackoverflow.com/questions/16064513/git-fatal-unable-to-write-new-index-file]上的这位哥们一下道破了问题的关键。

![image-20201113020424628](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113020424628.png)

喵个叽 果然和我磁盘空间脱不了关系。



#### 磁盘空间不足

我感觉我快接近问题的本质了！！！

我再分析下其他人的回答，好像这个空间是服务器端，也就是github那边的问题。和我设想的本地空间不足好像有点不一样啊。

再看这位的回答：

![image-20201113021244491](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113021244491.png)

妈呀我都快激动哭了，这么朋友也是由这个问题导致的commit不了。

那么我赶紧来试试这个命令

```bash
chmod -RN /path/to/repo #别忘了把/path/to/repo改成你本地仓库的地址哦
```

然后我们继续把刚刚中断的命令敲完：

```bash
git pull --rebase origin master #可能会有一些小冲突，按照提示走就行了。
git push origin master
```

上图！啊哈哈 虽然有些安全性上的小问题，但是我就先无视了。

![image-20201113022526522](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113022526522.png)

完结撒花～～～～～！！





什么？   你以为这就完结了，还是太天真了哦。

未完待续。。。。。。







嵌套git

![image-20201118102445779](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201118102445779.png)



#### 小插曲：

一开始把拉远程仓库的命令搞错了。

既不是 git  pull,  也不是 git pull master 🙅 。

正确的应该是 git pull origin master 🙆 ！

希望大家不要和我一样犯这个傻傻的错误哦。

![image-20201113023049984](/Users/lujiawen/Library/Application Support/typora-user-images/image-20201113023049984.png)

