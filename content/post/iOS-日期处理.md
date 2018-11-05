---
title: iOS - 日期处理
date: 2018-02-28
tags: ["iOS"]
---

## **日期格式**
---
**年**
* y 将年份(0~9)显示为不带前导零的数字
* yy 已带前导零的两位数字格式化显示年份
* yyy / yyyy 以四位数字格式显示年份
---
**月**
* M 将月份显示为不带前导零的数字(eg: 一月 即 1)
* MM 将月份显示为带前导零的数字(eg: 01/12/01)
* MMM 将月份显示为缩写形式(eg: Jan)
* MMMM 将月份显示为完整月份名(eg: January)
---
**日**
* d (eg: 1)
* dd (eg: 01)
---
**星期**
* EEE (eg: Sun)
* EEEE (eg: Sunday)
---
**小时**
* h 12小时制 (eg: 1:15:15 PM)
* hh 12小时制 (eg: 01:15:15 PM)
* H 24小时制 (eg: 1:15:15)
* HH 24小时制度 (eg: 13:15:15)
---
**分钟**
* m (eg: 12:1:15)
* mm (eg: 12:01:15)
---
**秒**
* s (eg: 12:15:5)
* ss (eg: 12:15:05)
* f 显示秒的小数部分
* ff 将精确到显示百分之一秒
* ffff 将精确显示到万分之一秒
* 用户定义的格式最多可以使用七个f
---
**上午下午**
* t 使用12小时制
* 中午之前用 A
* 中午之后用 P
* tt 使用12小时制
* 中午之前用 AM
* 中午之后用 PM
* 注意: 对于使用24小时制的不现实任何字符
---
**时区**
* z 显示不带前导零的时区偏移量
* zz (eg: -08)
* zzz 显示完整的时区偏移量 (eg: -0800)
---
**纪元**
* gg 显示时代/纪元字符串 (eg: A.D.)
---
## **时间转换**

```swift
  // 日期转字符串
  static func tn_dateString(delta: TimeInterval) -> String {
    
        let date = Date(timeIntervalSinceNow: delta)
        // 指定日期格式
        dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
        return dateFormatter.string(from: date)
        
    }
    
  //字符串转日期
  static func tn_sinaDate(string: String) -> Date? {
      
      // 1. 设置日期格式
      dateFormatter.dateFormat = "EEE MMM dd HH:mm:ss zzz yyyy"
      
      // 2. 转换并且返回日期
      return dateFormatter.date(from: string)
  }
```