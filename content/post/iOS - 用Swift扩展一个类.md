---
title: iOS - 用Swift扩展一个类
date: 2018-04-22
tags: ["iOS"]
---

<!--more-->

_用Swift扩展一个类_   

---------------

### 代码

```swift
// protocol + 泛型
// OC 中我们用 category 扩展原先的类
// SWIFT 用 extention 协议

import UIKit

public final class CIImageKit<Base> {
    public let base: Base
    public init(base: Base) {
        self.base = base
    }
}

// where Base: UIImageView 表示 CIImageKit只适合在 UIImageView调用
extension CIImageKit where Base: UIImageView {
    func setImage(url: URL, placeHolder: UIImage?) {
        // 实现下载图片并缓存展示逻辑
        print("\(url)")
    }
}

public protocol CIImageDownloaderProtocol {
    associatedtype type
    var ci: type { get }
}

/*
 我们声明了一个CIImageDownloaderProtocol协议，对于遵从了该协议的类，
 都有一个CIImageKit类型的对象
 */
extension CIImageDownloaderProtocol {
    public var ci: CIImageKit<Self> {
        get {
            return CIImageKit(base: self)
        }
    }
}

extension UIImageView: CIImageDownloaderProtocol {
    
}

```

### 调用

```swift
let image = UIImageView()
image.ci.setImage(url: URL.init(string: "http://toony.tk")!, placeHolder: nil)
```

