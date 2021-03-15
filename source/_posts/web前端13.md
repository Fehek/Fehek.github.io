---
title: web前端学习笔记13
abbrlink: 5f4b0425
tags:
  - web前端
  - Vue
categories: web前端学习笔记
date: 2021-03-14 17:51:00
index_img:
banner_img:
---

# 创建模板项目
[vue-cli](https://github.com/vuejs/vue-cli)：官方提供的脚手架工具
从 [vuejs-templates](https://github.com/vuejs-templates) 下载模板项目
- 创建 vue 项目
```bash
npm install -g vue-cli
vue init webpack vue_demo  #名称不能包含大写
# vue-router， unit tests 和 e2e tests 两个单元测试包暂时不需要，其余默认
cd vue_demo
npm run dev
# 访问: http://localhost:8080/
```
- 模板项目的结构
```md
|-- build : webpack 相关的配置文件夹(基本不需要修改)
  |-- dev-server.js : 通过 express 启动后台服务器
|-- config: webpack 相关的配置文件夹(基本不需要修改)
  |-- index.js: 指定的后台服务的端口号和静态资源文件夹
|-- node_modules
|-- src : 源码文件夹
  |-- components: vue 组件及其相关资源文件夹
  |-- App.vue: 应用根主组件
  |-- main.js: 应用入口 js
|-- static: 静态资源文件夹
|-- .babelrc: babel 的配置文件
|-- .eslintignore: eslint 检查忽略的配置
|-- .eslintrc.js: eslint 检查的配置
|-- .gitignore: git 版本管制忽略的配置
|-- index.html: 主页面文件
|-- package.json: 应用包配置文件
|-- README.md: 应用描述说明的 readme 文件
```

# 基本使用
- vue文件的组成
分为3部分：模板页面，JS 模块对象，样式
```vue
<template>
// 页面模板
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {},
  computed: {},
  components: {}
}
</script>

<style>
/* 样式定义 */
</style>
```

- 简单案例
```md
<!-- 结构目录 -->
|-- src
  |-- assets
    |-- logo.png
  |-- components
    |-- helloworld.vue
  |-- App.vue
  |-- main.js
|-- index.html
```
```vue
// helloworld.vue
<template>
  <div>
    <p class="msg">{{ msg }}</p>
  </div>
</template>

<script>
export default {  //配置对象(与Vue一致)
  data() {  //必须写函数
    return {
      msg: 'Hello Vue Component',
    }
  },
}
</script>

<style>
.msg {
  color: red;
  font-size: 30px;
}
</style>
```
```vue
// App.vue
<template>
  <div>
    <img class="logo" src="./assets/logo.png" alt="logo" />
    <!-- 3. 使用组件标签 -->
    <HelloWorld />
  </div>
</template>

<script>
// 1. 引入组件
import HelloWorld from './components/helloworld'
export default {
  // 2. 映射组件标签
  components: {
    HelloWorld,
  },
}
</script>

<style>
.logo {
  width: 200px;
  height: 200px;
}
</style>
```
```js
// main.js
// 入口JS：创建Vue实例
import Vue from 'vue'
import App from './App.vue'

new Vue({
  el: "#app", //index.html里写的名称
  components: { App }, //组件标签名
  template: '<App/>' // 模板插入到el所匹配的页面div里
})
```
```html
<body>
  <div id="app"></div>
  <!-- built files will be auto injected -->
</body>
```

# 项目打包与发布
- 静态服务器工具包
```bash
# 打包，会生成dist文件夹
npm run build
# 发布
npm install -g serve
serve dist
# 访问：http://localhost:5000
```
- 动态 web 服务器(tomcat)
    1. 修改配置
    ```js
    // webpack.prod.conf.js
    output: {
      publicPath: '/xxx/'
      //打包文件夹的名称
    }
    ```
    2. 重新打包: `npm run build`
    3. 修改 dist 文件夹为项目名称: xxx
    4. 将 xxx 拷贝到运行的 tomcat 的 webapps 目录下
    5. 访问: http://localhost:8080/xxx

# 组件间通信
## props
[todos源文件下载](https://www.baidupcs.com/rest/2.0/pcs/file?method=batchdownload&app_id=250528&zipcontent=%7B%22fs_id%22%3A%5B510549148196205%5D%7D&sign=DCb740ccc5511e5e8fedcff06b081203:YgER%2BTxcaEMzSWRWesdEFzOlH%2BI%3D&uid=4036931918&time=1615895027&dp-logid=559220838078483504&dp-callid=0&vuk=4036931918&zipname=src_todos%20%E7%AD%891%E4%B8%AA%E6%96%87%E4%BB%B6.zip)
1. 使用组件标签时
`<my-component name='tom' :age='3' :set-name='setName' />`
2. 定义 MyComponent 时
在组件内声明所有的props
    1. 只指定名称
    `props: ['name', 'age', 'setName']`
    2. 指定名称和类型
    ```vue
    props: {
      name: String,
      age: Number,
      setNmae: Function
    }
    ```
    3. 指定名称/类型/必要性/默认值
    ```vue
    props: {
      name: {type: String, required: true, default:xxx},
      age: {type: Number, required: true, default:xxx}
    }
    ```
- 注意
  - 此方式用于父组件向子组件传递数据
  - 所有标签属性都会成为组件对象的属性, 模板页面可以直接引用
  - 问题:
    - 如果需要向非子后代传递数据必须多层逐层传递
    - 兄弟组件间也不能直接 props 通信, 必须借助父组件才可以

## vue自定义事件
[todos 通信方式修改](https://www.baidupcs.com/rest/2.0/pcs/file?method=batchdownload&app_id=250528&zipcontent=%7B%22fs_id%22%3A%5B411051570951492%5D%7D&sign=DCb740ccc5511e5e8fedcff06b081203:wWKborYhac0WkseamHGH9jloe0s%3D&uid=4036931918&time=1615895014&dp-logid=559217391567055477&dp-callid=0&vuk=4036931918&zipname=src_todos_modify%20%E7%AD%891%E4%B8%AA%E6%96%87%E4%BB%B6.zip)
1. 绑定事件监听
    1. 通过 v-on 绑定
     ```html
     <!-- 给TodoHeader标签对象绑定addTodo事件监听 -->
     <TodoHeader @addTodo="addTodo" />
     ```
    2. 通过 $on() 
    ```vue
    <template>
      <TodoHeader ref="header" />
    </template>
    <script>
      this.$refs.header.$on('addTodo', this.addTodo)
    </script>
    ```
2. 触发事件
```js
// 触发事件(只能在父组件中接收)
this.$emit('addTodo', todo)
```
- 注意
  - 此方式只用于子组件向父组件发送消息(数据)
  - 问题: 隔代组件或兄弟组件间通信此种方式不合适

## 消息订阅与发布
- 安装 PubSubJS 库 `npm i --save pubsub-js`
绑定事件监听  ---订阅消息 
触发事件      ---发布消息
1. 订阅消息 `PubSub.subscribe('msg', function(msg, data){})`
2. 发布消息 `PubSub.publish('msg', data)`
- 注意
优点: 此方式可实现任意关系组件间通信(数据)
- 总结: 事件的 2 个重要操作
  1. 绑定事件监听 (订阅消息)
  目标: 标签元素 `<button>`
  事件名(类型): click/focus
  回调函数: `function(event){}`
  2. 触发事件 (发布消息)
  DOM 事件: 用户在浏览器上对应的界面上做对应的操作
  自定义: 编码手动触发

## slot
此方式用于父组件向子组件传递**标签**，而非数据。
1. 子组件 Child.vue
```html
<template>
  <div>
    <slot name="xxx">不确定的标签结构 1</slot>
    <div>组件确定的标签结构</div>
    <slot name="yyy">不确定的标签结构 2</slot>
  </div>
</template>
```
2. 父组件 Parent.vue
```html
<child>
  <div slot="xxx">xxx 对应的标签结构</div>
  <div slot="yyy">yyyy 对应的标签结构</div>
</child>
```

[todos 源文件最终版](https://www.baidupcs.com/rest/2.0/pcs/file?method=batchdownload&app_id=250528&zipcontent=%7B%22fs_id%22%3A%5B466857802587390%5D%7D&sign=DCb740ccc5511e5e8fedcff06b081203:dtyGJpquGTRv3QakfAPJeqdF%2BHE%3D&uid=4036931918&time=1615901719&dp-logid=561017168825414791&dp-callid=0&vuk=4036931918&zipname=src_todos_final%20%E7%AD%891%E4%B8%AA%E6%96%87%E4%BB%B6.zip)

# vue-ajax
## vue-resource
vue 插件, 非官方库, vue1.x 使用广泛。[文档](https://github.com/pagekit/vue-resource/blob/develop/docs/http.md)
下载 `npm install vue-resource --save`
```js
// 引入模块
import VueResource from 'vue-resource'
// 声明使用插件
// 内部会给vm对象和组件对象添加一个属性：$http
Vue.use(VueResourcejs)

// 通过 vue/组件对象发送 ajax 请求
this.$http.get('/someUrl').then((response) => {
  // success callback
  console.log(response.data) //返回结果数据
}, (response) => {
  // error callback
  console.log(response.statusText) //错误信息
})
```

## axios
通用的 ajax 请求库, 官方推荐, vue2.x 使用广泛。
下载 `npm install axios --save`
```js
// 引入模块
import axios from 'axios'
// 发送 ajax 请求
axios.get(url)
  .then(response => {
    console.log(response.data)
  })
  .catch(error => {
    console.log(error.message)
  })
```






# 网络攻击
XSS CSRF SQL Inject
