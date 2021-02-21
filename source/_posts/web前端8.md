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
```bash
# package.json 生成
npm init
# 引入 uniq 
npm install uniq
```

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
```bash
# 全局
npm install browserify -g
# 局部
npm install browserify --save-dev
# --save、-S参数意思是把模块的版本信息保存到 dependencies（生产环境依赖）中，
# 即 package.json 文件的 dependencies 字段中；
# --save-dev、-D参数意思是把模块版本信息保存到 devDependencies（开发环境依赖）中
# 即 package.json 文件的 devDependencies 字段中。
```
下载成功时：
```json
"dependencies": {
  "uniq": "^1.0.1"
}
"devDependencies": {
  "browserify": "^17.0.0"
}
```

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
|-test.html
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
```html
<!-- test.html 里的 script -->
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
- 创建项目结构
```md
-js 
 |-libs 
   |-angular.js 
   |-jquery.js
   |-require.js 
 |-modules 
   |-alerter.js 
   |-dataService.js 
 |-main.js|
-index.html
```
- 代码
require.js, -jquery.js, require.js 里分别引用对应文件
```js
// dataService.js
// 定义没有依赖的模块
define(function () {
  let name = 'dataService.js'
  function getName() {
    return name
  }
  // 暴露模块
  return { getName }
})
```
```js
// alerter.js
// 定义有依赖的模块
define(['dS', 'jquery'], function (dataService, $) {
  let msg = 'alerter.js'
  function showMsg() {
    console.log(msg, dataService.getName())
  }
  $('body').css('background', 'lightblue')
  // 暴露模块
  return { showMsg }
})
```
```js
// main.js
(function () {
  requirejs.config({
    // baseUrl: '', //基本路径
    paths: {  //配置路径
      dS: './modules/dataService', //路径不用加后缀名
      alerter: './modules/alerter',
      jquery: './libs/jquery',
      angular: './libs/angular'
    },
    shim: { //angular 不支持，单独配置
      angular: {
        exports: 'angular'
      }
    }
  });

  requirejs(['alerter', 'angular'], function (alerter, angular) {
    alerter.showMsg()
    console.log(angular)
  })
})()
```
```html
<!-- index.html 里的 script -->
<script data-main="js/main.js" src="js/libs/require.js"></script>
```
- 运行结果
```md
<!-- 背景为淡蓝色 -->
alerter.js dataService.js
{$interpolateMinErr: ƒ, element: ƒ, bootstrap: ƒ, copy: ƒ, extend: ƒ, …}
```


# CMD
Common Module Definition(通用模块定义)
专门用于浏览器端, 模块的加载是异步的。
模块使用时才会加载执行。
## 基本语法
- 定义暴露模块
```js
//定义没有依赖的模块
define(function(require, exports, module){
  exports.xxx = value
  module.exports = value
})

//定义有依赖的模块
define(function(require, exports, module){
  //引入依赖模块(同步)
  var module2 = require('./module2')
  //引入依赖模块(异步)
    require.async('./module3', function (m3) {
      
    })
  //暴露模块
  exports.xxx = value
})
```
- 引入使用模块
```js
define(function (require) {
  var m1 = require('./module1')
  var m4 = require('./module4')
  m1.show()
  m4.show()
})
```


## 实现
[Sea.js](https://seajs.github.io/seajs/docs/)
- 创建项目结构
```md
|-js
  |-libs
    |-sea.js
  |-modules
    |-module1.js
    |-module2.js
    |-module3.js
    |-module4.js
    |-main.js
|-index.html
```
- 代码
```js
// module1.js
// 定义没有依赖的模块
define(function (require, exports, module) {
  let msg = 'module1'
  function foo() {
    return msg
  }
  // 暴露模块
  module.exports = { foo }
})
```
```js
// module2.js
define(function (require, exports, module) {
  let msg = 'module2'
  function bar() {
    console.log(msg)
  }
  module.exports = bar
})
```
```js
// module3.js
define(function (require, exports, module) {
  let data = 'module3'
  function fun() {
    console.log(data)
  }
  exports.module3 = { fun }
})
```
```js
// module4.js
define(function (require, exports, module) {
  let msg = 'module4'
  // 同步引入
  let module2 = require('./module2')
  module2()
  // 异步引入
  require.async('./module3', function (module3) {
    module3.module3.fun()
  })
  function fun2() {
    console.log(msg)
  }
  exports.fun2 = fun2
})
```
```html
<!-- index.html 里的 script -->
<script type='text/javascript' src='js/libs/sea.js'></script>
<script type='text/javascript'>
    seajs.use('./js/modules/main.js');
</script>
```
- 运行结果
```md
module1
module2
module4
module3
```
module3 为异步引入，主程序执行完后执行

# ES6 Module
依赖模块需要编译打包处理
## 语法
- 导出模块 export
- 引入模块 import

## 实现
- 创建项目结构
```md
|-js
  |-src
    |-module1.js
    |-module2.js
    |-module3.js
    |-main.js
|-index.html
|-package.json
|-.babelrc
```

- 准备工作
使用 [Babel](https://www.babeljs.cn/repl) 将ES6编译为ES5代码
1. 定义package.json文件
```json
  {
    "name" : "es6-babel-browserify",
    "version" : "1.0.0"
  }
```
2. 安装 babel-cli, babel-preset-es2015
```bash
# cli: command line interface 命令行接口
# ！！！一定要在管理员权限下安装
npm install babel-cli browserify -g
# preset 预设（将es6转换成es5的所有插件打包）
npm install babel-preset-es2015 --save-dev 
```
3. 定义.babelrc文件
rc 文件（run control 运行时控制文件）
```
{
  "presets": ["es2015"]
}
```
4. 安装 jQuery
```bash
# 安装 jQuery 1.xxx 的最新版本
npm install jquery@1
```

- 代码
```js
// module1.js
// 暴露模块 分别暴露
export function foo() {
  console.log('foo() module1')
}
export function bar() {
  console.log('bar() module1')
}
export let arr = [1, 2, 3, 4, 5]
```
```js
// module2.js
// 统一暴露
function fun() {
  console.log('fun() module2')
}
function fun2() {
  console.log('fun2() module2')
}
export { fun, fun2 }
```
```js
// module3.js
// 默认暴露 
// 可以暴露任意数据类型，暴露什么数据接收到的就是什么数据
// export default value
export default {
  msg: '默认暴露',
  foo() {
    console.log(this.msg)
  }
}
```
```js
// main.js
// 引入其他的模块
// 语法：import xxx from '路径'
import $ from 'jquery'
import { foo, bar } from './module1'
import { fun, fun2 } from './module2'
import module3 from './module3'

$('body').css('background', 'lightblue')
foo()
bar()
fun()
fun2()
module3.foo()
```

- 转换打包
```bash
# 使用Babel将ES6编译为ES5代码(但包含CommonJS语法)
babel js/src -d js/build
# 使用Browserify编译js
browserify js/build/main.js -o js/dist/bundle.js
```
```html
<!-- index.html 里的 script -->
<script type='text/javascript' src='js/dist/bundle.js'></script>
```

- 结果
```md
<!-- 背景为淡蓝色 -->
foo() module1
bar() module1
fun() module2
fun2() module2
```


## 其他

每个模块有自己的作用域内执行
通过import创建的变量相当于const创建，所以不能单独放在等号左边
通过import创建的变量会跟其引入的来源绑定，来源变量发生变化，引入的变量也发生变化
```js
// 导入其它模块的默认导出
import xxx from './xxx.js'

// 导入俱名导出
import { a, b as bb , c as cc} from './x.js'

// 同时导入默认及俱名导出
import a, { b, c, d } from './mod.js'

// 创建默认导出
export default [1, 2, 3, 4]

// 创建俱名导出
export var a = 1
export var b = 2
var c = 3
var d = 4
export { c, d }

//将foo.js的所有导出导入到foo变量
import * as foo from './foo.js'
// foo.default 为默认导出
// foo.xxx 为俱名导出
// 且都为getter setter，但set是无效的

//将a.js的所有导出从当前文件再导出
export * from './a.js'
export * from './b.js'
import xx from yy + '.js'
````
所有的导入导出语句都必须出现在模块文件的最外层，即不能放入任何代码块，如if，函数，循环
所有导入的模块名称必须是静态的，不能有任何运算

