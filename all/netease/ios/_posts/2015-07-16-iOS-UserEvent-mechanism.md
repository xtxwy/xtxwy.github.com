---
published: true
layout: default
---

## 总述

在iOS设备中，当用户发起一个事件时，比如_触屏或者晃动_，系统会产生一个事件，
同时将事件发给_UIApplication_，然后UIApplication将此事件传给特定的_UIWindow_，
最后UIWindow将此事件传给具体的_对象_（即：first responder）进行处理。