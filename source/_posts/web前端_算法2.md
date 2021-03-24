---
title: 常见的算法题2
abbrlink: 44151e2d
tags:
  - front-end
  - JavaScript
categories: web前端学习笔记
date: 2020-12-23 15:59:00
index_img:
banner_img:
---
# 反转链表
```js
var reverseList = function(head) {
    let prev = null
    let curr = head
    while (curr) {
      let tmp = curr.next
      curr.next = prev
      prev = curr
      curr = tmp
    }
    return prev
};
```

# 反转位置 m 到 n 的链表
```js
var reverseBetween = function(head, m, n) {
  let curr = head
  let next = head
  let prev = null
  for (let i = 1; i < m; i++){
    prev = curr
    curr = curr.next
  }

  let prev2 = prev
  let curr2 = curr
  for(let i = m; i <= n; i++){
    next = curr.next
    curr.next = prev
    prev = curr
    curr = next
  }
  if(prev2 != null){
    prev2.next = prev
  }else{
    head = prev
  }
  curr2.next = curr
  return head
};
```

# 两两交换链表中的节点
```js
var swapPairs = function(head) {
  let dummy = new ListNode()
  dummy.next = head
  let p = dummy
  while(p.next && p.next.next){
    let n1 = p.next
    let n2 = p.next.next
    p.next = n2
    n1.next = n2.next
    n2.next = n1
    p = n1
  }
  return dummy.next
}
```

# 二叉树的所有节点
```js
var binaryTreePaths = function(root) {
  let res = []
  let createPath = (root, path) =>{
    if(root){
      if(!root.left && !root.right){
        path += root.val
        res.push(path)
      }else{
        path += root.val + '->'
        createPath(root.left, path)
        createPath(root.right, path)
      }
    }
  }
  createPath(root, '')
  return res 
};
```
```js
var binaryTreePaths = function (startNode, history = '', result = []) {
  if (startNode) {
    if (startNode.left == null && startNode.right == null) {
      result.push(history + startNode.val)
      return result
    }
    binaryTreePaths(startNode.left, history + startNode.val + '->', result)
    binaryTreePaths(startNode.right, history + startNode.val + '->', result)
  }
  return result
}
```

```js
// 可以停止的遍历函数，action返回false的时候遍历过程停止
function traverse(root, action) {
    if (root) {
        if (traverse(root.left, action) === false){
            return false
        }
        if (action(root) === false) {
            return false
        }
        if (traverse(root.right, action) === false) {
            return false
        }
    }
}
```

# 二叉树的层级遍历
```js
var levelOrder = function(root) {
  if(!root) return []
  let queue = [root]
  let res = []
  while(queue.length > 0){
    let arr = []
    let len = queue.length
    while(len--){
      let node = queue.shift()
      arr.push(node.val)
      if(node.left) queue.push(node.left)
      if(node.right) queue.push(node.right)
    }
    res.push(arr)
  }
  return res
};
```

# 构造一个向量函数
```js
function Vector(x, y) {
  this.x = x
  this.y = y
}

Vector.prototype.plus = function (v) {
  var x = this.x + v.x
  var y = this.y + v.y
  return new Vector(x, y)
}

Vector.prototype.minus = function (v) {
  var x = this.x - v.x
  var y = this.y - v.y
  return new Vector(x, y)
}

// length是一个属性
Object.defineProperty(Vector.prototype, 'length', {
  get: function () {
    var x = this.x
    var y = this.y
    return Math.sqrt(x * x + y * y)
  }
})
```
```js
Vector.prototype = {
  plus (v) {
    var x = this.x + v.x
    var y = this.y + v.y
    return new Vector(x, y)
  },
  minus (v) {
    var x = this.x - v.x
    var y = this.y - v.y
    return new Vector(x, y)
  },
  get length () {
    var x = this.x
    var y = this.y
    return Math.sqrt(x * x + y * y)
  }
}
```
```js
// 使属性不可枚举
function Vector(x, y) {
  this.x = x
  this.y = y
}
Object.defineProperties(Vector.prototype, {
  plus: {
    value: function (v) {
      var x = this.x + v.x
      var y = this.y + v.y
      return new Vector(x, y)
    }
  },
  minus: {
    value: function (v) {
      var x = this.x - v.x
      var y = this.y - v.y
      return new Vector(x, y)
    }
  },
  length: {
    get: function () {
      var x = this.x
      var y = this.y
      return Math.sqrt(x * x + y * y)
    }
  }
})
```

# 构造一个复数类
```js
function Complex(real, imag) {
  this.real = real
  this.imag = imag
}

// 静态方法
Complex.from = function (complexStr) {
  let real
  let imag
  if (complexStr.includes('+')) {
    let parts = complexStr.split('+')
    real = parseFloat(parts[0])
    imag = parseFloat(parts[1])
  } else {
    let parts = complexStr.split('-')
    if (parts.length == 2) {
      real = parseFloat(parts[0])
      imag = -parseFloat(parts[1])
    } else if (parts.length == 3) {
      real = -parseFloat(parts[1])
      imag = -parseFloat(parts[2])
    }
  }
  return new Complex(real, imag)
}

Complex.isEqual = function (a, b) {
  return a.real == b.real && a.imag == b.imag
}

// 实例方法
Complex.prototype.plus = function (c) {
  var real = this.real + c.real
  var imag = this.imag + c.imag
  return new Complex(real, imag)
}
Complex.prototype.minus = function (c) {
  var real = this.real - c.real
  var imag = this.imag - c.imag
  return new Complex(real, imag)
}

Complex.prototype.multiple = function (c) {
  var real = this.real * c.real - this.imag * c.imag
  var imag = this.real * c.imag + this.imag * c.real
  return new Complex(real, imag)
}
Complex.prototype.division = function (c) {
  var helper = new Complex(c.real, -c.imag)
  var m = helper.multiple(c)
  var z = helper.multiple(this)
  return new Complex(z.real / m.real, z.imag / m.real)
}

Complex.prototype.division2 = function (c) {
  var a = c.real
  var b = -c.imag
  var m = a * a + b * b
  var real = this.real * a - this.imag * b
  var imag = this.real * b + this.imag * a
  return new Complex(real / m, imag / m)
}

Complex.prototype.toString = function () {
  if (this.imag < 0) {
    return '(' + this.real + '' + this.imag + 'i)'
  }
  return '(' + this.real + '+' + this.imag + 'i)'
}
```

# 构造一个Set类
```js
// Set其实是一个特殊的Map，其中所有的元素都映射到自己
function MySet(initialValues) {
  this._elements = []  // 用下划线开头的属性表示这个属性是私有属性，不希望从外部访问

  for (var i = 0; i < initialValues.length; i++) {
    this.add(initialValues[i])
  }
}

MySet.prototype.add = function (val) {
  if (!this._elements.includes(val)) {
    this._elements.push(val)
  }
  return this
}
MySet.prototype.delete = function (val) {
  if (val !== val) {// val is NaN
    for (var i = 0; i < this._elements.length; i++) {
      if (this._elements[i] !== this._elements[i]) {
        this._elements.splice(i, 1)
        return true
      }
    }
    return false
  }
  var idx = this._elements.indexOf(val)
  if (idx >= 0) {
    this._elements.splice(idx, 1)
    return true
  }
  return false
}
MySet.prototype.has = function (val) {
  return this._elements.includes(val)
}
MySet.prototype.clear = function () {
  this._elements.length = 0
  // this._elements.splice(0, this._elements.length)
  // this._elements = []
}
Object.defineProperty(MySet.prototype, 'size', {
  get: function () {
    return this._elements.length
  }
})
```

# 构造一个Map类
```js
function MyMap(initialMaps = []) {
  this._keys = []
  this._vals = []

  for (var i = 0; i < initialMaps.length; i++) {
    var map = initialMaps[i]
    this.set(map[0], map[1])
  }
}

MyMap.prototype.forEach = function (iterator) {
  for (var i = 0; i < this.size; i++) {
    iterator(this._vals[i], this._keys[i], this)
  }
}
// 返回key在_keys中的下标，支持key为NaN
MyMap.prototype._keyIdx = function (key) {
  if (key !== key) {
    // 找出满足条件的元素的下标，找不到返回-1
    return this._keys.findIndex(it => it !== it)
  }
  return this._keys.indexOf(key)
  // return this._keys.indexOf(it => it === it)
}
MyMap.prototype.set = function (key, val) {
  var idx = this._keyIdx(key)
  if (idx >= 0) {
    this._vals[idx] = val
  } else {
    this._keys.push(key)
    this._vals.push(val)
  }
  return this
}
MyMap.prototype.get = function (key) {
  var idx = this._keyIdx(key)
  if (idx >= 0) {
    return this._vals[idx]
  } else {
    return undefined
  }
}
MyMap.prototype.has = function (key) {
  return this._keys.includes(key)
}
MyMap.prototype.delete = function (key) {
  var idx = this._keyIdx(key)
  if (idx >= 0) {
    this._keys.splice(idx, 1)
    this._vals.splice(idx, 1)
    return true
  } else {
    return false
  }
}
MyMap.prototype.clear = function () {
  this._keys.length = 0
  this._vals.length = 0
}
Object.defineProperty(MyMap.prototype, 'size', {
  get: function () {
    return this._keys.length
  }
})
```
