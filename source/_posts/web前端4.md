---
title: web前端学习笔记4
abbrlink: 318ca9a3
tags:
  - web前端
  - JavaScript
categories: web前端学习笔记
date: 2020-11-17 10:27:00
index_img:
banner_img:
---

# 函数
- function定义
```js
function square(x) {
  return x * x
}
// 当function关键字在一行的开头时，创建函数声明语句
// 函数声明中函数的名字不可以少
```
```js
var square = function a(x) {
  a()
  return x * x
}
// 当function不在一行开头时，创建函数表达式
// 变量声明，并将函数表达式的求值结果，即一个函数赋值给这个变量
// 函数表达式中，函数名可选，如果写了，只能在函数内部使用
// 在函数外部需要使用变量名
```
- let 与 var
let定义的变量在块级作用域内，var是在函数作用域
let定义的变量不能重复定义，也不能用var重新定义
let定义的变量没有将定义提升，但有TDZ行为，即该作用域内定义完成之前不能使用该变量
- 调用栈
计算内部用于存储函数返回位置，函数的局部变量的内存空间，叫调用栈
函数间的相互调用的逻辑关系，叫调用栈

# 编码
## 原码
- 第一位表示符号, 其余位表示值
[+1]<sub>原</sub> = 0000 0001
[-1]<sub>原</sub> = 1000 0001
- 第一位是符号位, 所以8位二进制数的取值范围就是:
[1111 1111 , 0111 1111]

## 反码
- 正数的反码是其本身
- 负数的反码是符号位不变，其余各位取反
[+1] = [00000001]<sub>原</sub> = [00000001]<sub>反</sub>
[-1] = [10000001]<sub>原</sub> = [11111110]<sub>反</sub>

## 补码
- 正数的补码是其本身
- 负数的补码是符号位不变，其余各位取反，最后+1(即其反码+1)
[+1] = [00000001]<sub>原</sub> = [00000001]<sub>反</sub> = [00000001]<sub>补</sub>
[-1] = [10000001]<sub>原</sub> = [11111110]<sub>反</sub> = [11111111]<sub>补</sub>

如果用原码表示，让符号位也参与计算，对于减法来说，结果是不正确的。
用反码计算减法，结果的真值部分是正确的。但会有 [0000 0000]<sub>原</sub> 和 [1000 0000]<sub>原</sub> 两个编码表示0。
补码的出现解决了这个问题。[1000 0000]<sub>补</sub> 就是-128，实际上是使用以前的-0的补码来表示-128， 所以-128并没有原码和反码表示。使用补码，不仅修复了0的符号以及存在两个编码的问题，而且还能够多表示一个最低数。

8位二进制，使用原码或反码表示的范围为[-127, +127]，而使用补码表示的范围为[-128, 127]。
常用到的32位，可以表示范围是[-231, 231-1]。

# 位运算
## 移位
- 左移 <<
将运算数的二进制整体左移指定位数，低位用0补齐。
num << 1，相当于num乘以2。
- 带符号右移 >>
高位补符号位，即正数补0，负数补1。
num >> 1，相当于num除以2，并向下取整Math.floor(num/2)
- 无符号右移 >>>
忽略符号位，高位补0。
移完以后的数字永远当正数理解。

js中浮点数参与位运算时，取其整数部分的低32bit参与运算。

## 位运算符
- 按位与 `&`
  - 每对比特位执行与（AND）操作
  - a 和 b 都是 1 时，a AND b 才是 1
  - `x & 0 = 0, x & -1 = x`

- 按位或 `|`
  - 每对比特位执行或（OR）操作
  - a 或 b 为 1，则 a OR b 结果为 1
  - `x | 0 = x, x | -1 = -1`

- 按位异或 `^`
  - 每对比特位执行异或（XOR）操作
  - a 和 b 不相同时，a XOR b 的结果为 1
  - `x ^ 0 = x, x ^ -1 = ~x`
  - `a ^ b ^ b = a ^ 0 = a`

- 按位非 `~`
  - 每个比特位执行非（NOT）操作，一元运算符
  - NOT a 结果为 a 的反转（即反码）
  - `~n = -(n+1)`

# 浮点数的表示
* 浮点数一般使用8字节即64bit存储，为双精度浮点数
  * 也有用4字节即32bit存储的单精度浮点数
* 最左边一位表示符号，0表示正，1表示负
* 接下来的11bit表示指数
* 剩余的表示科学记数法中的小数部分，不含整数部分，因为整数部分总是1
* 为什么指数的范围是-1023到1024而非-1024到1023？
  * 指数部分并没有使用补码进行存储
  * 正1024次方表示无穷大
* 为什么指数部分读出时要减1023，写入时要加1023？
  * 指数部分使用原码，范围是0到2047
  * 表示-1023到1024
  * 所以0表示-1023
  * 所以读出时减，写入时加
* 为什么指数不用补码？
  * 为了能够从左往右扫描即可确定两个浮点数的大小
  * 即除符号位以外只看指数部分即可以确定大小
    * 即指数部分谁先出现1谁更大
    * 如果指数部分完全相同，那么底数部分谁先出现1谁更大
* 为什么要如此在意浮点大小对比的效率？
  * 因为浮点数更多时候是对比大小而非对比相等
    * 因为浮点数表示的不精确
      * 很难保证数学上相等的两个计算路径在程序中的计算结果也是完全相同的
        * 在程序中 a * b / c 的结果跟 a / c * b 有可能不相同的
        * 所以在程序中很少判断两个浮点数的相等
          * 而是判断它们在数轴上的距离，即求其差的绝对值看是否小于某个精度
            * Math.abs(a - b) < Number.EPLSION
* 为什么不把底数的整数部分存储？
  * 因为底数的整数部分总是1
  * 效果就是二进制状态下的有效位数是53位
* 正因为有效位数有53位，如果用这53位全部分表示整数
  * 即2的53次方，可以保证在这个范围内的整数运算的精确
  * 所以有一个常量Number.MAX_SAFE_INTEGER的值即为2的53减1
  * 大于这个范围的数不是不能表示，只是不保证完全精确
    * 大于这个范围无法表示浮点数，且整数也不是连续表示的
* 因为总的有效数字只有53位
  * 所以如果整数部分使用的越多，小数部分就越小，反之亦然
  * 而数值越大，整数部分需要的有效位数就越多，而小数部分的有效位数就越少
    * 即数值越大，小数部分的精度就越低
  * 而数值越小，整数部分需要的有效位数就越少，而小数部分的有效位数就越多
    * 即数值越小，小数部分的精度就越高
* IEEE754
  * 单精度浮点数 float   float32   f32
    * 使用4字节表示，其指数部分8bit，底数23bit
  * 双精度浮点数 double  float64   f64
    * 使用8字节表示，指数部分11bit，底数52bit

# 对象与属性
对象是任意数量的（属性与值的对应关系）的集合
有些语言里对象也称关联数组（值与其名字的关联关系）
数组是值的有序集合（即值在数组里是有序的）
对象是值的俱名集合（即值在对象里是有名字的）

# 函数
## 数组
代码|意义
:-|:-
[Arry](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)|数组
var a = []|创建空数组
var a = [1,2,3+2,4]|创建有初始内容的数组
var a = Array(5)|创建长不为0的空数组
var num = a[i]|读取数组中的第i(可为表达式)项
a.push(value)|在数组末尾增加一项
var value = a.pop()|删除数组末尾的项并返回这一项的值
a.unshift(value)|在数组的前面增加一项
var value = a.shift()|删除数组第一项项并返回这一项的值
a.length|数组长度
[a.splice(i, n, 'add')](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)|从第i位开始删掉n项，并插入“add”，会改变原数组
[a.slice(i, j)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)|从第i项开始，j项(0开始,不包括)结束浅拷贝，返回一个新数组
[a.fill(0, i, j)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)|从第i位开始填充0，第j位结束
[a.indexOf(abc, 2)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)|从第2项开始找到"abc"的第一个索引，若不存在则返回-1
[a.lastIndexOf(a, 2)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf)|从第2项开始逆向查找"abc"的最后一个的索引，如果不存在则返回 -1
[a1.concat(a2)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)|合并a1和a2两个数组`[...a1, ...a2, ...a3]`
[a.includes(0, NaN)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)|数组是否包含一个指定的值，包含返回 true，否则返回false
[a.reverse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)|将数组中元素的位置颠倒，并返回该数组
[a.sort()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)|对数组的元素进行排序并返回，默认排序是将元素转换为字符串，比较它们的UTF-16代码单元值
[a.join(' + ')](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)|一个数组的所有元素连接成一个字符串并返回这个字符串

## 字符串
代码|意义
:-|:-
str.toUpperCase()|调用字符串转为大写并返回
str.toLowerCase()|调用字符串转为小写并返回
str.charCodeAt()|字母转为ascii码
String.fromCharCode(num)|ascii码转为字母
[str.charAt(i)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charAt)|返回字符串的第i个字符`(0 <= i <= length-1)`
[a.split(" ")](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)|将一个字符串分割成子字符串数组

## 数字
代码|意义
:-|:-
a.toFixed()|指定小数位数
a.toString()|转成几进制
parseInt()|丢弃小数部分,保留整数部分

## Math
代码|意义
:-|:-
Math.pow(x,y)|x 的 y 次幂
Math.abs(x)|x 的绝对值
Math.round()|四舍五入
Math.ceil()|向上取整,有小数就整数部分加1
Math.floor()|向下取整
Math.trunc()|取整数部分
Math.max(a,b,c)|取其最大的值

## Map
代码|意义
:-|:-
const map = new Map();|声明一个Map
map.size|返回Map对象中所包含的键值对个数
map.set(key, val)|向Map中添加新元素，若key已经有值，则键值会被更新，否则生成新键值对
map.get(key)|通过key值返回value值
map.has(key)|判断Map对象中是否存在key，若存在返回true，否则返回false
map.delete(key)|通过键值从Map中删除对应的数据
map.clear()|将这个Map中的所有元素删除
for(let k of map.key){console.log(k);}|遍历key值
for(let v of map.values()){console.log(v);}|遍历values值`Object.values()`
for(let [k, v] of map.entries()){console.log(k + "===" + v);}|遍历键值对，以数组形式存在
[map.forEach](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/forEach)|返回三个参数，value,key,map本身

## Set
代码|意义
:-|:-
const set = new Set();|声明一个Set
set.size|返回Set的成员总数
set.add(val)|添加某个值，返回Set结构本身（可链式调用），若相同值已存在，则不会添加
set.has(val)|返回一个布尔值，表示该值是否为Set的成员
set.delete(val)|删除某个值，删除成功返回true，否则返回false
set.clear()|清除所有成员
const set = new Set([...a, ...b]);|a与b数组的交集

# 高阶函数
## map
- 对数组中每一项运行给定函数，返回每次函数调用的结果组成的数组。

**for写法**
```js
let arr = [1,2,3,4,5]
for (let i = 0; i < arr.length; i++){
  arr[i] = arr[i] * 3
}
```
**map写法**
```js
// item为当前遍历到的项,和arr[i]一样
arr = arr.map(function(item){return item * 3})

// es6写法
arr = arr.map(item => {return item * 3})
// 或者
arr = arr.map(item => item * 3)
```

## filter
- 对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。

```js
var people = [{
    name:'aa',
    isChecked:null
},{
    name:'bb',
    isChecked:null
},{
    name:'bb',
    isChecked:true
}]
```
**for写法**
```js
// 选出已经打卡了的人

var arr = []
for(var i = 0; i < people.length; i++){
  if(people[i].isChecked){
    arr.push(people[i])
  }
}
```
**filter写法**
```js
var arr = people.filter(function(item){return item.isChecked})

// es6写法
var arr = people.filter(item => item.isChecked)
```

## forEach
- forEach()对数组中的每一项运行给定函数，没有返回值。本质上跟for没有区别，只是写法不一样。

**for方式**
```js
// 给每个人都加上一个属性gender，设置为male

for(var i = 0; i < people.length; i++){
  people[i].gender = 'male'
}
```
**forEach方式**
```js
var arr = people.forEach(function(item){item.gender = 'male'})

// es6写法
var arr = people.forEach(item => {item.gender = 'male'})
```

## reduce
- [reduce()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)：每个元素执行一个提供的reducer函数(升序执行)，将其结果汇总为单个返回值。一般用在累计累加上。
**for方式**
```js
var sum = 0,arr = [1,2,3,4,5,6];
for(var i=0; i<arr.length; i++){
    sum += arr[i]
}
```
**reduce方式**
```js
sum = arr.reduce(function (a,b) {return a + b})

// es6写法
sum = arr.reduce((a,b)=>{return a + b})
```
**reduce实现**
```js
function reduce(ary, reducer, result) {
  var start = 0
  if (result == undefined) {
    result = ary[0]
    start = 1
  }
  for (var i = start; i < ary.length; i++) {
    result = reducer(result, ary[i], i, ary)
  }
  return result
}
```

## every 和 some
- every()对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
- some()对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true。

**every实现**
```js
function every(ary, test) {
  for (var i = 0; i < ary.length; i++) {
    if (!test(ary[i], i, ary)) {
      return false
    }
  }
  return true
}
```
**for写法**
```js
// 要求每个人都签到才能合格

var isPass = true
for(var i = 0; i < people.length; i++){
  if(!sporter[i].isChecked){
    isPass = false
    break
  }
}
// 由于有两次没有签到，返回false
```
**every写法**
```js
var arr = people.every(function(item){return item.isChecked})

// es6写法
var arr = people.every(item => item.isChecked)
```
<hr>

**some实现**
```js
function some(ary, test) {
  for (var i = 0; i < ary.length; i++) {
    if (!test(ary[i], i, ary)) {
      return true
    }
  }
  return false
}
```
**for写法**
```js
// 要求任意一次都签到就能合格

var isPass = false
for(var i = 0; i < people.length; i++){
  if(!sporter[i].isChecked){
    isPass = true
    break
  }
}
// 由于有一次签到，返回true
```
**some写法**
```js
var arr = people.some(function(item){return item.isChecked})

// es6写法
var arr = people.some(item => item.isChecked)
```

## find 和 findIndex
- find：返回传入一个测试条件（函数）符合条件的数组第一个元素。
- findIndex：返回传入一个测试条件（函数）符合条件的数组第一个元素位置。
**find实现**
```js
function find(ary, predicate) {
  for (var i = 0; i < ary.length; i++) {
    if (predicate(ary[i], i, ary)) {
      return ary[i]
    }
  }
}
```
**findIndex实现**
```js
function findIndex(ary, predicate) {
  for (var i = 0; i < ary.length; i++) {
    if (predicate(ary[i], i, ary)) {
      return i
    }
  }
}
```
**例子**
```js
// 找出第一个大于30的元素
var getItem, arr = [11, 22, 33, 44, 55, 66]
for (var i = 0; i < ary.length; i++) {
  if (arr[i] > 30) {
    getItem = arr[i]
    break
  }
}

// find写法
arr.find(function (val) { return val > 30 })
// es6写法
arr.find(val => val > 30)
```
```js
// 找出第一个大于30的元素的位置
var getItemIndex, arr = [11, 22, 33, 44, 55, 66]
for (var i = 0; i < ary.length; i++) {
  if (arr[i] > 30) {
    getItemIndex = i
    break
  }
}

// findIndex写法
arr.findIndex(function (val) { return val > 30 })
// es6写法
arr.findIndex(val => val > 30)
```

## bind
```js
function bind(f, ...fixedArgs) {
  return function (...args) {
    return f(...fixedArgs, ...args)
  }
}
```

# 二叉树
## 特殊二叉树
-|满二叉树(Full Binary Tree)|完全二叉树(Complete Binary Tree)
:-:|:-|:-
概念|是每一层上的节点数都是最大节点数|若除最后一层外的其余层都是满的，并且最后一层要么是满的，要么在右边缺少连续若干节点
总结点k|2<sup>h</sup> - 1|2<sup>h-1</sup> <= k <= 2<sup>h</sup> - 1
树高h|log<sub>2</sub>(k + 1)|log<sub>2</sub>k + 1
其它|第 i 层有 2<sup>i-1</sup> 个结点|


- 二叉查找树（Binary Search Tree）

一棵空树 或者 具有下列性质的二叉树：
1. 若任意节点的左子树不空，则左子树上所有节点的值均小于它的根节点的值；
2. 若任意节点的右子树不空，则右子树上所有节点的值均大于或等于它的根节点的值；
3. 任意节点的左、右子树也分别为二叉查找树。

## 遍历二叉树
前（先）序、中序、后序遍历