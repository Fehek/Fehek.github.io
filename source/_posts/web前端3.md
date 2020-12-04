---
title: web前端学习笔记3
abbrlink: afe83c00
tags:
  - web前端
  - JavaScript
date: 2020-09-19 17:58:00
categories: web前端学习笔记
index_img:
banner_img:
---

# 简介
+ 每一条JS语句后面必须加分号。
+ 所有JS代码必须写在 script 标签里。为了规范，script 标签写在 head 标签中。
+ 可以引用多个 script 标签，顺序执行。
+ 用 src 可以引入外部js文件。若当前 script 标签作用为引入外部文件，不能再写代码。
+ 注释
```
// 单行注释   快捷键 Ctrl + /

 /*    
多行注释    快捷键 Ctrl + Shift + /
 */
```

# 基础代码
控制浏览器弹出一个警告框
`alert("XXXXX");`

在页面输出一个内容
`document.write("XXXX");`

向控制台输出一个内容
`console.log("XXXX");`

# 数据类型
## 标识
用户自定义的所有名字叫做标识符。
1. 标识符必须由数字、字母、下划线和美元符号$组成。
2. 不能以数字开头。
3. 区分大小写。

## 变量
+ 声明变量  `var 声明语句`
+ 当前常量/变量的数据类型  `typeof 变量/常量`
JS是弱语言，变量被赋值成什么类型就是什么类型。不要在后续代码中改变该变量的数据类型。

## 强制数据类型转换
### 运算中转换
1. 任何数据类型和字符串类型做相加操作，其他数据类型会自动转换成字符串类型。字符串拼接。
2. 任何数据除了和字符串做相加运算外，先要将字符串转成数字。
+ 与NaN做算数运算结果始终是NaN，包括NaN本身和NaN做算数运算。

### 转成布尔值
```js
var tmp = Boolean(1); //true
var tmp = Boolean(0); //false
var tmp = Boolean(-2); //true

var tmp = Boolean(""); //false
var tmp = Boolean("hello"); //true

var tmp = Boolean(null); //false
var tmp = Boolean(undefined); //false
```
+ 数字0转成布尔值为false，所有非0数字转成布尔值为true。
+ 空字符串转成布尔值为false，所有非空字符串转成布尔值为true。
+ null和undefined转成布尔值为false。
+ 除了undefined，null，-0，+0，NaN，""（空字符串）都转换为true。

### 转成数字
```js
var tmp = Number(true); //1
var tmp = Number(false); //0
var tmp = Number("20a"); //NaN
var tmp = Number(null); //0
var tmp = Number(undefined); //NaN

var tmp = parseInt("20a"); //20
var tmp = parseInt(true); //3

var tmp = parseFloat("10.33"); //10.33
alert(typof tmp); //number
```
+ `Number()`
1. 布尔值转换true → 1，false → 0。
2. 字符串如果是纯数字字符串会转成数字，否则转换成NaN。
3. 特殊数据类型null → 0，undefined → NaN。
+ `parseInt()`
兼容Number的功能，取整
+ `parseFloat()`
解析一个字符串，并返回一个浮点数。

# 被除数为0
```js
var tmp = 1 / 0; //Infinity 无穷大
var tmp = -1 / 0; //-Infinity 无穷小
```

# 运算符
## 一元运算符
只能操作一个值的运算符，叫做一元运算符。
`x+=a`简写`x=x+a`，`x-=a`简写`x=x-a`，`x*=a`简写`x=x*a`，`x/=a`简写`x=x/a`
+ `a++;`&nbsp;&nbsp;&nbsp;&nbsp;++后置，先取a的值，然后再进行+1操作。
+ `++a;`&nbsp;&nbsp;&nbsp;&nbsp;++后置，先进行+1操作，然后再取a的值。
```js
var a = 5;
alert(a++); //5
alert(a); //5

var a = 5;
alert(++a); //6
alert(a); //6

var a = 5;
alert(a--); //5
alert(a); //4

var a = 5 ;
alert(--a); //4
alert(a); //4
```

## 关系运算符
```js
alert(5 > 3); //true
alert("a" > "b"); //false
alert("abcf" > "abd"); //false
alert(2 > true); //true → 1 true

alert(1 == true); //true
alert(0 == false); //true

alert(20 == "20"); //true
alert(1 != NaN); //true
alert(NaN != NaN); //true

alert(20 === "20"); //false
alert(20 === Number("20")); //true
```
1. 两个操作数都是数值，则数值比较。
2. 两个操作数都是字符串，则比较两个字符串对应的字符编码值。
3. 两个操作数有一个是数值，则将另一个转换成数值，再进行数值比较。
4. 一个操作数是NaN，则 == 返回false，!= 返回true；并且NaN和自身不等。
5. 在全等和全不等判断上，只有值和类型都相等，才返回true，否则返回false。(===，比较前不需要自动转换)
6. ！= 和 ！== 两种不等运算符，区别与 == 和 === 类似。

## 逻辑运算符
+ **与 `&&`**
表达式1 && 表达式2
只有当两个表达式的结果都为真时，运算结果才为true。
【短路操作】当表达式1为false时，表达式2就不去执行，直接判断整个与运算为false。
有多个 && 连续时，从左往右返回第一个为假的值。
+ **或 `||`**
表达式1 || 表达式2
只有当两个表达式的结果都为假时，运算结果才为false。
【短路操作】当表达式1为true时，表达式2就不去执行，直接判断整个或运算为true。
可用于返回默认值。如果左侧表达式可能产生空值，右侧的表达式可作为替代。
有多个 || 连续时，从左往右返回第一个为真的值。
+ **非 `!`**
可以用于任意数据类型的值，运算结果会返回一个布尔值。
流程：先将这个值转成布尔值，然后取反。
+ 三元运算符`a ? b : c`
若问号左侧条件为真，选择左侧的的值，条件为假选择右侧的值。
```js
console.log(true ? 1 : 2); //1
console.log(false ? 1 : 2); //2
```

优先级： `||` < `&&` < `比较运算符(>,==)` < `其它运算符`

