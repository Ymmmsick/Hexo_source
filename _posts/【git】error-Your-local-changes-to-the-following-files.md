title: '【git】error: Your local changes to the following files'
date: 2015-07-26 16:15:27
categories: Git
tags:
---
今天在服务器上git pull是出现以下错误：

error: Your local changes to the following files would be overwritten by merge:

        application/config/config.php

        application/controllers/home.php

Please, commit your changes or stash them before you can merge.

Aborting

不知道什么原因造成的代码冲突，处理方法如下：

如果希望保留生产服务器上所做的改动,仅仅并入新配置项:

    git stash
    git pull
    git stash pop

然后可以使用git diff -w +文件名 来确认代码自动合并的情况.

如果希望用代码库中的文件完全覆盖本地工作版本. 方法如下:
	
	git reset --hard
	git pull