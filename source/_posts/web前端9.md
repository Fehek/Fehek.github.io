---
title: web前端学习笔记9
abbrlink: 4f3dd51e
tags:
  - web前端
  - Promise
categories: web前端学习笔记
date: 2021-02-21 21:24:00
index_img:
banner_img:
---

# 了解Promise
Promise是JS中进行异步编程的新解决方案（旧解决方案是单纯使用回调函数）
- 为什么要使用Promise
  1. 指定回调函数的方式更加灵活
  旧的：必须在启动异步任务前指定
  promise：启动异步任务 => 返回promise对象 => 给promise对象绑定回调函数（甚至可以在异步任务结束后指定多个）
  2. 支持链式调用，可以解决回调地狱
  回调地狱：回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调执行的条件。
  回调地狱的缺点：不便于阅读，不便于异常处理
  