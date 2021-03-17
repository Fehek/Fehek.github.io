---
title: web前端学习笔记11——NodeJS
subtitle: NodeJS
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

## 同步文件
### 写入
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
- fd 文件的描述符，通过openSync()获取
- string 要写入的内容
- position 写入的起始位置
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
### 读取
`fs.readSync(fd, buffer, [options])`

## 异步文件
### 写入
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
### 读取
`fs.read(fd, [options,] callback)`

## 简单文件
### 写入
```md
fs.writeFile(file, data[, options], callback)
fs.writeFileSync(file, data[, options])
  - file 要操作的文件路径
  - data 要写入的数据，可以是String或BUffer
  - options 选项，可以对写入进行一些设置
    - encoding <string> | <null> 默认值: 'utf8'
    - mode <integer> 默认值: 0o666
    - flag <string> 默认值: 'w'
      r 只读  w 可写  a 追加
  - callback 当写入完成以后执行的函数
```
[flag 标志](http://nodejs.cn/api/fs.html#fs_file_system_flags)
```js
var fs = require('fs')

// 路径或者改为 C:/Users/fe/Desktop/nodejs/hello3.txt
fs.writeFile('C:\\Users\\fe\\Desktop\\nodejs\\hello3.txt', '这是通过writeFile写入的内容', { flag: "w" }, function (err) {
  if (!err) {
    console.log('写入成功')
  } else {
    console.log(err)
  }
})
```
### 读取
```md
fs.readFile(path[, options], callback)
fs.readFileSync(path[, options])
  - path 要读取的文件的路径
  - options  <Object> | <string>
    - encoding <string> | <null> 默认值: null
    - flag <string> 默认值: 'r'
  - callback 回调函数，通过回调函数将读取到内容返回(err, data)
    err 错误对象
    data 读取到的数据，会返回一个Buffer
```
```js
var fs = require('fs')
fs.readFile('a.jpg', function (err, data) {
  if (!err) {
    // 将data写入到文件中
    fs.writeFile('hello.jpg', data, function (err) {
      if (!err) {
        console.log('写入成功！')
      }
    })
  }
})
// 实现了复制操作
```

## 流式文件
### 写入
同步、异步、简单文件的写入都不适合大文件的写入，性能较差，容易导致内存溢出。
```md
首先需要创建一个Writable对象
  fs.createWriteStream(path[, options])
    - path 文件路径
    - options {encoding:"",mode:"",flag:""}
打开了Writable文件流，就可以使用write()方法来写入它
写入完成后，再调用end()方法来关闭流
```
```js
var fs = require('fs')

// 创建一个可写流
var ws = fs.createWriteStream('hello4.txt')
// 可以通过监听流的open和close事件来监听流的打开和关闭
/*
on(事件字符串, 回调函数)
  - 可以为对象绑定一个事件
once(事件字符串, 回调函数)
  - 可以为对象绑定一个一次性的事件，该事件将会在触发一次以后自动失效
*/
ws.once('open', function () {
  console.log('流打开了')
})
ws.once('close', function () {
  console.log('流关闭了')
})

// 通过ws向文件中输出内容
ws.write('通过可写流写入文件的内容')
ws.write('今天天气真好')
ws.write('HELLO')
ws.write('WORLD')

// 关闭流
ws.close()
// ws.end()
```
### 读取
流式文件读取也适用于一些比较大的文件，可以分多次将文件读取到内容中
```md
首先需要创建一个Readable流对象
  fs.createReadStream(path[, options])
    - path 文件路径
    - options {encoding:"",mode:"",flag:""}
打开Readable文件流以后，可以通过readable事件和read()请求，或通过data事件处理程序从它读出。
```
```js
var fs = require('fs')

// 创建一个可读流
var rs = fs.createReadStream('ABC.mp3')
// 创建一个可写流
var ws = fs.createWriteStream('123.mp3')

// 监听流的开启和关闭
rs.once('open', function () {
  console.log('可读流打开了')
})
rs.once('close', function () {
  console.log('可读流关闭了')
  // 数据读取完毕，关闭可写流
  ws.end()
})
ws.once('open', function () {
  console.log('可写流打开了')
})
ws.once('close', function () {
  console.log('可写流关闭了')
})

// 如果要读取一个可读流中的数据，必须要为可读流绑定一个data事件，data事件绑定文笔，它会自动开始读取数据
rs.on('data', function (data) {
  // console.log(data)
  // 将读取到的数据写入到可写流中
  ws.write(data)
})
// 复制一个大文件
```
```js
// 另一种写法
var fs = require('fs')

// 创建一个可读流
var rs = fs.createReadStream('ABC.mp3')
// 创建一个可写流
var ws = fs.createWriteStream('456.mp3')

// 监听流的开启和关闭
rs.once('open', function () {
  console.log('可读流打开了')
})
rs.once('close', function () {
  console.log('可读流关闭了')
})
ws.once('open', function () {
  console.log('可写流打开了')
})
ws.once('close', function () {
  console.log('可写流关闭了')
})

// pipe()可以将可读流中的内容，直接俄输出到可写流中
rs.pipe(ws)
```

## 其它方法
- 检查一个文件是否存在
`fs.existsSync(path)`
```js
var isExists = fs.existsSync('a.mp3')
console.log(isExists)
// 存在输出 true，不存在输出 false
```

- 获取文件信息
[fs.Stats类 文档](http://nodejs.cn/api/fs.html#fs_class_fs_stats)
```md
fs.stat(path[, options], callback)
fs.statSync(path[, options])
  会返回一个对象，这个对象中保存了当前对象状态的相关信息
```
```js
fs.stat('ABC.mp3', function (err, stat) {
  /*
  isFile() 是否是一个文件
  isDirectory() 是否是一个文件夹
  */
  // console.log(stat.size)
  console.log(stat.isFile())
})
```

- 删除文件
```md
fs.unlink(path, callback)
fs.unlinkSync(path)
```
```js
fs.unlinkSync('hello.txt')
```

- 列出文件
```md
fs.readdir(path[, options], callback)
fs.readdirSync(path[, options])
```
```js
fs.readdir('.', function (err, files) {
  if (!err) {
    console.log(files)
  }
})
// files是一个字符串数组，每一个元素就是一个文件夹或文件的名字
```

- 截断文件
```md
fs.truncate(path, len, callback)
fs.truncateSync(path, len)
```
```js
fs.truncateSync('hello2.txt', 10)
截断文件，将文件修改为指定大小
```

- 建立目录
```md
fs.mkdir(path[, mode], callback)
fs.mkdirSync(path[, mode])
```
```js
fs.mkdirSync('hello')
```

- 删除目录
```md
fs.rmdir(path, callback)
fs.rmdirSync(path)
```
```js
fs.rmdirSync('hello')
```

- 重命名文件和目录
```md
fs.rename(oldPath, newPath, callback)
fs.renameSync(oldPath, newPath)
  - oldPath 旧路径
  - newPath 新路径
  - callback 回调函数
```
```js
fs.rename('hello3.txt', 'C:/Users/fe/Desktop/555.txt', function (err) {
  if (!err) {
    console.log('修改成功！')
  }
})
// 也可以实现剪切功能
```

- 监视文件更改写入
```md
fs.watchFile(filename[, options], listener)
  - filename 要监视的文件的名字
  - options 配置选项
    - bigint <boolean> 默认值: false
    - persistent <boolean> 默认值: true
    - interval <integer> 默认值: 5007 间隔时间
  - listener 回调函数，当文件发生变化时，回调函数会执行。有两个参数：
    curr 当前文件的状态
    prev 修改前文件的状态
      这两个对象都是status对象

```
```js
fs.watchFile('hello4.txt', { interval: 1000 }, function (curr, prev) {
  console.log('修改前文件大小：' + prev.size)
  console.log('修改后文件大小：' + curr.size)
})
```

# EventEmitter
```js
// EventEmitter类 实现
class Eventer {
  #eventListeners = {}

  constructor() { }

  on(eventName, handler) {
    if (!(eventName in this.#eventListeners)) {
      this.#eventListeners[eventName] = []
    }

    this.#eventListeners[eventName].push(handler)
    return this
  }

  emit(eventName, ...args) {
    var handlers = this.#eventListeners[eventName]
    if (handlers && handlers.length > 0) {
      for (var handler of handlers) {
        handler.call(this, ...args)
      }
      return true
    } else {
      return false
    }
  }
}
```
