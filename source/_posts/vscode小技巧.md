---
title: VS Code小技巧
categories: 实用
abbrlink: eab54a24
tags:
  - VS Code
  - Emment
date: 2020-10-07 09:53:00
index_img:
---

# 快捷键
按键|功能
:--|:--
Ctrl + D|选中下一个相同字符
Ctrl + Shift + L|选择所有出现的当前选择
Ctrl + F2|选择所有出现的当前单词
Alt + 鼠标点击|多行光标
中键下拉|多行光标
按住 Ctrl|对整个单词进行处理
Alt + ↑/↓|上/下移动行
Ctrl + B|侧边栏显示/隐藏
Ctrl + L|选择当前行
Ctrl + Tab|切换选项卡
Ctrl + Shift + PgUp/PgDown|向左/右移动选项卡
Ctrl + Shift + P|打开命令窗口

# Emment
首先将emmet.triggerExpansionOnTab设置为true，可以按Tab启用Emmet展开缩写。
## 语法
写法|代表
:--|:--
E|HTML标签。
E#id|id属性
E.class|class属性
E[attr=foo]|某一特定属性
E{foo}|标签包含的内容是foo
E>N|N是E的子元素
E+N|N是E的同级元素
E^N|N是E的上级元素

## 用法
### 元素
```html
div             => <div></div>、
br              => <br>
html:5          => 生成html5标准的包含body为空基本dom
a:mail          => <a href="mailto:"></a>
a:link          => <a href="http://"></a>
base            => <base href="">
link            => <link rel="stylesheet" href="">
script:src      => <script src=""></script>
form:get        => <form action="" method="get"></form>
label           => <label for=""></label>
input           => <input type="text">
inp             => <input type="text" name="" id="">
input:password  => <input type="password" name="" id="">
select          => <select name="" id=""></select>
option          => <option value=""></option>
bq              => <blockquote></blockquote>
btn             => <button></button>
btn:s           => <button type="submit"></button>
btn:r           => <button type="reset"></button>
```

### 文本操作符
```html
div{这是一段文本}
<div>这是一段文本</div>
a{点我}
<a href="">点我点我</a>  
```

### 属性操作符
```html
div.abc   => <div class="abc"></div>
<!-- 绑定多个类名 -->
div.a.b.c => <div class="a b c"></div>
div#id1   => <div id="id1"></div>
<!-- 省略写会自动联想补全 -->
.abcd     => <div class="abcd"></div>
#id       => <div id="id2"></div>
table>.row>.col
=>
<table>
    <tr class="row">
        <td class="col"></td>
    </tr>
</table>
<!-- 自定义属性使用 [attr1='' attr2=''] -->
a[href='#' target='_blank']
=>
<a href="#" target="_blank"></a>
```

### 嵌套操作符
- 子级 >
```html
div#id1>ul>li 
=> 
<div id="id1">
    <ul>
        <li></li>
    </ul>
</div>
```
- 同级 +
```html
div#id1+div.abc
=>
<div id="id1"></div>
<div class="abc"></div>
```
- 父级 ^
用于生成父级元素的同级元素,从这个字符所在位置开始,查找左侧最近的元素的父级元素并生成其兄弟级元素.
```html
div>p.parent>span.child^ul.brother>li
=>
<div>
    <p class="parent"><span class="child"></span></p>
    <ul class="brother">
        <li></li>
    </ul>
</div>
```

### 乘法
```html
ul>li*3
=>
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

### 自动计数
```html
ul>li.item${item number:$}*3
<ul>
    <li class="item1">item number:1</li>
    <li class="item2">item number:2</li>
    <li class="item3">item number:3</li>
</ul>
```
如果生成两位数则使用两个连续的$$，更多位数以此类推。..
使用@修饰符，可以更改编号方向（升序或降序）和基数（例如起始值）。这个操作符在$之后添加。
@-表示降序，@+表示升序，默认使用升序。
@N可以改变起始值，如果配合升降序使用的话N是放到+-符后。
<font color="red"><strong>! ! ! 经过测试，发现@-并不能用，目前还不知道原因。</strong></font>
```html
ul>li.item$@-*3
=>
<ul>
    <li class="item3"></li>
    <li class="item2"></li>
    <li class="item1"></li>
</ul>
---------------------------
ul>li.item$@-10*3
=>
<ul>
    <li class="item12"></li>
    <li class="item11"></li>
    <li class="item10"></li>
</ul>
```

### 包装文本
1. 选中文本，打开命令窗口（Ctrl + Shife + P），输入ewrap，打开Emmet:使用缩写进行包装(Wrap with Abbreviation)。
2. 输入缩写字符，按下回车。
```html
第一行
第二行
第三行
<!-- 輸入div>ul>li*，回车，可以看到 -->
<div>
  <ul>
    <li>第一行</li>
    <li>第二行</li>
    <li>第三行</li>
  </ul> 
</div> 

1.第一行
2.第二行
3.第三行
<!-- 加 |t 可以删除序号 -->
<!-- 輸入div>ul>li*|t，回车，可以看到 -->
<ul>
  <li>第一行</li>
  <li>第二行</li>
  <li>第三行</li>
</ul> 
```