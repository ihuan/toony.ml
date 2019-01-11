---
title: Swift - Struct妙用之值存储
date: 2018-05-03
tags: ["iOS","Swift"]
---

<!--more-->

_Struct妙用之值存储_   

---------------

> 理解概念

- struct与class的区别是struct是一个值类型，只要结构中的值发生改变，都是Copy，当然苹果对这种Copy是有优化的，不用担心会影响性能，比如只Copy改动的值； 而class是指针（引用）类型，分配在栈
- 因为calss是指针类型，当用多核CPU做并发操作时，就会产生线性不安全；而 struct 因为是Value-Type， 所以不会出现这样的问题。这也就是swift函数式编程得到推广的原因
- 因为是值类型，我们可以直接用来存储。

> Demo

- 这个例子是当一个玩家 Player 对Health的积分改动后，直接将值用 UserDefaults 存储
- 注意 Player 和 Health 都是结构体

```swift
public struct Health {
    public var points: Int = 100
    
    public init(dictionary: [String:AnyObject]) {
        points = (dictionary["points"] as? Int) ?? 100
    }
    
    public func serialize() -> [String:AnyObject] {
        return ["points": points as AnyObject]
    }
}

public struct Player {
    public var name: String = ""
    public var health: Health
}

extension Player {
    public init(dictionary: [String:AnyObject]) {
        let healthDict = dictionary["health"] as? [String:AnyObject]
        health = Health(dictionary: healthDict ?? [:])
        name = (dictionary["name"] as? String) ?? ""
    }
    
    func serialize() -> [String:AnyObject] {
        return ["name": name as AnyObject, "health": health.serialize() as AnyObject]
    }
}
```
- 存储

```swift
userDefaults.set(player.serialize() as Any, forKey: "player")
```

> 其他新得

- 协议的妙处,我们可以让已经实现了 objectForKey 和 setValueForKey 的 UserDefaults 遵循 Storage协议
- 让已经实现的类的方法，只暴露出想要的方法（即协议中的方法)

```swift
public protocol Storage {
    func object(forKey defaultName: String) -> Any?
    func set(_ value: Any?, forKey defaultName: String)
    func synchronize() -> Bool
}

extension UserDefaults: Storage { }
```

> 项目地址

[struct妙用之值存储](https://github.com/ihuan/iOS-StudyDemo/tree/master/%E9%AB%98%E7%BA%A7%E5%BC%80%E5%8F%91/02-struct%E5%A6%99%E7%94%A8%E4%B9%8B%E5%80%BC%E5%AD%98%E5%82%A8/Tony_GamePlayground)