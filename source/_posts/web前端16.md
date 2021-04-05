---
title: web前端学习笔记16——Vue3
subtitle: Vue3
abbrlink: ae93be73
tags:
  - front-end
  - Vue
categories: web前端学习笔记
date: 2021-04-01 21:31:00
index_img:
banner_img:
---

# TypeScript
## 特点
- 始于JavaScript，归于JavaScript
TypeScript 可以编译出纯净、 简洁的 JavaScript 代码，并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或更高版本）的JavaScript 引擎中。
- 强大的类型系统
类型系统允许 JavaScript 开发者在开发 JavaScript 应用程序时使用高效的开发工具和常用操作比如静态检查和代码重构。
- 先进的 JavaScript
TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件。

## 安装配置
```bash
# 安装
npm install -g typescript
# 检查是否安装成功
tsc -v
````
ts的文件中如果直接书写js语法的代码，那么在html文件中直接引入ts文件，在谷歌浏览器中是可以直接使用的
如果ts文件中有了ts的语法代码，那么就需要把这个ts文件编译成js文件，在html文件中引入js的文件来使用 `tsc ./xxx.ts`
ts文件中的函数中的形参，如果使用了某个类型进行修饰，那么最终在编译的js文件中是没有这个类型的
ts文件中的变量使用的是let进行修饰，编译的js文件中的修饰符救变成了var
- vscode自动编译
```md
1. 生成配置文件tsconfig.json
    tsc --init
2. 修改tsconfig.json配置
    "outDir": "./js",  // 输出默认文件夹
    "strict": false,   // 严格模式   
3. 启动监视任务
    终端 -> 运行任务 -> 监视tsconfig.json
```

## 使用
- 类型注解
TypeScript 里的类型注解是一种轻量级的为函数或变量添加约束的方式
```ts
function greeter (person: string) {
  return 'Hello, ' + person
}
let user = 'World'
console.log(greeter(user))
```
- 接口
```ts
interface Person {
  firstName: string
  lastName: string
}
function greeter (person: Person) {
  return 'Hello, ' + person.firstName + ' ' + person.lastName
}
let user = {
  firstName: 'F',
  lastName: 'K'
}
console.log(greeter(user))
```
- 类
```ts
// 定义一个类型
class User {
  // 定义公共的字段（属性）
  fullName: string
  firstName: string
  lastName: string
  // 定义一个构造器函数
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName
    this.lastName = lastName
    this.fullName = firstName + ' ' + lastName
  }
}

// 定义一个接口
interface Person {
  firstName: string
  lastName: string
}

// 定义一个函数
function greeter(person: Person) {
  return 'Hello, ' + person.firstName + ' ' + person.lastName
}

// 实例化对象
const user = new User('F', 'K')
console.log(greeter(user))
```
TypeScript 里的类只是一个语法糖，本质上还是 JavaScript 函数的实现。

## webpack打包TS



# Vue3.0和2.0的区别
- 支持Teleport，类似React.createPortal
- 支持组合式API，类似React Hooks，但原理完全不同
    - VCA的setup函数只运行一次，没有闭包陷阱。VCA是通过返回可监控对象来实现的更新，所以VCA没有useCallback, useMemo之类的函数
    - React Hooks，组件实例每次更新都会运行一次组件对应的函数，每次运行对应的函数相应的useState等hook函数都会返回新状态值
- 对数据响应式监控使用的不是gettter，setter，而是代理对象Proxy
- 性能更高：通过只更新数据变化了的部分的DOM来提升性能
    - 原理是能够监控到数据在DOM中的哪位位置用过，数据变的时候只更新对应位置的DOM
    - 在特定情况下不做整个组件树的diff
