title: android drawable和mipmap的区别
date: 2016-02-23 16:57:02
categories: Android
tags: android,drawable,mipmap
---


以前被坑了，好多人都说新版本的sdk中图片资源放在mipmap中会有优化，官方原话是这样的：

><B>drawable-<density>/</B>
Directories for drawable resources, other than launcher icons, designed for various densities.

><B>mipmap/</B>
Launcher icons reside in the mipmap/ folder rather than the drawable/ folders. This folder contains the ic_launcher.png image that appears when you run the default app.

很明显，官方的意思只是表明launcher图片放在mipmap中会有渲染优化，并没有说所有资源多放在mipmap下面，所以，还是放回drawable吧。