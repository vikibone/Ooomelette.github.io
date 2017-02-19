---
layout:  post
title: 开发小技巧
tags: devtips
categories: devtips
---



1.Object.keys() 方法会返回一个由给定对象的所有可枚举自身属性的属性名组成的数组，数组中属性名的排列顺序和使用for-in循环遍历该对象时返回的顺序一致（两者的主要区别是 for-in 还会遍历出一个对象从其原型链上继承到的可枚举属性）。
可以配合forEach使用，效果更佳。

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys

