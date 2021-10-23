---
title: 1～2～3 三步创建一个 Django 项目 【SOP】
date: 2020-01-03
tag: 
---



进入到运行 Django 的虚拟环境。workon Django_web（虚拟环境篇）

找一个自己喜欢的位置，比如桌面。

## 1 建 Django 项目

在系统终端 或者 VScode 终端：

```
django-admin startproject 项目名
```

## 2 建子应用

2.1 创造

进到这个目录中去，`cd 项目名。`

```
django-admin startapp 应用名
```

2.2 注册

进入到项目同名目录，里面有个 settings.py 文件。

找到INSTALLED_APPS，把应用名添加到这里面。

## 3 启动项目 

`python manage.py runserver`



完成～～
![](https://tva1.sinaimg.cn/large/008eGmZEly1got35wx1jbj30u00j5405.jpg)



导入介绍语（why need 后台管理）～～～～

**后台管理：**

首先创建一个管理员账号：

python manage.py createsuperuser

终端面板会提示你设置用户名和密码。



然后打开admin.py，注册一下你的模型类：

```
from yourapp.models import yourModel1, yourModel2

admin.site.register(yourModel)
admin.site.register(yourModel2)
```



通过 localhost:8000/admin 访问后台面板，就能看到自己注册的模型类的具体信息啦。