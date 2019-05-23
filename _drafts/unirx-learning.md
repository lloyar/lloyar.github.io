---
layout: article
title: UniRx Learning
date: '2019-05-23 15:05:00 +08:00'
key: '2019-05-23_15:05'
tags:
  - Programming Language
---

这篇文章简要介绍了 UniRx 的用途及其特性和语法，并在最后通过 UniRx 实现一个简易 MVP 架构。

<!-- more -->

 # 何为 UniRx

UniRx 是⼀个 Unity3D 的编程框架。

专注于解决异步逻辑，使得异步逻辑的实现更加简洁优雅。

## UniRx 设计动机

在游戏中，大部分的逻辑在时间上是异步的。往往在实现异步逻辑的时候经常用到大量的回调，最终随着项目的扩张导致传说中的"回调地狱"。

相对较好的方法则是使用消息/事件的发送，结果导致"消息满天飞"，导致代码非常难以阅读。

使用 Coroutine 也不错，但是 Coroutine 本身的定义，是以一个方法的格式定义的，写起来是非常面向过程的。当逻辑稍微复杂一点，就很容易造成 Coroutine 嵌套 Coroutine，代码是非常不利于阅读的（强耦合）。

⽽ UniRx 的出现刚好解决了这个问题，它介于回调和事件之间。

它有事件的概念，只不过他的事件是像水一样流过来，而我们要做的则是简单地进行组织、变换、过滤、合并。

它也⽤用到了回调，只不过事件组织之后，只有简单⼀个回调就可以进行事件的处理了。

它的原理和 Coroutine(迭代器模式) 非常类似，但是比 Coroutine 强大得多。

UniRx 将（时间上）异步事件转化为 (响应式的) 事件序列，通过 LINQ 操作可以很简单地组合起来， 还支持时间操作。

## 为什么要用 UniRx

总结为⼀句话就是，游戏本身有大量的（时间上）异步逻辑，而 UniRx 恰好擅长处理（时间上）异步逻辑，使用 UniRx 可以节省我们的时间，同时让代码更简洁易读。

Rx 只是⼀套标准，在其他语言也有实现，如果在 Unity 中熟悉了这套标准，在其他语言上也是可以很 快上手的。比如 RxJava、Rx.cpp、SwiftRx 等等。

# UniRx 介绍

## 使用 UniRx 实现一个定时器： Timer

```cs
Observable.Timer(TimeSpan.FromSeconds(5))
          .Subscribe(_ =>
          {
            //do something
          });
```

## Update

在某种情况下，我们不得不让 Update 中充斥着大量的判断，以让程序选择不同的分支进行执行。但 UniRx 可以将多个分支选择语句独立出来，每一种情况对应一个独立的Update。

```cs
private void Update()
{
  if (A) {  ...  }

  if (B)
  {
    ...
    if (D) {  ...  }
    else {    }
  }

  switch (C) {  ...  }

  if (Input.GetMouseButtonUp(0)) {  ...  }
}
```

使用 UniRx 改写后：

```cs
void Start()
{
  // A 逻辑,实现了了 xx
  Observable.EveryUpdate()
            .Subscribe(_ => { if (A) { ... } })
            .AddTo(this);
  // B 逻辑,实现了了 xx
  Observable.EveryUpdate()
            .Subscribe(_ => { if (B) { ... if (D) { ... } else {} } })
            .AddTo(this);
  // C 逻辑，实现了了 xx
  Observable.EveryUpdate()
            .Subscribe(_ => { switch (C) { ... }})
            .AddTo(this);
  // ⿏鼠标点击检测逻辑
  Observable.EveryUpdate()
            .Subscribe(_ => { if (Input.GetMouseButtonUp(0)) { ... } })
            .AddTo(this);
}
```

## AddTo

字面意思很简单，就是添加到。添加到哪里呢？添加到 Unity 的 GameObject 或者 MonoBehaviour。

### 为什么要添加到 GameObject 或者 MonoBehaviour

是因为，GameObject 和 MonoBehaviour 可以获取到 OnDestroy 事件。也就是 GameObject 或者 MonoBehaviour 的销毁事件。 这个销毁事件可以通过 AddTo 操作符 与 UniRx 进行销毁事件的绑定，也就是当 GameObject 或者 MonoBehaviour 被销毁时，同样去销毁正在进行的 UniRx 任务。

### AddTo 实现

本质上， AddTo 是一个静态扩展关键字，他对 IDisposable 进行了扩展。

只要任何实现了 IDisposable 的接口，都可以使用 AddTo API，不管是不是 UniRx 的 API。

当 GameObject 销毁时，就会调用 IDisposable 的 OnDispose 这个方法。

### AddTo 设计动机

有了 AddTo，在开启 Observable.EveryUpdate 时调用当前脚本的方法，则不会造成引用异常等错误，它使得 UniRx 的使用更加安全。

## UniRx 的基本语法格式

前面介绍了 UniRx 的 Timer、Update 和 AddTo 这三个API，但是没有介绍过代码。接下来看一看 UniRx 的基本语法。

`Observable.XXX().Subscribe()` 是非常典型的 UniRx 格式。

### 关键字

Observable: 可观察的，形容词，形容后边的词(Timer) 是可观察的，我们可以粗暴地把 Observable 后边的理解成发布者。

Timer: 定时器，名词，被 Observable 描述，所以是发布者，是事件的发送方。

Subscribe: 订阅，动词，订阅谁呢？当然是前边的 Timer，这里可以理解成订阅者，也就是事件的接收方。

连起来则是:可被观察(监听)的.Timer( ).订阅( ) -> 订阅可被观察的定时器。

逻辑关系：

- Timer 是可观察的。
- 可观察的才能被订阅

但是 UniRx 的侧重点，不是发布者和订阅者这两个概念如何使用，而是事件从发布者到订阅者之间的过程如何处理。

所以两个点不重要，重要的是两点之间的线，也就是事件的传递过程，接下来将逐步了解这个过程。

## 操作符 Where

Where 可以理解为一个条件语句，相当于 if，用于过滤掉不满足的条件。请观察下列代码的差异：

```cs
Observable.EveryUpdate()
          .Subscribe(_ =>
            {
              if (Input.GetMouseButtonUp(0))
              {
                 // do something
              }
            })
          .AddTo(this);
```

使用 Where 操作符如下：

```cs
Observable.EveryUpdate()
          .Where(_ =>  )
          .Subscribe(_ => Input.GetMouseButtonUp(0))
            {
                 // do something
            })
          .AddTo(this);
```

上面这段代码可以理解为：

1. EveryUpdate 是事件的发布者。他会每帧会发送一个事件过来。
2. Subscribe 是事件的接收者，接收的是 EveryUpdate 发送的事件。
3. Where 则是在事件的发布者和接收者之间的一个过滤操作。会过滤掉不满足条件的事件。

通过两张图可以更容易的理解。

![unirx-where](/images/2019/05/unirx-where.png)

![unirx-where-2](/images/2019/05/unirx-where-2.png)

事件的本身可以是参数，但是 EveryUpdate 没有参数，所以在 Where 这行代码中不需要接收参数，所以使用 _ 来表示，不用参数。当然 Subscribe 也是⽤用了一个 _ 来接收参数。

## 操作符 First

两张图解决。

![unirx-first](/images/2019/05/unirx-first.png)

![unirx-first-2](/images/2019/05/unirx-first-2.png)

## 对 UGUI 的支持
按钮点击事件注册:

```cs
mButton.OnClickAsObservable()
       .Subscribe(_ =>
         {
           // do something
         });
```
Toggle:

```cs
mToggle.OnValueChangedAsObservable()
       .Subscribe(on =>
         {
           if (on)
           {
           // do something
           }
         });
```

当然 UGUI 的事件也支持操作符。

比如，上述 Toggle 的代码可以简化如下:

```cs
mToggle.OnValueChangedAsObservable()
       .Where(on=>on)
       .Subscribe(on =>
         {
           // do some thing
         });
```

再比如，只能点击一次按钮：

```cs
mButton.OnClickAsObservable()
       .First()
       .Subscribe(_ =>
         {
           // do something
         });
```

不止如此，还⽀支持 EventSystem 的各种 Trigger 接口的监听。
比如：Image 本身是 Graphic 类型的，Graphic 类，只要实现 IDragHandler 就可以进行拖拽事件监听。
但是使用 UniRx 就不用那么麻烦，无需自己实现一些接口。
代码如下:

```cs
mImage.OnBeginDragAsObservable().Subscribe(_=>{}); mImage.OnDragAsObservable().Subscribe(eventArgs=>{}); mImage.OnEndDragAsObservable().Subscribe(_=>{})
```

Unity 的 Event 也是可以使用 AsObservable 进行订阅。
比如：

```cs
UnityEvent mEvent;
void Start()
{
  mEvent.AsObservable()
        .Subscribe(_ =>
        {
          // process event
        });
}
```

上文中 Button 组件的 `OnClickAsObservable` 方法其实是对 `button.onClick()` 这个 UnityEvent 进行订阅，即 `button.onClick().AsObservable`。

## ReactiveProperty

ReactiveProperty，响应式属性，是 UniRx 中一个十分强大的概念，它可以方便的监听一个值是否发生了改变。通常方法，需要在属性的访问器中进行值的监听，以选择不同的语句分支，如下所示：

```cs
public int Age
{
  get { ... }
  set
  {
    if (mAge != value)
    {
      mAge = value; // send event
      OnAgeChanged(); // call delegate
    }
  }
}

public void OnAgeChanged() {}
```

利用 delegate 还可以在类的外部进行监听。

下面再给出一个 UniRx 实现：

```cs
public ReactiveProperty<int> Age = new ReactiveProperty<int>();
void Start()
{
  Age.Subscribe(age =>
  {
    // do age
  });
  Age.Value = 5;
}
```

也可以把 `ReactiveProperty<int>` 替换成 `IntReactiveProperty` 以进行 Json 序列化。

当任何时候，Age 的值被设置，就会通知所有 Subscribe 的回调函数。

而 Age 可以被 Subscribe 多次。

并且同样支持 First、Where 等操作符。

这样可以实现一个叫做 MVP 的架构模式。

也就是在 Ctrl 中，进行 Model 和 View 的绑定。

Model 的所有属性都是用 ReactiveProperty，然后在 Ctrl 中进行订阅。

通过 View 更改 Model 的属性值。

形成一个 View->Ctrl->Model->Ctrl->View 这么一个事件响应环。

## MVP 实现

上文提到过， UniRx 对 UGUI 进行了极大的增强，而这种增强，可以令我们十分简单的实现 MVP 框架。

### UGUI 增强

UniRx 对 UGUI 的增强原理很简单，就是对 UnityEvent 提供了 AsObservable 方法。
代码如下：
