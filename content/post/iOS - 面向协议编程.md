---
title: iOS - 面向协议编程
date: 2019-01-09
tags: ["iOS"]
---

<!--more-->

_Swift面向协议编程_   

---------------

> # Demo

<center>

![面向协议编程.gif](/resources/20190109/9246F2094271C2B7FE767B47122B95EB.gif)
</center>

### 1. 定义协议
* 与Label相关的协议

```swift
protocol TitlePresentable {
    var title: String { get }
    var titleColor: UIColor { get }
    var titleFont: UIFont { get }
    
    func updateTitleLabel(label: UILabel)
}
```

* 与UIImageView相关的协议

```swift
protocol ThumbnailPresentable {
    var thumbnailUrl: String { get }
    var thumbnailHandler: (() -> Void)? { get }
    
    func updateImageView(imageView: UIImageView)
}
```

### 2. 在ViewModel中继承这两个组合后的协议 NewsPresentable

```swift
typealias NewsPresentable = TitlePresentable & ThumbnailPresentable

struct NewsViewModel: NewsPresentable {
 
    var title: String
    var titleColor: UIColor
    var thumbnailUrl: String
    var thumbnailHandler: (() -> Void)?
    
    init(news: News, thumbnailHandler:@escaping (() -> Void)) {
        self.title = news.title
        if news.titleColor == "orange" {
            self.titleColor = UIColor.orange
        } else {
            self.titleColor = UIColor.black
        }
        self.thumbnailUrl = news.thumbnailUrl
        self.thumbnailHandler = thumbnailHandler
    }
}
```

### 3. 在View中，根据传入的ViewModel，实际是NewsPresentable，这里就把面向协议编程的好处体现的淋漓尽致了

```swift
typealias NewsPresentable = TitlePresentable & ThumbnailPresentable

struct NewsViewModel: NewsPresentable {
 
    var title: String
    var titleColor: UIColor
    var thumbnailUrl: String
    var thumbnailHandler: (() -> Void)?
    
    init(news: News, thumbnailHandler:@escaping (() -> Void)) {
        self.title = news.title
        if news.titleColor == "orange" {
            self.titleColor = UIColor.orange
        } else {
            self.titleColor = UIColor.black
        }
        self.thumbnailUrl = news.thumbnailUrl
        self.thumbnailHandler = thumbnailHandler
    }
}
```

### 4. 调用
```swift
// 仿一个Model
let news = News(title: "谢谢点击，我的笑容够灿烂吧!", thumbnailUrl: "2.png", titleColor: "black")
// 根据Model，仿一个 ViewModel
let newsViewModel = NewsViewModel(news: news) {
    print("闭包")
}
// 传入这个实现了 NewsPresentable 协议的 ViewModel
newsView.updateWithPresenter(presenter: newsViewModel)
```

### 5. 分析
* 通过继承不同的协议实现不能的UI展现，可以即插即拔
* 代码更加复用，即其他类或struct也可以继承这些写好的和实现的协议
* 可以扩展新的功能，只需要多继承一个协议
* UI的改动对代码影响也很小

> # 需要了解的协议知识点

### 1. 协议可以干什么
* 继承: 协议之间可以相互继承
* 组装
* 扩展
* 作用在 class, structs, enums上

### 2. 协议里面可以有什么
* 不支持存储性属性， 可以是计算性属性
* 构造函数
* 类方法
* 实例方法

### 3. Class 与 Struct & Enum
* Class 是指针（引用）类型， Struct是值类型
* 值类型， 把B赋给A， A与B是完全不同；其实就是复制了一份
* 引用类型的缺点，如果存在多个变量引用一个对象，只要其中一个改动，其他都会被改

### 4. 协议的扩展
* 默认实现
* 让一个已经存在的类型去实现这个协议
* 声明一个协议的实现
  - 前提可能某个类或结构体已经实现了协议里面的所有方法
  - 如果需要用到这个协议的实现方法，只需要在这个类或者结构体extension下（里面的方法可以为空，因为已经全部实现了）
* 加约束
  - 譬如，只让协议在 UIViewController里面实现
* 泛型的协议 (AssociatedTypes)
  - 不用具体指明是什么类型

### 6. 项目地址
[面向协议编程](https://github.com/ihuan/iOS-StudyDemo/tree/master/%E9%AB%98%E7%BA%A7%E5%BC%80%E5%8F%91/01-%E9%9D%A2%E5%90%91%E5%8D%8F%E8%AE%AE%E7%BC%96%E7%A8%8B/)