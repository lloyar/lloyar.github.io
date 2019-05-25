---
layout: article
title: UniRx Learning（一）
date: '2019-05-25 10:06:00 +08:00'
key: '2019-05-23_15:05'
tags:
  - Language
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

```cs
public Button mButton;

void Start()
{
    mButton.onClick.AsObservable().Subscribe(_ => Debug.Log("clicked"));
}
```

然后我们再看一下 `onClick` 的定义：

```cs
public Button.ButtonClickedEvent onClick
{
  get
  {
    return this.m_OnClick;
  }
  set
  {
    this.m_OnClick = value;
  }
}
```

然后是 `Button.ButtonClickedEvent` 的定义：

```cs
public class ButtonClickedEvent : UnityEvent
{
}
```

在此基础上，进一步对每一个 UGUI 控件进行封装，从而可以像这种方式在 UGUI 中使用 UniRx  `mButton.OnClickAsObservable().Subscribe(_ => Debug.Log("clicked"))`，其实它等同于 `mButton.onClick.AsObservable().Subscribe(_ => Debug.Log("clicked"))`。

使用 UniRx 可以很容易地实现 MVP（MVRP）设计模式。
MVP 的结构图如下所示：

![unirx-mvp](/images/2019/05/unirx-mvp.png)

虽然在 Unity 里无法真的实现 Model-View 绑定，但 Observables 可以通知订阅者，功能上也差不多。这种模式叫 Reactive Presenter：

```cs
public class ReactivePresenter : MonoBehaviour
{
// Presenter is aware of its View (binded in the inspector)
    public Button mButton;

    public Toggle mToggle;

// State-Change-Events from Model by ReactiveProperty
    Enemy enemy = new Enemy(1000);

    void Start()
    {
// Rx supplies user events from Views and Models in a reactive manner
        mButton.OnClickAsObservable().Subscribe(_ => enemy.CurrentHp.Value -=
            99);
        mToggle.OnValueChangedAsObservable().SubscribeToInteractable(mButton);
// Models notify Presenters via Rx,and Presenters update their views
        enemy.CurrentHp.SubscribeToText(GetComponent<Text>());
        enemy.IsDead.Where(isDead => isDead)
            .Subscribe(_ => { mToggle.interactable = mButton.interactable = false; });
    }
}

// The Mode. All property notify when their values change
public class Enemy
{
    public ReactiveProperty<long> CurrentHp { get; private set; }
    public IReadOnlyReactiveProperty<bool> IsDead { get; private set; }

    public Enemy(int initialHp)
    {
        // Declarative Property
        CurrentHp = new ReactiveProperty<long>(initialHp);
        IsDead = CurrentHp.Select(x => x <= 10).ToReactiveProperty();
    }
}
```

在 Unity 中，我们把 Scene 中的 GameObject 当做视图层，这些是在 Unity 的 Hierarchy 中定义的。
展示/控制层在 Unity 初始化时将视图层绑定。
SubscribeToText and SubscribeToInteractable 都是简洁的类似绑定的辅助函数。

View -> ReactiveProperty -> Model -> RectiveProperty - View 完全⽤响应式的⽅式连接。UniRx 提供
了所有的适配⽅法和类，不过其他的 MVVM (or MV*) 框架也可以使⽤。UniRx/ReactiveProperty 只是
⼀个简单的⼯具包。

## 操作符 Merge

很简单，作用是将多个事件流合并为一个。

```cs
var leftMouseClickStream = Observable.EveryUpdate().Where(_ => Input.GetMouseButtonDown(0));
var rightMouseClickStream = Observable.EveryUpdate().Where(_ => Input.GetMouseButtonDown(1));
Observable.Merge(leftMouseClickStream, rightMouseClickStream)
          .Subscribe(_ =>
          {
            // do something
          });
```

以上代码的实现的逻辑是“当⿏标左键或右键点击时都会进⾏处理”。

也就是说，Merge 操作符将 leftMouseClickStream 和 rightMouseClickStream 合并成了⼀个事件流。

如下图所示:

![unirx-merge](/images/2019/05/unirx-merge.png)

## 操作符 Select

`Select` 操作符原本是 LINQ 中的操作符，是选择的意思，⼀般是传⼊⼀个索引 i/index 然后根据索引返回具体的值，请看如下代码：

```cs
var testNumbers = new List<int>(){ 1,2,3}
var selectedValue = testNumbers[2];
```

但是 `Select` 本质，其实是进行了一次 **映射** 操作，它将一个变量 `x`，映射成了 `List<T>[x]`。这其实是函数式编程的思维。如果把 `Select` 理解为 **映射变换** ，而不单单只是选择列表中的某些元素，那在 UniRx 内，`Select` 操作符的意义就能和 LINQ 中的意义进行统一了。在 UniRx 中的 `Select`，请看下图：

![unirx-select](/images/2019/05/unirx-select.png)

# 结束语

到目前位置，我们只谈到了 UniRx 一个极为简短的概括，和它具体实施起来的一些操作。这是我的学习资料导致的，我本人其实更加倾向于写概括性的东西，尤其是在学习的开始阶段，对于整体的把握，我认为要胜过对具体的把握。所以在文章的最后，我到 [ReactiveX](http://reactivex.io/) 官方网站翻了翻文档，他们给出了一个极为全面并且重要的[概述](http://reactivex.io/intro.html)，简洁的表达了 ReactiveX 的核心思想，以及几个重要组件。我认为十分有必要翻阅，并且仔细研读。
