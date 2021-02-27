---
title: web前端学习笔记10
abbrlink: c642559f
tags:
  - web前端
  - NodeJS
categories: web前端学习笔记
date: 2021-02-24 21:06:00
index_img:
banner_img:
---

# 基础
- 环境变量（windows系统中变量）  
当我们在命令行窗口打开一个文件，或调用一个程序时
系统会首先在当前目录下寻找文件程序，如果找到了则直接打开
如果没有找到则会依次到环境变量path的路径中寻找，直到找到为止
如果没找到则报错
所以我们可以将一些经常需要访问的程序和文件的路径添加到path中，这样我们就可以在任意位置来访问这些文件和程序了
- 进程和线程
进程就是一个一个的工作计划。
线程是计算机中最小的计量单位，负责执行进程中的程序。
**JS是单线程，但是在后台拥有一个I/O线程池。**

# node.js简介
Node.js是一个能够在服务器端运行JavaScript的开放源代码、跨平台JavaScript运行环境。
Node采用Google开发的V8引擎运行js代码，使用**事件驱动**、**非阻塞**和**异步I/O模型**等技术来提高性能，可优化应用程序的传输量和规模。

## 模块
[CommonJS模块](http://nodejs.cn/api/modules.html)
具体例子可见[笔记8](./383ae588.html)

## 包
- CommonJS的包规范由包结构和包描述文件两个部分组成。

包结构：用于组织包中的各种文件
包实际上就是一个压缩文件，解压以后还原为目录。符合规范的目录，应该包含如下文件：
```md
– package.json  描述文件
– bin           可执行二进制文件
– lib           js代码
– doc           文档
– test          单元测试
```
包描述文件：描述包的相关信息，以供外部读取分析
包描述文件用于表达非代码相关的信息，它是一个JSON格式的文件 – package.json，位于包的根目录下，是包的重要组成部分。

## NPM
NPM(Node Package Manager)
对于Node而言，NPM帮助其完成了第三方模块的发布、安装和依赖等。借助NPM，Node与第三方模块之间形成了很好的一个生态系统。

指令|操作
:-|:-
npm –v|查看npm的版本 包名|在当前目录安装包
npm r/remove 包名|删除一个模块
npm i 包名 --save|安装包并添加到依赖中
npm install|下载当前项目所依赖的包
npm i 包名 –g|全局安装包（全局安装的包一般都是一些工具）
npm i 文件路径|从本地安装
npm i 包名 –registry=地址|从镜像源安装
npm config set registry 地址|设置镜像源

通过npm下载的包都放到node_modules文件夹中。
通过npm下载的包，直接通过包名引入即可。
node在使用模块名字来引入模块时，它会首先在当前目录的node_modules中寻找是否含有该模块，如果有则直接使用。
如果没有则去上一级目录的node_modules中寻找，如果有则直接使用。
如果没有则再去上一级目录寻找，直到找到为止。
直到找到磁盘的根目录，如果依然没有，则报错。

# Buffer（缓冲区）
[Buffer 文档](http://nodejs.cn/api/buffer.html#buffer)
1. Buffer的结构和数组很像，操作的方法也和数组类似
2. 数组中不能存储二进制的文件，而buffer就是专门用来存储二进制数据
3. 使用buffer不需要引入模块，直接使用即可
4. 在buffer中存储的都是二进制数据，但是在显示都是以16进制的形式显示。
buffer中每一个元素的范围是从 00 - ff / 0 - 255 / 00000000 - 11111111
buffer中的一个元素，占用内存一个字节(byte)

- 使用Buffer保存字符串
```js
const str = "Hello 前端" 
const buf = Buffer.from(str)
// 使用Buffer保存字符串

console.log(buf.length)
// 占用内存的大小
// 12(5+1+3*2)

console.log(str.length) 
//字符串的长度
// 8

console.log(buf)
// Buffer(12) [72, 101, 108, 108, 111, 32, 229, 137, 141, 231, 171, 175]

const buf2 = Buffer.from('我是文本')
console.log(buf2.toString()) //我是文本
// buf.toString() 将缓冲区中的数据转换为字符串
```
- 创建指定大小的Buffer对象
```js
const buf = Buffer.alloc(10)
// 创建一个长度为 10、以零填充的 Buffer

buf[0] = 255
// 会转换成16进制 ff

buf[10] = 1 //不会有改变
// Buffer的大小一旦确定，则不能修改
// Buffer实际上是对底层内存的直接操作

buf[1] = 256 //0
buf[2] = 556 //44
// 会舍弃8位之前的数字
// 556 = 0b1000101100，后8位为0b00101100 = 0x44

consol.log(buf[0]) //255
//只要数字在控制台或页面输出一定是10进制
consol.log(buf[0].toString(2)) //11111111
//转换成别的进制

const buf2 = Buffer.allocUnsafe(10)
// 新创建的 Buffer 的内容是未知的，可能包含敏感数据(不清空数据)
```

# fs（文件系统）
[fs 文档](http://nodejs.cn/api/fs.html)
fs(File System)，文件系统。简单来说就是通过Node来操作系统中的文件。
服务器的本质就是将本地的文件发送给远程的客户端。
1. 要使用fs模块，首先需要对其进行加载 `const fs = require("fs")`
2. fs模块中所有的操作都有两种形式可供选择，同步(Sync)和异步(callback)。
同步文件系统会阻塞程序的执行，也就是除非操作完毕，否则不会向下执行代码。
异步文件系统不会阻塞程序的执行，而是在操作完成时，通过回调函数将结果返回。

## 同步文件的写入
1. 打开文件
```md
fs.openSync(path[, flags, mode])
- path 要打开文件的路径
- flags 打开文件要做的操作的类型
    r 只读的
    w 可写的
- mode 设置文件的操作权限，一般不传
该方法会返回一个文件的描述符作为结果，可以通过该描述符来对文件进行各种操作
```
2. 向文件中写入内容
```md
fs.writeSync(fd, string[, position[, encoding]])
- fd 文件的描述符，需要传递要写入的文件的描述符
- string 要写入的内容
- position 写入的其实位置
- encoding 写入的编码，默认utf-8
```
3. 保存并关闭文件
```md
fs.closeSync(fd)
- fd 要关闭文件的描述符
```

```js
var fs = require('fs')

// 打开文件
var fd = fs.openSync('hello.txt', 'w')
// 向文件中写入内容
fs.writeSync(fd, '今天天气真好。', 2)
// 关闭文件
fs.closeSync(fd)
```
## 异步文件的写入
1. 打开文件
```md
fs.open(path[, flags[, mode]], callback)
异步调用的方法，结果都是通过回调函数的参数返回的
回调函数两个参数
err 错误对象，如果没有错误则为null
fd 文件的描述符
```
2. 异步写入一个文件
```md
fs.write(fd, string[, position[, encoding]], callback)
```
3. 关闭文件
```md
fs.close(fd, callback)
```

```js
var fs = require('fs')

// 打开文件
fs.open('hello2.txt', 'w', function (err, fd) {
  // 判断是否出错
  if (!err) {
    // 如果没有出错，则对文件进行写入操作
    fs.write(fd, '这是异步写入的内容', function (err) {
      if (!err) {
        console.log('写入成功')
      }
      // 关闭文件
      fs.close(fd, function (err) {
        if (!err) {
          console.log('文件已关闭')
        }
      })
    })
  } else {
    console.log(err)
  }
})
console.log('程序向下执行')
```