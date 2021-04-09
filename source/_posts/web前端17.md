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
# 基本了解
## 认识Vue3
1. 相关信息
    - Vue3支持vue2的大多数特性
    - 更好的支持Typescript
2. 性能提升
    - 打包大小减少，初次渲染和更新渲染变快，内存减少54%
    - **使用Proxy代替defineProperty实现数据响应式**
    - **重写虚拟DOM的实现和Tree-Shaking**
3. 新增特性
    - **Composition (组合) API**
    - setup
        - ref 和 reactive
        - computed 和 watch
        - 新的生命周期函数
        - provide与inject
        - ...
    - 新组件
        - Fragment - 文档碎片
        - Teleport - 瞬移组件的位置
        - Suspense - 异步加载组件的loading界面
    - 其它API更新
        - 全局API的修改
        - 将原来的全局API转移到应用对象
        - 模板语法变化

## 创建
1. 使用vue-cli
```bash
## 安装或者升级
npm install -g @vue/cli
## 保证 vue cli 版本在 4.5.0 以上
vue --version
## 创建项目
vue create my-project
```
接下来的步骤：
Please pick a preset - 选择 Manually select features
Check the features needed for your project - 选择上 TypeScript ，按空格是选择，回车是下一步
Choose a version of Vue.js that you want to start the project with - 选择 3.x (Preview)
之后全部回车
2. 使用vite
    - Vite 是一个 web 开发构建工具，由于其原生 ES 模块导入方式，可以实现闪电般的冷服务器启动。
    - 它做到了**本地快速开发启动**, 在生产环境下基于 Rollup 打包。
        - 快速的冷启动，不需要等待打包操作；
        - 即时的热模块更新，替换性能和模块数量的解耦让更新飞起；
        - 真正的按需编译，不再等待整个应用编译完成，这是一个巨大的改变。
    - 使用 npm
    ```bash
    npm init @vitejs/app <project-name>
    cd <project-name>
    npm install
    npm run dev
    ```

# 





























# Vue3.0和2.0的区别
- 支持Teleport，类似React.createPortal
- 支持组合式API，类似React Hooks，但原理完全不同
    - VCA的setup函数只运行一次，没有闭包陷阱。VCA是通过返回可监控对象来实现的更新，所以VCA没有useCallback, useMemo之类的函数
    - React Hooks，组件实例每次更新都会运行一次组件对应的函数，每次运行对应的函数相应的useState等hook函数都会返回新状态值
- 对数据响应式监控使用的不是gettter，setter，而是代理对象Proxy
- 性能更高：通过只更新数据变化了的部分的DOM来提升性能
    - 原理是能够监控到数据在DOM中的哪位位置用过，数据变的时候只更新对应位置的DOM
    - 在特定情况下不做整个组件树的diff
