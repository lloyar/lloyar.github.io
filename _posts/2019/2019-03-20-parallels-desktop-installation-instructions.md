---
layout: article
title: Parallels Desktop installation instructions
date: '2019-03-20 22:22:00 +08:00'
key: '2019-03-20_22:08'
tags:
  - Tools
---

Mojave 10.14.3 成功安装 Parallels Desktop 14.1.2-45479

<!--more-->

## Mojave安装说明
#### 打开下载的名为“Parallels Desktop 14.1.2-45479 [TNT] .dmg”的dmg文件

![dmg_file](/images/2019/03/dmg-file.png)
![installfile](/images/2019/03/installfile.png)

#### 按Command + Shift + .（点）显示隐藏文件，你可以找到一个名为'Parallels Desktop.app'的文件。记得把窗口拉大一点，右边就是我们要找的文件，如下图所示。（可以再次按相同的快捷键以隐藏所有隐藏文件。）

![notseefile](/images/2019/03/notseefile.png)

#### 将隐藏文件'Parallels Desktop.app'复制到目录/ Applications /

![applicationflord](/images/2019/03/applicationflord.png)

#### 打开Terminal.app
#### 逐个执行以下命令：
`xattr -cr /Applications/Parallels\ Desktop.app`

`chflags nohidden /Applications/Parallels\ Desktop.app`

`codesign --sign - --force --deep /Applications/Parallels\ Desktop.app`

 在/ Applications /目录中打开 Parallels Desktop.app

如果碰到 `xcrun: error: invalid active developer path ...` 错误,应该是你的命令行工具出现了问题，不过你可以先试试看能不能打开软件。解决方式如下：
{:.error}
- 以前没有安装过 Command Line Tools 就在终端运行`xcode-select --install`;
- 如果你之前安装过 Xcode 后又将其卸载掉，就在终端运行`sudo xcode-select --reset`。
