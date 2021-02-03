---
title: web前端学习笔记7
abbrlink: a885f819
tags:
  - web前端
  - AJAX
categories: web前端学习笔记
date: 2021-01-26 22:00:00
index_img:
banner_img:
---

# AJAX
## 简介
AJAX 全称为 Asynchronous JavaScript And XML，就是异步的 JS 和 XML。
通过 AJAX 可以在浏览器中向服务器发送异步请求，最大的优势：无刷新获取数据。
AJAX 不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式。

## 特点
- 优点
可以无需刷新页面而与服务器端进行通信
允许根据用户事件来更新部分页面内容
- 缺点
没有浏览历史，不能回退
存在跨域问题(同源)
SEO 不友好

# HTTP协议
## 请求报文
```md
行      POST  /s?ie=utf-8  HTTP/1.1 
头      Host: atguigu.com
        Cookie: name=guigu
        Content-type: application/x-www-form-urlencoded
        User-Agent: chrome 83
空行
体      username=admin&password=admin
```
在 Chrome 浏览器中，
Headers => Request Headers 查看请求报文行、头
Headers => From Data 查看请求体

## 响应报文
```md
行      HTTP/1.1  200  OK
头      Content-Type: text/html;charset=utf-8
        Content-length: 2048
        Content-encoding: gzip
空行    
体      <html>
            <head>
            </head>
            <body>
                <h1>XXX</h1>
            </body>
        </html>
```
Headers => Response Headers 查看响应行、头
Response 查看响应体

# 原生方法
## GET
```js
const express = require('express')
const app = express()
app.get('/server', (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader('Access-control-Allow-Origin', '*')
  // 设置响应体
  response.send('Hello Ajax')
})

app.listen(8000, () => {
  console.log("服务已经启动，8000端口监听中......")
})
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AJAX GET 请求</title>
  <style>
    #result {
      width: 200px;
      height: 100px;
      border: solid 1px red;
    }
  </style>
</head>

<body>
  <button>点击发送请求</button>
  <div id="result"></div>

  <script>
    // 获取button元素
    const btn = document.getElementsByTagName('button')[0]
    const result = document.getElementById('result')
    // 绑定事件
    btn.onclick = function () {
      // 1.创建对象
      const xhr = new XMLHttpRequest()
      // 2.初始化 设置请求方法和url
      xhr.open('GET', 'http://127.0.0.1:8000/server')
      // 3.发送
      xhr.send()
      // 4.事件绑定 处理服务端返回的结果
      xhr.onreadystatechange = function () {
        // 判断服务端返回了所有的结果
        if (xhr.readyState == 4) {
          // 判断响应状态码 200 404 403 401 500
          // 2xx 成功
          if (xhr.status >= 200 && xhr.status < 300) {
            // 处理结果 行 头 空行 体
            // 1.响应行
            // console.log(xhr.status)  //状态码
            // console.log(xhr.statusText)  //状态字符串
            // console.log(xhr.getAllResponseHeaders())  //所有响应头
            // console.log(xhr.response)  //响应体

            // 设置result文本
            // 可以实现不刷新页面直接获取响应体
            result.innerHTML = xhr.response
          }
        }
      }
    }
  </script>
</body>

</html>
```
- 设置请求参数
直接在url后以?开始加上，如上述代码url参数可改成`http://127.0.0.1:8000/server?a=100&b=200&c=300`

## POST
```js
const express = require('express')
const app = express()

app.post('/server', (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader('Access-control-Allow-Origin', '*')
  // 设置响应体
  response.send('Hello Ajax POST')
})

app.listen(8000, () => {
  console.log("服务已经启动，8000端口监听中......")
})
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AJAX POST 请求</title>
  <style>
    #result {
      width: 200px;
      height: 100px;
      border: solid 1px red;
    }
  </style>
</head>

<body>
  <!-- 把鼠标放在div上时，发送post请求，结果在div中显示 -->
  <div id="result"></div>
  <script>
    const result = document.getElementById('result')
    result.addEventListener('mouseover', function () {
      const xhr = new XMLHttpRequest()
      xhr.open('POST', 'http://127.0.0.1:8000/server')
      // 设置请求头
      xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
      xhr.send('a=100&b=200&c=300')
      xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
          if (xhr.status >= 200 && xhr.status < 300) {
            result.innerHTML = xhr.response
          }
        }
      }
    })
  </script>
</body>

</html>
```
- 设置请求参数
在`xhr.send()`里面添加，格式灵活，不限于'a=100&b=200'的样式。
- 设置请求头
在`xhr.setRequestHeader()`里设置
若要自定义请求头如`xhr.setRequestHeader('name', 'fehek')`，需要在server.js里面添加响应头，新建一个app.all函数（可以接收任意类型的请求），在里面添加`response.setHeader('Access-control-Allow-Headers', '*')`

## JSON响应
```js
const express = require('express')
const app = express()

app.all('/json-server', (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader('Access-control-Allow-Origin', '*')
  // 响应头
  response.setHeader('Access-control-Allow-Headers', '*')
  // 响应一个数据
  const data = {
    name: 'fehek'
  }
  // 对对象进行字符串转换
  let str = JSON.stringify(data)
  // 设置响应体
  response.send(str)
})

app.listen(8000, () => {
  console.log("服务已经启动，8000端口监听中......")
})
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>JSON响应</title>
  <style>
    #result {
      width: 200px;
      height: 100px;
      border: solid 1px red;
    }
  </style>
</head>

<body>
  <div id="result"></div>
  <script>
    const result = document.getElementById('result')
    window.onkeydown = function () {
      const xhr = new XMLHttpRequest()
      xhr.responseType = 'json'  // 2.自动转换
      xhr.open('GET', 'http://127.0.0.1:8000/json-server')
      xhr.send()
      xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
          if (xhr.status >= 200 && xhr.status < 300) {
            // 1.手动对数据转化
            // let data = JSON.parse(xhr.response)
            // console.log(data)
            // result.innerHTML = data.name

            // 2.自动转换
            console.log(xhr.response)
            result.innerHTML = xhr.response.name
          }
        }
      }
    }
  </script>
</body>

</html>
```

## IE缓存问题
IE在实现ajax时，会优先以缓存的结果展现
解决方法：在请求中加一个时间戳，这样每次实际发送的请求都会不一样
如`xhr.open('GET', 'http://127.0.0.1:8000/ie?t=' + Date.now())`

## 请求超时
```js
const express = require('express')
const app = express()

app.get('/delay', (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader('Access-control-Allow-Origin', '*')
  setTimeout(() => {
    response.send('延时响应')
  }, 3000)
})

app.listen(8000, () => {
  console.log("服务已经启动，8000端口监听中......")
})
```
```js
btn.addEventListener('click', function () {
  const xhr = new XMLHttpRequest()
  // 超时设置（2s）
  xhr.timeout = 2000
  // 超时回溯
  xhr.ontimeout = function () {
    alert("网络异常，请稍后重试")
  }
  // 网络异常回溯
  xhr.onerror = function () {
    alert("你的网络似乎出了一些问题")
  }
  xhr.open('GET', 'http://127.0.0.1:8000/delay')
  xhr.send()
  xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
      if (xhr.status >= 200 && xhr.status < 300) {
        result.innerHTML = xhr.response
      }
    }
  }
})
```

## 取消请求
调用abort
```html
<body>
  <button>点击发送</button>
  <button>点击取消</button>
  <script>
    const btns = document.querySelectorAll('button')
    let xhr = null

    btns[0].onclick = function () {
      xhr = new XMLHttpRequest()
      xhr.open('GET', 'http://127.0.0.1:8000/delay')
      xhr.send()
    }

    // abort
    btns[1].onclick = function () {
      xhr.abort()
    }
  </script>
</body>
```

## 重复请求
若重复发送相同请求，取消之前的请求，实现最新的请求
```html
<body>
  <button>点击发送</button>
  <script>
    const btn = document.getElementsByTagName('button')[0]
    let xhr = null
    // 标识变量，是否正在发送ajax请求
    let isSending = false

    btn.onclick = function () {
      // 判断标识变量
      // 如果正在发送，则取消该请求，创建一个新的请求
      if (isSending) xhr.abort()
      xhr = new XMLHttpRequest()
      // 修改 标识变量的值
      isSending = true
      xhr.open('GET', 'http://127.0.0.1:8000/delay')
      xhr.send()
      xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
          isSending = false
        }
      }
    }
  </script>
</body>
```

# 其它方法
## JQuery发送AJAX
```js
const express = require('express')
const app = express()

app.all('/jQuery-server', (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader('Access-control-Allow-Origin', '*')
  // 设置响应头，允许自定义请求头
  response.setHeader('Access-control-Allow-Headers', '*')
  // response.send('Hello jQuery AJAX')
  const data = { name: 'fehek' }
  response.send(JSON.stringify(data))
})

app.listen(8000, () => {
  console.log("服务已经启动，8000端口监听中......")
})

```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>jQuery 发送 AJAX 请求</title>
  <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
</head>

<body>
  <div class="container">
    <h2 class="page-header">jQuery发送AJAX请求</h2>
    <button>GET</button>
    <button>POST</button>
    <button>通用型方法ajax</button>
  </div>
  <script>
    $('button').eq(0).click(function () {
      $.get('http://127.0.0.1:8000/jQuery-server', { a: 100, b: 200 }, function (data) {
        console.log(data)
      }, 'json')  //输出对象
    })
    $('button').eq(1).click(function () {
      $.post('http://127.0.0.1:8000/jQuery-server', { a: 100, b: 200 }, function (data) {
        console.log(data)  //输出字符串
      })
    })
    $('button').eq(2).click(function () {
      $.ajax({
        // url
        url: 'http://127.0.0.1:8000/jQuery-server',
        // 参数
        data: { a: 100, b: 200 },
        // 请求类型
        type: 'GET',
        // 响应体结果
        dataType: 'json',
        // 成功的回调
        success: function (data) {
          console.log(data)
        },
        // 超时时间
        timeout: 2000,
        // 失败的回调
        error: function () {
          console.log('ERROR!')
        },
        // 头信息
        headers: {
          c: 300,
          d: 400
        }
      })
    })
  </script>
</body>

</html>
```

## axios发送AJAX
[axios](https://github.com/axios/axios)
```js
const express = require('express')
const app = express()

app.all('/axios-server', (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader('Access-control-Allow-Origin', '*')
  // 设置响应头，允许自定义请求头
  response.setHeader('Access-control-Allow-Headers', '*')
  // response.send('Hello jQuery AJAX')
  const data = { name: 'fehek' }
  response.send(JSON.stringify(data))
})

app.listen(8000, () => {
  console.log("服务已经启动，8000端口监听中......")
})
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>axios 发送 AJAX 请求</title>
  <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.js"></script>
</head>

<body>
  <button>GET</button>
  <button>POST</button>
  <button>通用型方法ajax</button>
  <script>
    const btns = document.querySelectorAll('button')
    // 配置 baseURL
    axios.defaults.baseURL = 'http://127.0.0.1:8000'

    btns[0].onclick = function () {
      // GET 请求
      axios.get('/axios-server', {
        // url 参数
        params: {
          id: 100,
          level: 5
        },
        // 请求头信息
        headers: {
          name: 'fehek',
          age: 21
        }
      }).then(value => {
        console.log(value)
      })
    }

    btns[1].onclick = function () {
      // POST 请求
      //第一个参数是url，第二个参数是请求体，第三个参数是其它配置
      axios.post('/axios-server', {
        // 请求体
        username: 'admin',
        password: 'admin'
      }, {
        // url 参数
        params: {
          id: 100,
          level: 5
        },
        // 请求头信息
        headers: {
          height: 170,
          weight: 170,
        }
      })
    }

    btns[2].onclick = function () {
      axios({
        // 请求方法
        method: 'POST',
        // url
        url: '/axios-server',
        // url 参数
        params: {
          id: 100,
          level: 5
        },
        // 头信息
        headers: {
          a: 100,
          b: 200
        },
        // 请求体参数
        data: {
          username: 'admin',
          password: 'admin'
        }
      }).then(response => {
        // 响应状态码
        console.log(response.status)
        // 响应状态字符串
        console.log(response.statusText)
        // 响应头信息
        console.log(response.headers)
        // 响应体
        console.log(response.data)
      })
    }
  </script>
</body>

</html>
```

## fetch发送AJAX
[fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/fetch)
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>fetch 发送 AJAXA 请求</title>
</head>

<body>
  <button>AJAX请求</button>
  <script>
    const btn = document.querySelector('button');
    btn.onclick = function () {
      fetch('http://127.0.0.1:8000/fetch-server?id=10', {
        //请求方法
        method: 'POST',
        //请求头
        headers: {
          name: 'fehek'
        },
        //请求体
        body: 'username=admin&password=admin'
      }).then(response => {
        // return response.text()
        return response.json()
      }).then(response => {
        console.log(response)
      })
    }
  </script>
</body>

</html>
```

# 跨域
同源：协议、域名、端口号必须完全相同。
违背同源策略就是跨域。

## 解决方法
### JSONP
JSONP(JSON with Padding)是一个非官方的跨域解决方案，只支持 get 请求。
在网页有一些标签天生具有跨域能力，比如：img，link，iframe，script。JSONP 就是利用 script 标签的跨域能力来发送请求的。
- 原生实现
1. 动态的创建一个 script 标签
`var script = document.createElement("script")`
2. 设置 script 的 src，设置回调函数
```js
script.src = "http://localhost:3000/testAJAX?callback=abc"
function abc(data) {
  alert(data.name)
}
```
3. 将 script 添加到 body 中
`document.body.appendChild(script)`
4. 服务器中路由的处理
```js
router.get("/testAJAX", function (req, res) {
  console.log("收到请求");
  var callback = req.query.callback;
  var obj = {
    name: "fehek",
    id: 100
  }
  res.send(callback + "(" + JSON.stringify(obj) + ")");
})
```

### CORS
[CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
CORS(Cross-Origin Resource Sharing)，跨域资源共享。
CORS 是官方的跨域解决方案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持 get 和 post 请求。
```js
// 设置响应头，来允许跨域请求
//res.set("Access-Control-Allow-Origin","http://127.0.0.1:3000");
res.set("Access-Control-Allow-Origin","*")
res.set("Access-Control-Allow-Headers","*")
res.set("Access-Control-Allow-Method","*")
```