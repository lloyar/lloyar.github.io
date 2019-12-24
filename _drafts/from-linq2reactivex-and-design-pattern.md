---
layout:     article
title:      从 LINQ 到 ReactiveX 再到设计模式
date:       '2019-12-24 11:44:00 +08:00'
key:        '2019-12-24_11:44'
tags:
    - C#
    - Design Pattern
---

由设计模式贯穿始终的一场旅行

<!--more-->

# 缘起

ReactiveX -> LINQ -> Design Pattern

# LINQ 和 ReactiveX 的联系

LINQ：迭代器模式是行为模式的一种范例, 行为模式是一种简化对象之间通信的设计模式。 这是一种非常易于理解和使用的模式。实际上,它允许你访问一个数据项序列中的所有元素,而无须关心序列是什么类型——数组、 列表、 链表或任何其他类型。 它能非常有效地构建出一个数据管道, 经过一系列不同的转换或过滤后再从管道的另一端出来。实际上,这也是LINQ的核心模式之一。（C# in Depth Chapter 6）

[关于LINQ框架的设计模型参考][b68bca2c]

  [b68bca2c]: https://www.cnblogs.com/wangiqngpei557/archive/2012/11/22/2783357.html "LINQ Design Pattern"

ReactiveX：响应式编程扩展。他是LINQ的二元性对应产物，LINQ通过主动拉取数据驱动，ReactiveX通过被动的获取数据驱动。

[ReactiveX 设计者演讲][d6f6cc41]

  [d6f6cc41]: https://v.youku.com/v_show/id_XNDcwMjQ0MTY4.html "ReactiveX Video"

[对上述视频的解读](https://zhuanlan.zhihu.com/p/35189325)

# 深入理解的路线图

```


```
