---
title: web前端学习笔记10——Promise
subtitle: Promise
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
- 异步编程
  - fs 文件操作
  ```js
  require('fs').readFile('./index.html', (err, data) => {})
  ```
  - 数据库操作
  - AJAX
  ```js
  $.get('/server', (data) => {})
  ```
  - 定时器
  ```js
  setTimeout(() => {}, 2000)
  ```
- Promise 的状态
实例对象中的一个属性 【PromiseState】
  - pending 未决定的
  - resolved / fullfilled 成功
  - rejected 失败

  状态改变
  - pending 变为 resolved
  - pending 变为 rejected
  只有这2种，且一个promise对象只能改变一次
  成功的结果数据一般称为value，失败的结果数据一般称为reason

- Promise 对象的值
实例对象中的另一个属性 【PromiseResult】
保存着对象【成功/失败】的结果
  - resolve
  - reject

- Promise 工作流程
![promise工作流程](/image/post/promise工作流程.jpg)

# 基本使用
## setTimeout()
```html
<body>
  <div class="container">
    <h2 class="page-header">Promise 初体验</h2>
    <button id="btn">点击抽奖</button>
  </div>
  <script>
    //生成随机数
    function rand(m, n) {
      return Math.ceil(Math.random() * (n - m + 1)) + m - 1
    }
    /*
        点击按钮,  1s 后显示是否中奖(30%概率中奖)
            若中奖弹出    恭喜恭喜
            若未中奖弹出  再接再厉
    */
    const btn = document.querySelector('#btn')
    //绑定单击事件
    btn.addEventListener('click', function () {
      // 定时器
      // setTimeout(() => {
      //获取从1 - 100的一个随机数
      //   let n = rand(1, 100)
      //   if (n <= 30) {
      //     alert('恭喜恭喜')
      //   } else {
      //     alert('再接再厉')
      //   }
      // }, 1000)

      //Promise 形式实现
      // resolve 解决  函数类型的数据
      // reject  拒绝  函数类型的数据
      const p = new Promise((resolve, reject) => {
        setTimeout(() => {
          let n = rand(1, 100)
          if (n <= 30) {
            resolve(n) // 将 promise 对象的状态设置为 『成功』
          } else {
            reject(n) // 将 promise 对象的状态设置为 『失败』
          }
        }, 1000)
      })

      console.log(p)
      //调用 then 方法
      // value 值
      // reason 理由
      p.then((value) => {
        alert('恭喜恭喜, 您的中奖数字为 ' + value)
      }, (reason) => {
        alert('再接再厉, 您的号码为 ' + reason)
      })
    })

  </script>
</body>
```
## fs
```js
const fs = require('fs')

// 回调函数形式
// fs.readFile('./resources/content.txt', (err, data) => {
//   if (err) throw err
//   console.log(data.toString())
// })

// Promise 形式
let p = new Promise((resolve, reject) => {
  fs.readFile('./resources/content.tx', (err, data) => {
    // 如果出错
    if (err) reject(err)
    // 如果成功
    resolve(data)
  })
})
// 调用then
p.then(value => {
  console.log(value.toString())
}, reason => {
  console.log(reason)
})
```
## AJAX
```html
<body>
  <div class="container">
    <h2 class="page-header">Promise 封装 AJAX 操作</h2>
    <button id="btn">点击发送 AJAX</button>
  </div>
  <script>
    const btn = document.querySelector('#btn')
    btn.addEventListener('click', function () {

      const p = new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest()
        xhr.open('GET', 'https://api.apiopen.top/getJoe')
        xhr.send()
        xhr.onreadystatechange = function () {
          if (xhr.readyState === 4) {
            if (xhr.status >= 200 && xhr.status < 300) {
              resolve(xhr.response)
            } else {
              reject(xhr.status)
            }
          }
        }
      })
      // 调用then
      p.then(value => {
        console.log(value)
      }, reason => {
        console.warn(reason)
      })
    })
  </script>
</body>
```
## fs.readFile()封装
```js
/*
封装一个函数 myReadFile 读取文件内容
参数：path 文件路径
返回：promise 对象
*/

function myReadFile(path) {
  return new Promise((resolve, reject) => {
    // 读取文件
    require('fs').readFile(path, (err, data) => {
      // 判断
      if (err) reject(err)
      // 成功
      resolve(data)
    })
  })
}
myReadFile('./resources/content.txt')
  .then(value => {
    console.log(value.toString())
  }, reason => {
    console.log(reason)
  })
```
```js
// util.promisify 方法
// http://nodejs.cn/api/util.html#util_util_promisify_original

const util = require('util')
const fs = require('fs')
let myReadFile = util.promisify(fs.readFile)
myReadFile('./resources/content.txt')
  .then(value => {
    console.log(value.toString())
  })
```
## AJAX GET封装
```html
<body>
  <script>
    /*
    封装一个函数 sendAJAX 发送 GET AJAX 请求
    参数：URL
    返回结果：Promise对象
    */
    function sendAJAX(url) {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest()
        xhr.responseType = 'json'
        xhr.open('GET', url)
        xhr.send()
        xhr.onreadystatechange = function () {
          if (xhr.readyState === 4) {
            if (xhr.status >= 200 && xhr.status < 300) {
              resolve(xhr.response)
            } else {
              reject(xhr.status)
            }
          }
        }
      })
    }
    sendAJAX('https://api.apiopen.top/getJoke')
      .then(value => {
        console.log(value)
      }, reason => {
        console.warn(reason)
      })
  </script>
</body>
```
# Promise方法
## Promise 构造函数
`Promise{executor}{}`
executor 函数：执行器 {resolve, reject} => {}
resolve 函数：内部定义成功时调用函数 value => {}
reject 函数：内部定义失败时调用函数 reason => {}
说明：executor 会在 Promise 内部立即**同步调用**，异步操作在执行器中执行
## Promise.prototype.then
`{onResolved, onRejected} => {}`
onResolved 函数：成功的回调函数 (value) => {}
onRejected 函数：失败的回调函数 (reason) => {}
说明：value的成功回调和reason的失败回调返回一个新的promise对象
## Promise.prototype.catch
`onRejected => {}`
onRejected 函数：失败的回调函数 (reason) => {}
说明：then()的语法糖，相当于：then(undefined, onRejected)
```js
let p = new Promise((resolve, reject) => {
  // 修改promise对象的状态
  reject('error')
})

p.catch(reason => {
  console.log(reason)
})
```
## Promise.resolve
`(value) => {}`
value：成功的数据或promise对象
说明：返回一个成功/失败的promise对象
```js
// 如果传入的参数为 非Promise类型的对象，则返回的结果为成功的promise对象
// 如果传入的参数为 Promise对象，则参数的结果决定了 resolve 的结果

// let p1 = Promise.resolve(332)
/*
Promise {<fulfilled>: 332}
  __proto__: Promise
  [[PromiseState]]: "fulfilled"
  [[PromiseResult]]: 332
*/
let p2 = Promise.resolve(new Promise((resolve, reject) => {
  // resolve('OK')
  /*
  Promise {<fulfilled>: "OK"}
    __proto__: Promise
    [[PromiseState]]: "fulfilled"
    [[PromiseResult]]: "OK"
  */
  reject('Error')
  /*
  Promise {<rejected>: "Error"}
    __proto__: Promise
    [[PromiseState]]: "rejected"
    [[PromiseResult]]: "Error"
  */
}))

// console.log(p2)
p2.catch(reason => {
  console.log(reason)
})
```
## Promise.reject
`(reason) => {}`
value：失败的原因
说明：返回一个失败的promise对象
```js
// let p = Promise.reject('123') //Promise {<rejected>: "123"}
let p2 = Promise.reject(new Promise((resolve, reject) => {
  resolve('OK')
}))
console.log(p2) //Promise {<rejected>: Promise}
```
## Promise.all
`(promises) => {}`
promises：包括n个promise的数组
说明：返回一个新的promise，只有所有的promise都成功才成功，只要有一个失败了就直接失败
```js
let p1 = new Promise((resolve, reject) => {
  resolve('OK')
})
// let p2 = Promise.resolve('Success')
let p2 = Promise.reject('Error')
let p3 = Promise.resolve('Yeah')

const result = Promise.all([p1, p2, p3])
console.log(result)
/*
全部成功，返回3个promise数组
Promise {<pending>}
  __proto__: Promise
  [[PromiseState]]: "fulfilled"
  [[PromiseResult]]: Array(3)
    0: "OK"
    1: "Success"
    2: "Yeah"
    length: 3
    __proto__: Array(0)
*/
/*
p2失败，返回p2的失败结果
Promise {<pending>}
  __proto__: Promise
  [[PromiseState]]: "rejected"
  [[PromiseResult]]: "Error"
*/
```
## Promise.race
`(promises) => {}`
promises：包括n个promise的数组
说明：返回一个新的promise，第一个完成的promise的结果状态就是最终的结果状态，不管结果本身是成功状态还是失败状态。
```js
let p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('OK')
  }, 1000)
})
let p2 = Promise.reject('Error')
let p3 = Promise.resolve('Yeah')

const result = Promise.race([p1, p2, p3])
console.log(result)
/*
返回p2的结果状态
Promise {<pending>}
  __proto__: Promise
  [[PromiseState]]: "rejected"
  [[PromiseResult]]: "Error"
*/
```
# 几个问题
1. 如何改变promise的状态？
    1.  resolve(value) pending => resolved
    2.  reject(reason) pending => rejected
    3.  抛出异常 pending => rejected
    ```js
    let p = new Promise((resolve, reject) => {
      // resolve('OK')
      // reject('Error')
      throw '出问题了'
    })
    console.log(p)
    ```
<br>
2. 一个promise指定多个成功/失败回调函数，都会调用吗？
当promise改变为对应状态时都会调用
```js
let p = new Promise((resolve, reject) => {
  resolve('OK')
})

// 指定回调 1
p.then(value => {
  console.log(value)
})
// 指定回调 2
p.then(value => {
  alert(value)
})
```
<br>
3. 改变 promise 状态和指定回调函数谁先谁后?
(1) 都有可能, 正常情况下是先指定回调再改变状态【异步任务】, 但也可以先改状态再指定回调
(2) 如何先改状态再指定回调?
&nbsp;&nbsp;&nbsp;&nbsp;① 在执行器中直接调用 resolve()/reject()【同步任务】
&nbsp;&nbsp;&nbsp;&nbsp;② 延迟更长时间才调用 then()
(3) 什么时候才能得到数据（回调函数什么时候执行）?
&nbsp;&nbsp;&nbsp;&nbsp;① 如果先指定的回调, 那当状态发生改变时, 回调函数就会调用, 得到数据
&nbsp;&nbsp;&nbsp;&nbsp;② 如果先改变的状态, 那当指定回调时, 回调函数就会调用, 得到数据

4. promise.then()返回的新 promise 的结果状态由什么决定?
(1) 简单表达: 由 then()指定的回调函数执行的结果决定
(2) 详细表达:
&nbsp;&nbsp;&nbsp;&nbsp;① 如果抛出异常, 新 promise 变为 rejected, reason 为抛出的异常
&nbsp;&nbsp;&nbsp;&nbsp;② 如果返回的是非 promise 的任意值, 新 promise 变为 resolved, value 为返回的值
&nbsp;&nbsp;&nbsp;&nbsp;③ 如果返回的是另一个新 promise, 此 promise 的结果就会成为新 promise 的结果

5. promise 如何串连多个操作任务?
(1) promise 的 then()返回一个新的 promise, 可以生成 then()的链式调用
(2) 通过 then 的链式调用串连多个同步/异步任务
```js
let p = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('OK')
  }, 1000)
})

p.then(value => {
  return new Promise((resolve, reject) => {
    resolve("success")
  })
}).then(value => {
  console.log(value) //success
}).then(value => {
  console.log(value) //undefined
})
```
<br>
6. promise 异常传透?
(1) 当使用 promise 的 then 链式调用时, 可以在最后指定失败的回调
(2) 前面任何操作出了异常, 都会传到最后失败的回调中处理
```js
let p = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('OK');
    // reject('Err');
  }, 1000);
});

p.then(value => {
  // console.log(111);
  throw '失败啦!';
}).then(value => {
  console.log(222);
}).then(value => {
  console.log(333);
}).catch(reason => {
  console.warn(reason); //失败啦！
});
```
<br>
7. 中断 promise 链?
当使用 promise 的 then 链式调用时, 在中间中断, 不再调用后面的回调函数。
办法: 在回调函数中返回一个 pending 状态的 promise 对象
```js
let p = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('OK');
  }, 1000);
});

p.then(value => {
  console.log(111);
  //有且仅有一种方式
  return new Promise(() => { });
}).then(value => {
  console.log(222);
}).then(value => {
  console.log(333);
}).catch(reason => {
  console.warn(reason);
});
```

# 自定义封装
```js
class Promise {
  // 构造方法
  constructor(executor) {
    this.PromiseState = 'pending'
    this.PromiseResult = null
    this.callbacks = []
    // 保存实例对象的 this 值
    const self = this //self _this that

    // resolve函数
    function resolve(data) {
      // 判断状态，只能更改一次
      if (self.PromiseState !== 'pending') return
      // 1. 修改对象的状态(PromiseState)
      self.PromiseState = 'fulfilled'
      // 2. 修改对象结果值(PromiseResult)
      self.PromiseResult = data
      // 调用成功的回调函数
      // 在resolve中而不是then中执行回调，改变状态之后才能执行回调
      // 多个回调
      // then方法是异步执行，加一个定时器(2)
      setTimeout(() => {
        self.callbacks.forEach(item => {
          item.onResolved(data)
        })
      })
    }

    // reject函数
    function reject(data) {
      if (self.PromiseState !== 'pending') return
      self.PromiseState = 'rejected'
      self.PromiseResult = data
      setTimeout(() => {
        self.callbacks.forEach(item => {
          item.onRejected(data)
        })
      })
    }

    try {
      // 同步调用【执行器函数】
      executor(resolve, reject)
    } catch (e) {
      // 修改 promise 对象状态为 失败
      reject(e)
    }
  }

  // then方法
  then(onResolved, onRejected) {
    const self = this
    // 判断回调函数参数
    if (typeof onRejected !== 'function') {
      onRejected = reason => {
        throw reason
      }
    }
    if (typeof onResolved !== 'function') {
      onResolved = value => value
    }
    return new Promise((resolve, reject) => {
      //封装函数
      function callback(type) {
        try {
          //获取回调函数的执行结果
          let result = type(self.PromiseResult)
          //判断
          if (result instanceof Promise) {
            //如果是 Promise 类型的对象
            result.then(v => {
              resolve(v)
            }, r => {
              reject(r)
            })
          } else {
            //结果的对象状态为『成功』
            resolve(result)
          }
        } catch (e) {
          reject(e)
        }
      }
      //调用回调函数  PromiseState
      if (this.PromiseState === 'fulfilled') {
        // then方法是异步执行，加一个定时器(1)
        setTimeout(() => {
          callback(onResolved)
        })
      }
      if (this.PromiseState === 'rejected') {
        setTimeout(() => {
          callback(onRejected)
        })
      }
      //判断 pending 状态
      if (this.PromiseState === 'pending') {
        //保存回调函数
        this.callbacks.push({
          onResolved: function () {
            callback(onResolved)
          },
          onRejected: function () {
            callback(onRejected)
          }
        })
      }
    })
  }

  // catch方法
  catch(onRejected) {
    return this.then(undefined, onRejected)
  }

  // 添加resolve方法
  // 属于类，不属于实例对象
  static resolve(value) {
    return new Promise((resolve, reject) => {
      if (value instanceof Promise) {
        value.then(v => {
          resolve(v)
        }, r => {
          reject(r)
        })
      } else {
        resolve(value)
      }
    })
  }

  // 添加reject方法
  static reject(reason) {
    return new Promise((resolve, reject) => {
      reject(reason)
    })
  }

  // 添加all方法
  static all(promises) {
    return new Promise((resolve, reject) => {
      let count = 0
      let arr = []
      for (let i = 0; i < promises.length; i++) {
        promises[i].then(v => {
          // 得知对象的状态是成功
          count++
          arr[i] = v
          if (count === promises.length) {
            // 每个promise对象都成功才能调用resolve()修改状态
            resolve(arr)
          }
        }, r => {
          reject(r)
        })
      }
    })
  }

  // 添加race方法
  static race(promises) {
    return new Promise((resolve, reject) => {
      for (let i = 0; i < promises.length; i++) {
        promises[i].then(v => {
          resolve(v)
        }, r => {
          reject(r)
        })
      }
    })
  }
}
```

# async 和 await
## async
- 函数的返回值为 promise 对象
- promise 对象的结果由 async 函数执行的返回值决定
```js
// 和then方法返回结果规则是一样的
async function main() {
  // 1. 如果返回值是一个非Promise类型的对象
  // return 123
  // 2. 如果返回的是一个Promise对象
  // return new Promise((resolve, reject) => {
  //   resolve('OK')
  //   reject('ERR')
  // })
  // 3. 抛出异常
  throw 'NO'
}
let result = main()
console.log(result)
```

## await
await 右侧的表达式一般为 promise 对象, 但也可以是其它的值
- 如果表达式是 promise 对象, await 返回的是 promise 成功的值
- 如果表达式是其它值, 直接将此值作为 await 的返回值

注意：await 必须写在 async 函数中, 但 async 函数中可以没有 await。
如果 await 的 promise 失败了, 就会抛出异常, 需要通过 try...catch 捕获处理。
```js
async function main() {
  let p = new Promise((resolve, reject) => {
    // resolve('OK')
    reject('Error')
  })
  //1. 右侧为promise的情况
  // let res = await p
  //2. 右侧为其他类型的数据
  // let res2 = await 20
  //3. 如果promise是失败的状态
  try {
    let res3 = await p
  } catch (e) {
    console.log(e)
  }
}
main()
```

## async与await结合
- 读取文件
```js
// 读取 resource 1.html 2.html 3.html 文件内容
// 回调函数的方式
fs.readFile('./resource/1.html', (err, data1) => {
  if (err) throw err
  fs.readFile('./resource/2.html', (err, data2) => {
    if (err) throw err
    fs.readFile('./resource/3.html', (err, data3) => {
      if (err) throw err
      console.log(data1 + data2 + data3)
    })
  })
})
```
```js
const fs = require('fs')
const util = require('util')

const myReadFile = util.promisify(fs.readFile)

//async 与 await
async function main() {
  try {
    //读取第一个文件的内容
    let data1 = await myReadFile('./resource/1.html')
    let data2 = await myReadFile('./resource/2.html')
    let data3 = await myReadFile('./resource/3.html')
    console.log(data1 + data2 + data3)
  } catch (e) {
    console.log(e)
  }
}

main()
```
- 发送AJAX请求
```html
<body>
  <button id="btn">点击获取段子</button>
  <script>
    function sendAJAX(url) {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest
        xhr.responseType = 'json'
        xhr.open('GET', url)
        xhr.send()
        xhr.onreadystatechange = function () {
          if (xhr.readyState === 4) {
            if (xhr.status >= 200 && xhr.status < 300) {
              resolve(xhr.response)
            } else {
              reject(xhr.status)
            }
          }
        }
      })
    }
    let btn = document.querySelector('#btn')
    btn.addEventListener('click', async function () {
      let duanzi = await sendAJAX('https://api.apiopen.top/getJoke')
      console.log(duanzi)
    })
  </script>
</body>
```

# run
```js
function run(generatorFunction) {
  return new Promise((resolve, reject) => {
    var generator = generatorFunction()
    var generated = generator.next()

    step()

    function step() {
      if (!generated.done) {
        generated.value.then(val => {
          try {
            generated = generator.next(val)
          } catch (e) {
            reject(e)
            return
          }
          step()
        }, reason => {
          try {
            generated = generator.throw(reason)
          } catch (e) {
            reject(e)
            return
          }
          step()
        })
      } else {
        resolve(generated.value)
      }
    }
  })
}
```
```js
// 另一种写法
function run(generatorFunc) {
  return new Promise((resolve, reject) => {
    var generator = generatorFunc()
    var gen = generator.next()

    step()

    function tick(methodName, val) {
      gen = generator[methodName](val)
      step()
    }

    function step() {
      if (gen.done) {
        resolve(gen.value)
      } else {
        gen.value.then(tick.bind(null, 'next'), tick.bind(null, 'throw'))
      }
    }
  })
}
```
```js
// 简版
function run(generatorFunc) {
  var generator = generatorFunc()
  var generated = generator.next()

  step()

  function step() {
    generated.value.then(val => {
      generated = generator.next(val)
      step()
    })
  }
}
```
