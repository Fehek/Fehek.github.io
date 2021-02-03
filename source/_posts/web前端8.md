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