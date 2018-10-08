---
layout:     "post"
title:      "mathjax in jekyll of cdn"
subtitle:   ""
date:       "2018-10-08 22:55"
author:     "Lee"
header-img: "images/2018/10/"
tags:
    - "Web"
---



# 前言
> 首先声明一下，这篇文章的大部分内容来着互联网，这里摘抄一下做个笔记。

Jekyll 在生成静态站点的时候，会将 `_layout` 文件夹中相应的布局文件，按照文件头部 YAML 语法的声明套用在页面上。举个例子，比如站点的主页就是 `index.html` 可以看到这个文件头布声明为 `layout: default`，因此在生成主页的时候，就会将 `_layout/default.html` 中的布局套用过来。同样， `_posts` 路径下的文章，一般头部都声明为 `layout: post`，因此在生成文章页面的时候，就会套用 `_layout/post.html`。

Jekyll 的第二个特点是，它支持使用 Liquid 语言。引入 Liquid 语言有几个好处，包括：可以定义和使用变量、可以在文件中包含别的文件、以及可以使用逻辑和循环语法。其实这几个功能都可以大大简化我们对站点的设置过程。这里要说的就是第二点，可以在文件中包含别的文件。

可以看到，在 `default.html` 中有：

```Liquid
\{% include head.html %}
\{% include header.html %}
\{% include footer.html %}
```

这些就是用 Liquid 语言将 `head.html`, `header.html`, `footer.html` 包含到 `default.html` 文件中的用法。Jekyll 生成站点时，{% raw %}`{% include <filename> %}`{% endraw %} 语句会在 `_includes` 文件夹中查找到被包含的文件，然后将其插入的相应的位置。

回到本文讨论的内容，如何为网页添加 MathJax 显示数学公式。[MathJax Getting Start](http://docs.mathjax.org/en/latest/start.html) 告诉我们，只需要将前面提到的那段 html 代码放到需要使用 MathJax 生成公式的页面的 `<head>` 标签内，这样在浏览器加载页面时就会通过 MathJax 的 CDN 将公式显示出来。

Jekyll 自动创建的站点的所有页面都是基于 `_layout/default.html` 生成的，包括我们需要显示公式的文章页面。而 `default.html` 会使用 {% raw %}`{% include head.html %}`{% endraw %} 引入 `_includes/head.html` 的内容。
=======
这些就是用 Liquid 语言将 `head.html`, `header.html`, `footer.html` 包含到 `default.html` 文件中的用法。Jekyll 生成站点时，`{% include <filename> %}` 语句会在 `_includes` 文件夹中查找到被包含的文件，然后将其插入的相应的位置。

回到本文讨论的内容，如何为网页添加 MathJax 显示数学公式。[MathJax Getting Start](http://docs.mathjax.org/en/latest/start.html) 告诉我们，只需要将前面提到的那段 html 代码放到需要使用 MathJax 生成公式的页面的 `<head>` 标签内，这样在浏览器加载页面时就会通过 MathJax 的 CDN 将公式显示出来。

Jekyll 自动创建的站点的所有页面都是基于 `_layout/default.html` 生成的，包括我们需要显示公式的文章页面。而 `default.html` 会使用 `{% include head.html %}` 引入` _includes/head.html` 的内容。

因此，基于以上描述，只要将 MathJax 提供的代码片段放在 `_includes/head.html` 中即可。

# 正文
