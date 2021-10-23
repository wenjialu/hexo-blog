---

title: 集成 Auzure ad 的 msal 库 

date: 2021-03-19

tags: 

---



之前我写了一个简单的 web app：[文章链接](https://wenjialu.github.io/hexo_blog/2021/12/22/smartBrain%20react%20-%E4%BA%BA%E8%84%B8%E8%AF%86%E5%88%AB%E7%BD%91%E7%AB%99/)🔗。

但是登录注册功能比较简单，而且要自己建数据库存储用户的信息。

正好最近有接触到微软的azure ad，它是企业级的授权验证方案，。。

可以方便app开发者和用户。。

所以我决定在我的应用中集成这个功能, 来管理&便利我的用户登录 ～





Azure portal --> app registration ---> register--> 填写我的应用名称 + 重定向 URI https://wenjia-smart-brain.herokuapp.com/ --> 在 [Quickstart](https://portal.azure.com/?l=en.en-us#blade/Microsoft_AAD_RegisteredApps/ApplicationMenuBlade/Quickstart/quickStartType//sourceType/Microsoft_AAD_IAM/appId/7db5d000-e566-4ab0-a385-358b807c54fc/objectId/71296739-6f12-41a1-9584-0515d8b13907/isMSAApp//defaultBlade/Overview/appSignInAudience/AzureADandPersonalMicrosoftAccount/servicePrincipalCreated/true) 中按照说明下载对应app的文件并修改配置【[比如我的应用是 node.js 写的， 我就参考的是 node.js  应用的授权教程](https://docs.microsoft.com/zh-cn/azure/active-directory/develop/quickstart-v2-nodejs-webapp-msal?WT.mc_id=Portal-Microsoft_AAD_RegisteredApps)】（鹿鹿贴心提醒，除了官方提醒，还别忘了修改redirect uri。）

1. Go to the [Azure portal - App registrations](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/PythonQuickstartPage/sourceType/docs) quickstart experience.
2. Enter a name for your application and select **Register**.
3. Follow the instructions to download and automatically configure your new application.





成功后就可以访问 127.0.0.1:3000, 可以看到：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvp4s0l1iej60be0dwgm602.jpg)


报错：（鹿鹿贴心提醒，除了官方提醒，还别忘了修改redirect uri。不然会有以下报错。）

![](https://tva1.sinaimg.cn/large/008i3skNgy1gvp4mbgcmfj60hn0i6my902.jpg)





如果遇到问题，可以通过 https://github.com/wenjialu 给我留言～