---
title: iOS - 核心动画 (CAAnimation)
date: 2018-03-28
tags: ["iOS"]
---

<!--more-->

_核心动画(T1)_   

---------------

### 1. 位置

<center>

![Core Animation.png](./post/resources/F9AB39A750CC347315E7438BBF54A351.png)
</center>

* 核心动画位于UIKit的下一层，相比UIView动画，它可以实现更复杂的动画效果。
* UIView动画可以看成是对核心动画的封装，和UIView动画不同的是，通过核心动画改变layer的状态（比如position），动画执行完毕后实际上是没有改变的（表面上看起来已改变）
* 我是

### 2. 优点
* 性能强大，使用硬件加速，可以同时向多个图层添加不同的动画效果。
* 接口易用，只需要少量的代码就可以实现复杂的动画效果。
* 运行在后台线程中，在动画过程中可以响应交互事件（UIView动画默认动画过程中不响应交互事件。

### 3. 层次结构
<center>

![Core Animation struct.png](./post/resources/C494D043B0116EF7B0242ED303681D75.png)
</center>

* CAAnimation: 所有动画对象的父类，实现CAMediaTiming协议，负责控制动画的时间、速度和时间曲线等等，是一个抽象类，不能直接使用。
* CAPropertyAnimation 是CAAnimation的子类，它支持动画地显示图层的keyPath，一般不直接使用
* CASpringAnimation iOS9.0之后新增，它实现弹簧效果的动画，是CABasicAnimation的子类。

### 4. 例子

```swift
    // MARK: - position 位置
    func position() {
        let anim = CABasicAnimation(keyPath: "position")
        anim.toValue = NSValue(cgPoint: redLable.center)
        anim.isRemovedOnCompletion = false
        anim.fillMode = .forwards
        anim.timingFunction = CAMediaTimingFunction(name: .easeInEaseOut)
        purpleLable.layer.add(anim, forKey: "PositionAni")
    }
```

* 初始化 CAAnimation
* 设置动画的相关属性，时间，曲线，keyPath目标，代理等等
* 添加动画