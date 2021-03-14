---
title: web前端学习笔记12
abbrlink: 284c34b3
tags:
  - web前端
  - Vue
categories: web前端学习笔记
date: 2021-03-12 22:04:00
index_img:
banner_img:
---

# 了解
- 介绍
渐进式 JavaScript 框架
作者：尤雨溪
作用：动态构建用户界面

- 特点
遵循 MVVM 模式
编码简洁，体积小，运行效率高，适合移动/PC端开发
它本身只关注UI，可以轻松引入 vue 插件或其它第三方库开发项目

- 与其它前端 JS 框架的关联
借鉴 angular 的**模板**和**数据绑定**技术
借鉴 react 的**组件化**和**虚拟DOM**技术

- 扩展插件
```md
vue-cli: vue 脚手架
vue-resource(axios): ajax 请求
vue-router: 路由
vuex: 状态管理
vue-lazyload: 图片懒加载
vue-scroller: 页面滑动相关
mint-ui: 基于vue的UI组件库(移动端)
element-ui: 基于vue的UI组件库(PC端)
```

# 基本使用
```html
<body>
  <!-- 1. 引入Vue.js
  2. 创建Vue对象
  el : 指定根element(选择器)
  data : 初始化数据(页面可以访问)
  3. 双向数据绑定 : v-model
  4. 显示数据 : {{xxx}}
  5. 理解vue的mvvm实现 -->

  <div id="app">
    <input type="text" v-model='username'>
    <p>hello {{username}}</p>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    const vm = new Vue({
      el: '#app',
      data: {
        username: 'world'
      }ccc
    })
  </script>
</body>
```
![Vue模型](/image/post/vue.png)
MVVM
model： 模型，数据对象(data)
view: 视图，模板页面
viewmodel: 视图模型(vue的实例)

# 模板语法
- 模板：动态的html页面，包含了一些JS语法代码。
大括号表达式，指令(以v-开头的自定义标签属性)
- 双大括号表达式
语法: `{{exp}}` 或 `{{{exp}}}`
功能: 向页面输出数据
可以调用对象的方法
- 指令一: 强制数据绑定
功能: 指定变化的属性值
完整写法: `v-bind:xxx='yyy'  //yyy会作为表达式解析执行`
简洁写法: `:xxx='yyy'`
- 指令二: 绑定事件监听
功能: 绑定指定事件名的回调函数
完整写法: `v-on:click='xxx'`
简洁写法: `@click='xxx'`

```html
<body>
  <div id="app">
    <h2>1. 双大括号表达式</h2>
    <p>{{content}}</p>
    <p>{{content.toUpperCase()}}</p>
    <p v-text='msg'></p> <!-- textContent -->
    <p v-html='msg'></p> <!-- innerHTML -->

    <h2>2. 指令一: 强制数据绑定</h2>
    <img src="imgUrl">
    <!-- 这种写法不对，可用下面两种写法 -->
    <img v-bind:src="imgUrl">
    <img :src="imgUrl">

    <h2>3. 指令二: 绑定事件监听</h2>
    <button v-on:click="test">点我</button>
    <button @click="test2('abc')">点我2</button>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    new Vue({
      el: '#app',
      data: {
        content: 'hello world',
        msg: '<a href="https://cn.vuejs.org/">Vue</a>',
        imgUrl: 'https://cn.vuejs.org/images/logo.png'
      },
      methods: {
        test() {
          alert('HELLO!')
        },
        test2(w) {
          alert(w)
        }
      }
    })
  </script>
</body>
```

# 计算属性和监视
- 计算属性
  在computed属性对象中定义计算属性的方法
  在页面中使用`{{方法名}}`来显示计算的结果
- 监视属性
  通过通过vm对象的`$watch()或`watch配置`来监视指定的属性
  当属性变化时, 回调函数自动调用, 在函数内部进行计算
- 计算属性高级
  通过getter/setter实现对属性数据的显示和监视
  计算属性存在缓存, 多次读取只执行一次getter计算
  getter: 属性的 get 方法
  setter: 属性的 set 方法

```html
<body>
  <div id="demo">
    姓: <input type="text" placeholder="First Name" v-model="firstName"><br>
    名: <input type="text" placeholder="Last Name" v-model="lastName"><br>
    姓名1(单向): <input type="text" placeholder="Full Name1" v-model="fullName1"><br>
    姓名2(单向): <input type="text" placeholder="Full Name2" v-model="fullName2"><br>
    姓名3(双向): <input type="text" placeholder="Full Name3" v-model="fullName3"><br>

    <!-- 只调用了一次fullName1()，用了缓存 -->
    <p>{{fullName1}}</p>
    <p>{{fullName1}}</p>
    <p>{{fullName1}}</p>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    const vm = new Vue({
      el: '#demo',
      data: {
        firstName: 'A',
        lastName: 'B',
        fullName2: 'A B'
      },
      computed: {
        // 什么时候执行: 初始化显示/相关的data属性数据发生改变
        fullName1() {
          console.log('fullName1()');
          return this.firstName + ' ' + this.lastName
        },
        fullName3: {
          // 回调函数，当需要读取当前属性时回调，根据相关的数据计算并返回当前属性的值
          get() {
            return this.firstName + ' ' + this.lastName
          },
          // 回调函数，监视当前属性值的变化，当属性值发生改变时回调，更新相关的属性值
          set(value) { //value就是fullName3的最新属性值
            const names = value.split(' ')
            this.firstName = names[0]
            this.lastName = names[1]
          }
        }
      },
      watch: { //配置监视，用对象
        firstName: function (value) {
          this.fullName2 = value + ' ' + this.lastName
        }
      }
    })
    vm.$watch('lastName', function (value) { //用实例方法
      this.fullName2 = this.firstName + ' ' + value
    })
  </script>
</body>
```

# class 与 style 绑定
- 在应用界面中, 某个(些)元素的样式是变化的
  class/style绑定就是专门用来实现动态样式效果的技术
- class绑定  `:class='xxx'`
  xxx可以是字符串，对象，数组
- style绑定  `:style="{ color: activeColor, fontSize: fontSize + 'px' }"`
  其中activeColor/fontSize是data属性

```html
<head>
  <meta charset="UTF-8">
  <title>class与style绑定</title>
  <style>
    .aClass {
      color: red;
    }
    .bClass {
      color: blue;
    }
    .cClass {
      font-size: 30px;
    }
  </style>
</head>

<body>
  <div id="demo">
    <h2>1. class绑定 :class='xxx'</h2>
    <p class="cClass" :class="a">xxx是字符串</p>
    <!-- 类名：boolean -->
    <p :class="{aClass:isA, bClass:isB}">xxx是对象</p>
    <p :class="['aClass','cClass']">xxx是数组</p>

    <h2>2. style绑定</h2>
    <p :style="{color: activeColor, fontSize: fs + 'px'}">style绑定</p>
    <button @click='update'>更新</button>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    new Vue({
      el: "#demo",
      data: {
        a: 'aClass',
        isA: true,
        isB: false,
        activeColor: 'red',
        fs: 20
      },
      methods: {
        update() {
          this.a = 'bClass'
          this.isA = false
          this.isB = true
          this.activeColor = 'green'
          this.fs = 30
        }
      }
    })
  </script>
</body>
```

# 条件渲染
`v-if  v-else  v-show`
```html
<body>
  <div id="demo">
    <!-- v-if, v-else做隐藏是通过标签移除
    v-show是通过样式隐藏，标签并未移除
    如果需要频繁切换 v-show 较好 -->
    <p v-if="ok">Success</p>
    <p v-else>Fail</p>

    <p v-show="ok">成功</p>
    <p v-show="!ok">失败</p>

    <button @click="ok=!ok">切换</button>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    new Vue({
      el: '#demo',
      data: {
        ok: false
      }
    })
  </script>
</body>
```

# 列表渲染
- 列表显示
  数组: v-for / index
  对象: v-for / key
- 列表的更新显示
  删除item，替换item

```html
<body>
  <div id="demo">
    <h2>测试：v-for 遍历数组</h2>
    <ul>
      <li v-for="(p, index) in persons" :key="index">
        {{index}}--{{p.name}}--{{p.age}}
        --<button @click="deleteP(index)">删除</button>
        --<button @click="updateP(index, {name:'Cat', age:10})">更新</button>
      </li>
    </ul>
    <button @click="addP({name:'Toby', age: 18})">添加</button>

    <h2>测试：v-for 遍历对象</h2>
    <li v-for="(value, key) in persons[1]" :key="key">
      {{value}}--{{key}}
    </li>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    // vue本身只是监视了persons的改变，没有监视数组内部数据的改变
    // vue重写了数组中的一系列改变数组内部数据的方法（先调用原生，更新界面）=> 数组内部改变，界面自动变化
    new Vue({
      el: '#demo',
      data: {
        persons: [
          { name: 'Jack', age: 20 },
          { name: 'Bob', age: 25 },
          { name: 'Tom', age: 23 },
          { name: 'Mary', age: 18 }
        ]
      },
      methods: {
        deleteP(index) {
          // 删除persons中指定index的p
          this.persons.splice(index, 1)
        },
        updateP(index, newP) {
          //并没有改变persons本身，数组内部发生了变化，但并没有调用变异方法，vue不会更新界面
          // this.persons[index] = newP
          this.persons.splice(index, 1, newP)
          // 增加一项
          // this.persons.splice(index, 0, newP)
        },
        addP(newP) {
          this.persons.push(newP)
        }
      }
    })
  </script>
</body>
```

- 过滤与排序

```html
<body>
  <div id="demo2">
    <input type="text" v-model="searchName">
    <ul>
      <li v-for="(p, index) in filterPersons" :key=" index">
        {{index}}--{{p.name}}--{{p.age}}
      </li>
    </ul>
    <button @click="setOrderType(1)">年龄升序</button>
    <button @click="setOrderType(2)">年龄降序</button>
    <button @click="setOrderType(0)">原本顺序</button>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    new Vue({
      el: "#demo2",
      data: {
        searchName: '',
        orderType: 0, //0表示原本顺序
        persons: [
          { name: 'Jack', age: 20 },
          { name: 'Bob', age: 25 },
          { name: 'Tom', age: 23 },
          { name: 'Mary', age: 18 }
        ]
      },
      computed: {
        filterPersons() {
          // 取出相关的数据
          const { searchName, persons, orderType } = this
          // 最终需要显示的数组
          let fPersons
          // 对persons进行过滤
          fPersons = persons.filter(p => p.name.indexOf(searchName) !== -1)

          // 排序
          if (orderType !== 0) {
            fPersons.sort(function (p1, p2) { 
              // sort() 返回负数p1在前，返回正数p2在前
              // 1表示升序，2表示降序
              if (orderType === 2) {
                return p2.age - p1.age
              }
              if (orderType === 1) {
                return p1.age - p2.age
              }
            })
          }
          return fPersons
        }
      },
      methods: {
        setOrderType(orderType) {
          this.orderType = orderType
        }
      }
    })
  </script>
</body>
```

# 事件处理
- 绑定监听
  v-on:xxx="fun"
  @xxx="fun"
  @xxx="fun(参数)"
  默认事件形参: event
  隐含属性对象: $event
- 事件修饰符
  .prevent : 阻止事件的默认行为 event.preventDefault()
  .stop : 停止事件冒泡 event.stopPropagation()
- 按键修饰符
  .keycode : 操作的是某个keycode值的健
  .enter : 操作的是enter键

```html
<body>
  <div id="demo">
    <h2>1. 绑定监听</h2>
    <button @click="test1">test1</button>
    <button @click="test2('hello world')">test2</button>
    <button @click="test3">test3</button>
    <button @click="test4(123, $event)">test4</button>

    <h2>2. 时间修饰符</h2>
    <div style="width: 200px; height: 200px; background: red" @click="test5">
      <div style="width: 100px; height: 100px; background: blue" @click.stop="test6"></div>
    </div>
    <a href="https://cn.vuejs.org/" @click.prevent="test7">Vue官网</a>

    <h2>3. 案件修饰符</h2>
    <input type="text" @keyup.13="test8">
    <input type="text" @keyup.enter="test8">
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    new Vue({
      el: "#demo",
      data: {
        test1() {
          alert('test1')
        },
        test2(msg) {
          alert(msg)
        },
        test3(event) {
          alert(event.target.innerHTML)
        },
        test4(number, event) {
          alert(number + '--' + event.target.innerHTML)
        },
        test5() {
          alert('out')
        },
        test6() {
          // 停止事件冒泡
          // event.stopPropagation()
          alert('inner')
        },
        test7() {
          // 阻止事件的默认行为
          // event.preventDefault()
          alert('点击了')
        },
        test8() {
          // if (event.keyCode === 13)
          alert(event.target.value)
        }
      }
    })
  </script>
</body>
```

# 表单输入绑定
使用v-model(双向数据绑定)自动收集数据
`text/textarea  checkbox  radio  select`

```html
<body>
  <div id="demo">
    <form action="/xxx" @submit.prevent="handleSubmit">
      <span>用户名：</span>
      <input type="text" v-model="username"><br>

      <span>密码：</span>
      <input type="password" v-model="pwd"><br>

      <span>性别：</span>
      <input type="radio" id="female" value="女" v-model="gender">
      <label for="female">女</label>
      <input type="radio" id="male" value="男" v-model="gender">
      <label for="male">男</label><br>

      <span>爱好：</span>
      <input type="checkbox" id="basketball" value="bk" v-model="hobbit">
      <label for="basketball">篮球</label>
      <input type="checkbox" id="football" value="ft" v-model="hobbit">
      <label for="football">足球</label>
      <input type="checkbox" id="pingpang" value="pp" v-model="hobbit">
      <label for="pingpang">乒乓</label><br>

      <span>城市：</span>
      <select v-model="cityId">
        <option value="">未选择</option>
        <option :value="city.id" v-for="(city, index) in allCitys" :key="index">{{city.name}}</option>
      </select><br>
      <span>介绍：</span>
      <textarea rows="10" v-model="description"></textarea><br><br>

      <input type="submit" value="注册">
    </form>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    new Vue({
      el: "#demo",
      data: {
        username: '',
        pwd: '',
        gender: '女',
        hobbit: ['bk', 'ft'],
        allCitys: [{ id: 1, name: '北京' }, { id: 2, name: '上海' }, { id: 3, name: '深圳' }],
        cityId: '3',
        description: ''
      },
      methods: {
        handleSubmit() {
          console.log(this.username, this.pwd, this.gender, this.hobbit, this.cityId, this.description);
        }
      }
    })
  </script>
</body>
```

# Vue实例的生命周期
![Vue生命周期](/image/post/Vue生命周期.png)
- vue对象的生命周期
  1. 初始化显示
      * beforeCreate()
      * created()
      * beforeMount()
      * mounted()
  2. 更新显示 this.xxx = value
      * beforeUpdate()
      * updated()
  3. 销毁vue实例: vm.$destroy()
      * beforeDestroy()
      * destroyed()
- 常用的生命周期方法
  mounted(): 发送ajax请求, 启动定时器等异步任务
  beforeDestroy(): 做收尾工作, 如: 清除定时器

```html
<body>
  <div id="demo">
    <button @click="destroyVM">destroy vm</button>
    <p v-show="isShow">测试</p>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    new Vue({
      el: "#demo",
      data: {
        isShow: true
      },

      // 1. 初始化阶段
      beforeCreate() {
        console.log('beforeCreate()')
      },
      created() {
        console.log('created()')
      },
      beforeMount() {
        console.log('beforeMount()')
      },
      mounted() { //初始化显示之后立即调用(1次)
        console.log('Mounted()')
        this.intervalId = setInterval(() => {
          console.log('------');
          this.isShow = !this.isShow //更新数据
        }, 1000)
      },

      // 2. 更新阶段
      beforeUpdate() {
        console.log('beforeUpdate()')
      },
      updated() {
        console.log('updated()')
      },

      // 3. 死亡阶段
      beforeDestroy() { //死亡之前调用(1次)
        console.log('beforeDestroy()')
        // 清除定时器
        clearInterval(this.intervalId)
      },
      destroyed() {
        console.log('destroyed()')
      },

      methods: {
        destroyVM() {
          this.$destroy()
        }
      }
    })
  </script>
</body>
```

# 过渡与动画
1. vue动画的理解
  操作css的trasition或animation
  vue会给目标元素添加/移除特定的class
2. 基本过渡动画的编码
    1. 在目标元素外包裹`<transition name="xxx">`
    2. 定义class样式
        - 指定过渡样式: transition
        - 指定隐藏时的样式: opacity/其它
3. 过渡的类名
  xxx-enter-active: 指定显示的transition
  xxx-leave-active: 指定隐藏的transition
  xxx-enter: 指定隐藏时的样式
![Vue过渡](/image/post/Vue过渡.png)

```html
<head>
  <meta charset="UTF-8">
  <title>Vue 过渡效果</title>
  <style>
    /* 显示/隐藏的过渡效果 */
    .xxx-enter-active, .xxx-leave-active {
      transition: opacity 1s;
    }

    /* 隐藏时的样式 */
    .xxx-enter, .xxx-leave-to {
      opacity: 0;
    }

    /* 显示的过渡效果 */
    .move-enter-active {
      transition: all 1s;
    }

    /* 隐藏的过渡效果 */
    .move-leave-active {
      transition: all 3s;
    }

    /* 隐藏时的样式 */
    .move-enter, .move-leave-to {
      opacity: 0;
      transform: translateX(20px);
    }
  </style>
</head>

<body>
  <div id="demo">
    <button @click="isShow = !isShow">toggle</button>
    <transition name="xxx">
      <p v-show="isShow">hello</p>
    </transition>
  </div>
  <div id="demo2">
    <button @click="isShow = !isShow">toggle</button>
    <transition name="move">
      <p v-show="isShow">hello</p>
    </transition>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    new Vue({
      el: "#demo",
      data() {
        return {
          isShow: true
        }
      }
    })
    new Vue({
      el: "#demo2",
      data() {
        return {
          isShow: true
        }
      }
    })
  </script>
</body>
```
```html
<head>
  <meta charset="UTF-8">
  <title>Vue 动画</title>
  <style>
    .bounce-enter-active {
      animation: bounce-in .5s;
    }
    .bounce-leave-active {
      animation: bounce-in .5s reverse;
    }

    @keyframes bounce-in {
      0% {
        transform: scale(0);
      }
      50% {
        transform: scale(1.5);
      }
      100% {
        transform: scale(1);
      }
    }
  </style>
</head>

<body>
  <div id="example">
    <button @click="show = !show">Toggle show</button>
    <br>
    <transition name="bounce">
      <p v-if="show" style="display: inline-block">Lorem</p>
    </transition>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script>
    new Vue({
      el: '#example',
      data: {
        show: true
      }
    })
  </script>
</body>
```

# 过滤器
1. 理解过滤器
  功能: 对要显示的数据进行特定格式化后再显示
  注意: 并没有改变原本的数据, 可是产生新的对应的数据
2. 编码
    1. 定义过滤器
    ```js
    Vue.filter(filterName, function(value[,arg1,arg2,...]{
      // 进行一定的数据处理
      return newValue
    })
    ```
    2. 使用过滤器
    ```html
    <div>{{myData | filterName}}</div>
    <div>{{myData | filterName(arg)}}</div>
    ```

```html
<body>
  <div id="demo">
    <h2>显示格式化的日期时间</h2>
    <p>{{date}}</p>
    <p>完整版：{{date | dateString}}</p>
    <p>年月日：{{date | dateString('YYYY-MM-DD')}}</p>
    <p>时分秒：{{date | dateString('HH:mm:ss')}}</p>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.29.1/moment.js"></script>
  <script type="text/javascript">
    // 自定义过滤
    Vue.filter('dateString', function (value, format = 'YYYY-MM-DD HH:mm:ss') {
      return moment(value).format(format)
    })
    new Vue({
      el: "#demo",
      data: {
        date: new Date()
      }
    })
  </script>
</body>
```

# 指令
## 内置指令
常用内置指令|含义
:-|:-
v-text : |更新元素的 textContent
v-html : |更新元素的 innerHTML
v-if : |如果为 true, 当前标签才会输出到页面
v-else: |如果为 false, 当前标签才会输出到页面
v-show : |通过控制 display 样式来控制显示/隐藏
v-for : |遍历数组/对象
v-on : |绑定事件监听, 一般简写为@
v-bind : |强制绑定解析表达式, 可以省略 v-bind, 简写为一个冒号
v-model : |双向数据绑定
ref : |指定唯一标识, vue 对象通过$els 属性访问这个元素对象
v-cloak : |防止闪现, 与 css 配合: [v-cloak] { display: none }

```html
<head>
  <meta charset="UTF-8">
  <title>ref 和 v-cloak 指令</title>
  <style>
    [v-cloak] {
      display: none;
    }
  </style>
</head>

<body>
  <div id="demo">
    <p ref="content">test</p>
    <button @click="hint">提示</button>
    <!-- 解析之前存在，解析之后没有 -->
    <p v-cloak>{{msg}}</p>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    alert('----')
    new Vue({
      el: "#demo",
      data: {
        msg: '测试'
      },
      methods: {
        hint() {
          alert(this.$refs.content.textContent)
        }
      }
    })
  </script>
</body>
```

## 自定义指令
```js
// 1. 注册全局指令
Vue.directive('my-directive', function (el, binding) {
  el.innerHTML = binding.value.toupperCase()
})

// 2. 注册局部指令
directives: {
  'lower-text'(el, binding) {
    el.textContent = binding.value.toLowerCase()
  }
}

// 3. 使用指令
v-my-directive='xxx'
```

```html
<!-- 需求: 自定义2个指令
  1. 功能类型于v-text, 但转换为全大写
  2. 功能类型于v-text, 但转换为全小写 -->
<body>
  <div id="demo1">
    <p v-upper-text="msg1"></p>
    <p v-lower-text="msg1"></p>
  </div>
  <div id="demo2">
    <p v-upper-text="msg2"></p>
    <p v-lower-text="msg2"></p>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript">
    // 定义全局指令
    // el: 指令属性所在的标签对象
    // binding: 包含指令相关信息数据的对象
    Vue.directive('upper-text', function (el, binding) {
      console.log(el, binding)
      el.textContent = binding.value.toUpperCase()
    })
    new Vue({
      el: "#demo1",
      data: {
        msg1: 'Hello World'
      },
      directives: { //注册局部指令: 只在当前vm管理范围内有效
        'lower-text'(el, binding) {
          el.textContent = binding.value.toLowerCase()
        }
      }
    })
    new Vue({
      el: "#demo2",
      data: {
        msg2: 'I Got It!'
      }
    })
  </script>
</body>
```

# 插件
```js
// vue-mine.js
// vue的插件库
(function () {
  // 需要向外暴露的插件对象
  const MyPlugin = {}

  // 插件对象必须有一个install()
  MyPlugin.install = function (Vue, options) {
    // 1. 添加全局方法或属性
    Vue.myGlobalMethod = function () {
      console.log('Vue函数对象的方法myGlobalMethod()')
    }

    // 2. 添加全局资源
    Vue.directive('my-directive', function (el, binding) {
      el.textContent = binding.value.toUpperCase()
    })

    // 3. 添加实例方法
    Vue.prototype.$myMethod = function () {
      console.log('Vue实例对象的方法$myMethod()')
    }
  }

  // 向外暴露
  window.MyPlugin = MyPlugin
})()
```

```html
<body>
  <div id="demo">
    <p v-my-directive="msg"></p>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script type="text/javascript" src="./vue-mine.js"></script>
  <script type="text/javascript">
    // 声明使用插件
    Vue.use(MyPlugin) //内部会执行MuPlugin.install(Vue)
    Vue.myGlobalMethod()

    const vm = new Vue({
      el: "#demo",
      data: {
        msg: 'Hello World'
      }
    })
    vm.$myMethod()
  </script>
</body>
```
