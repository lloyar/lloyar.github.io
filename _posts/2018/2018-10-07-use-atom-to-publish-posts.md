---
layout: article
title: 使用Atom发布posts
key: post-2
date: 2018-10-07 19:50:00 +08:00
tags:
  - Tools
---

Markdown的编辑器有很多，在线的有[Cmd Markdown][1]，[小书匠][2]，还有最近才从[冯乐乐Blgo][3]上知道的[Prose](http://prose.io/)。离线客户端有[Mark Text](https://github.com/marktext/marktext)，[MWeb](https://zh.mweb.im/index.html)，以及安装了Markdown插件的宇宙最强编辑器[Visual Studio Code][4]。但是把上面所有的选择挨个尝试了后，才发现，他们所有都不能与Jekyll配合的很好。只有Prose是专门为Jekyll优化过的，可是不能自定义posts的frontMatter[^1]，导致每次都要重新填写一遍。

<!--more-->

[^1]: 我不知道怎么称呼这个部分，按照Markdown Writer 的说法叫 Custom fields

## 新的工具！

虽然我也不是经常写博客，但是对于一个坚决不做任何重复性劳动的人来说，必须想办法解决这件事情！下午研究了下Visual Studio Code的自定义代码块的功能，可以说是完美解决了这个问题。在安装了插件[Jekyll Snippets][5]后，无论怎么折腾，都无法在 `.md` 文件中触发自定义代码片段。初步怀疑是无法识别 `.md` 后缀名导致的，因为坑爹的Markdown有太多后缀名了—— `.markdown`、`.mkd`、`.mmark`……晕倒！我也懒得去一个个查找然后全部尝试一遍了。而且最为关键的问题是，每次在VS Code中创建post时，都需要打一大串长长的文件名，还不准打错，这就很折磨人。遂果断放弃我大VS Code。一番寻觅后，还是在[Jekyll](https://jekyllrb.com/resources/)官网上面发现了宝贝（果真除了官方都是坑啊！）。Jekyll官网上给出了5个编辑器，他们分别是：

- jekyll-atom: A collection of snippets and tools for Jekyll in Atom
- markdown-writer: An Atom package for Jekyll. It can **create new posts/drafts**, manage tags/categories, **insert link/images** and add many useful key mappings.
- sublime-jekyll: A Sublime Text package for Jekyll static sites...
- vim-jekyll: A vim plugin to generate new posts and run jekyll build all without leaving vim.
- WordPress2Jekyll: A WordPress plugin that allows you to use WordPress as your editor and (automatically) export content in to Jekyll...

而这篇文章是在第二个Atom插件Markdown Writer的帮助下写成的，Workflow非常的顺畅。不仅解决了我之前提到的两个苦恼，而且还带了一些惊喜。那就是插入链接与图片的功能。具体流程为：在网上找张图片，然后右键复制一下，切换到Atom编辑器内，直接Ctrl + Shift + I ，输入名称，自动保存图片到指定路径，自动插入连接到文章内，整个过程一气呵成，再也不用像以前那样，手动下载图片，手打路径啦！

## 关于自定义配置

我简单再说一次Markdown Writer的配置问题（为什么要说“再”？因为我刚刚辛苦打完后忘了保存 T_T）。

在Atom中安装完成后打开菜单栏

Packages -> Warkdown Writer -> Configurations -> Create Project Configs

然后就能在项目目录下看到一个名为 `_mdwriter.cson` 的文件，打开后就能愉快配置啦~
MdWriter的自定义能力非常强，甚至能自定义Markdown的语法规则。但是我只是想简简单单写个博客而已，这些复杂的功能就留给喜欢折腾的人慢慢摸索吧~

[1]:https://www.zybuluo.com/mdeditor
[2]:http://soft.xiaoshujiang.com/index.html
[3]:http://candycat1992.github.io/2017/07/14/particle-material/
[4]:https://marketplace.visualstudio.com/search?term=markdown&target=VSCode&category=All%20categories&sortBy=Relevance
[5]:https://marketplace.visualstudio.com/items?itemName=ginfuru.vscode-jekyll-snippets
