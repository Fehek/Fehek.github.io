---
title: web前端学习笔记17——Vue3
subtitle: Vue3
abbrlink: 4d268486
tags:
  - front-end
  - Vue
categories: web前端学习笔记
date: 2021-04-06 22:11:00
index_img:
banner_img:
---
# 基本了解
## 认识Vue3
1. 相关信息
    - Vue3支持vue2的大多数特性
    - 更好的支持Typescript
2. 性能提升
    - 打包大小减少，初次渲染和更新渲染变快，内存减少54%
    - **使用Proxy代替defineProperty实现数据响应式**
    - **重写虚拟DOM的实现和Tree-Shaking**
3. 新增特性
    - **Composition (组合) API**
    - setup
        - ref 和 reactive
        - computed 和 watch
        - 新的生命周期函数
        - provide与inject
        - ...
    - 新组件
        - Fragment - 文档碎片
        - Teleport - 瞬移组件的位置
        - Suspense - 异步加载组件的loading界面
    - 其它API更新
        - 全局API的修改
        - 将原来的全局API转移到应用对象
        - 模板语法变化

## 创建
1. 使用vue-cli
```bash
## 安装或者升级
npm install -g @vue/cli
## 保证 vue cli 版本在 4.5.0 以上
vue --version
## 创建项目
vue create my-project
```
接下来的步骤：
Please pick a preset - 选择 Manually select features
Check the features needed for your project - 选择上 TypeScript ，按空格是选择，回车是下一步
Choose a version of Vue.js that you want to start the project with - 选择 3.x (Preview)
之后全部回车
2. 使用vite
    - Vite 是一个 web 开发构建工具，由于其原生 ES 模块导入方式，可以实现闪电般的冷服务器启动。
    - 它做到了**本地快速开发启动**, 在生产环境下基于 Rollup 打包。
        - 快速的冷启动，不需要等待打包操作；
        - 即时的热模块更新，替换性能和模块数量的解耦让更新飞起；
        - 真正的按需编译，不再等待整个应用编译完成，这是一个巨大的改变。
    - 使用 npm
    ```bash
    npm init @vitejs/app <project-name>
    cd <project-name>
    npm install
    npm run dev
    ```

# Composition API
## setup
新的option, 所有的组合API函数都在此使用, 只在初始化时执行一次
函数如果返回对象, 对象中的属性或方法, 模板中可以直接使用

## ref
- 作用: 定义一个数据的响应式
- 语法: const xxx = ref(initValue):
    - 创建一个包含响应式数据的引用(reference)对象
    - js中操作数据: xxx.value
    - 模板中操作数据: 不需要.value
- 一般用来定义一个基本类型的响应式数据
```vue
<template>
  <h2>{{ count }}</h2>
  <hr />
  <button @click="update">更新</button>
</template>

<script>
import { ref } from 'vue'
export default {
  /* 在Vue3中依然可以使用data和methods配置, 但建议使用其新语法实现
  data () {
    return {
      count: 0
    }
  },
  methods: {
    update () {
      this.count++
    }
  } */

  // 使用vue3的composition API
  setup() {
    // 定义响应式数据ref对象
    const count = ref(1)
    console.log(count)

    // 更新响应式数据的函数
    function update() {
      // alert('update')
      count.value = count.value + 1
    }

    return {
      count,
      update
    }
  }
}
</script>
```

## reactive
- 作用: 定义多个数据的响应式
- const proxy = reactive(obj): 接收一个普通对象然后返回该普通对象的响应式代理器对象
- 响应式转换是“深层的”：会影响对象内部所有嵌套的属性
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据都是响应式的 
```vue
<template>
  <h2>name:{{ state.name }}</h2>
  <h2>age:{{ state.age }}</h2>
  <h2>study:{{ state.study }}</h2>
  <hr />
  <button @click="update">更新</button>
</template>

<script>
import { reactive } from 'vue'
export default {
  setup () {
    const state = reactive({
      name: 'Tom',
      age: 18,
      study: {
        level: '+',
        time: 1
      }
    })
    console.log(state, state.study)

    const update = () => {
      state.name += '--'
      state.age += 1
      state.study.level += '++'
      state.study.time += 2
    }
    return { state, update }
  }
}
</script>
```

## Vue2与Vue3的响应式
### Vue2的响应式
- 核心:
    - 对象: 通过defineProperty对对象的已有属性值的读取和修改进行劫持(监视/拦截)
    - 数组: 通过重写数组更新数组一系列更新元素的方法来实现元素修改的劫持
```js
Object.defineProperty(data, 'count', {
    get () {}, 
    set () {}
})
```
- 问题
    - 对象直接新添加的属性或删除已有属性, 界面不会自动更新
    - 直接通过下标替换元素或更新length, 界面不会自动更新 arr[1] = {}

### Vue3的响应式
- 核心:
    - 通过 Proxy(代理): 拦截对data任意属性的任意操作, 包括属性值的读写, 属性的添加, 属性的删除等...
    - 通过 Reflect(反射): 动态对被代理对象的相应属性进行特定的操作
    - 文档:
    https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy
    https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

```html
<body>
  <script type="text/javascript">
    // 目标对象
    const user = {
      name: '小明',
      age: 28,
      wife: {
        name: '小红',
        age: 25
      }
    }
    // 把目标对象变成代理对象
    // 参数1：user=>target目标对象
    // 参数2：handler=>处理器对象，用来监视数据及数据的操作
    const proxyUser = new Proxy(user, {
      // 获取目标对象的某个属性
      get(target, prop) {
        console.log('get方法调用了')
        return Reflect.get(target, prop)
      },
      // 修改目标对象的属性值/为目标对象添加新的属性
      set(target, prop, val) {
        console.log('set方法调用了')
        return Reflect.set(target, prop, val)
      },
      // 删除目标对象上的某个属性
      deleteProperty(target, prop) {
        console.log('delete方法调用了')
        return Reflect.set(target, prop)
      }
    })

    // 通过代理对象获取目标对象中的某个属性值
    console.log(proxyUser.name)
    // 通过代理对象更新目标对象上的某个属性值
    proxyUser.name = '小王'
    console.log(user)
    // 通过代理对象向目标对象中添加一个新的属性
    proxyUser.gender = '男'
    console.log(user)
    // 删除目标对象上的某个属性
    delete proxyUser.name
    console.log(user)
    // 更新目标对象中的某个属性对象中的属性值
    proxyUser.wife.name = '小花'
    console.log(user)
  </script>
</body>
```

## 细节
### setup
- 执行的时机
    - 在beforeCreate之前执行(一次), 此时组件对象还没有创建
    - this是undefined, 不能通过this来访问data/computed/methods/props
    - 其实所有的composition API相关回调函数中也都不可以
- 返回值
    - 一般都返回一个对象: 为模板提供数据, 也就是模板中可以直接使用此对象中的所有属性/方法
    - 返回对象中的属性会与data函数返回对象的属性合并成为组件对象的属性，返回对象中的方法会与methods中的方法合并成功组件对象的方法。如果有重名, setup优先。
    - 一般不要混合使用: methods中可以访问setup提供的属性和方法, 但在setup方法中不能访问data和methods
    - setup不能是一个async函数: 因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性数据
- 参数 `setup(props, context) / setup(props, {attrs, slots, emit})`
    - props: 包含props配置声明且传入了的所有属性的对象
    - context: 是一个对象，里面有attrs对象，slots对象，emit方法
    - attrs: 包含没有在props配置中声明的属性的对象, 相当于 this.$attrs
    - slots: 包含所有传入的插槽内容的对象, 相当于 this.$slots
    - emit: 用来分发自定义事件的函数, 相当于 this.$emit

### ref与reactive
- 是Vue3的 composition API中2个最重要的响应式API
- ref用来处理基本类型数据, reactive用来处理对象(递归深度响应式)
- 如果用ref对象/数组, 内部会自动将对象/数组转换为reactive的代理对象
- ref内部: 通过给value属性添加getter/setter来实现对数据的劫持
- reactive内部: 通过使用Proxy来实现对对象内部所有数据的劫持, 并通过Reflect操作对象内部数据
- ref的数据操作: 在js中要.value, 在模板中不需要(内部解析模板时会自动添加.value)
```vue
<template>
  <h3>m1:{{m1}}</h3>
  <h3>m2:{{m2}}</h3>
  <h3>m3:{{m3}}</h3>
  <hr />
  <button @click="update">update</button>
</template>

<script lang="ts">
import { defineComponent, ref, reactive } from 'vue'
export default defineComponent({
  name: 'App',
  setup() {
    const m1 = ref('abc')
    const m2 = reactive({
      name: 'Tom',
      wife: { name: 'Jenny' }
    })
    const m3 = ref({
      name: 'Tom',
      wife: { name: 'Jenny' }
    })
    const update = () => {
      // ref中如果放入的是一个对象，那么是经过了reactive的处理，形成了一个Proxy类型的对象
      console.log(m3)
      m1.value += '='
      m2.wife.name += '='
      m3.value.name += '='
      m3.value.wife.name += '='
      console.log(m3.value.wife)
    }
    return { m1, m2, m3, update }
  }
})
</script>
```

## 计算属性与监视
```vue
<template>
  <fieldset>
    <legend>姓名操作</legend>
    姓氏：<input type="text" placeholder="请输入姓氏" v-model="user.firstName" /><br />
    名字：<input type="text" placeholder="请输入名字" v-model="user.lastName" /><br />
  </fieldset>
  <fieldset>
    <legend>计算属性和监视的演示</legend>
    姓名：<input type="text" placeholder="显示姓名" v-model="fullName1" /><br />
    姓名：<input type="text" placeholder="显示姓名" v-model="fullName2" /><br />
    姓名：<input type="text" placeholder="显示姓名" v-model="fullName3" /><br />
  </fieldset>
</template>

<script lang="ts">
import { computed, defineComponent, reactive, ref, watch, watchEffect } from 'vue'
export default defineComponent({
  name: 'App',
  // 定义一个响应式对象
  setup() {
    const user = reactive({
      firstName: '李',
      lastName: '小明'
    })
    // 计算属性的函数中如果只传入一个回调函数，表示的是get
    // 返回的是一个Ref类型的对象
    const fullName1 = computed(() => {
      return user.firstName + '_' + user.lastName
    })

    const fullName2 = computed({
      get() {
        return user.firstName + '_' + user.lastName
      },
      set(val: string) {
        const names = val.split('_')
        user.firstName = names[0]
        user.lastName = names[1]
      }
    })

    const fullName3 = ref('')
    watch(user,({ firstName, lastName }) => {
        fullName3.value = firstName + '_' + lastName
      }, { immediate: true, deep: true }
      // immediate 默认会执行一次watch; deep 深度监视
    )

    // 监视，不需要配置immediate，本身默认就会执行一次
    // watchEffect(() => {
    //   fullName3.value = user.firstName + '_' + user.lastName
    // })

    // 当使用watch监视非相应式数据时，需要使用回调函数的形式
    watch([() => user.firstName, () => user.lastName, fullName3], () => {
      console.log('===')
    })

    return { user, fullName1, fullName2, fullName3 }
  }
})
</script>
```

## 生命周期
![vue3生命周期](https://v3.cn.vuejs.org/images/lifecycle.svg)
![vue2生命周期](https://cn.vuejs.org/images/lifecycle.png)
- 与 2.x 版本生命周期相对应的组合式 API
    - ~~beforeCreate~~ -> 使用 setup()
    - ~~created~~ -> 使用 setup()
    - beforeMount -> onBeforeMount
    - mounted -> onMounted
    - beforeUpdate -> onBeforeUpdate
    - updated -> onUpdated
    - beforeDestroy -> onBeforeUnmount
    - destroyed -> onUnmounted
    - errorCaptured -> onErrorCaptured

## 自定义hook函数
- 使用Vue3的组合API封装的可复用的功能函数
- 自定义hook的作用类似于vue2中的mixin技术
- 自定义Hook的优势: 很清楚复用功能代码的来源, 更清楚易懂

```
<!-- 目录 -->
|-- src
  |-- App.vue
  |-- hooks
    |-- useMousePosition.ts
```
```ts
// 需求：用户在页面中点击页面，把点击的位置的横坐标收集起来并展示出来
// useMousePosition.ts
import { onBeforeUnmount, onMounted, ref } from 'vue'
export default function () {
  const x = ref(-1)
  const y = ref(-1)
  // 点击事件的回调函数
  const clickHandler = (event: MouseEvent) => {
    x.value = event.pageX
    y.value = event.pageY
  }
  // 页面已经加载完毕了，再进行点击操作
  // 页面加载完毕的生命周期组合API
  onMounted(() => {
    window.addEventListener('click', clickHandler)
  })
  onBeforeUnmount(() => {
    window.removeEventListener('click', clickHandler)
  })
  return { x, y }
}
```
```vue
// App.vue
<template>
  <h2>自定义hook函数操作</h2>
  <h2>x:{{x}}, y:{{y}}</h2>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import useMousePosition from './hooks/useMousePosition'
export default defineComponent({
  name: 'App',
  setup() {
    const { x, y } = useMousePosition()
    return { x, y }
  }
})
</script>
```

## toRefs
把一个响应式对象转换成普通对象，该普通对象的每个 property 都是一个 ref
应用: 当从合成函数返回响应式对象时，toRefs 非常有用，这样消费组件就可以在不丢失响应式的情况下对返回的对象进行分解使用

问题: reactive 对象取出的所有属性值都是非响应式的
解决: 利用 toRefs 可以将一个响应式 reactive 对象的所有原始属性转换为响应式的 ref 属性
```vue
<template>
  <h2>toRefs使用</h2>
  <h3>foo: {{foo}}</h3>
  <h3>bar: {{bar}}</h3>
</template>

<script lang="ts">
import { reactive, toRefs } from 'vue'
export default {
  setup() {
    const state = reactive({
      foo: 'a',
      bar: 'b'
    })
    const stateAsRefs = toRefs(state)
    setInterval(() => {
      state.foo += '++'
      state.bar += '++'
    }, 1000)

    return {
      // ...state,
      ...stateAsRefs
    }
  }
}
</script>
```
```vue
<template>
  <h2>toRefs使用</h2>
  <h3>foo2: {{foo2}}</h3>
  <h3>bar2: {{bar2}}</h3>
</template>

<script lang="ts">
import { reactive, toRefs } from 'vue'
export default {
  setup() {
    return { foo2, bar2 }
  }
}
const { foo2, bar2 } = useRef()
function useRef() {
  const state = reactive({
    foo2: 'a',
    bar2: 'b'
  })
  setInterval(() => {
    state.foo2 += '++'
    state.bar2 += '++'
  }, 1000)
  return toRefs(state)
}
</script>
```

## ref获取元素
利用ref函数获取组件中的标签元素
```vue
// 功能需求: 让输入框自动获取焦点
<template>
  <h2>ref获取焦点</h2>
  <input type="text">---
  <input type="text" ref="inputRef">
</template>

<script lang="ts">
import { onMounted, ref } from 'vue'
export default {
  setup() {
    // 默认是空的，页面加载完毕，说明组件已经存在了，获取文本框元素
    const inputRef = ref<HTMLElement | null>(null)
    // 页面加载后的生命周期组合API
    onMounted(() => {
      inputRef.value && inputRef.value.focus()
    })
    return { inputRef }
  }
}
</script>
```

# Composition API 其它部分
## shallowReactive 与 shallowRef
- shallowReactive : 只处理了对象内最外层属性的响应式(也就是浅响应式)
- shallowRef: 只处理了value的响应式, 不进行对象的reactive处理
- 什么时候用浅响应式呢?
    - 一般情况下使用ref和reactive即可
    - 如果有一个对象数据, 结构比较深, 但变化时只是外层属性变化 => shallowReactive
    - 如果有一个对象数据, 后面会产生新的对象来替换 => shallowRef

## readonly 与 shallowReadonly
- readonly:
    - 深度只读数据。
    - 获取一个对象 (响应式或纯对象) 或 ref 并返回原始代理的只读代理。
    - 只读代理是深层的：访问的任何嵌套 property 也是只读的。
- shallowReadonly
    - 浅只读数据。
    - 创建一个代理，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换。
- 应用场景:
    - 在某些特定情况下, 可能不希望对数据进行更新的操作, 那就可以包装生成一个只读代理对象来读取数据, 而不能修改或删除。

## toRaw 与 markRaw
- toRaw
    - 返回由 reactive 或 readonly 方法转换成响应式代理的普通对象。
    - 这是一个还原方法，可用于临时读取，访问不会被代理/跟踪，写入时也不会触发界面更新。
- markRaw
    - 标记一个对象，使其永远不会转换为代理。返回对象本身。
- 应用场景:
    - 有些值不应被设置为响应式的，例如复杂的第三方类实例或 Vue 组件对象。
    - 当渲染具有不可变数据源的大列表时，跳过代理转换可以提高性能。

## toRef
- 为源响应式对象上的某个属性创建一个 ref对象, 二者内部操作的是同一个数据值, 更新时二者是同步的
- 区别ref: 拷贝了一份新的数据值单独操作, 更新时相互不影响
- 应用: 当要将 某个prop 的 ref 传递给复合函数时，toRef 很有用`const a = fn(toRef(props, 'foo'))`
```js
const state = reactive({
  foo: 1,
  bar: 2
})
const foo = toRef(state, 'foo')
const foo2 = ref(state.foo)
```

## customRef
创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制
```vue
// 需求: 使用 customRef 实现 debounce 的示例
<template>
  <h2>debounce示例</h2>
  <input v-model="keyword" placeholder="搜索关键字" />
  <p>{{keyword}}</p>
</template>

<script lang="ts">
import { ref, customRef } from 'vue'
export default {
  setup() {
    const keyword = useDebouncedRef('', 500)
    console.log(keyword)
    return { keyword }
  }
}

// 实现函数防抖的自定义ref
function useDebouncedRef<T>(value: T, delay = 200) {
  let timeout: number
  return customRef((track, trigger) => {
    return {
      get() {
        // 告诉Vue追踪数据
        track()
        return value
      },
      set(newValue: T) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          value = newValue
          // 告诉Vue去触发界面更新
          trigger()
        }, delay)
      }
    }
  })
}
</script>
```

## provide 与 inject
provide和inject提供依赖注入，功能类似 2.x 的provide/inject
可以不经过父级组件，直接向孙子组件传输数据
```vue
// App.vue
<template>
  <h2>provide 与 inject</h2>
  <p>当前颜色：{{color}}</p>
  <button @click="color='red'">red</button>
  <button @click="color='yellow'">yellow</button>
  <button @click="color='green'">green</button>
  <hr />
  <Son />
</template>

<script lang="ts">
import { defineComponent, ref, provide } from 'vue'
import Son from './components/Son.vue'
export default defineComponent({
  name: 'App',
  components: { Son },
  setup() {
    const color = ref('red')
    // 提供数据
    provide('color', color)
    return { color }
  }
})
</script>
```
```vue
components/Son.vue
<template>
  <h3>Son子级组件</h3>
  <hr />
  <GrandSon />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import GrandSon from './GrandSon.vue'
export default defineComponent({
  name: 'Son',
  components: { GrandSon }
})
</script>
```
```vue
components/GrandSon.vue
<template>
  <h3 :style="{color}">GrandSon孙子组件</h3>
</template>

<script lang="ts">
import { defineComponent, inject } from 'vue'
export default defineComponent({
  name: 'GrandSon',
  setup() {
    const color = inject('color')
    return { color }
  }
})
</script>
```

## 响应式数据的判断
- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 reactive 创建的响应式代理
- isReadonly: 检查一个对象是否是由 readonly 创建的只读代理
- isProxy: 检查一个对象是否是由 reactive 或者 readonly 方法创建的代理

# 手写组合API
## shallowReactive 与 reactive
```js
// 定义一个reactiveHandler处理对象
const reactiveHandler = {
  // 获取属性值
  get (target, prop) {
    const result = Reflect.get(target, prop)
    console.log('拦截了读取数据', prop, result)
    return result
  },
  // 修改属性值或是添加属性
  set (target, prop, value) {
    const result = Reflect.set(target, prop, value)
    console.log('拦截了修改数据或者是添加属性', prop, value)
    return result
  },
  // 删除某个属性
  deleteProperty (target, prop) {
    const result = Reflect.deleteProperty(target, prop)
    console.log('拦截了删除数据', prop)
    return result
  }
}

// 定义一个shallowReactive函数，传入一个目标对象
function shallowReactive (target) {
  // 判断当前的目标对象是不是object类型（对象/数组）
  if (target && typeof target === 'object') {
    return new Proxy(target, reactiveHandler)
  }
  // 如果传入的数据是基本类型的数据，那么就直接返回
  return target
}

// 定义一个reactive函数，传入一个目标对象function shallowReactive (target) {
function reactive (target) {
  // 判断当前的目标对象是不是object类型（对象/数组）
  if (target && typeof target === 'object') {
    // 对数组或者是对象中多有数据进行reactive的递归处理
    // 先判断当前的数据是不是数组
    if (Array.isArray(target)) {
      // 数组的数据要进行遍历操作
      target.forEach((item, index) => {
        target[index] = reactive(item)
      })
    } else {
      // 再判断当前的数据是不是对象
      // 对象的数据也要进行遍历操作
      Object.keys(target).forEach(key => {
        target[key] = reactive(target[key])
      })
    }
    return new Proxy(target, reactiveHandler)
  }
  // 如果传入的数据是基本类型的数据，那么就直接返回
  return target
}
```
```html
<body>
  <script src="index.js"></script>
  <script type="text/javascript">
    const proxyUser1 = shallowReactive({
      name: '小明',
      car: {
        color: 'red'
      }
    })
    // proxyUser1.name = '小红' // 拦截到了写的数据
    // proxyUser1.name += '==' // 拦截到了读和写的数据
    // proxyUser1.car.color += '==' // 拦截到了读数据，但是拦截不到写的数据
    // delete proxyUser1.name // 拦截到了删除数据
    // delete proxyUser1.car.color // 只拦截到了读，但是拦截不到删除

    const proxyUser2 = reactive({
      name: '小明',
      car: {
        color: 'red'
      }
    })
    // proxyUser2.name += '==' // 拦截到了读和修改的数据
    // proxyUser2.car.color += '==' // 拦截到了到了读和修改的数据
    // delete proxyUser2.name // 拦截到了删除
    // delete proxyUser2.car.color // 拦截到了读和删除
  </script>
</body>
```

## shallowReadonly 与 readonly
```js
// 定义了一个readonlyHandler处理器
const readonlyHandler = {
  get (target, prop) {
    const result = Reflect.get(target, prop)
    console.log('拦截到了读取数据', prop, result)
    return result
  },
  set (target, prop, value) {
    console.warn('只能读取数据，不能修改数据或添加数据')
    return true
  },
  deleteProperty (target, prop) {
    console.warn('只能读取数据，不能删除数据')
    return true
  }
}

// 定义一个shallowReadonly函数
function shallowReadonly (target) {
  // 需要判断当前的数据是不是对象
  if (target && typeof target === 'object') {
    return new Proxy(target, readonlyHandler)
  }
  return target
}

// 定义一个readonly函数
function readonly (target) {
  // 需要判断当前的数据是不是对象
  if (target && typeof target === 'object') {
    // 判断target是不是数组
    if (Array.isArray(target)) {
      target.forEach((item, index) => {
        // 遍历数组
        target[index] = readonly(item)
      })
    } else { // 判断target是不是对象
      // 遍历对象
      Object.keys(target).forEach(key => {
        target[key] = readonly(target[key])
      })
    }
    return new Proxy(target, readonlyHandler)
  }
  // 如果不是对象或数组，那么直接返回
  return target
}
```
```html
<script type="text/javascript">
  const proxyUser3 = shallowReadonly({
    name: '小明',
    cars: ['奔驰', '宝马']
  })
  // console.log(proxyUser3.name) // 可以读取
  // proxyUser3.name = '==' // 不能修改
  // delete proxyUser3.name // 不能删除
  // proxyUser3.cars[0] = '奥迪' // 拦截到了读取，可以修改
  // delete proxyUser3.cars[0] // 拦截到了读取，可以删除

  const proxyUser4 = readonly({
    name: '小明',
    cars: ['奔驰', '宝马']
  })
  // console.log(proxyUser4.name) // 可以读取
  // proxyUser4.name = '小红' // 只读的
  // proxyUser4.cars[0] = '奥迪' // 只读的
  // delete proxyUser4.name // 只读的
  // delete proxyUser4.cars[0] // 只读的
</script>
```

## shallowRef 与 ref
```js
// 定义一个shallowRef
function shallowRef (target) {
  return {
    // 保存target数据
    _value: target,
    get value () {
      console.log('劫持到了读取数据')
      return this._value
    },
    set value (val) {
      console.log('劫持到了修改数据，准备更新界面', val)
      this._value = val
    }
  }
}

// 定义一个ref函数
function ref (target) {
  target = reactive(target)
  return {
    // 保存target数据
    _value: target,
    get value () {
      console.log('劫持到了读取数据')
      return this._value
    },
    set value (val) {
      console.log('劫持到了修改数据，准备更新界面', val)
      this._value = val
    }
  }
}
```
```html
<script>
  const ref1 = shallowRef({
    name: '小明',
    cars: {
      color: 'red'
    }
  })
  // console.log(ref1.value)
  // ref1.value = '==' // 劫持到
  // ref1.value.car = '==' // 劫持不到

  const ref2 = ref({
    name: '小明',
    cars: {
      color: 'red'
    }
  })
  // console.log(ref2.value)
  // ref2.value = '==' // 劫持到
  // ref2.value.car = '==' // 劫持到
</script>
```

## isRef, isReactive 与 isReadonly
```js
function ref (target) {
  return { 
    _is_ref: true, // 标识当前的对象是ref对象
  }
}
// 定义一个函数isRef，判断当前对象是不是ref对象
function isRef (obj) {
  return obj && obj._is_ref
}

const reactiveHandler = {
  get (target, prop) {
    if (prop === '_is_reactive') return true
  }
}
// 定义一个函数isReactive，判断当前对象是不是reactive对象
function isReactive (obj) {
  return obj && obj._is_reactive
}

const readonlyHandler = {
  get (target, prop) {
    if (prop === '_is_readonly') return true
  }
}
// 定义一个函数isReadonly，判断当前对象是不是readonly对象
function isReadonly (obj) {
  return obj && obj._is_readonly
}

// 定义一个函数isProxy，判断当前对象是不是reactive对象或者readonly对象
function isProxy (obj) {
  return isReactive(obj) || isReadonly(obj)
}
```
```html
<script>
  console.log(isRef(ref({}))) // true
  console.log(isReactive(reactive({}))) // true
  console.log(isReadonly(readonly({}))) // true
  console.log(isProxy(reactive({}))) // true
  console.log(isProxy(readonly({}))) // true
</script>
```
[手写API 下载](https://www.baidupcs.com/rest/2.0/pcs/file?method=batchdownload&app_id=250528&zipcontent=%7B%22fs_id%22%3A%5B333493587833468%5D%7D&sign=DCb740ccc5511e5e8fedcff06b081203:l5mTTQ3zQs2Wa%2BHY6ZpvSc4t4jg%3D&uid=4036931918&time=1618573715&dp-logid=125354098947303274&dp-callid=0&vuk=4036931918&zipname=%E6%89%8B%E5%86%99API%20%E7%AD%891%E4%B8%AA%E6%96%87%E4%BB%B6.zip)

# 新组件和API
## 新组件
### Fragment(片断)
- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用
```vue
<template>
  <h2>aaaa</h2>
  <h2>bbbb</h2>
</template>
```

###  Teleport(瞬移)
- Teleport 提供了一种干净的方法, 让组件的html在父组件界面外的特定标签(很可能是body)下插入显示
```vue
<template>
  <teleport to="body">
    ...
  </teleport>
</template>
```

### Suspense(不确定的)
- 它们允许我们的应用程序在等待异步组件时渲染一些后备内容，可以让我们创建一个平滑的用户体验
```vue
<template>
  <Suspense>
    <template v-slot:default>
      <AsyncComp />
    </template>

    <template v-slot:fallback>
      <h1>LOADING...</h1>
    </template>
  </Suspense>
</template>

<script>
/* 
异步组件 + Suspense组件
*/
</script>
```

## 新API
### 全新的全局API
- createApp()
- defineProperty()
- defineAsyncComponent()
- nextTick()

### 将原来的全局API转移到应用对象
- app.component()
- app.config()
- app.directive()
- app.mount()
- app.unmount()
- app.use()

### 模板语法变化
- v-model的本质变化
    - prop：value -> modelValue；
    - event：input -> update:modelValue；
- .sync修改符已移除, 由v-model代替
- v-if优先v-for解析

# 综合使用




# Vue3.0和2.0的区别
- 支持Teleport，类似React.createPortal
- 支持组合式API，类似React Hooks，但原理完全不同
    - VCA的setup函数只运行一次，没有闭包陷阱。VCA是通过返回可监控对象来实现的更新，所以VCA没有useCallback, useMemo之类的函数
    - React Hooks，组件实例每次更新都会运行一次组件对应的函数，每次运行对应的函数相应的useState等hook函数都会返回新状态值
- 对数据响应式监控使用的不是gettter，setter，而是代理对象Proxy
- 性能更高：通过只更新数据变化了的部分的DOM来提升性能
    - 原理是能够监控到数据在DOM中的哪位位置用过，数据变的时候只更新对应位置的DOM
    - 在特定情况下不做整个组件树的diff
