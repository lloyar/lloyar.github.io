---
layout: article
title: Markdown 语法备忘录
date: '2018-11-30 16:26:00 +08:00'
key: '2018-11-03_23:57'
tags:
  - Tools
---

这篇文章总结了 GFM、Markdown、kramdown 以及 jekyll-TeXt-theme （本主题）尚未掌握的语法。在还不能完全记住之前，写在这里以备随时查看，免去了去各大官网查找的痛苦。

<!--more-->

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

- Following is a list of all the characters (character sequences) that can be escaped:

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

- Defined：

  ```
  Name
  ： *
  ```

- ToC:

  ```
  # Contents
  {:.no_toc}

  * A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
  {:toc}
  ```

## Additional styles

### Alert

Success Text.
{:.success}

Info Text.
{:.info}

Warning Text.
{:.warning}

Error Text.
{:.error}

markdown:

```
Success Text.
{:.success}
```
```
Info Text.
{:.info}
```
```
Warning Text.
{:.warning}
```
```
Error Text.
{:.error}
```

### Tag

`success`{:.success}

`info`{:.info}

`warning`{:.warning}

`error`{:.error}

markdown:

```
`success`{:.success}
```
```
`info`{:.info}
```
```
`warning`{:.warning}
```
```
`error`{:.error}
```

### Image

**Border**
![Image](/assets/android-chrome-512x512.png){:.border}

```
![Image](/assets/android-chrome-512x512.png){:.border}
```

**Shadow**
![Image](/assets/android-chrome-512x512.png){:.shadow}

```
![Image](/assets/android-chrome-512x512.png){:.shadow}
```

**Rounded**
![Image](/assets/android-chrome-512x512.png){:.rounded}

```
![Image](/assets/android-chrome-512x512.png){:.rounded}
```

**Circle**
![Image](/assets/android-chrome-512x512.png){:.circle}

```
![Image](/assets/android-chrome-512x512.png){:.circle}
```

**Mixture**
![Image](/assets/android-chrome-512x512.png){:.border.rounded}

```
![Image](/assets/android-chrome-512x512.png){:.border.rounded}
```

![Image](/assets/android-chrome-512x512.png){:.circle.shadow}

```
![Image](/assets/android-chrome-512x512.png){:.circle.shadow}
```

![Image](/assets/android-chrome-512x512.png){:.circle.border.shadow}

```
![Image](/assets/android-chrome-512x512.png){:.circle.border.shadow}
```
