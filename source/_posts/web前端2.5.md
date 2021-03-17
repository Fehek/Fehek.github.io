---
title: web前端学习笔记2.5——CSS画图
subtitle: CSS画图
categories: web前端学习笔记
abbrlink: 32c066a8
tags:
  - web前端
  - CSS
date: 2020-10-08 22:16:00
index_img:
---
用CSS画出图形

# 矩形
<style>
#rectangle {
  width: 200px;
  height: 100px;
  background: teal;
	}
</style>
<body>
  <div id="rectangle"></div>
</body>
<br>

```css
#rectangle {
  width: 100px;
  height: 100px;
  background: teal;
} 
```
# 三角形
## 等腰三角形
1. 上三角
<style>
#triangle-up {
   width: 0;
   height: 0;
   border-left: 50px solid transparent;
   border-right: 50px solid transparent;
   border-bottom: 100px solid teal;
}
</style>
<body>
  <div id="triangle-up"></div>
</body>
<br>

```css
#triangle-up {
   width: 0;
   height: 0;
   /*  三角形的高度  */
   border-left: 50px solid transparent;
   border-right: 50px solid transparent;
   /*  三角形的底长  */
   border-bottom: 100px solid teal;
}
```

2. 下三角
<style>
#triangle-down {
   width: 0;
   height: 0;
   border-left: 50px solid transparent;
   border-right: 50px solid transparent;
   border-top: 100px solid teal;
}
</style>
<body>
  <div id="triangle-down"></div>
</body>
<br>

```css
#triangle-down {
   width: 0;
   height: 0;
   border-left: 50px solid transparent;
   border-right: 50px solid transparent;
   border-top: 100px solid teal;
}
```

3. 左三角
<style>
#triangle-left {
   width: 0;
   height: 0;
   border-top: 50px solid transparent;
   border-bottom: 50px solid transparent;
   border-right: 100px solid teal;
}
</style>
<body>
  <div id="triangle-left"></div>
</body>
<br>

```css
#triangle-left {
   width: 0;
   height: 0;
   border-top: 50px solid transparent;
   border-bottom: 50px solid transparent;
   border-right: 100px solid teal;
}
```

4. 右三角
<style>
#triangle-right {
   width: 0;
   height: 0;
   border-top: 50px solid transparent;
   border-bottom: 50px solid transparent;
   border-left: 100px solid teal;
}
</style>
<body>
  <div id="triangle-right"></div>
</body>
<br>

```css
#triangle-right {
   width: 0;
   height: 0;
   border-top: 50px solid transparent;
   border-bottom: 50px solid transparent;
   border-left: 100px solid teal;
}
```

## 直角三角形
1. 左上三角
<style>
#triangle-topleft {
   width: 0;
   height: 0;
   border-top: 120px solid teal;
   border-right: 100px solid transparent;     
}
</style>
<body>
  <div id="triangle-topleft"></div>
</body>
<br>

```css
#triangle-topleft {
   width: 0;
   height: 0;
   /*  三角形的高  */
   border-top: 120px solid teal;
   /*  三角形的上底  */
   border-right: 100px solid transparent;     
}
```

2. 右上三角
<style>
#triangle-topright {
   width: 0;
   height: 0;
   border-top: 120px solid teal;
   border-left: 100px solid transparent;     
}
</style>
<body>
  <div id="triangle-topright"></div>
</body>
<br>

```css
#triangle-topright {
   width: 0;
   height: 0;
   border-top: 120px solid teal;
   border-left: 100px solid transparent;     
}
```

3. 左下三角
<style>
#triangle-bottomleft {
   width: 0;
   height: 0;
   border-bottom: 120px solid teal;
   border-right: 100px solid transparent;     
}
</style>
<body>
  <div id="triangle-bottomleft"></div>
</body>
<br>

```css
#triangle-bottomleft {
   width: 0;
   height: 0;
   border-bottom: 120px solid teal;
   border-right: 100px solid transparent;     
}
```

4. 右下三角
<style>
#triangle-bottomright {
   width: 0;
   height: 0;
   border-bottom: 120px solid teal;
   border-left: 100px solid transparent;     
}
</style>
<body>
  <div id="triangle-bottomright"></div>
</body>
<br>

```css
#triangle-bottomright {
   width: 0;
   height: 0;
   border-bottom: 120px solid teal;
   border-left: 100px solid transparent;     
}
```
