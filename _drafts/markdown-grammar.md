---
layout:     "article"
title:      "Markdown grammar"
date:       "2018-11-03 23:57:00 +08:00"
key:        "2018-11-03_23:57"
tags:
    -
---


在kramdown的官网上查看扩展语法

定义：

```
Name
： *
```

toc:

```
# Contents
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}
```

## Including images and resourcesPermalink
Including an image asset in a post:

```
![My helpful screenshot](/assets/screenshot.jpg)

```

Linking to a PDF for readers to download:

```
... you can [get the PDF](/assets/mydoc.pdf) directly.
```

## [kramdown Syntax](https://kramdown.gettalong.org/syntax.html)

Following is a list of all the characters (character sequences) that can be escaped:
```
\         backslash
.         period
*         asterisk
_         underscore
+         plus
-         minus
=         equal sign
`         back tick
()[]{}<>  left and right parens/brackets/braces/angle brackets
#         hash
!         bang
<<        left guillemet
>>        right guillemet
:         colon
|         pipe
"         double quote
'         single quote
$         dollar sign
```

## 附加样式

### 提示
```
Success Text.
{:.success}
Info Text.
{:.info}
Warning Text.
{:.warning}
Error Text.
{:.error}
```

### 标签
```
`success`{:.success}
`info`{:.info}
`warning`{:.warning}
`error`{:.error}
```

### 图片

**Border**
![Image](path-to-image){:.border}

**Shadow**
![Image](path-to-image){:.shadow}

**Rounded**
![Image](path-to-image){:.rounded}

**Circle**
![Image](path-to-image){:.circle}

**Mixture**
![Image](path-to-image){:.border.rounded}

![Image](path-to-image){:.circle.shadow}

![Image](path-to-image){:.circle.border.shadow}
