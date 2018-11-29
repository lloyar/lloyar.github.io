---
layout: article
title: 使用 Event System 响应场景中3D物体的点击事件
date: '2018-11-29 12:14:00 +08:00'
key: '2018-11-28_20:51'
tags:
  - Unity
---

> When you add an Event System component to a GameObject
 you will notice that it does not have much functionality exposed, this is because the Event System itself is designed as a manager and facilitator of communication between Event System modules. [See Here.][ce714bef]

  [ce714bef]: https://docs.unity3d.com/Manual/EventSystem.html "Event System"

Unity 的Event System 是作为一个事件中心存在的。它本身不能提供功能，它被设计成各个Event Systems Modules 之间的管理者和协调者。

<!--more-->

## Event System 的主要功能

- Manage which GameObject is considered selected
- Manage which Input Module is in use
- Manage Raycasting (if required)
- Updating all Input Modules as required

## Event System 的主要协作者

### Input Modules
:它是控制Event System 行为的主要逻辑部件。

常被用于以下用途：
- Handling Input
- Managing event state
- Sending events to scene objects.

### Raycaster
:用于计算鼠标指针的结束。输入模块常常使用场景中的Raycaster 配置来计算指针设备的结束。

默认提供了三种Raycaster ：
- Graphic Raycaster - 用于UI 元素
- Physics 2D Raycaster - 用于2D 物理元素
- Physics Raycaster - 用于3D 物理元素

场景如果添加了2d / 3d Raycaster 的射线检测，那么EventSystem 也会检测相应的物理元素。

Raycaster 组件只能绑定在有Camera 组件的GameObjetc 上。
{:.info}

## 如何使 Event System 运作

1. 确保场景中有一个 Event System ，添加方式为 Hierarchy -> Creat -> UI -> Event System 。
2. 添加完成后可以看到 Event System 对象下存在三个组件，一个是所有 GameObject 都必须要有的 Transform ，其次是 Event System 和 Standalone Input Module 。
3. 在摄像机上附加 Physics Raycaster 组件，该组件必须与 Camera 组件绑定到同一个 GameObject 上。
4. 创建一个Cube(或者其他任意一个3D物体，但是确保物体上有collion组件，以确保碰撞器检测射线)，作为事件触发的3d物体
5. 在要触发事件的物体上（刚刚创建的Cube），附加上一个脚本，内容大致如下：

``` C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.EventSystems;

public class ClickObject : MonoBehaviour, IPointerClickHandler
{
    // Update is called once per frame
    void Start()
    {

    }

    public void OnPointerClick(PointerEventData eventData)
    {
        // 当点击此物体时，会自动执行此方法内的代码
        Debug.Log("您点击了该物体")
        // Debug.Log(eventData.pointerCurrentRaycast.worldPosition) // 碰撞点的世界空间坐标
        // Debug.log(eventData.pointerCurrentRaycast.worldNormal) // 碰撞点的世界法线向量
    }
}

```

上述程序运行结果是，当你点击Cube时，控制台上会弹出Debug信息“您点击了该物体”。

另外， OnPointerClick 方法传递进来的 PointerEventData 类含有相当多有用的信息。它可以获得碰撞点的世界空间坐标以及法线向量(上述注释内容)。

ClickObject 类通过实现 IPointerClickHandler 接口的 OnPointerClick 方法，与 Event System 进行沟通，监听点击事件。

关于更多 Event System 所支持的监听事件，请参考[此处][0802090a]

  [0802090a]: https://docs.unity3d.com/Manual/SupportedEvents.html "Supported Events"
