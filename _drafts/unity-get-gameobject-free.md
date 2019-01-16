---
layout:     article
title:      Unity Get GameObject free
date:       '2019-01-13 23:04:00 +08:00'
key:        '2019-01-13_23:04'
tags:
    - null
---

Overview

<!--more-->


## Unity3D之随心所欲的获取对象

### 位置修改

``` cs
player.transform.position = new Vector3( Camera.main.transform.position.x, Camera.main.transform.position.y+3);
player.GetComponent<Rigidbody2D> ().velocity = new Vector2 (0,0);
player.transform.localRotation = Quaternion.Euler(0,0,0);
```

### 对象、子对象、孙子对象的获取

``` cs
GameObject Obj = GameObject.Find ("对象名");　　//用于找hierarchy下的第一层对象
Transform[] AllChildrenRTran = parent.transform.GetComponentInChildren();//用来找多个对象或者不知道具体名字的对象
foreach(Transform childTran in AllChildrenTran) {
  GameObject childObj = childTran.gameObject;　　
}
```

### 知道parent对象的子对象的子对象名字叫 name2 ,就可以这样直截了当的获取了，就是这样毫无人性，注意没有双引号

``` cs
parent.transform.FindChild(name1/name2).gameObject;
```

### 获取组件、组件属性　　

``` cs
Vector2D speed =  player.GetComponent<Rigidbody2D> ().velocity;　　//还是这个例子，获得刚体的速度
```

### 脚本获取游戏对象

`脚本文件类对象.transform.gameobject`

### 脚本文件获取被绑定对象的子对象，这个方法也是让人耳目一新

`脚本文件类对象.transform.GetChild(i)`  //i是指第几个子对象，子对象个数用 -> 脚本文件类对象.transform.childCount <- 表示
