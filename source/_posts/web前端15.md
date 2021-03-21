---
title: web前端学习笔记15——Vue源码分析
subtitle: Vue源码分析
abbrlink: c12f9186
tags:
  - web前端
  - Vue
categories: web前端学习笔记
date: 2021-03-16 09:46:00
index_img:
banner_img:
---

# 说明
1. 分析 vue 作为一个 MVVM 框架的基本实现原理
数据代理，模板解析，数据绑定
2. 不直接看 vue.js 的源码
3. 剖析 github 上某个仿 vue 实现的 [mvvm 库](https://github.com/DMQ/mvvm)

# 准备知识
1. `[].slice.call(lis)`: 将伪数组转换为真数组(本质是一个对象)
2. `node.nodeType`: 得到节点类型
```html
<body>
  <div id="test">Test</div>
  <ul id="fragment_test">
    <li>test1</li>
    <li>test2</li>
    <li>test3</li>
  </ul>

  <script>
    // 1.[].slice.call(lis): 将伪数组转换为真数组
    const lis = document.getElementsByTagName('li') // lis是伪数组
    console.log(lis instanceof Array, lis[1].innerHTML, lis.forEach);
    // false "test2" undefined
    const lis2 = Array.prototype.slice.call(lis)
    console.log(lis2 instanceof Array, lis2[1].innerHTML, lis2.forEach);
    // true "test2" ƒ forEach() { [native code] }

    // 2. node.nodeType: 得到节点类型
    const elementNode = document.getElementById('test')
    const attrNode = elementNode.getAttributeNode('id')
    const textNode = elementNode.firstChild
    console.log(elementNode.nodeType, attrNode.nodeType, textNode.nodeType)
    // 1 2 3
  </script>
</body>  
```
3. `Object.defineProperty(obj, propertyName, {})`: 给对象添加属性(指定描述符)
可参考这里[Object](./468b9935.html#Object)
```js
const obj = {
  firstName: 'A',
  lastName: 'B'
}
// 给obj添加fullName属性
// obj.fullName = 'A-B'，赋值变化后不能改变
/* 
属性描述符：
1. 数据描述符
    configurable: 是否可以重新定义
    enumerable: 是否可以枚举
    value: 初始值
    writable: 是否可以修改属性值
2. 访问描述符
    get: 回调函数，根据其它相关的属性动态计算得到当前属性值
    set: 回调函数，监视当前属性值的变化，更新其它相关的属性值
*/
Object.defineProperty(obj, 'fullName', {
  get() {
    return this.firstName + '-' + this.lastName
  },
  set(value) {
    const names = value.split('-')
    this.firstName = names[0]
    this.lastName = names[1]
  }
})
console.log(obj.fullName); // A-B
obj.firstName = 'C'
obj.lastName = 'D'
console.log(obj.fullName); // C-D
obj.fullName = 'E-F'
console.log(obj.firstName, obj.lastName); // E F


Object.defineProperty(obj, 'fullName2', {
  configurable: false,
  enumerable: true,
  value: 'G-H',
  writable: false
})
console.log(obj.fullName2); // G-H
obj.fullName2 = 'J-k'
console.log(obj.fullName2); // G-H
/* Object.defineProperty(obj, 'fullName2', { // 不能重新定义
  configurable: false,
  enumerable: false,
  value: 'G-H',
  writable: true
}) */
```
4. `Object.keys(obj)`: 得到对象自身可枚举属性组成的数组
```js
const names = Object.keys(obj)
console.log(names); // ["firstName", "lastName", "fullName2"]
// 结果里没有fullName
// enumerable默认为false，不能枚举
```
5. `obj.hasOwnProperty(prop)`: 判断prop是否是obj自身的属性
```js
console.log(obj.hasOwnProperty('fullName'), obj.hasOwnProperty('.toString'));
// true false
```
6. `DocumentFragment`: 文档碎片(高效批量更新多个节点)
```js
// document: 对应显示的页面，包含n个element。一旦更新document内部的某个元素，界面更新。
// documentFragment: 内存中保存n个element的容器对象(不与界面关联)，如果更新fragment中的某个element，界面不变
/* <ul id="fragment_test">
      <li>test1</li>
      <li>test2</li>
      <li>test3</li>
    </ul> */

const ul = document.getElementById('fragment_test')
// 1. 创建fragment
const fragment = document.createDocumentFragment()

// 2. 取出ul中所有子节点保存到fragment
let child
while (child = ul.firstChild) { // 一个节点只能有一个父亲
  fragment.appendChild(child) // 先将child从ul中移除，添加为fragment子节点
}

// 3. 更新fragment中所有li的文本
Array.prototype.slice.call(fragment.childNodes).forEach(node => {
  if (node.nodeType == 1) { // 元素节点<li>
    node.textContent = 'modify'
  }
})

// 4. 将fragment插入ul
ul.appendChild(fragment)
```

# 1. 数据代理
1. vue数据代理: data对象的所有属性的操作(读/写)由vm对象来代理操作
2. 好处: 通过vm对象就可以方便的操作data中的数据
3. 实现:
    1. 通过Object.defineProperty(vm, key, {})给vm添加与data对象的属性对应的属性
    2. 所有添加的属性都包含get/set方法
    3. 在get/set方法中去操作data中对应的属性

```html
<body>
  <div id="test"></div>
  <script type="text/javascript" src="js/mvvm/compile.js"></script>
  <script type="text/javascript" src="js/mvvm/mvvm.js"></script>
  <script type="text/javascript" src="js/mvvm/observer.js"></script>
  <script type="text/javascript" src="js/mvvm/watcher.js"></script>
  <script type="text/javascript">
    const vm = new MVVM({
      el: "#test",
      data: {
        name: 'Amy'
      }
    })
    console.log(vm.name, vm)  // 读取的是data中的name, vm代理对data的读操作
    vm.name = 'Bob' // 数据保存到data中的name上, vm代理对data的写操作
    console.log(vm.name, vm._data.name)
  </script>
</body>
```
```js
// mnnm.js
/*
相关于Vue的构造函数
 */
function MVVM(options) {
  // 将选项对象保存到vm
  this.$options = options;
  // 将data对象保存到vm和data变量中
  var data = this._data = this.$options.data;
  //将vm保存在me变量中
  var me = this;

  // 遍历data中所有属性
  Object.keys(data).forEach(function (key) { // key是data的某个属性名(name)
    // 对指定属性实现代理
    me._proxy(key);
  });
}

MVVM.prototype = {
  // 对指定属性实现代理
  _proxy: function (key) {
    // 保存vm
    var me = this;
    // 给vm添加指定属性名的属性(使用属性描述)
    Object.defineProperty(me, key, {
      configurable: false, // 不能再重新定义
      enumerable: true, // 可以枚举遍历

      // 当通过vm.name读取属性值时自动调用
      // 读取data中对应属性值返回(实现代理读操作)
      get: function proxyGetter() {
        return me._data[key];
      },
      // 当通过vm.name = 'xxx'时自动调用
      // 将最新的值保存到data中对应的属性上(实现代理写操作)
      set: function proxySetter(newVal) {
        me._data[key] = newVal;
      }
    });
  }
};
```

# 2. 模板解析
```html
<head>
  <meta charset="UTF-8">
  <title>模板解析</title>
  <style>
    .aa {
      color: red
    }
    .bb{
      font-size: 30px
    }
  </style>
</head>

<body>
  <div id="test">
    <p>{{name}}</p><hr>
    <button v-on:click="show">测试</button><hr>
    <p v-text="msg"></p>
    <p v-html="msg"></p>
    <p class="bb" v-class="myClass">Hello</p>
  </div>

  <script type="text/javascript" src="js/mvvm/compile.js"></script>
  <script type="text/javascript" src="js/mvvm/mvvm.js"></script>
  <script type="text/javascript" src="js/mvvm/observer.js"></script>
  <script type="text/javascript" src="js/mvvm/watcher.js"></script>
  <script type="text/javascript">
    new MVVM({
      el: '#test',
      data: {
        name: 'Amy',
        msg: '<a href="https://cn.vuejs.org/" > VUE </a> ',
        myClass: 'aa'
      },
      methods: {
        show() {
          alert(this.msg)
        }
      }
    })
  </script>
</body>
```
<hr>

1. 将 el 的所有子节点取出, 添加到一个新建的文档 fragment 对象中
2. 对 fragment 中的所有层次子节点递归进行编译解析处理
    - 对大括号表达式文本节点进行解析
    - 对元素节点的指令属性进行解析
    - 事件指令解析
    - 一般指令解析
3. 将解析后的 fragment 添加到 el 中显示
<hr>
- 大括号表达式解析
    1. 根据正则对象得到匹配出的表达式字符串: 子匹配/RegExp.$1(name)
    2. 从 data 中取出表达式对应的属性值
    3. 将属性值设置为文本节点的 textContent
- 事件指令解析
    1. 从指令名中取出事件名
    2. 根据指令的值(表达式)从 methods 中得到对应的事件处理函数对象
    3. 给当前元素节点绑定指定事件名和回调函数的 dom 事件监听
    4. 指令解析完后, 移除此指令属性
- 一般指令解析
    1. 得到指令名和指令值(表达式)
        - text/html/class
        - msg/myClass
    2. 从 data 中根据表达式得到对应的值
    3. 根据指令名确定需要操作元素节点的什么属性
        - v-text---textContent 属性
        - v-html---innerHTML 属性
        - v-class--className 属性
    4. 将得到的表达式的值设置到对应的属性上
    5. 移除元素的指令属性

```js
// mvvm.js
function MVVM(options) {
  ...
  // 创建一个用来编译模板的compile对象
  this.$compile = new Compile(options.el || document.body, this)
}
// compile.js
function Compile(el, vm) {
  // 保存vm到compile对象
  this.$vm = vm;
  // 将el对应的元素对象保存到compile对象中
  this.$el = this.isElementNode(el) ? el : document.querySelector(el);

  // 如果el元素存在
  if (this.$el) {
    // 1. 取出el中所有子节点, 封装在一个fragment对象中
    this.$fragment = this.node2Fragment(this.$el);
    // 2. 编译fragment中所有层次子节点
    this.init();
    // 3. 将编译好的fragment添加到el中
    this.$el.appendChild(this.$fragment);
  }
}
```
```js
// compile.js
Compile.prototype = {
  node2Fragment: function (el) {
    var fragment = document.createDocumentFragment(),
      child;
    // 将el中所有子节点转移到fragment
    while (child = el.firstChild) {
      fragment.appendChild(child);
    }
    return fragment;
  },

  init: function () {
    // 编译指定元素(所有层次的子节点)
    this.compileElement(this.$fragment);
  },

  compileElement: function (el) {
    // 取出最外层的所有子节点
    var childNodes = el.childNodes,
      // 保存compile对象
      me = this;

    // 遍历所有子节点(text/element)
    [].slice.call(childNodes).forEach(function (node) {
      // 得到节点的文本内容
      var text = node.textContent;
      // 创建正则对象(匹配大括号表达式)
      var reg = /\{\{(.*)\}\}/;  // {{name}}

      // 如果是元素节点
      if (me.isElementNode(node)) {
        // 编译它(解析指令)
        me.compile(node);
        // 如果是一个大括号表达式格式的文本节点
      } else if (me.isTextNode(node) && reg.test(text)) {
        // 编译大括号表达式格式的文本节点
        me.compileText(node, RegExp.$1);
      }

      // 如果子节点还有子节点，递归调用实现所有层次节点的编译
      if (node.childNodes && node.childNodes.length) {
        me.compileElement(node);
      }
    });
  },

  compile: function (node) {
    // 得到标签的所有属性
    var nodeAttrs = node.attributes,
      me = this;
    // 遍历所有属性
    [].slice.call(nodeAttrs).forEach(function (attr) {
      // 得到属性名(v-on:click)
      var attrName = attr.name;
      // 判断是否是指令属性
      if (me.isDirective(attrName)) {
        // 得到表达式(属性值)(show)
        var exp = attr.value;
        // 得到指令名(on:click)
        var dir = attrName.substring(2);
        // 是否是事件指令
        if (me.isEventDirective(dir)) {
          // 解析事件指令
          compileUtil.eventHandler(node, me.$vm, exp, dir);
          // 普通指令
        } else {
           // 编译普通指令
          compileUtil[dir] && compileUtil[dir](node, me.$vm, exp);
        }
        // 移除指令属性
        node.removeAttribute(attrName);
      }
    });
  },

  compileText: function (node, exp) {
    // 调用编译工具对象解析
    compileUtil.text(node, this.$vm, exp);
  },

  isDirective: function (attr) {
    return attr.indexOf('v-') == 0;
  },

  isEventDirective: function (dir) {
    return dir.indexOf('on') === 0;
  },

  isElementNode: function (node) {
    return node.nodeType == 1;
  },

  isTextNode: function (node) {
    return node.nodeType == 3;
  }
};
```
```js
// compile.js
// 包含多个解析指令方法的对象
var compileUtil = {
  // 解析: v-text/{{}}
  text: function (node, vm, exp) {
    this.bind(node, vm, exp, 'text');
  },
  // 解析: v-html
  html: function (node, vm, exp) {
    this.bind(node, vm, exp, 'html');
  },
  // 解析: v-model
  model: function (node, vm, exp) {
    this.bind(node, vm, exp, 'model');

    var me = this,
      val = this._getVMVal(vm, exp);
    node.addEventListener('input', function (e) {
      var newValue = e.target.value;
      if (val === newValue) {
        return;
      }

      me._setVMVal(vm, exp, newValue);
      val = newValue;
    });
  },
  // 解析: v-class
  class: function (node, vm, exp) {
    this.bind(node, vm, exp, 'class');
  },

  // 真正用于解析指令的方法
  bind: function (node, vm, exp, dir) {
    // 根据指令名得到对应的更新节点函数
    var updaterFn = updater[dir + 'Updater'];
    // 如果存在调用来更新节点
    updaterFn && updaterFn(node, this._getVMVal(vm, exp));
  },

  // 事件处理
  eventHandler: function (node, vm, exp, dir) {
    // 得到事件名/类型(click)
    var eventType = dir.split(':')[1],
      // 从methods中得到表达式所对应的函数(事件回调函数)(show(){})
      fn = vm.$options.methods && vm.$options.methods[exp];
    // 如果都存在
    if (eventType && fn) {
      // 绑定指定事件名和回调函数的DOM事件监听, 将回调函数中的this强制绑定为vm
      node.addEventListener(eventType, fn.bind(vm), false);
    }
  },

  // 从vm得到表达式对应的value(表达式可能有多层)
  _getVMVal: function (vm, exp) {
    var val = vm._data;
    exp = exp.split('.');
    exp.forEach(function (k) {
      val = val[k];
    });
    return val;
  },

  _setVMVal: function (vm, exp, value) {
    var val = vm._data;
    exp = exp.split('.');
    exp.forEach(function (k, i) {
      // 非最后一个key，更新val的值
      if (i < exp.length - 1) {
        val = val[k];
      } else {
        val[k] = value;
      }
    });
  }
};

// 包含多个用于更新节点方法的对象
var updater = {
  // 更新节点的textContent
  textUpdater: function (node, value) {
    node.textContent = typeof value == 'undefined' ? '' : value;
  },

  // 更新节点的innerHTML
  htmlUpdater: function (node, value) {
    node.innerHTML = typeof value == 'undefined' ? '' : value;
  },

  // 更新节点的className
  classUpdater: function (node, value, oldValue) {
    // 静态class属性的值
    var className = node.className;
    // 将静态class属性的值与动态class值进行合并后设置为新的className属性值
    node.className = className + (className ? ' ' : '') + value;
  },

  // 更新节点的value
  modelUpdater: function (node, value, oldValue) {
    node.value = typeof value == 'undefined' ? '' : value;
  }
};
```

# 3. 数据绑定
- 数据绑定
一旦更新了data中的某个属性数据，所有界面上直接使用或间接使用了此属性的节点都会更新。
- 数据劫持
    1. 数据劫持是 vue 中用来实现数据绑定的一种技术
    2. 基本思想: 通过 defineProperty()来监视 data 中所有属性(任意层次)数据的变化, 一旦变化就去更新界面
- 四个重要对象
    1. Observer
        - 用来对 data 所有属性数据进行劫持的构造函数
        - 给 data 中所有属性重新定义属性描述(get/set)
        - 为 data 中的每个属性创建对应的 dep 对象
    2. Dep(Depend)
        - data 中的每个属性(所有层次)都对应一个 dep 对象
        - 创建的时机:
            * 在初始化 define data 中各个属性时创建对应的 dep 对象
            * 在 data 中的某个属性值被设置为新的对象时
        - 对象的结构
        ```js
        {
          id, // 每个 dep 都有一个唯一的 id
          subs // 包含 n 个对应 watcher 的数组(subscribes 的简写)
        }
        ```
        - subs 属性说明
            * 当 watcher 被创建时, 内部将当前 watcher 对象添加到对应的 dep 对象的 subs 中
            * 当此 data 属性的值发生改变时, subs 中所有的 watcher 都会收到更新的通知, 从而最终更新对应的界面
    3. Compiler
        - 用来解析模板页面的对象的构造函数(一个实例)
        - 利用 compile 对象解析模板页面
        - 每解析一个表达式(非事件指令)都会创建一个对应的 watcher 对象, 并建立  watcher 与 dep 的关系
        - complie 与 watcher 关系: 一对多的关系
    4. Watcher
        - 模板中每个非事件指令或表达式都对应一个 watcher 对象
        - 监视当前表达式数据的变化
        - 创建的时机: 在初始化编译模板时
        - 对象的组成
        ```js
        {
          vm, // vm 对象
          exp, // 对应指令的表达式
          cb, // 当表达式所对应的数据发生改变的回调函数
          value, // 表达式当前的值
          depIds // 表达式中各级属性所对应的 dep 对象的集合对象
                // 属性名为 dep 的 id, 属性值为 dep
        }
        ```
- 总结: dep 与 watcher 的关系: 多对多
    - data 中的一个属性对应一个 dep, 一个 dep 中可能包含多个 watcher(模板中有几个表达式使用到了同一个属性)
    - 模板中一个非事件表达式对应一个 watcher, 一个 watcher 中可能包含多个 dep(表达式是多层: a.b)
    - 数据绑定使用到 2 个核心技术
        * defineProperty()
        * 消息订阅与发布

```js
// mvvm.js
function MVVM(options) {
  // 对data中所有层析的属性通过数据劫持实现数据绑定
  observe(data, this);
}
```
```js
// compile.js
bind: function(node, vm, exp, dir){
  ...
  // 为表达式创建一个对应的watcher，实现节点的更新显示
  new Watcher(vm, exp, function (value, oldValue) { 
    // 当表达式对应的一个属性值变化时回调
    // 更新界面中的指定节点
    updaterFn && updaterFn(node, value, oldValue);
  });
}
```
```js
// watcher.js
function Watcher(vm, exp, cb) {
  this.cb = cb;  // 更新界面回调
  this.vm = vm;
  this.exp = exp; // 表达式
  this.depIds = {};  // 包含所有相关dep的容器对象
  this.value = this.get(); // 得到表达式初始值保存
}

Watcher.prototype = {
  update: function () {
    this.run();
  },
  run: function () {
    var value = this.get();
    var oldVal = this.value;
    if (value !== oldVal) {
      this.value = value;
      // 调用回调函数更新界面
      this.cb.call(this.vm, value, oldVal);
    }
  },
  addDep: function (dep) {
    // 判断dep与watcher关系是否已经建立
    if (!this.depIds.hasOwnProperty(dep.id)) {
      // 将watcher添加到dep，用于更新
      dep.addSub(this);
      // 将dep添加到watcher，用于防止重复建立关系
      this.depIds[dep.id] = dep;
    }
  },
  // 得到表达式对应的值，建立dep与watcher的关系
  get: function () {
    // 给dep指定当前的watch
    Dep.target = this;
    // 获取表达式的值, 内部调用get建立dep与watcher的关系
    var value = this.getVMVal();
    // 取出dep中指定的当前watcher
    Dep.target = null;
    return value;
  },
  // 得到表达式对应的值
  getVMVal: function () {
    var exp = this.exp.split('.');
    var val = this.vm._data;
    exp.forEach(function (k) {
      val = val[k];
    });
    return val;
  }
};
```
```js
// observer.js
function Observer(data) {
  // 保存data对象
  this.data = data;
  // 走起
  this.walk(data);
}

Observer.prototype = {
  walk: function (data) {
    // 保存observer对象
    var me = this;
    // 遍历data中所有属性
    Object.keys(data).forEach(function (key) {
      // 对指定的属性实现响应式的数据绑定
      me.defineReactive(me.data, key, data[key]);
    });
  },

  defineReactive: function (data, key, val) {
    // 创建属性对应的dep对象 dependency
    var dep = new Dep();
    // 间接递归调用实现对data中所有层次属性的劫持
    var childObj = observe(val);
    // 给data重新定义属性(添加set/get)
    Object.defineProperty(data, key, {
      enumerable: true, // 可枚举
      configurable: false, // 不能再define
      get: function () {
        // 建立dep与watcher的关系
        if (Dep.target) {
          dep.depend();
        }
        // 返回属性值
        return val;
      },
      set: function (newVal) { // 监视key属性的变化，更新界面
        if (newVal === val) {
          return;
        }
        val = newVal;
        // 新的值是object的话，进行监听
        childObj = observe(newVal);
        // 通知所有相关的订阅者
        dep.notify();
      }
    });
  }
};

function observe(value, vm) {
  // value必须是对象, 因为监视的是对象内部的属性
  if (!value || typeof value !== 'object') {
    return;
  }
  // 创建一个对应的观察都对象
  return new Observer(value);
};

var uid = 0;

function Dep() {
  // 标识属性
  this.id = uid++;
  // 相关的所有watcher的数组
  this.subs = [];
}

Dep.prototype = {
  // 添加watcher到dep中
  addSub: function (sub) {
    this.subs.push(sub);
  },
  // 去建立dep与watcher之间的关系
  depend: function () {
    Dep.target.addDep(this);
  },
  removeSub: function (sub) {
    var index = this.subs.indexOf(sub);
    if (index != -1) {
      this.subs.splice(index, 1);
    }
  },
  notify: function () {
    // 遍历所有watcher，通知watcher更新
    this.subs.forEach(function (sub) {
      sub.update();
    });
  }
};

Dep.target = null;
```


# MVVM原理
![MVVM 原理图](/image/post/MVVM原理图1.png)
![MVVM 原理图](/image/post/MVVM原理图2.png)
[MVVM 实现 源码](https://www.baidupcs.com/rest/2.0/pcs/file?method=batchdownload&app_id=250528&zipcontent=%7B%22fs_id%22%3A%5B1090386846218427%5D%7D&sign=DCb740ccc5511e5e8fedcff06b081203:qiyANTDUp3R5mBKma5x7lbyKs6o%3D&uid=4036931918&time=1616336215&dp-logid=8748101793799066579&dp-callid=0&vuk=4036931918&zipname=mvvm%20%E7%AD%891%E4%B8%AA%E6%96%87%E4%BB%B6.zip)

# 双向数据绑定
- 双向数据绑定是建立在单向数据绑定(model => View)的基础之上的
- 双向数据绑定的实现流程:
    1. 在解析 v-model 指令时, 给当前元素添加 input 监听
    2. 当 input 的 value 发生改变时, 将最新的值赋值给当前表达式所对应的 data 属性
```js
// compile.js
var compileUtil = {
  // 解析v-model
  model: function (node, vm, exp) {
    // 实现数据的初始化显示和创建对应watcher
    this.bind(node, vm, exp, 'model');

    var me = this,
      // 得到表达式的值
      val = this._getVMVal(vm, exp);
    // 给节点绑定input事件监听(输入改变时)
    node.addEventListener('input', function (e) {
      // 得到输入的最新值
      var newValue = e.target.value;
      // 如果没有变化，直接结束
      if (val === newValue) {
        return;
      }
      // 将最新value保存给表达式所对应的属性
      me._setVMVal(vm, exp, newValue);
      // 保存最新的值
      val = newValue;
    });
  }
}
```

# 函数实现
```js
//监控obj对象的变化，在变化后调用callback，暂时只考虑对象只有一层(watch)
//方法是通过把obj的每个属性都改成getter，setter，在setter调用时，调用callback
function observe(obj, callback) {
  for (let key in obj) {
    let val = obj[key]
    Object.defineProperty(obj, key, {
      get: function () {
        return val
      },
      set: function (newVal) {
        if (newVal === val) {
          return
        }
        val = newVal
        callback()
      }
    })
  }
}

var obj = { a: 1, b: 2 }
observe(obj, function () {
  console.log('obj changed')
})
```
```js
// 对象有多层
function observe(obj, callback) {
  if (obj && typeof obj == 'object') {
    for (let key in obj) {
      let val = obj[key]
      observe(val, callback)

      Object.defineProperty(obj, key, {
        get: function () {
          return val
        },
        set: function (newVal) {
          if (newVal === val) {
            return
          }
          val = observe(newVal, callback)
          callback()
        }
      })
    }
  }
  return obj
}
```
