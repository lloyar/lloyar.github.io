---
layout: article
title: Why need Input/Output semantics of Shader programming
date: '2018-11-03 23:55:00 +08:00'
key: '2018-11-03_22:50'
tags:
  - Computer Graphics
  - Shader
---

**Overview**

之前一直不太理解为什么在High Level Shader Language (HLSL)中需要语义(semantics)这种语法。

在[HLSL的官方文档][1] 中，我找到了答案。

<!--more-->

## Graphics Pipeline | 图形管线

因为[Graphics Pipeline][1] 的存在,原本完整的数据处理流程强行被图形管线分出的步骤给打断了。图形管线的工作流程如下：

![Graphics Pipeline](/images/2018/11/graphics-pipeline.png)

造成数据被打断的根本原因在于GPU只是部分可编程的硬件，上图中只有圆角矩形的阶段是可编程阶段。这就像原本一通到底的水管突然被截断，并对里面的数据流做了些奇怪的事情后，需要重新接好水管，将它送往到该去的地方，而语义就相当于门牌号，防止我们把 “石油”输送到 “天然气”管道。总的来说，这是因为GPU不完全可编程导致的原因，数据流需要在数个阶段做停顿。

其实在每一个Shader内部，数据的传输根本不需要“语义”去做指定，在以前的编程过程中，数据的处理永远不必交付给“另外一个”硬件，就算有，也已经被CPU处理完了，我们根本不需要去理会数据的交接过程。但是现在情况不同了，假设没有语义的帮助，那么就需要设置一套非常奇怪的规则，用以规定数据被处理完后，应该传递给谁。

Shader编程不像传统编程，我以前写的程序不需要在软件层面与硬件层面来回传递数据，所以突然在变量声明时每次都要在后面附加个语义，让我感到非常不适应。不过现在来看，这种设置，其实是为了更加清晰整个处理流程才设计的。

## 一些疑问

不过对于图形管线，我仍旧有一些疑问，Direct3D 的这套流程，是否是根据显卡制定的。这套处理流程到底是Direct3D 为显卡抽象出来的呢，还是显卡本就是这种处理构造。如果说显卡本就是这样的构造，那我们为啥不直接跟显卡驱动打交道，而要跟它上面的附加层Direct3D 打交道。而且Direct3D 的这套流程明显仅仅针对3D处理（废话，不然能叫Direct3D嘛）。但实际上显卡还可以做通用计算，比如深度学习，大规模并行矩阵计算……很明显，这套流程根本不可能被套用在图形处理以外的工作。想解决这个问题，我需要了解显卡驱动，以及Nvidia的CUDA，与DirectX 有何不同。

[1]: https://docs.microsoft.com/zh-cn/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9#pixel-shader-basics
[2]: https://docs.microsoft.com/zh-cn/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline
