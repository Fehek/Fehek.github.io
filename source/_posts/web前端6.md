---
title: web前端学习笔记6
abbrlink: df82c88f
tags:
  - web前端
  - JavaScript
  - DOM
categories: web前端学习笔记
date: 2021-01-12 21:19:00
index_img:
banner_img:
---

# DOM简介
## 定义
文件对象模型（Document Object Model），是W3C组织推荐的可扩展标记语言（HTML或XML）的标准编程接口。
通过DOM接口可以改变网页的内容、结构和样式。

## DOM树
文档：一个页面就就是一个文档，DOM中使用document表示
元素：页面中所有标签都是元素，DOM中使用element表示
节点：网页中所有内容都是节点（标签、属性、文本、注释等），DOM中使用node表示
DOM把以上内容都看做是对象

# 获取元素
- [document.getElementById()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById)：根据ID获取
参数是大小写敏感的字符串，返回的是一个元素对象；
console.dir 打印返回的元素对象，更好的查看里面的属性和方法。
- [document.getElementsByTagName()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByTagName)：根据标签名获取
返回带有指定标签名的对象的集合，以伪数组的形式存储；
如果页面中只有一个元素，返回的还是伪数组的形式；
如果页面中没有这个元素，返回的是空的伪数组的形式；
想要操作里面的元素就需要遍历；
还可以获取某个元素（父元素）内部所有指定标签名的子元素，父元素必须是单个对象（必须指明是哪一个元素对象），获取时不包括父元素自己
`element.getElementsByTagName('标签名')`
- [document.getElementsByClassName()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByClassName)：根据类名返回元素对象集合
- [document.querySelector()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector)：根据指定选择器返回第一个元素对象，里面的选择器需要加符号（.box #nav)
- [document.querySelector()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelectorAll)：根据指定选择器返回所有元素对象集合
- doucment.body：获取body元素
document.documentElement：获取html元素

## 事件基础
事件由三部分组成，事件源、事件类型、事件处理程序，也称为事件三要素
事件源：事件被触发的对象 谁 按钮 
事件类型：如何触发 什么事件 比如鼠标点击(onclick)还是鼠标经过还是键盘按下
事件处理程序：通过一个函数赋值的方式完成
```html
<body>
  <botton id="btn">CLICK</botton>
  <script>
    var btn = document.getElementById('btn');
    btn.onclick = function () {
      alert('Boom!!!!');
    }
  </script>
</body>
```
执行事件的步骤：
获取事件源，注册事件（绑定事件），添加事件处理程序（采取函数赋值形式）

鼠标事件|触发条件
:-|:-
onclick|鼠标点击左键触发
mouseenter|当鼠标移入某元素时触发
mouseleave|当鼠标移出某元素时触发
mouseover|鼠标经过触发，移入和移出其子元素时也会触发
mouseout|鼠标离开触发，移入和移出其子元素时也会触发
mousemove|鼠标移动触发，即使在其子元素上也会触发
focus|获得鼠标焦点触发
blur|失去鼠标焦点触发
mouseup|鼠标弹起触发
mousedown|鼠标按下触发

# 操作元素
- 改变元素内容
element.innerText：不识别html标签，去掉空格和换行
element.innerHTML：识别html标签，保留空格和换行
```html
<body>
  <button>显示当前时间</button>
  <div>TIME</div>
  <script>
    // 点击按钮，div里面的文字发生变化
    // 1.获取元素
    var btn = document.querySelector('button');
    var div = document.querySelector('div');
    // 2.注册事件
    btn.onclick = function () {
      // div.innerHTML = '2021 - 1 - 12'
      var myDate = new Date();
      div.innerHTML = myDate.toLocaleString()
    }
  </script>
</body>
```

- 改变元素属性
src,href,id,alt,title等
```html
<body>
  <style>
    img {
      width: 200px;
    }
  </style>
  <button id="cat">猫</button>
  <button id="dog">狗</button><br>
  <img src="https://cdn.mos.cms.futurecdn.net/VSy6kJDNq2pSXsCzb6cvYF-1024-80.jpg.webp" alt="" title="猫">
  <script>
    // 修改元素属性 src
    // 1.获取元素
    var cat = document.getElementById('cat');
    var dog = document.getElementById('dog');
    var img = document.querySelector('img');
    // 2.注册事件 处理程序
    dog.onclick = function () {
      img.src = 'https://cdn.pixabay.com/photo/2020/03/31/19/20/dog-4988985_1280.jpg'
      img.title = '狗'
    }
    cat.onclick = function () {
      img.src = 'https://cdn.mos.cms.futurecdn.net/VSy6kJDNq2pSXsCzb6cvYF-1024-80.jpg.webp'
      img.title = '猫'
    }
  </script>
</body>
```

- 改变表单属性
type,value,checked,selected,disabled
```html
<body>
  <button>按钮</button>
  <input type="text" value="输入内容">
  <script>
    // 1.获取元素
    var btn = document.querySelector('button')
    var input = document.querySelector('input')
    btn.onclick = function () {
      // input.innerHTML = '点击了' 
      // 这个是普通盒子比如 div 标签里的内容
      // 表单里面的值 文字内容是通过 value 来修改的
      input.value = '被点击了'
      // 表单被禁用，不能再点击
      // btn.disabled = true
      this.disabled = true
      // this指向的是事件函数的调用者
    }
  </script>
</body>
```

- 改变样式属性
1. element.style     行内样式操作
里面的样式采取驼峰命名法，如fontSize、backgroundColor
修改的是行内样式，css权重比较高
如果样式比较少或者功能简单的情况下使用
2. element.className 类名样式操作
适合于样式较多或者功能复制的情况
会直接更改元素的类名，会覆盖原先的类名
`this.className = '直接填写被修改后的类名'`
如果想要保留原先的类名，可以使用多类名选择器
`e.g. this.className = 'first change'`


# 被动事件
某些的事件的默认会影响用户体验，从用户的角度，希望默认行为是即刻触发的
但是浏览器需要先调用事件处理函数，在确认事件处理函数没有调用 preventDefault后
才能执行默认行为，但执行函数需要时间，就让会默认的发生更晚一些，让用户感觉到延迟。
更多时候，处理函数并没有调用 preventDefault，但浏览器还是要等到函数运行完才能滚动
于是有了passive事件，在事件绑定时，就告知浏览器，该事件处理函数不会调用 preventDefault
这样，浏览器可以在运行函数的同时，就直接开始默认行为的执行，消除了这个延迟。
```js
window.addEventListener('mousewheel', function(e) {
    e.preventDefault() 
}, {
    passive: false//本事件处理函数是否会调用 preventDefault, true代表不会
})
```