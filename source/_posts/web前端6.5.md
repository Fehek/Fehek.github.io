---
title: web前端学习笔记6.5——JS简例
subtitle: JS简例
categories: web前端学习笔记
abbrlink: 7e41b4b8
tags:
  - front-end
  - JavaScript
date: 2021-01-14 22:13:00
index_img:
---

# 搜索栏
```html
<html>
<head>
  <style>
    input {
      color: #999
    }
  </style>
</head>

<body>
  <input type="text" value="手机">

  <script>
    // 1.获取元素
    var text = document.querySelector('input')
    // 2.注册事件 获得焦点事件 onfocus
    text.onfocus = function () {
      if (this.value === '手机') {
        this.value = ''
      }
      // 获得焦点需要把文本框里的颜色变深
      this.style.color = '#333'
    }
    // 3.注册事件 失去焦点事件 onblur
    text.onblur = function () {
      if (this.value === '') {
        this.value = '手机'
      }
      // 失去焦点需要把文本框里的颜色变深
      this.style.color = '#999'
    }
  </script>
</body>

</html>
```
