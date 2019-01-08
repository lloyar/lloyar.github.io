---
layout:     article
title:      UnityEvent
date:       '2018-12-16 22:48:00 +08:00'
key:        '2018-12-16_22:48'
tags:
    -
---

Overview
<!--more-->


## 总结 C# delegate 、 Event 和 UnityEvent 的关系

  [Look this.](https://blog.csdn.net/qq_28849871/article/details/78366236)

  Idol类中需要包含委托及事件的声明。这里特别强调一下，事件的声明需要定义成静态的，以方便订阅者调用。
  在Start()方法中调用事件。


## UnityEvent 的两个调用版本（静态和动态）

  [Look this ](https://docs.unity3d.com/Manual/UnityEvents.html)[and this.](https://forum.unity.com/threads/unityaction-unityevent-parameters-and-serialization.469240/)

## 具体用法

### 在 Inspector 绑定

```cs
[Serializable]
public class MoveObjectEvent : UnityEvent<T>{}

public class ClickObjectSystem : MonoBehaviour{
  public MoveObjectEvent MoveObjectEvent;

  if (MoveObjectEvent == null)
  {
    MoveObjectEvent = new MoveObjectEvent();
  }
  MoveObjectEvent = new MoveObjectEvent();

  MoveObjectEvent.Invoke(T);
}
```
