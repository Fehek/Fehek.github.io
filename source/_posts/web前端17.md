---
title: web前端学习笔记17——Vue3
subtitle: Vue3
abbrlink: 4d268486
tags:
  - front-end
  - Vue
categories: web前端学习笔记
date: 2021-04-06 22:11:00
index_img:
banner_img:
---
# Vue3.0和2.0的区别
- 支持Teleport，类似React.createPortal
- 支持组合式API，类似React Hooks，但原理完全不同
    - VCA的setup函数只运行一次，没有闭包陷阱。VCA是通过返回可监控对象来实现的更新，所以VCA没有useCallback, useMemo之类的函数
    - React Hooks，组件实例每次更新都会运行一次组件对应的函数，每次运行对应的函数相应的useState等hook函数都会返回新状态值
- 对数据响应式监控使用的不是gettter，setter，而是代理对象Proxy
- 性能更高：通过只更新数据变化了的部分的DOM来提升性能
    - 原理是能够监控到数据在DOM中的哪位位置用过，数据变的时候只更新对应位置的DOM
    - 在特定情况下不做整个组件树的diff
