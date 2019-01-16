---
layout: article
title: Windows Subsystem for Linux
key: post-4
date: 2018-10-09 22:37:00 +08:00
tags:
  - Tools
---

**Overview**

之前的[博文](https://lloyar.github.io/2018/09/28/first-post/)我也提到了，为了搭建这个博客，Windows带来了超级多的巨坑！最最令我头痛的一点就是安装Jekyll的过程了，折磨人的程度不敢想象。不过今天我要介绍的东西，能够极大的改善这种情况。

<!--more-->

## 方案一

因为Jekyll对Windows的支持非常差劲，所以不得不想办法弄个Linux的环境。最开始想到的方案当然是虚拟机了，但是开了虚拟机后，我这台陈旧笔记本就显得有些力不从心了，不过最不能接受的还是每次写完博客想测试一下都要打开虚拟机，等待漫长的启动过程，这才是最糟糕的事情。你说怎么我想写个博客就得这么大动干戈呢？于是第一个方案就这么被抛弃了。

## 方案二

我的第二个方案是使用Docker，直接 `docker image pull jekyll` 接着 `docker container run jekyll` 完美！不过事情远没我想的那般美好，糟心的事情又来了。它竟然无法读取Windows下的磁盘分区！这意味着我要在Docker里面搭建一套工作流程！可是怎么才能在Docker里面编辑Markdown呢！难道要我用Vim？！

好了，最后知道其实是有办法跟物理磁盘建立联系的，不过要输入一串超级长的命令（主要是我的文件目录太长😅)。行，为了能够好好写博客，我忍了>\_<！大不了把命令保存下来每次复制粘贴嘛。一番痛苦的折腾之后， `jekyll serve` 打开浏览器，输入 `localhost:4000` 总算看到了！我😭😭😭。

可惜我还是太天真，这种方式Jekyll无法监听文件的变化，意思就是说每当我做了一次改动，哪怕只是给文章分了个段落，打了个回车，变化都无法反映到网页上去。只能关了Jekyll Serve 重新开。

## 主角登场——WSL

Windows Subsystem for Linux 简称WSL。Win10通过开启WSL功能，可以原生支持Bash。

安装方式[在此](https://docs.microsoft.com/en-us/windows/wsl/install-win10)。

之后就可以直接在PowerShell里面直接敲 `bash` ，就能愉快的玩耍Linux啦。这意味着Windows也可以作为开发平台了！再也不用眼馋Mac OS了！原生支持Bash的速度是虚拟机没法比的，而且能与原系统进行交互，磁盘文件共享，这都是传统虚拟机无法做到的事情。在Windows安装 X Server （[VcXsrv](https://sourceforge.net/projects/vcxsrv/) 和 [Xming](https://sourceforge.net/projects/xming/) 运行表现良好）后，甚至可以直接使用Linux上的桌面应用程序(注意，下图的桌面背景是win10的，但是窗口中的应用来自Linux)。具体信息请看[这里](https://github.com/Microsoft/WSL/issues/637)。

![posts wsl xwindow app](/images/2018/10/post-wsl-xwindow-app.png)

至此，两个系统完美融合在了一起，所有问题迎刃而解,终于可以专心写博客了😭！

## 写在最后

现在，写博客的工作流程如下：

打开 `Atom` 编辑博文 -> 打开 `Bash on Windows` -> 运行 `jekyll serve` 预览效果 -> 修改完毕确认发布 ->  `git push origin master`

整个过程行云流水，开心😋！
