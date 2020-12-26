---
title: web前端学习笔记5
abbrlink: 468b9935
tags:
  - web前端
  - JavaScript
categories: web前端学习笔记
date: 2020-12-23 15:52:00
index_img:
banner_img:
---

# 语言类型
静态类型：所有变量及表达式的类型都是确定的（java，c，c++，typescript）
动态类型：变量的类型不确定（js，python，ruby）
强类型：不会做自动类型转换的语言（python）
弱类型：会做自动类型转换的语言（java，c，js，ts）

# UTF-8
Unicode 和 UTF-8 之间的转换关系表 ( x 字符表示码点占据的位 )

bit|字节数|形式 
:-:|:-:|:-
7 |1|0xxxxxxx 
11|2|110xxxxx 10xxxxxx
16|3|1110xxxx 10xxxxxx 10xxxxxx
21|4|11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
26|5|111110xx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx
31|6|1111110x 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx

# Object
- [Object.prototype.toString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)：返回一个表示该对象的字符串。
  - toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的 [[Class]] 。这是一个内部属性，其格式为 [object Xxx] ，其中 Xxx 就是对象的类型。
  - 对于 Object 对象，直接调用 toString()  就能返回 [object Object] 。而对于其他对象，则需要通过 call / apply 来调用才能返回正确的类型信息。

```js
Object.prototype.toString.call('')            // [object String]
Object.prototype.toString.call(1)             // [object Number]
Object.prototype.toString.call(true)          // [object Boolean]
Object.prototype.toString.call(Symbol())      // [object Symbol]
Object.prototype.toString.call(undefined)     // [object Undefined]
Object.prototype.toString.call(null)          // [object Null]
Object.prototype.toString.call(new Function()) // [object Function]
Object.prototype.toString.call(new Date())     // [object Date]
Object.prototype.toString.call([])            // [object Array]
Object.prototype.toString.call(new RegExp())   // [object RegExp]
Object.prototype.toString.call(new Error())    // [object Error]
Object.prototype.toString.call(document)      // [object HTMLDocument]
Object.prototype.toString.call(window)        // [object global] window 是全局对象 global 的引用
```

- [Object.create()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)：创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。


- [Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)：直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

```js
Object.defineProperty(obj, 'baz', {
  value: 11,            
  enumerable: false,    // 是否可枚举（在for in中出现）
  writable: true,       // 是否可修改（写入）
  configurable: false,  // 是否可以重新定义（可配置）
})
```

# this
* this相当于函数的一个隐藏参数，通过函数的不同调用方式确定，而非直接传递
  * 一个函数以函数的形式调用：f()，即从一个“变量名”里读出来，其内部的this为window
  * 一个函数以对象方法的形式调用：foo.bar.obj.f()，即从一个对象中读出并立刻调用，其内部的this为这个对象
  * 函数当成构造函数调用：new f()，即前面调用前面加new，其内部的this为一个新对象，这个对象以函数的prototype属性为原型。
  * 函数被call或apply，即 foo.bar.f.call(obj1, a, b), foo.bar.f.apply(obj1, [ a, b ])，函数内部的this为给call或apply传入的第一个参数
  * 箭头函数里this不再是隐含参数，而是相当于普通变量，直接向外层查找，类似于“词法作用域”
  * 函数被绑定后，this不再可变，即，f2 = f.bind(obj2), f2.call(obj3)，f2的调用还是相当于f的this为obj2。但当把f2当构造函数调用时，this的绑定会失效，new f2()时f内部的this为一个新的对象，且其原型为f.prototype
  ```js
    function bind(f, thisArg, ...fixedArgs) {
      return function(...args) {
        return f.call(thisArg, ...fixedArgs, ...args)
      }
    }
  ```
* 原型(\_\_proto\_\_)与原型属性(prototype)
  * 每个值（除null与undefined）都有一个原型。
    * 可以通过Object.getPrototypeOf(val)获取到val的原型
    * 也可以通过val.__proto__获取到其原型
  * 原型本身也是一个对象，它自身也会有原型，直到Object.prototype
  * 原型属性指的是函数上的一个名为prototype的属性，注意并不是函数的原型，函数的原型是f.__proto__，f.prototype一般称为原型属性，是用来做为该函数的实例的原型而用的。
    * 即 var a = new f()，a的原型(a.\_\_proto\_\_)为f的原型属性(f.prototype)
  * 每种类型的值都有自身的构造函数，所以同一类型的值之间是共享原型的，即它们构造函数的原型属性。
    * 数值的构造函数为Number
    * 字符串的构造函数为String
    * 布尔值的构造函数为Boolean
    * 数组的构造函数为Array
    * 对象的构造函数为Object
    * 函数的构造函数为Function
    * undefined和null不是对象，没有构造函数，也没有原型
      * 但typeof null为“object”，这属于js设计上的失误。
        * 原因在于，java中，一个变量如果要指向一个对象，但声明时指向的对象还不确定，会让这个变量指向null，用做对象的占位值。
    * 所以各构造函数的原型属性上都有为自身类型专门设计的方法。
* 构造函数
  * 当用new调用一个函数时，该函数可称做为构造函数
  * 构造函数内部的this为一个新对象，这个对象的原型是该构造函数的原型属性
  * 如果构造函数不返回一个对象类型的值，则this就会是这个new调用的求值结果
    * 如果返回一个对象类型的值，则该对象会成为new调用的求值结果
  * 函数的prototype属性会自动指向一个有一个属性的对象，该属性名为constructor，指向函数自己。
    * 所以可以通过任何值的constructor属性获知其构造函数。
* 对象的杂项
  * 属性的可枚举性：指属性会不会在for in循环中出出
    * 普通属性都是会的，如果想要创建不可枚举的属性，需要通过Object.defineProperty(obj,propName, 属性描述符)
  * 属性是否自有属性（对应于属性来源于原型链），可以通过obj.hasOwnProperty判断
    * 考虑到这个函数可能被覆盖，所以用Object.prototype.hasOwnProperty.call(obj, properyName)
  * 为对象增加属性永远增加在自有属性上，不会增加到原型上。修改也不会。
  * Object.create(proto，初始属性的属性描述符集合)
  * 属性描述符
    * 一个用来描述属性的特性的对象：
      * {
        value: 设置这个属性的值
        writable: 设置这个属性是否可修改
        enumerable: 设置这个属性是否可枚举
        configurable: 设置这个属性是否可重新定义
      }


# call, apply, bind
- [Function.prototype.call()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
  - 使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。
  - 语法：`function.call(thisArg, arg1, arg2, ...)`

- [Function.prototype.apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
  - 使用一个指定的 this 值和一个数组（或类数组对象）提供的参数。
  - 语法：`function.apply(thisArg, [argsArray])`
  - 区别：call() 方法的作用和 apply() 方法类似，区别就是 call() 方法接受的是参数列表，而 apply() 方法接受的是一个参数数组。

- [Function.prototype.bind()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
  - 创建一个**新函数**，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。
  - 语法：`function.bind(thisArg[, arg1[, arg2[, ...]]])`

# 堆
- 堆（Heap）是一种特别的完全二叉树。
父节点的值恒小于等于子节点的值，此堆称为最小堆。
父节点的值恒大于等于子节点的值，此堆称为最大堆。
- 堆支持两种操作
  - 往堆里增加一个元素【log(n)】
先将新元素放入堆的末尾，然后至底向上调整以维护堆的性质
  - 从堆顶删除并返回元素【log(n)】
先将堆顶元素删除，将堆尾元素放入堆顶，然后至顶向下调整以维护堆的性质
- 将一个随机分布的数组就地调整成一个堆
方法一：将结点顺序从前往后以每个结点开始至底向上调整即可【不会低于n\*log(n)】
方法二：从后往前，认为遇到的结点的两个子树都是正确的堆，然后向下调整【log(n)】

```js
// 假定ary是一个合法的堆
// pop函数删除堆顶的元素并维护堆的性质
function pop(ary){
  if(ary.length == 0){
    return
  }
  if(ary.length == 1){
    return ary.pop()
  }
  var result = ary[0]
  ary[0] = ary.pop()
  heapDown(ary, 0)
  return result
}

function push(ary, val){
  ary.push(val)
  heapUp(ary, ary.length -1)
  return ary
}
```
```js
function swap(ary, i, j){
  var t = ary[i]
  ary[i] = ary[j]
  ary[j] = t
  return ary
}

// 从堆的pos位置开始执行至顶向下的堆调整
function heapDown(ary, pos){
  var leftIdx = pos * 2 + 1
  var rightIdx = pos * 2 + 2
  var maxIdx = pos
  var len = ary.length
  if(leftIdx < len && ary[leftIdx] > ary[[maxIdx]){
    maxIdx = leftIdx
  }
  if(rightIdx < len && ary[leftIdx] < ary[[maxIdx]){
    maxIdx = rightIdx
  }
  if(pos !== maxIdx){
    swap(ary, pos, maxIdx)
    heapDown(ary, maxIdx)
  }
}

// 不使用递归
function heapDown2(ary, pos){
  var len = ary.length
  while(true){
    var leftIdx = pos * 2 + 1
    var rightIdx = pos * 2 + 2
    var maxIdx = pos
    if(leftIdx < len && ary[leftIdx] > ary[maxIdx]){
      maxIdx = leftIdx
    }
    if(rightIdx < len && ary[leftIdx] < ary[maxIdx]){
      maxIdx = rightIdx
    }
    if(pos !== maxIdx){
      swap(ary, pos, maxIdx)
      pos = maxIdx
    }else{
      break
    }
  }
}
```
```js
// 从堆的pos位置开始执行至底向上的堆调整
function heapUp(ary, pos){
  var pIdx = (pos - 1) >> 1  // 父结点的位置
  if(pos >= 0 && ary[pos] > ary[pIdx]){
    swap(ary, pos, pIdx)
    heapUp(ary, pIdx)
  }
}

// 非递归写法
function heapUp2(ary, pos){
  while(pos > 0){
    var pIdx = (pos - 1) >> 1
    if(ary[pos] > ary[pIdx]){
      swap(ary, pos, pIdx)
      pos = pIdx
    }else{
      break
    }
  }
}
```
```js
// 将一个不是堆的数组，就地调整成一个满足堆性质的数组
function heapify(ary){
  for(var i = (ary.length - 1) >> 1; i >= 0; i--){
    heapDown(ary, i)
  }
  return ary
}

function heapify2(ary){
  for(var i = 1; i < ary.length; i++){
    heapUp(ary, i)
  }
  return ary
}
```

写入原型属性
```js
function PriorityQueue(initial = []){
  this._elements = [...initial]  //用来保存堆中的元素
  this._heapify()
}

PriorityQueue.prototype._heapify = function heapify(){
  for(var i = (this._elements.length - 1) >> 1; i >= 0; i--){
    this._heapDown(i)
  }
  return this
}

PriorityQueue.prototype.push = function(val){
  this._elements.push(val)
  this._heapUp(this._elements.length - 1)
  return this
}

PriorityQueue.prototype.pop = function(val){
  if(this._elements.length < 2){
    return this._elements.pop()
  }
  var result = this._elements[0]
  this._elements[0] = this._elements.pop()
  this._heapDown(0)
  return result
}

// 查看堆顶的元素
PriorityQueue.prototype.peek = function(){
  return this._elements[0]
}

PriorityQueue.prototype._swap = function(i, j){
  var t = this._elements[i]
  this._elements[i] = this._elements[j]
  this._elements = t
}

PriorityQueue.prototype._heapDown = function(pos){
  var leftIdx = pos * 2 + 1
  var rightIdx = pos * 2 + 2
  var maxIdx = pos
  var ary = this._elements
  var len = ary.length
  if(leftIdx < len && ary[leftIdx] > ary[[maxIdx]){
    maxIdx = leftIdx
  }
  if(rightIdx < len && ary[leftIdx] < ary[[maxIdx]){
    maxIdx = rightIdx
  }
  if(pos !== maxIdx){
    this._swap(pos, maxIdx)
    this._heapDown(maxIdx)
  }
}

PriorityQueue.prototype._heapUp = function(pos){
  var ary = this._elements
  var pIdx = (pos - 1) >> 1
  if(pos >= 0 && ary[pos] > ary[pIdx]){
    this._swap(pos, pIdx)
    this._heapUp(pIdx)
  }
}
```

排序
```js
// 增加heapDown的结束条件
function heapDown(ary, pos,stopIdx = ary.length){
  var leftIdx = pos * 2 + 1
  var rightIdx = pos * 2 + 2
  var maxIdx = pos
  if(leftIdx < stopIdx && ary[leftIdx] > ary[[maxIdx]){
    maxIdx = leftIdx
  }
  if(rightIdx < stopIdx && ary[leftIdx] < ary[[maxIdx]){
    maxIdx = rightIdx
  }
  if(pos !== maxIdx){
    swap(ary, pos, maxIdx)
    heapDown(ary, maxIdx, stopIdx)
  }
}

// 建立一个新数组排序
function heapSort(ary){
  var p = new PriorityQueue()
  for(var item of ary){
    p.push(item)
  }
  for(var i = ary.length - 1; i >= 0; i--){
    ary[i] = p.pop()
  }
  return ary
}

// 就地排序
function heapSort2(ary){
  heapify(ary)
  for(var i = ary.length - 1; i >= 1; i--){
    swap(ary, i, 0)
    heapDown(ary, 0, i)
  }
  return ary
}
```

# 排序算法的稳定性
指排序算法在排序前后是否会改变相同元素的相对位置。
不改变相同元素的相对位置，则为稳定的排序算法，反之则为不稳定的排序算法。
排序稳定性用在类似成绩单排序的多优先级排序的场景中。

冒泡排序：稳定
选择排序：不稳定
插入排序：稳定
BST排序：稳定
归并排序：稳定
快速排序：不稳定
堆排序：不稳定

# class实现
```js
// 被调用时必须要加new，原型上的方法默认不能被枚举
class PriorityQueue{
  // 构造函数
  constructor(predicate = it => it, initial = []){
  	this._predicate() = predicate
  	this._elements = [...initial]
  	this._heapify()
  }

  // 静态方法，PriorityQueue.heapify()，用PriorityQueue.xxx调用
  static heapify(){

  }
  static from(ary){
  	return new PriorityQueue(it => it, ary)
  }

  // 实例方法，PriorityQueue.prototype._heapify()
  _heapify(){
    for(var i = (this._elements.length - 1) >> 1; i >= 0; i--){
      this._heapDown(i)
    }
    return this
  }
  push(val){
    this._elements.push(val)
    this._heapUp(this._elements.length - 1)
    return this
  }
  get size(){
  	return this._elements.length
  }
  set size(val){

  }
}

class PriorityQueue extends Array{  //继承
  constructor(){

  }
  push(val){
  	super() //父类，Array.call(this)
  	super.push(val)
  	this.push(val)  //写法错误
  }
  poll(val){
  	this.push()  //不同名称才能用this
  }
}

class A{
  #a = 8  //A.a禁止访问
  b = 9  //可以直接写在前面
  constructor(){
  	this.b = 9
  }
  getA(){
    return this.#a  //内部可以访问
  }
}
```

# 处理错误
## 重试
假设有一个函数 primitiveMulitiply，在50%的情况下将两个数相乘，在另外50%的情况下会触发 MultiplicatorUnitFailure 类型的异常。编写一个函数，调用这个容易出错的函数，不断尝试直到调用成功并返回结果为止。
确保只处理你期望的异常。【选自 JavaScript编程精解（第2版）P115】
```js
  class MultiplicatorUnitFailure extends Error {
    constructor(msg) {
      super(msg)
    }
  }
  function primitiveMultiply(a, b) {
    if (Math.random() < 0.5) {
      return a * b
    } else {
      throw new MultiplicatorUnitFailure()
    }
  }
  function multiply(a, b) {
    for (; ;) {
      try {
        var result = primitiveMultiply(a, b)
        return result
      } catch (e) {
        if (e instanceof MultiplicatorUnitFailure) {
          continue
        } else {
          throw e
        }
      }
    }
  }

  // primitiveMultiply(a, b) 会有50 % 的几率失败
  // multiply(a, b) 在尝试失败时会重新尝试直至成功
```

## 上锁的箱子
```js
var box = {
  locked: true;
  unlock: function(){ this.locked = false; },
  lock: function(){ this.locked = true; },
  _content: [],
  get content(){
    if(this.locked) throw new Error("locked!");
    return this._content;
  }
};
```
这是一个带锁的箱子。其中存放了一个数组，但只有在箱子被解锁时，才可以访问数组，不允许直接访问 \_content 属性。
编写一个名为 withBoxContent 的函数，接受一个函数类型的参数，其作用是解锁箱子，执行该函数，无论是正常返回还是抛出异常，在 withBoxContent 函数返回前都必须锁上箱子。【选自 JavaScript编程精解（第2版）P115】
```js
function withBoxUnlocked(f){
  try{
    box.unlock()
    return f(box.content)
  }finally{
    box.lock()
  }
}

withBoxContent((content) => {
  // do something with content
})
```
**类似python with语句的高阶函数**
```js
function With(...args) {//rest parameter
  var f = args.pop()
  try {
    f(...args)
  } finally {
    for (var arg of args) {
      arg.close()
    }
  }
}
```
```py
# python with 语句
With(open('a.txt'), open('b.txt'), (a, b) => {
   # do sth with a and b
})
```

# 正则表达式
## 创建正则表达式
1. 使用RegExp构造函数 
`var re1 = new RegExp("abc")`
2. 使用斜杠（/） 
`var re2 = /abc/`
一些字符如问号、加号在正则表达式中有特殊含义，如果想要表示其字符本身，需要在字符前加上反斜杠。如`var eighteenPlus = /eighteen\+/`

## 匹配测试
test方法接受传递的字符串，并返回一个布尔值，表示字符串中是否包含能与表达式模式匹配的字符串
```js
console.log(/abc/.test("abcde"))  //true
console.log(/abc/.test("abxde"))  //false
```

## 匹配字符集
- 一组字符放在方括号之间，匹配方括号中的任意字符
方括号中的两个字符间插入连字符（-）指定字符范围，字符顺序由Unicode决定。
```js
console.log(/[0123456789]/.test("in 1992"))  //true，匹配到 1 就结束了
console.log(/[0-9]/.test("in 1992"))  //true
```
- 许多字符组都有其内置的快捷写法
简写|意义
:-:|:-
\d|任意数字符号
\w|字母和数字符号（单词符号）
\s|任意空白符号（空格，制表符，换行符等类似符号）
\D|非数字符号
\W|非字母和数字符号
\S|非空白符号
.|除了换行符以外的任意符号

- 左方括号后添加 `^` 来排除某个字符集
```js
var notBianry = /[^01]/
console.log(notBianry.test("1100100010100110"))  //false
console.log(notBianry.test("1100100010200110"))  //true
```

## 部分模式重复
- 在正则表达式某个元素后添加 `+` ，表达该元素至少重复一次。
- `*` 含义与加号类似，但是可以匹配模式不存在的情况。只有无法找到可以匹配的文本才会考虑匹配该元素从未出现的情况。
```js
console.log(/'\d+'/.test("'123'"))  //true
console.log(/'\d+'/.test("''"))     //false
console.log(/'\d*'/.test("'123'"))  //true
console.log(/'\d*'/.test("''"))     //true
```
- 元素后面跟一个问号表示这部分模式“可选”，即模式可能出现 0 次或 1 次。
```js
var neighbor = /neighbou?r/
console.log(neighbor.test("neighbour"))  //true
console.log(neighbor.test("neighbor"))   //true
```
- 花括号准确指明某个模式的出现次数
例如，某个元素后加 {4}，则该模式需要出现且仅能出现 4 次。{2,4} 表示该元素至少出现 2 次，至多出现 4 次。
也可省略任意一侧的数字，表示不限制这一侧的数量。 {,5} 表示 0 到 5 次，{5,} 表示至少 5 次
```js
var dateTime = /\d\d-\d\d-\d\d\d\d \d\d:\d\d/
console.log(dateTime.test("25-12-2020 11:20"))  //true

var dateTime = /\d{1,2}-\d{1,2}-\d{4} \d{1,2}:\d{2}/
console.log(dateTime.test("25-1-2020 9:20"))  //true
```

## 子表达式分组
对多个元素使用 `*`  或者 `+` ，需要使用圆括号将这个元素包围起来，创建一个分组。
```js
var cartoonCrying = /boo+(hoo+)+/i
console.log(cartoonCrying.test("Boohoooohoohooo"))  //true
```
第一个和第二个 `+` 分别作用于 boo 与 hoo 的 o 字符，第三个 + 作用于整个元组 hoo+ 。
末尾的 i 表示正则表达式不区分大小写。

## 匹配和分组
- exec（执行，execute）方法，无法匹配模式返回null，否则返回一个表示匹配字符串信息的对象
exec 方法返回的对象包含 index 属性，表示字符串成功匹配的起始位置
```js
var m = /\d+/.exec("one two 100")
console.log(m)        //["100"]
console.log(m.index)  //8
```
- 字符串有一个类似的 match 方法
```js
console.log("one two 100".match(/\d+/))  //["100"]
```

## 日期
[国际标准时间的表示方法](https://zh.wikipedia.org/zh-hans/ISO_8601)
[moment](https://momentjs.com/)
- JavaScript中使用从 0 开始的数字表示月份（因此用 Jan 表示 12 月），使用从 1 开始的数字表示日期。
构造函数的后四个参数（小时，分钟，秒，毫秒）是可选的，若没有指定，则参数为 0 。
```js
d = new Date()
// Fri Dec 25 2020 14:52:31 GMT+0800
d.toUTCString()  //格林威治时间

console.log(new Date(2020, 12, 25))
// Mon Jan 25 2021 00:00:00 GMT+0800
console.log(new Date(2020, 12, 25, 12, 59, 59, 999))
// Mon Jan 25 2021 12:59:59 GMT+0800
```

- 从 1970 年开始流逝的毫秒数表示时间戳，如在 1970 年前，则使用负数。Data 对象的 getTime 方法返回这种时间戳。
Data 对象提供了一些方法来提取时间中的某些数值，如 getYear、getMonth、getDate、getHours、getMinutes、getSeconds。还有getYear会返回使用两位数字表示的年份（如 99 或 20 ），但很少用到。
```js
console.log(new Date(2020, 12, 25).getTime())
// 1611504000000
console.log(new Date(1611504000000))
// Mon Jan 25 2021 00:00:00 GMT+0800
```
- 在希望捕获的那部分模式字符串两边加上圆括号，通过字符串创建对应的 Date 对象。
```js
function findDate(string){
  var dateTime = /(\d{1,2})-(\d{1,2})-(\d{4})/
  var match = dateTime.exec(string)
  return new Date(Number(match[3]), Number(match[2]), Number(match[1]))
}
console.log(findDate("12-25-2020"))
// Sat Feb 12 2022 00:00:00 GMT+0800
```

## 单词和字符串边界
[regular expression 101](https://regex101.com/)
- `^` 表示输入字符串起始的位置，`$` 表示字符串结束位置，因此`^\d+$`匹配完全由一个或多个数字组成的字符串。
`^!` 匹配任意以感叹号开头的字符串，`x^` 不匹配任何字符串（ 字符串之前不可能有字符x）
- 如果要确保日期字符串起始结束位置在单词边界上，可以使用 `\b` 标记。
单词边界，指的是起始和结束位置都是单词字符，而起始位置的前一个字符以及结束位置的后一个字符不是单词字符。
```js
console.log(/cat/.test("concatenate"))      //true
console.log(/\bcat\b/.test("concatenate"))  //false
```
- 零宽断言
零宽断言扩展了^ $ \b这几个匹配符
它匹配一个位置，可以要求某个位置满足或不满足特定条件。

分为四种情况：
1. positive look ahead assition
要求某个位置的右边满足某种条件（右边遇到某种模式）
(?=expr) 匹配一个位置，要求其右边要出现expr的匹配
2. negative look ahead assition
要求某个位置的右边不满足某种条件（右边不能遇到某种模式）
(?!expr)
3. positive look behand assition
要求某个位置的左边满足某种条件（右边遇到某种模式）
(?<=expr)
4. negative look behand assition
要求某个位置的左边不满足某种条件（右边不能遇到某种模式）
(?<!expr)

\b == (?=\w)(?<=\W)|(?=\W)(?<=\w)|^|$
^  == (?<![^])
$  == (?![^])

## 选项模式
管道符号 `|` 表示从其左侧的模式和右侧的模式任意选择一个进行匹配
```js
var animalCount = /\b\d+ (pig|cow|chicken)s?\b/
console.log(animalCount.test("15 pigs"))         //true
console.log(animalCount.test("15 pigchickens"))  //false
```