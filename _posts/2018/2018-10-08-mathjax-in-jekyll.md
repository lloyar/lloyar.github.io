---
layout: post
title: 在Jekyll中使用LaTex
subtitle: MathJax配置方法以及一些问题探讨
date: '2018-10-08 22:37'
author: Lee
header-img: "images/2018/10/post-hi-mathjax.png"
tags:
  - Web
---

# Contents
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

# 前言
> 首先声明一下，这篇文章的大部分内容来自互联网，这里摘抄一下做个笔记。[^1] [^2]

[^1]:[https://www.weining.me/2014/04/29/jekyll-with-kramdown-and-mathjax](https://www.weining.me/2014/04/29/jekyll-with-kramdown-and-mathjax)
[^2]:[https://jetorz.github.io/2017-02-16-math-with-jekyll.html](https://jetorz.github.io/2017-02-16-math-with-jekyll.html)

在这里先介绍一下LaTex与MathJax的关系：
: MathJax是一个开源JavaScript库。它支持LaTeX、MathML、AsciiMath符号，可以运行于所有流行浏览器上。 对大部分用户而言它不需要安装，所以网页作者可以编写包含数学公式的页面并有信心让读者可以自然而容易的浏览到它们。 MathJax允许页面作者使用TeX、LaTeX符号和 MathML 或者 AsciiMath 去书写公式。
所以，为了能够显示LaTex公式，我选择了MathJax作为本站的数学公式渲染引擎。那么下面我们就开始正式讨论问题吧。

## MathJax 是如何被Jekyll调用的
Jekyll 在生成静态站点的时候，会将 `_layout` 文件夹中相应的布局文件，按照文件头部 YAML 语法的声明套用在页面上。举个例子，比如站点的主页就是 `index.html` 可以看到这个文件头布声明为 `layout: default`，因此在生成主页的时候，就会将 `_layout/default.html` 中的布局套用过来。同样， `_posts` 路径下的文章，一般头部都声明为 `layout: post`，因此在生成文章页面的时候，就会套用 `_layout/post.html`。

Jekyll 的第二个特点是，它支持使用 Liquid 语言。引入 Liquid 语言有几个好处，包括：可以定义和使用变量、可以在文件中包含别的文件、以及可以使用逻辑和循环语法。其实这几个功能都可以大大简化我们对站点的设置过程。这里要说的就是第二点，可以在文件中包含别的文件。

可以看到，在 `default.html` 中有：

```
{ % include head.html %}
{ % include header.html %}
{ % include footer.html %}
```

这些就是用 Liquid 语言将 `head.html`, `header.html`, `footer.html` 包含到 `default.html` 文件中的用法。Jekyll 生成站点时， `{ % include <filename> %}` 语句会在 `_includes` 文件夹中查找到被包含的文件，然后将其插入的相应的位置。

回到本文讨论的内容，如何为网页添加 MathJax 显示数学公式。[MathJax Getting Start](http://docs.mathjax.org/en/latest/start.html) 告诉我们，只需要将前面提到的那段 html 代码放到需要使用 MathJax 生成公式的页面的 `<head>` 标签内，这样在浏览器加载页面时就会通过 MathJax 的 CDN 将公式显示出来。

Jekyll 自动创建的站点的所有页面都是基于 `_layout/default.html` 生成的，包括我们需要显示公式的文章页面。而 `default.html` 会使用 `{ % include head.html %}` 引入` _includes/head.html` 的内容。

因此，基于以上描述，只要将 MathJax 提供的代码片段放在 `_includes/head.html` 中即可。

# 正文
## 1. 问题

研究数据科学少不了和公式打交道。Markdown 本身是不支持公式的，好在 Markdown 最后可以编译为 HTML 文件，而 HTML 文件可以通过一些办法支持公式。

## 2. 方法

通过两个办法，可以很方便的记录公式。本地编辑我用 Atom，通过 Markdown-Preview-Enhanced 插件可以调用 MathJax 显示公式。这就是选择一些更为通用和开放的软件的好处，类似 DayOne、Ulysses 之类的封闭软件就只能等作者，而这些小众的需求很可能根本不在作者考虑范围之内。

在线分享的话，因为技术类文档我都放在 Github 上，而 Jekyll 也可以通过 kramdown 调用 MathJax 支持公式。想起来阳老说的，要去技术的源头，真是不能不服。Github 这类网站，是全世界最聪明的大脑们聚集的地方，他们所使用的工具比路人工具，类似新浪博客之类不知高多少个层面——什么时候新浪博客才可能支持 LaTeX？互联网给了我们普通人类似的机会，一定要想办法去这些人聚集的地方，你会收获很多。

Jekyll 当前版本是3.3.1，其默认主题是 Minima，一个 Gem-based 主题。这种主题把很多设置都隐藏了起来，从而节省用户需要考虑的东西；但需要的时候还可以自定义，同时拥有了简洁和强大，实在是不能再赞了。

在 Terminal 里输入 `open $(bundle show minima)` ，从打开的 Finder 窗口里将 `_layout/post.html` 复制到自己的 Jekyll 文件夹里，比如我的文件夹是 `jetorz.github.io`，就复制到 `jetorz.github.io/_layout/post.html` 里。用 Atom 打开 post.html，在  <header> 里输入 MathJax 脚本[m]：

```
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```

保存后，就可以在文章里面调用 MathJax 渲染 Markdown 里面的 LaTeX 公式了。比如输入

```
$$
R_{\mu \nu} - {1 \over 2}g_{\mu \nu}\,R + g_{\mu \nu} \Lambda
= {8 \pi G \over c^4} T_{\mu \nu}
$$
```

最后显示为

$$
R_{\mu \nu} - {1 \over 2}g_{\mu \nu}\,R + g_{\mu \nu} \Lambda
= {8 \pi G \over c^4} T_{\mu \nu}
$$

## 3. 怎样利用 Markdown + MathJax 同时在本地和远程同步显示 LaTeX

MathJax 文档里面明确说到

>By default, the tex2jax preprocessor defines the LaTeX math delimiters, which are `\(...\)` for in-line math, and `\[...\]` for displayed equations.

LaTeX 文档里面则说

> Using the `$$...$$` should be avoided, as it may cause problems, particularly with the AMS-LaTeX macros. Furthermore, should a problem occur, the error messages may not be helpful.

然后 MathJax 里面又说

> `$` is not defined in tex2jax.

互联网真是像极了春秋战国，诸子百家各种众说纷纭。想想也正常。HTML、LaTeX、MathJax、Markdown，这么多种服务混杂在一起，不冲突才怪了。但对于我们想要的大时间周期积累，这些基础性的服务最好在一开始就配置好，免得以后调整。所以干脆自己测试一下。考虑到 inline 公式和 display 公式方法不同，故分开测试。

### 3.1 display 方式测试

在 display 方式的三种可能的表示方法，`$$` / `\[...\]` / `\\[...\\]` 里面，输入如下公式

`i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1`

#### 3.1.1 \$\$ 方法

输入

```
$$
i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1
$$
```

可得

$$
i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1
$$

#### 3.1.2 \\[ 方法

输入

```
\[
i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1
\]
```

可得

\[
i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1
\]

#### 3.1.3 \\\\[ 方法

输入

```
\\[
i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1
\\]
```

可得

\\[
i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1
\\]

综上，统计如下表：

location      | \$\$ | \\[ | \\\\[
--------------|------|-----|------
MMP           | OK   | OK  | NG
Jekyll local  | OK   | NG  | OK
Jekyll GitHub | OK   | NG  | OK

可见，虽然 LaTeX 官方文档不推荐采用 `$$`，但由于它不牵扯转移符 `\`,所以在 Atom 和 Jekyll 里显示一致，就目前阶段而言是比较推荐的方案。

### 3.2 inline 方式测试

#### 3.2.1 \$ 方法

输入

```
so $i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1$ inline display?
```

可得

so $i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1$ inline display?

#### 3.2.2 \\$ 方法

输入

```
so \$i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1\$ inline display?
```

可得

so \$i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1\$ inline display?

#### 3.2.3 \\( 方法

输入

```
so \(i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1\) inline display?
```

可得

so \(i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1\) inline display?

#### 3.2.4 \\\\( 方法

输入

```
so \\(i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1\\) inline display?
```

可得

so \\(i={\sqrt[{n}]{\left({\frac {FV}{PV}}\right)}}-1\\) inline display?

综上，统计如下表：

location      | \$ | \\$ | \\( | \\\\(
--------------|----|-----|-----|------
MMP           | OK | NG  | OK  | NG
Jekyll local  | NG | NG  | NG  | OK
Jekyll GitHub | NG | NG  | NG  | OK

可见，由于 MathJax 不支持 `$`，所以尽管通过 MMP 插件可以让公式在 Atom 里面正确显示，但 Jekyll 无法正确显示。`\(` 和 `\\(` MMP 和 Jekyll 倒是都支持，但由于对转移符`\`的解释不通，导致两方面并不同步。所以最佳解决方案就是更改MMP内关于inline的设置，去掉 `$` 并把 `\(` 更改为 `\\(`，使得本地和远程都可完美显示。

## 4. 结论

在 wikiBook 上面找了一些简明教材，LaTeX 公式相关的部分，一共分为 22 个章节，一下子研究明白貌似不太可能了，而且也没有必要，毕竟我只是想简简单单地写个博客而已。

## 5. 体会

1. 遇问题先再通用搜索引擎查一下这个问题能不能解决，可以借用什么工具。有些功能是用现有工具暂时无法实现的，这时候就只能另想办法；
2. 问题能用现在工具解决的话，要有能力阅读工具的官方文档。官方文档一定要详读，因为这才是最权威的。比如 Jekyll 官方文档上解释的如何显示公式，显然应该赋予更高的权重；
3. 不要轻易迷信网上的文章，比如有篇文章说要把 MathJax 的 Script 代码放在 page.html 里，害得我死活通不过……要自己思考，这么做是不是合理；
4. 没有什么完美的解决方案，尤其在需要很多方协作的时候。比如通过 Markdown 调用 MathJax，MathJax 又部分的实现了 LaTeX，最后渲染成 HTML……将就着用吧。至于到底选哪个，没有什么太绝对的标准，毕竟技术在不断的进步，标准也会更新。最好的办法，是通过一套结构化的操作，使得以后即使有变化，也可以通过批量替换的方式解决；

至此，数学公式的问题完美解决。

## 6. 来源

- [Stackoverflow 讨论](http://stackoverflow.com/questions/26275645/how-to-supported-latex-in-github-pages)
- [Jekyll 官方文档](http://jekyllrb.com/docs/home/)
- [MathJax 官方文档](http://docs.mathjax.org/en/latest/start.html)
- [kramdown 官方文档](https://kramdown.gettalong.org/syntax.html#math-blocks)
- m: [MathJax CDN shutting down on April 30, 2017.](https://www.mathjax.org/cdn-shutting-down/)
