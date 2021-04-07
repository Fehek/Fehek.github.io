---
title: web前端学习笔记16——TypeScript
subtitle: TypeScript
abbrlink: ae93be73
tags:
  - front-end
  - TypeScript
categories: web前端学习笔记
date: 2021-04-01 21:31:00
index_img:
banner_img:
---

# 特点
- 始于JavaScript，归于JavaScript
TypeScript 可以编译出纯净、 简洁的 JavaScript 代码，并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或更高版本）的JavaScript 引擎中。
- 强大的类型系统
类型系统允许 JavaScript 开发者在开发 JavaScript 应用程序时使用高效的开发工具和常用操作比如静态检查和代码重构。
- 先进的 JavaScript
TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件。

# 安装配置
```bash
# 安装
npm install -g typescript
# 检查是否安装成功
tsc -v
```
- ts的文件中如果直接书写js语法的代码，那么在html文件中直接引入ts文件，在谷歌浏览器中是可以直接使用的
如果ts文件中有了ts的语法代码，那么就需要把这个ts文件编译成js文件，在html文件中引入js的文件来使用 `tsc ./xxx.ts`
ts文件中的函数中的形参，如果使用了某个类型进行修饰，那么最终在编译的js文件中是没有这个类型的
ts文件中的变量使用的是let进行修饰，编译的js文件中的修饰符救变成了var
- vscode自动编译
```md
1. 生成配置文件tsconfig.json
    tsc --init
2. 修改tsconfig.json配置
    "outDir": "./js",  // 输出默认文件夹
    "strict": false,   // 严格模式   
3. 启动监视任务
    终端 -> 运行任务 -> 监视tsconfig.json
```

# 使用
## 类型注解
TypeScript 里的类型注解是一种轻量级的为函数或变量添加约束的方式
```typescript
function greeter (person: string) {
  return 'Hello, ' + person
}
let user = 'World'
console.log(greeter(user))
```
## 接口
```ts
interface Person {
  firstName: string
  lastName: string
}
function greeter (person: Person) {
  return 'Hello, ' + person.firstName + ' ' + person.lastName
}
let user = {
  firstName: 'F',
  lastName: 'K'
}
console.log(greeter(user))
```
## 类
```ts
// 定义一个类型
class User {
  // 定义公共的字段（属性）
  fullName: string
  firstName: string
  lastName: string
  // 定义一个构造器函数
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName
    this.lastName = lastName
    this.fullName = firstName + ' ' + lastName
  }
}

// 定义一个接口
interface Person {
  firstName: string
  lastName: string
}

// 定义一个函数
function greeter(person: Person) {
  return 'Hello, ' + person.firstName + ' ' + person.lastName
}

// 实例化对象
const user = new User('F', 'K')
console.log(greeter(user))
```
TypeScript 里的类只是一个语法糖，本质上还是 JavaScript 函数的实现。

# 基础类型
## 布尔值 boolean
```ts
let flag: boolean = false
flag = false
flag = 10 // 报错
```

## 数字 number
```ts
let a1: number = 10 // 十进制
let a2: number = 0b1010  // 二进制
let a3: number = 0o12 // 八进制
let a4: number = 0xa // 十六进制
a1 = 'hello' // 报错
```

## 字符串 string
```ts
let name:string = 'Jack'
name = 10 // 报错
```
**总结：ts中变量一开始是什么类型，那么后期赋值的时候，只能用这个类型的数据，是不允许用其他类型的数据赋值给当前的这个变量中**

## undefined 和 null
默认情况下 null 和 undefined 是所有类型的子类型。 就是说可以把 null 和 undefined 赋值给 number 类型的变量。（关闭严格模式）
```ts
let u: undefined = undefined
let n: null = null
let num: number = undefined
let num2: number = null
```

## 数组
数组定义后，里面的数据类型必须和定义数组的时候的类型是一致的。
```ts
// 方式1
// let 变量名: 数据类型[] = [值1, 值2, 值3]
let arr1: number[] = [10, 20, 30, 40]
// 方式2：泛型
// let 变量名: Array<数据类型> = [值1, 值2, 值3]
let arr2: Array<number> = [100, 200, 300]
```

## 元组
在定义数组的时候，类型和数据的个数一开始就已经限定了。
元组类型在使用时，数据类型的位置和数据的个数应该和在定义时是一致的。
```ts
let arr3: [string, number, boolean] = ['hello', 100.12345, true]
console.log(arr3[0].split(''))
console.log(arr3[1].toFixed(2))
```

## 枚举
枚举里面的每个数据值都可以叫元素，每个元素都有自己的编号，默认编号从0开始，依次递增1
```ts
enum Color{
  red, green, blue
}
let color: Color = Color.red
console.log(color) // 0
console.log(Color.red, Color.green, Color.blue) // 0 1 2
```
或者可以手动赋值
```ts
enum Color {red = 1, green, blue}
let c: Color = Color.green
console.log(c) // 2
```
枚举类型提供的一个便利是可以由枚举的值得到它的名字
```ts
enum Color {red = 1, green, blue}
let colorName: string = Color[2]
console.log(colorName)  // 'green'
```

## any
```ts
let notSure: any = 1
notSure = 'string'
notSure = false

let arr: any[] = [100, 'hello', true]
console.log(arr[0].split('')) // 编译时不会报错，但结果不能执行
```

## void
某种程度上来说，void 类型像是与 any 类型相反，它表示没有任何类型。当一个函数没有返回值时，通常会见到其返回值类型是 void。
```ts
// 表示没有任何类型, 一般用来说明函数的返回值不能是undefined和null之外的值 
function fn(): void {
  console.log('fn()')
  // return undefined
  // return null
  // return 1 // error
}

// 声明一个 void 类型的变量没什么用，因为只能赋值 undefined 和 null
let unusable: void = undefined
```

## object
object 表示非原始类型，也就是除 number，string，boolean之外的类型。
```ts
// 定义一个函数，参数是object类型，返回值也是object类型
function fn2(obj: object): object {
  console.log(obj)
  return {}
}
console.log(fn2({ name: 'abc', age: 1 }));
// console.log(fn2('abc') // error
console.log(fn2(String))
console.log(fn2(new String('abc')))
```
## 联合类型
表示取值可以为多种类型中的一种
```ts
// 定义一个函数得到一个数字或字符串值的字符串形式值
function getString(str: number | string): string {
  return str.toString()
}
console.log(getString('123'))
console.log(getString(123))
```

## 类型断言
类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript 会假设你已经进行了必须的检查。
- 类型断言方式1：<类型>变量名
- 类型断言方式2：值 as 类型
```ts
// 定义一个函数得到一个数字或字符串值的长度
function getString(str: number | string): number {
  // return str.toString().length
  // 如果str本身是string类型，是没有必要调用toString()方法

  if ((<string>str).length) {
    return (str as string).length
  } else {
    return str.toString().length
  }
}
console.log(getString('12345'))
console.log(getString(123))
```

## 类型推断
TS会在没有明确的指定类型的时候推测出一个类型
有下面2种情况: 1. 定义变量时赋值了, 推断为对应的类型. 2. 定义变量时没有赋值, 推断为any类型
```ts
// 定义变量时赋值了, 推断为对应的类型
let a = 123 // number
// a = 'abc' // error

// 定义变量时没有赋值, 推断为any类型
let b // any类型
b = 123
b = 'abc'
```

# 接口
TypeScript 的核心原则之一是对值所具有的结构进行类型检查。我们使用接口（Interfaces）来定义对象的类型。
接口是对象的状态(属性)和行为(方法)的抽象(描述)
## 初探
```ts
// 需求: 创建人的对象, 对人的属性进行一定的约束
/* id是number类型, 必须有, 只读的
name是string类型, 必须有
age是number类型, 必须有
gender是string类型, 可以没有 */

// 定义一个接口，该接口作为person对象的类型使用，限定或是约束该对象中的属性数据
interface IPerson {
  // 只读属性 readonly
  readonly id: number,
  name: string,
  age: number,
  // 可选属性 在属性名后加一个 ? 
  gender?: string
}

// 定义一个对象，该对象的类型就是定义的接口IPerson
const person: IPerson = {
  id: 1,
  name: 'Rob',
  age: 18,
  // gender: 'male'
}
```
const & readonly
作为变量使用用 const，作为属性则使用 readonly

## 函数类型
为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。
```ts
// 定义一个函数，用来作为某个函数的类型使用
interface SearchFunc {
  // 定义一个调用签名
  (source: string, subString: string): boolean
}

// 定义一个函数，该类型就是上面定义的接口
const mySearch: SearchFunc = function (source: string, sub: string): boolean {
  return source.search(sub) > -1
}
// 调用函数
console.log(mySearch('abcd', 'bc')) // true
```

## 类类型
与 C# 或 Java 里接口的基本作用一样，TypeScript 也能够用它来明确的强制一个类去符合某种契约。
```ts
// 定义一个接口
interface IFly {
  // 该方法没有任何的实现
  fly()
}

// 定义一个类，这个类的类型就是上面定义的接口
// 也可以理解为，IFly接口约束了当前的这个Person类
class Person implements IFly {
  fly() {
    console.log('I Can Fly!');
  }
}
// 实例化对象
const person = new Person()
person.fly() // I Can Fly!

// 定义一个接口
interface ISwim {
  swim()
}

// 定义一个类，这个类的类型就是IFly和ISwim
// 当前这个类可以实现多个接口，一个类同时可以被多个接口约束
class Person2 implements IFly, ISwim {
  fly() {
    console.log('fly!')
  }
  swim() {
    console.log('swim!')
  }
}
// 实例化对象
const person2 = new Person2()
person2.fly() // fly!
person2.swim() // swim!
```
总结：类可以通过接口的方式，来定义当前这个类的类型
类可以实现一个接口，类也可以实现多个接口，要注意，接口中的内容都要真正的实现
```ts
// 定义一个接口，继承其他的多个接口
interface MyFlyAndSwim extends IFly, ISwim { }

// 定义一个类，直接实现MyFlyAndSwim这个接口
class Person3 implements MyFlyAndSwim {
  fly() {
    console.log('f')
  }
  swim() {
    console.log('s')
  }
}
const person3 = new Person3()
person3.fly() // f
person3.swim() // s
```
总结：接口和接口之间叫继承（使用的是extends关键字），类和接口之间叫实现（使用的是implements）
