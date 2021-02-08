---
title: web前端学习笔记8
abbrlink: 383ae588
tags:
  - web前端
  - 模块化
categories: web前端学习笔记
date: 2021-02-03 20:09:00
index_img:
banner_img:
---

# 介绍
在浏览器用script标签加载很多模块是不可行的。
JS语言里没有模块系统。
- 为什么要模块系统？
模块化开发便于维护，也便于解耦。
不同功能模块的代码放在不同的文件及文件夹里甚至放在不同的软件包里。

- 实现好的模块系统在浏览器中实际上不可用
因为浏览器如果还一个一个加载模块文件，就太慢了。
从网络上加载大量的小文件总体速度是很慢的，而在所有模块加载完成之前，模块系统是不会开始执行入口模块的代码的，进而相当于不执行任何模块代码，也就是页面中的功能都不可用。
视模块数量、依赖关系以及网络速度，这个不可用的时间可能很久，甚至可能长达10秒以上，所以需要将所有的模块打包成一整个文件。
一个10M的文件比1000个10k的文件下载速度快。

- 打包原理即为浏览器中异步加载模块的基本原理
从入口文件开始，加载并缓存所有模块的依赖，等所有的依赖都加载完成的时候，构建好的对象其实就是打包结果的一部分，将其跟require函数一起输出为一个文件，即为打包结果。

# CommonJS
## 基本语法
- 暴露模块
`module.expoers = value`
`exports.xxx = value`
暴露的模块本质是 exports 对象
- 引入模块
`require(xxx)`
第三方模块：xxx为模块名
自定义模块：xxx为模块文件路径

## 实现
服务端实现：[Node.js](https://nodejs.org/)
- 创建项目结构
```md
|-modules
  |-module1.js
  |-module2.js
  |-module3.js
|-app.js
|-package.json
```
package.json 用`npm init`生成
引入 uniq `npm install uniq`

- 代码
```js
// module1.js 文件
// module.expoers = value 暴露一个对象
module.exports = {
  msg:'module1',
  foo(){
    console.log(this.msg)
  }
}
```
```js
// module2.js 文件
// 暴露一个函数 module.exports = function(){}
module.exports = function(){
  console.log('module2')
}
```
```js
// module3.js 文件
// exports.xxx = value
exports.foo = function(){
  console.log('foo() module3')
}

exports.bar = function(){
  console.log('bar() module3')
}

exports.arr = [2,3,4,5,4,5,1,11]
```
```js
// 将其它模块汇集到主模块
let uniq = require('uniq')

let module1 = require('./modules/module1')
let module2 = require('./modules/module2')
let module3 = require('./modules/module3')

module1.foo()
module2()

module3.foo()
module3.bar()

let result = uniq(module3.arr)
console.log(result)
```

- 运行结果
```md
module1
module2
foo() module3
bar() module3
[ 1, 11, 2, 3, 4, 5 ]
```

---
浏览器实现：[Browserify](http://browserify.org/)
- 创建项目结构
```md
|-js
  |-dist //打包生成文件的目录
  |-src //源码所在的目录
    |-module1.js
    |-module2.js
    |-module3.js
    |-app.js //应用主源文件
|-index.html
|-package.json
```
- 下载 browserify
全局: `npm install browserify -g`
局部: `npm install browserify --save-dev`
下载成功时：
```json
"dependencies": {
  "uniq": "^1.0.1"
}
"devDependencies": {
  "browserify": "^17.0.0"
}
```
\--save、-S参数意思是把模块的版本信息保存到 dependencies（生产环境依赖）中，即 package.json 文件的 dependencies 字段中；
\--save-dev、-D参数意思是把模块版本信息保存到 devDependencies（开发环境依赖）中，即 package.json 文件的 devDependencies 字段中。

- 代码
app.js, module1.js, module2.js, module3.js 和上面的一样

- 打包处理 js
`browserify js/src/app.js -o js/dist/build.js`
`-o` output，输出。它之前为原始路径，之后为处理之后的路径。

- 页面引入
在 index.html 中引入打包处理之后的文件
`<script type="text/javascript" src="./js/dist/build.js"></script>`

# AMD
Asynchronous Module Definition(异步模块定义)
专门用于浏览器端, 模块的加载是异步的。
## 基本语法
- 定义暴露模块
```js
//定义没有依赖的模块
define(function(){
  return 模块
})
//定义有依赖的模块
define(['module1', 'module2'], function(m1, m2){
  return 模块
})
```
- 引用使用模块
```js
require(['module1', 'module2'], function(m1, m2){
  使用m1/m2
})
```


## 实现
### 未使用AMD
- 创建项目结构
```md
|-js
  |-alerter.js
  |-dataService.js
|-app.js
|-test1.html
```
- 代码
```js
// alerter.js
// 定义一个有依赖的模块
(function (window, dataService) {
  let msg = 'alerter.js'
  function showMsg() {
    console.log(msg, dataService.getName())
  }
  window.alerter = { showMsg }
})(window, dataService)
```
```js
// dataService.js
// 定义一个没有依赖模块
(function (window) {
  let name = 'dataService.js'
  function getName() {
    return name
  }
  window.dataService = { getName }
})(window)
```
```js
// app.js
(function (alerter) {
  alerter.showMsg()
})(alerter)
```
```js
// test1.html 里的 script
<script type="text/javascript" src="./js/dataService.js"></script>
<script type="text/javascript" src="./js/alerter.js"></script>
<script type="text/javascript" src="./app.js"></script>
```
- 运行结果
```md
alerter.js dataService.js
```


### 使用AMD
[Require.js](http://101.132.137.99/home.html)




# es modules

每个模块有自己的作用域内执行
通过import创建的变量相当于const创建，所以不能单独放在等号左边
通过import创建的变量会跟其引入的来源绑定，来源变量发生变化，引入的变量也发生变化

导入其它模块的默认导出
import xxx from './xxx.js'
导入俱名导出
import { a, b as bb , c as cc} from './x.js'
同时导入默认及俱名导出
import a, { b, c, d } from './mod.js'
创建默认导出
export default [1, 2, 3, 4]
创建俱名导出
export var a = 1
export var b = 2
var c = 3
var d = 4
export { c, d }

import * as foo from './foo.js'//将foo.js的所有导出导入到foo变量
foo.default 为默认导出
foo.xxx 为俱名导出
且都为getter setter，但set是无效的

export * from './a.js'//将a.js的所有导出从当前文件再导出
export * from './b.js'
import xx from yy + '.js'

所有的导入导出语句都必须出现在模块文件的最外层，即不能放入任何代码块，如if，函数，循环
所有导入的模块名称必须是静态的，不能有任何运算

