---
permalink: /:year-:month-:day/:title
layout: post
title:  "python连接MySQL出错"
author: "Lei Lie"
categories: Study
tag: notdouban
---
报错：
mysql.connector.errors.NotSupportedError: Authentication plugin 'caching_sha2_password' is not supported

参考的是：[廖雪峰MySQL教程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017802264972000)

导入了包`pip install mysql-connector`。

但是报错。

解决方法：[这篇文章](https://blog.csdn.net/Bee_AI/article/details/102767400)的评论。

![img1]({{site.page}}/images/img-2021-08-03/img1.png)

但是，安装之后仍然报错：

mysql.connector.errors.NotSupportedError: Authentication plugin 'caching_sha2_password' is not supported

因为PyCharm的 Python 控制台没有重启，它的环境还是没有安装`MySQL-connector-python`的环境。重启一下 Python 控制台就可以了，之后就不会报错了。

思考：廖雪峰的博客教程有纰漏，不仅要`pip install mysql-connector`，还要`pip install MySQL-connector-python`。两个包都得导入才行。