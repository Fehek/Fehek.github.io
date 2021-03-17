---
title: web前端——html/css简例
subtitle: html/css简例
categories: web前端学习笔记
abbrlink: eef7064f
tags:
  - web前端
  - HTML
  - CSS
date: 2020-10-14 17:13:00
index_img:
---

# fixed图片应用
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body {
      margin: 0;
    }

    div:not([class]) {
      height: 50vh;
      background-color: white;
    }

    .a,
    .b,
    .c {
      height: 100vh;
      -webkit-background-size: ;
      background-size: cover;
      background-attachment: fixed;
      background-position: center;
    }

    .a {
      background-image: url(http://img2.1sucai.com/190807/330812-1ZPH3594356.jpg);
    }

    .b {
      background-image: url(https://www.lei168.com/attachment/editor/201708/1501832154entsk.jpg);
    }

    .c {
      background-image: url(https://pic3.zhimg.com/80/v2-6d436bfd5d2ae80a0def6f86e4c7c55e_1440w.jpg);
    }
  </style>
</head>

<body>
  <div class="a"></div>
  <div>介绍文字</div>
  <div class="b"></div>
  <div>介绍文字</div>
  <div class="c"></div>
  <div>介绍文字</div>
</body>

</html>
```

# sticky列表分组
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px cornflowerblue;
    }

    ul {
      margin: 0;
    }

    h1 {
      position: sticky;
      top: 0;
      margin: 0;
    }
  </style>
</head>

<body>
  <div class="group">
    <h1>分组1</h1>
    <ul>
      <li>好友01</li>
      <li>好友02</li>
      <li>好友03</li>
      <li>好友04</li>
      <li>好友05</li>
      <li>好友06</li>
      <li>好友07</li>
      <li>好友08</li>
      <li>好友09</li>
      <li>好友10</li>
    </ul>
  </div>
  <div class="group">
    <h1>分组2</h1>
    <ul>
      <li>好友01</li>
      <li>好友02</li>
      <li>好友03</li>
      <li>好友04</li>
      <li>好友05</li>
      <li>好友06</li>
      <li>好友07</li>
      <li>好友08</li>
      <li>好友09</li>
      <li>好友10</li>
    </ul>
  </div>
  <div class="group">
    <h1>分组3</h1>
    <ul>
      <li>好友01</li>
      <li>好友02</li>
      <li>好友03</li>
      <li>好友04</li>
      <li>好友05</li>
      <li>好友06</li>
      <li>好友07</li>
      <li>好友08</li>
      <li>好友09</li>
      <li>好友10</li>
    </ul>
  </div>
  <div class="group">
    <h1>分组4</h1>
    <ul>
      <li>好友01</li>
      <li>好友02</li>
      <li>好友03</li>
      <li>好友04</li>
      <li>好友05</li>
      <li>好友06</li>
      <li>好友07</li>
      <li>好友08</li>
      <li>好友09</li>
      <li>好友10</li>
    </ul>
  </div>
  <div class="group">
    <h1>分组5</h1>
    <ul>
      <li>好友01</li>
      <li>好友02</li>
      <li>好友03</li>
      <li>好友04</li>
      <li>好友05</li>
      <li>好友06</li>
      <li>好友07</li>
      <li>好友08</li>
      <li>好友09</li>
      <li>好友10</li>
    </ul>
  </div>
</body>

</html>
```

# clip-path应用
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px red;
    }

    ul {
      padding: 0;
      margin: 0;
    }

    body {
      margin: 0;
    }

    section {
      height: 300px;
      position: relative;
    }

    div {
      border: 1px solid blue;
      position: absolute;
      height: 100%;
      width: 100%;
      xclip: rect(auto, auto, auto, auto);
      clip-path: border-box;
    }

    ul {
      position: fixed;
      right: 0;
      margin: auto;
      top: 0;
      bottom: 0;
      height: 110px;
      list-style-type: none;
    }

    section:nth-child(1) ul {
      color: red;
    }

    section:nth-child(2) ul {
      color: lime;
    }

    section:nth-child(3) ul {
      color: tan;
    }

    section:nth-child(4) ul {
      color: violet;
    }

    section:nth-child(5) ul {
      color: magenta;
    }
  </style>
</head>

<body>
  <section>
    <div>
      <ul>
        <li>001</li>
        <li>002</li>
        <li>003</li>
        <li>004</li>
        <li>005</li>
        <li>006</li>
      </ul>
    </div>
  </section>
  <section>
    <div>
      <ul>
        <li>001</li>
        <li>002</li>
        <li>003</li>
        <li>004</li>
        <li>005</li>
        <li>006</li>
      </ul>
    </div>
  </section>
  <section>
    <div>
      <ul>
        <li>001</li>
        <li>002</li>
        <li>003</li>
        <li>004</li>
        <li>005</li>
        <li>006</li>
      </ul>
    </div>
  </section>
  <section>
    <div>
      <ul>
        <li>001</li>
        <li>002</li>
        <li>003</li>
        <li>004</li>
        <li>005</li>
        <li>006</li>
      </ul>
    </div>
  </section>
  <section>
    <div>
      <ul>
        <li>001</li>
        <li>002</li>
        <li>003</li>
        <li>004</li>
        <li>005</li>
        <li>006</li>
      </ul>
    </div>
  </section>
</body>

</html>
```
重点：
ul{margin: auto;top: 0;bottom: 0;}可以将菜单栏垂直居中。

# 菜单栏
## 竖向菜单栏
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px blue;
    }

    ul {
      list-style-type: none;
      margin: 0;
      padding: 0;
    }

    li {
      position: relative;
      width: 100px;
    }

    ul ul {
      position: absolute;
      top: 0;
      left: 100%;
      display: none;
    }

    li:hover {
      background-color: cornflowerblue;
    }

    li:hover>ul {
      display: block;
    }
  </style>
</head>

<body>
  <Ul>
    <li>
      文件
      <ul>
        <li>新建文件</li>
        <li>新建窗口</li>
        <li>保存</li>
        <li>首选项
          <ul>
            <li>设置</li>
            <li>联机服务设置</li>
            <li>扩展</li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      查看
      <ul>
        <li>命令面板</li>
        <li>打开视窗</li>
        <li>外观
          <ul>
            <li>全屏</li>
            <li>居中布局</li>
          </ul>
        </li>
        <li>编辑器布局
          <ul>
            <li>向上拆分</li>
            <li>单列</li>
            <li>双列</li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      转到
      <ul>
        <li>切换编辑器</li>
        <li>切换组</li>
      </ul>
    </li>

    <li>
      运行
      <ul>
        <li>启动调试</li>
        <li>打开视窗</li>
        <li>切换断点 </li>
        <li>新建断点
          <ul>
            <li>条件断点</li>
            <li>内联断点</li>
            <li>函数断点</li>
            <li>记录点</li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</body>

</html>
```
重点：
`ul ul{display: none;}`和`li:hover>ul{display: block;}`。前者将二级及以后菜单隐藏，而后者在hover菜单时将它的对应的后一级菜单栏显示出来。
`li{position: relative;}`和`ul ul{position: absolute; top: 0; left:100%;` 。可以使二级及以后菜单相对前一级菜单定位，并且设置位置为最上面与前一级菜单相同高度，右移前一个菜单的距离。

## 横向菜单栏
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px plum;
    }

    ul {
      list-style-type: none;
      margin: 0;
      padding: 0;
      font-size: 0;
    }

    li ul {
      display: none;
    }

    li:hover>ul {
      display: block;
    }

    li:hover {
      background-color: cornflowerblue;
    }

    ul ul {
      position: absolute;
      left: 0;
      top: 100%;
    }

    ul ul ul {
      top: 0%;
      left: 100%;
    }

    li {
      white-space: nowrap;
      position: relative;
      font-size: 16px;
      padding: 2px 8px;
      display: inline-block;
    }

    li li {
      display: block;
    }
  </style>
</head>

<body>
  <Ul>
    <li>
      文件
      <ul>
        <li>新建文件</li>
        <li>新建窗口</li>
        <li>保存</li>
        <li>首选项
          <ul>
            <li>设置</li>
            <li>联机服务设置</li>
            <li>扩展</li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      查看
      <ul>
        <li>命令面板</li>
        <li>打开视窗</li>
        <li>外观
          <ul>
            <li>全屏</li>
            <li>居中布局</li>
          </ul>
        </li>
        <li>编辑器布局
          <ul>
            <li>向上拆分</li>
            <li>单列</li>
            <li>双列</li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      转到
      <ul>
        <li>切换编辑器</li>
        <li>切换组</li>
      </ul>
    </li>

    <li>
      运行
      <ul>
        <li>启动调试</li>
        <li>打开视窗</li>
        <li>切换断点 </li>
        <li>新建断点
          <ul>
            <li>条件断点</li>
            <li>内联断点</li>
            <li>函数断点</li>
            <li>记录点</li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</body>

</html>
```
重点：
`li{display: inline-block;}`，`li li{display: block;}`。使所有li变成行内块，从而横向排布。以及二级及以后菜单变回块状元素。
重新定位二级及以后菜单`ul ul`和三级及以后菜单`ul ul ul`。
`ul{font-size: 0;}`，`li{font-size: 16px;}`。使元素之间没有空格。

# 图片选择缓动
## 滑动缓动
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    div {
      border: 5px solid cornflowerblue;
      width: 300px;
      height: 200px;
      position: relative;
      overflow: hidden;
    }

    img {
      width: 100%;
      height: 100%;
      position: absolute;
      left: 100%;
      /* transition: 1s; */
    }

    input:nth-child(1):checked~div img:nth-child(1),
    input:nth-child(2):checked~div img:nth-child(2),
    input:nth-child(3):checked~div img:nth-child(3),
    input:nth-child(4):checked~div img:nth-child(4),
    input:nth-child(5):checked~div img:nth-child(5) {
      display: block;
      left: 0;
      z-index: 10;
    }
  </style>
</head>

<body>
  <input type="radio" name="slides" checked>
  <input type="radio" name="slides">
  <input type="radio" name="slides">
  <input type="radio" name="slides">
  <input type="radio" name="slides">
  <div>
    <img src="https://img1.gtimg.com/dajia/pics/hv1/12/79/2245/146001282.jpg" alt="">
    <img src="https://www.zhifure.com/upload/images/2018/7/5141455828.jpg" alt="">
    <img src="http://s.news.bandao.cn/news_upload/201603/20160302092648_J3MUHKOQ.jpg" alt="">
    <img src="https://images.shobserver.com/news/690_390/2018/5/26/09fae5d0-9e46-48aa-a519-84e6a7a0c0ce.jpg" alt="">
    <img src="https://www.zhifure.com/upload/images/2018/6/27121831935.jpg" alt="">
  </div>
</body>

</html>
```
重点：
灵活运用选择器将radio和图片关联起来。
`img{left: 100%;}`和`...{left: 0;}`。将图片都放在边框的右边，触发按钮时才出现，并在transition中显示滑动效果。
`...{z-index: 10;}`。可以使图片滑动时是一直向左的效果，若不加，往前面选择图片时是从下层滑出。

## 水平滑动
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px red;
    }

    div {
      width: 200px;
      height: 150px;
      border: 3px solid red;
      position: relative;
    }

    ul {
      width: 9999px;
      padding: 0;
      margin: 0;
      height: 100%;
      position: absolute;
      transition: 1s;
    }

    ul li {
      display: inline-block;
      height: 100%;
      width: 200px;
    }

    input:nth-child(1):checked~div ul {
      left: 0;
    }

    input:nth-child(2):checked~div ul {
      left: -100%;
    }

    input:nth-child(3):checked~div ul {
      left: -200%;
    }
  </style>
</head>

<body>
  <input type="radio" name="s1" checked>
  <input type="radio" name="s1">
  <input type="radio" name="s1">
  <div>
    <ul>
      <li>aaaaaaaaa</li>
      <li>bbbbbbbbbb</li>
      <li>cccccc</li>
    </ul>
  </div>
</body>

</html>
```
重点：
`ul {width: 9999px;}`。将宽度设置为很大，以便所有照片都能在横向排列。
`input:nth-child(1):checked~div ul {left: 0;} input:nth-child(2):checked~div ul {left: -100%;} input:nth-child(3):checked~div ul {left: -200%;}`。因为所有图片都是横向排列，所以选中时会根据位置向左移动几个图片框的距离。

## 轮播图
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    ul {
      list-style-type: none;
      padding: 0;
      margin: 0;
    }

    section {
      width: 300px;
      height: 200px;
      border: 3px solid;
      position: relative;
    }

    section>input {
      display: none;
    }

    section ul {
      width: 100%;
      height: 100%;
      position: relative;
    }

    section ul li {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      opacity: 0;
      transition: .6s;
    }

    section ul li img {
      display: block;
      width: 100%;
      height: 100%;
    }

    section ul li label {
      background-color: rgb(0, 0, 0, 0.5);
      cursor: pointer;
      width: 25px;
      text-align: center;
      line-height: 50px;
      height: 50px;
      margin: auto;
      top: 0;
      bottom: 0;
      position: absolute;
    }

    section ul li label:hover {
      background-color: gold;
    }

    section ul li label:nth-last-child(2) {
      left: 0;
    }

    section ul li label:nth-last-child(1) {
      right: 0;
    }


    input:nth-child(1):checked~ul.slides>li:nth-child(1),
    input:nth-child(2):checked~ul.slides>li:nth-child(2),
    input:nth-child(3):checked~ul.slides>li:nth-child(3),
    input:nth-child(4):checked~ul.slides>li:nth-child(4),
    input:nth-child(5):checked~ul.slides>li:nth-child(5) {
      opacity: 1;
      z-index: 8;
    }

    section .indicators-wrapper {
      width: 100%;
      bottom: 0;
      height: 30px;
      position: absolute;
      z-index: 30;
      text-align: center;
    }

    section .indicators {
      display: inline-block;
      background-color: white;
      border-radius: 25px;
    }

    section .indicators label {
      width: 10px;
      height: 10px;
      border-radius: 5px;
      display: inline-block;
      background-color: red;
      margin-left: 3px;
      margin-right: 3px;
      cursor: pointer;
    }

    input:nth-child(1):checked~div.indicators-wrapper label:nth-child(1),
    input:nth-child(2):checked~div.indicators-wrapper label:nth-child(2),
    input:nth-child(3):checked~div.indicators-wrapper label:nth-child(3),
    input:nth-child(4):checked~div.indicators-wrapper label:nth-child(4),
    input:nth-child(5):checked~div.indicators-wrapper label:nth-child(5) {
      background-color: blue;
    }
  </style>
</head>

<body>
  <section>
    <input type="radio" name="pic" id="p1" checked>
    <input type="radio" name="pic" id="p2">
    <input type="radio" name="pic" id="p3">
    <input type="radio" name="pic" id="p4">
    <input type="radio" name="pic" id="p5">
    <ul class="slides">
      <li>
        <a href=""><img src="https://img1.gtimg.com/dajia/pics/hv1/12/79/2245/146001282.jpg" alt=""></a>
        <label for="p5">&lt;</label>
        <label for="p2">&gt;</label>
      </li>
      <li>
        <a href=""><img src="https://www.zhifure.com/upload/images/2018/7/5141455828.jpg" alt=""></a>
        <label for="p1">&lt;</label>
        <label for="p3">&gt;</label>
      </li>
      <li>
        <a href=""><img src="http://s.news.bandao.cn/news_upload/201603/20160302092648_J3MUHKOQ.jpg" alt=""></a>
        <label for="p2">&lt;</label>
        <label for="p4">&gt;</label>
      </li>
      <li>
        <a href=""><img
            src="https://images.shobserver.com/news/690_390/2018/5/26/09fae5d0-9e46-48aa-a519-84e6a7a0c0ce.jpg"
            alt=""></a>
        <label for="p3">&lt;</label>
        <label for="p5">&gt;</label>
      </li>
      <li>
        <a href=""><img src="https://www.zhifure.com/upload/images/2018/6/27121831935.jpg" alt=""></a>
        <label for="p4">&lt;</label>
        <label for="p1">&gt;</label>
      </li>
    </ul>
    <div class="indicators-wrapper">
      <div class="indicators">
        <label for="p1"></label>
        <label for="p2"></label>
        <label for="p3"></label>
        <label for="p4"></label>
        <label for="p5"></label>
      </div>
    </div>
  </section>
</body>

</html>
```

# 三栏等高自适应
## table
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>document</title>
  <style>
    section {
      display: table;
    }

    aside,
    main,
    div {
      display: table-cell;
    }

    aside {
      width: 100px;
      background-color: tomato;
    }

    div {
      width: 120px;
      background-color: rosybrown;
    }

    main {
      background-color: cornflowerblue;
    }
  </style>
</head>

<body>
  <section>
    <aside>Lorem ipsum dolor sit amet consectetur adipisicing elit. Reprehenderit aliquam sit, culpa dolore iste asperiores</aside>
    <main>Lorem ipsum dolor sit amet consectetur adipisicing elit. Vitae, harum, qui corrupti impedit nihil, laudantium unde dicta eligendi nemo sed suscipit ea! Itaque nam, deleniti sunt expedita magnam unde quidem.</main>
    <div>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Delectus vero modi
    </div>
  </section>
</body>

</html>
```
## float
```html

<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width ">
  <title>document</title>
  <style>
    section {
      overflow: hidden;
    }

    aside {
      float: left;
      width: 100px;
      background-color: tomato;
    }

    div {
      float: right;
      width: 120px;
      background-color: red;
    }

    main {
      overflow: hidden;
      background-color: cornflowerblue;
    }

    aside,
    div,
    main {
      padding-bottom: 333310px;
      margin-bottom: -333310px;
    }
  </style>
</head>

<body>
  <section>
    <aside>Lorem ipsum dolor sit amet consectetur adipisicing elit. Reprehenderit aliquam sit, culpa dolore iste asperiores</aside>
    <main>Lorem ipsum dolor sit amet consectetur adipisicing elit. Vitae, harum, qui corrupti impedit nihil, laudantium unde dicta eligendi nemo sed suscipit ea! Itaque nam, deleniti sunt expedita magnam unde quidem.</main>
    <div>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Delectus vero modi
    </div>
  </section>
</body>

</html>
```
## flex
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>document</title>
  <style>
    section {
      display: flex;
    }

    aside {
      width: 100px;
      background-color: tomato;
    }

    div {
      width: 120px;
      background-color: rosybrown;
    }

    main {
      background-color: cornflowerblue;
    }
  </style>
</head>

<body>
  <section>
    <aside>Lorem ipsum dolor sit amet consectetur adipisicing elit. Reprehenderit aliquam sit, culpa dolore iste asperiores</aside>
    <main>Lorem ipsum dolor sit amet consectetur adipisicing elit. Vitae, harum, qui corrupti impedit nihil, laudantium unde dicta eligendi nemo sed suscipit ea! Itaque nam, deleniti sunt expedita magnam unde quidem.</main>
    <div>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Delectus vero modi
    </div>
  </section>
</body>

</html>
```

# icon-font应用
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px blue;
    }

    @font-face {
      font-family: FontAwesome;
      src: url(fontawesome-webfont.woff2);
    }

    section {
      display: flex;
      flex-direction: row-reverse;
      justify-content: flex-end;
    }

    .star {
      font-family: FontAwesome;
      font-size: 30px;
    }

    .star::before {
      content: "\f006";
      padding-right: 5px;
    }

    .star:hover::before,
    .star:hover~span:before {
      content: "\f005";
    }

  </style>
</head>

<body>
  <section>
    <span class="star"></span>
    <span class="star"></span>
    <span class="star"></span>
    <span class="star"></span>
    <span class="star"></span>
  </section>
</body>

</html>
```

# flex外框自适应包裹
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    ul {
      list-style: none;
      padding: 0;
      float: left;
      display: flex;
      flex-flow: column wrap;
      align-content: flex-start;
      height: 300px;
    }

    ul li {
      height: 60px;
      width: 200px;
      background-color: cornflowerblue;
    }

    ul li:last-child {
      flex-grow: 1;
    }
  </style>
</head>

<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
    <li>8</li>
    <li>9</li>
    <li>10</li>
    <li>11</li>
    <li>12</li>
  </ul>
</body>

</html>
```

# 保持比例和大小的卡片阵列
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.08);
      box-shadow: inset 0 0 1px red;
    }

    section {
      display: flex;
      flex-flow: wrap;
    }

    div {
      flex-grow: 1;
      width: 200px;
      xheight: 100px;
      xfloat: left;

    }

    div::after {
      content: '';
      display: block;
      padding-bottom: 100%;
    }

    div:nth-last-child(-n + 8) {
      height: 0px;
    }
  </style>
</head>

<body>
  <section>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
  </section>
</body>

</html>
```
重点：
1. 等比例
当 padding 为百分比时，padding-top 和 padding-bottom 的计算是根据该元素的 width 决定的。所以若元素的宽度是 100px，padding-top 为 100%，padding-top 就是 100px。
同样，如果需要 16:9 的比例，只需将 padding-bottom 设成 56.25%（9 / 16 = 0.5625）即可。
2. 最后一行
`flex-grow: 1;`会导致最后一行的大小变化，直接把最后一行的几个元素变成高度为0。

# hover应用
## hover后颜色保持
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px blue;
    }

    div:hover span {
      color: black;
      transition: none;
    }

    div span:hover {
      color: red;
      transition: none;
    }

    span {
      float: left;
      transition: color 0s 99999s;
    }
  </style>
</head>

<body>
  <div>
    <span>Lorem.</span>
    <span>Rem?</span>
    <span>Qui.</span>
    <span>Provident.</span>
  </div>
</body>

</html>
```
## hover切换选项卡
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.08);
      box-shadow: inset 0 0 1px cornflowerblue;
    }

    .foo {
      float: right;
      display: flex;
      position: relative;
      justify-content: flex-end;
    }

    span {
      height: 40px;
      color: rosybrown;
      display: inline-block;
      line-height: 40px;
      transition: color 0s 999999s;
    }

    /* 默认选中第一个选项卡 */
    div.foo span:first-child {
      color: cornflowerblue;
    }

    /* hover到了span所有选项卡恢复颜色，选中了某个选项卡就会变色，由于优先级相同，后一个选择器生效。 */
    div.foo:hover span {
      color: rosybrown;
      transition: none;
    }

    div.foo span:hover {
      color: cornflowerblue;
      transition: none;
    }

    div.foo div {
      visibility: hidden;
      position: absolute;
      right: 0;
      top: 100%;
      width: 100%;
      height: 300px;
      /* 使可见性延迟很长时间才消失，即选中某个选项卡后保持状态 */
      transition: visibility 0s 99999s;
    }

    /* 默认显示第一个选项卡内容 */
    div.foo span:first-child>div {
      visibility: visible;
    }

    /* hover到整个元素块任何地方都会隐藏选项卡内容，但hover到特定的span就会显示其选项卡内容，优先级和上面类似。 */
    div.foo:hover span div {
      visibility: hidden;
      transition: none;
    }

    div.foo span:hover>div {
      visibility: visible;
      transition: none;
    }
  </style>
</head>

<body>
  <div class="foo">
    <span>Lorem.<div>content111</div></span>
    <span>Fugit!<div>content222</div></span>
    <span>Eaque.<div>content333</div></span>
  </div>
</body>

</html>
```

# footer实现
```html
<!-- 实现当页面内容很少时，页面的footer处于视口的底部；当页面内容较多时（多于视口大小），footer显示在页面的尾部 -->
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    html {
      height: 100%;
    }

    p {
      margin: 0;
    }

    body {
      display: flow-root;
      min-height: 100%;
      position: relative;
      margin: 0;
      box-sizing: border-box;
      padding-bottom: 25px;
    }

    footer {
      width: 100%;
      position: absolute;
      bottom: 0;
    }
  </style>
</head>

<body>
  <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ut, explicabo.</p>
  <footer>Lorem ipsum dolor sit.</footer>
</body>

</html>
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    html {
      height: 100%;
    }

    p {
      margin: 0;
      margin-bottom: auto;
    }

    body {
      display: flex;
      flex-direction: column;
      min-height: 100%;
      position: relative;
      margin: 0;
      box-sizing: border-box;
    }
  </style>
</head>

<body>
  <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ut, explicabo.</p>
  <footer>Lorem ipsum dolor sit.</footer>
</body>

</html>
```
# animation应用
元素公转但不自转
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      box-shadow: 0 0 1px plum;
      background-color: rgba(0, 0, 0, 0.1);
    }

    @keyframes ro {
      to {
        transform: rotate(360deg);
      }
    }

    /* 公转 */
    div {
      margin-top: 100px;
      width: 200px;
      animation: ro 5s linear infinite;
    }

    /* 公转不自转 */
    span {
      display: inline-block;
      animation: ro 5s linear reverse infinite;
    }
  </style>
</head>

<body>
  <div>我</div>
  <div><span>我</span></div>
</body>

</html>
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      box-shadow: 0 0 1px plum;
      background-color: rgba(0, 0, 0, 0.1);
    }

    @keyframes ro2 {
      from {
        transform: rotate(0deg) translateX(150px) rotate(0deg)
      }

      to {
        transform: rotate(360deg) translateX(150px) rotate(-360deg)
      }
    }

    section {
      width: 50px;
      height: 50px;
      margin: auto;
      margin-top: 200px;
      transition: 1s;
      animation: ro2 5s linear infinite;
    }
  </style>
</head>

<body>
  <section></section>
</body>

</html>
```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    @keyframes sin {
      from {
        margin-left: 0;
      }

      to {
        margin-left: 200px;
      }
    }

    @keyframes cos {
      from {
        margin-top: 0;
      }

      to {
        margin-top: 200px;
      }
    }
    
    /* https://cubic-bezier.com/  贝塞尔曲线模拟 */
    strong {
      display: inline-block;
      animation: sin 3s cubic-bezier(.36, 0, .64, 1) alternate infinite, cos 3s -1.5s cubic-bezier(.36, 0, .64, 1) alternate infinite;
    }
  </style>
</head>

<body>
  <strong>TA</strong>
</body>

</html>
```

# 几行代码
## 有没有滚动条都居中显示
```css
body{
  padding-left: calc(100vw - 100%);
}
```
