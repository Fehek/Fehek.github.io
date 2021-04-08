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

# 基础使用
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

# 类型
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

# 类
## 基本实例
类可以理解为模板，通过模板可以实例化对象
```ts
// ts中类的定义及使用
class Person {
  // 定义属性
  name: string
  age: number
  gender: string
  // 定义构造函数：为了将来实例化对象时，可以直接对属性的值进行初始化
  constructor(name: string = '小明', age: number = 18, gender: string = '男') {
    this.name = name
    this.age = age
    this.gender = gender
  }
  // 定义实例方法
  sayHi(str: string) {
    console.log(`你好，我是${this.name}，今年已经${this.age}岁了，是个${this.gender}孩子，${str}`)
  }
}

// ts中使用类，实例化对象，可以直接进行初始化操作
const person = new Person('小红', 10, '女')
person.sayHi('你叫什么名字')
```

## 继承
继承：类与类之间的关系
A类继承了B类，A类叫子类（派生类），B类叫基类（超类 / 父类）
```ts
// 定义一个类，继承自Person
class Student extends Person {
  constructor(name: string, age: number, gender: string) {
    // 调用的是父类中的构造函数，使用的是super
    super(name, age, gender)
  }
  // 可以调用父类中的方法
  sayHi() {
    console.log('我是学生类中的sayHi方法')
    // 调用父类中的sayHi方法
    super.sayHi('haha')
  }
}
const stu = new Student('小张', 21, '女')
stu.sayHi()
```
总结：类和类之间如果要有继承关系，需要使用extends关键字
子类中可以调用父类中的构造函数，使用的是super关键字（包括调用父类中的实例方法，也可以使用super）
子类中可以重写父类的方法

## 多态
父类型的引用指向了子类型的对象，不同类型的对象针对相同的方法，产生了不同的行为
```ts
// 定义一个父类
class Animal {
  name: string
  constructor(name: string) {
    this.name = name
  }
  run(distance: number = 0) {
    console.log(`跑了${distance}米这么远的距离`, this.name)
  }
}

// 定义一个子类
class Dog extends Animal {
  constructor(name: string) {
    super(name)
  }
  run(distance: number = 5) {
    console.log(`跑了${distance}米这么远的距离`, this.name)
  }
}

// 定义一个子类
class Pig extends Animal {
  constructor(name: string) {
    super(name)
  }
  run(distance: number = 10) {
    console.log(`跑了${distance}米这么远的距离`, this.name)
  }
}

// 实例化父类对象
const ani: Animal = new Animal('动物')
ani.run()
// 实例化子类对象
const dog: Dog = new Dog('小狗')
dog.run()
const pig: Pig = new Pig('小猪')
pig.run()
console.log('========')

// 也可以写成这样
const dog2: Animal = new Dog('大狗')
dog2.run()
const pig2: Animal = new Pig('大猪')
pig2.run()
console.log('========')

function showRun(ani: Animal) {
  ani.run()
}
showRun(dog2)
showRun(pig2)
```

## 修饰符
- public
默认修饰符，任何位置都可以访问类中的成员
- private
不能在声明它的类的外部访问
- protected
外部无法无法访问这个成员数据，但是子类中是可以访问的
- readonly
```ts
// 修饰类中的成员属性
// 修饰后，该属性成员就不能在外部被随意修改了
// 构造函数中可以对只读属性成员的数据进行修改
class Person {
  readonly name: string = 'abc'
  constructor(name: string) {
    this.name = name
  }
}

let stu = new Person('John')
console.log(stu)
// stu.name = 'Peter' // error
```
```ts
// 修饰类中的构造函数中的参数（参数属性）
class Person2 {
  constructor(readonly name: string) {
    // 被修饰后，有了一个name属性成员，外部无法进行修改
    // 也可以使用 public, private, protected 进行修饰，无论是哪个进行修饰，该类中都会自动添加这么一个属性成员
    // this.name = name
  }
}

const p = new Person2('Jack')
console.log(p)
// p.name = 'Kat' // error
console.log(p.name)
```

## 存取器
可以有效控制对对象中成员的访问，通过getter和setter来进行操作
```ts
class Person {
  firstName: string
  lastName: string
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName
    this.lastName = lastName
  }
  // 读取器=》负责读取数据
  get fullName() {
    console.log('getting...')
    return this.firstName + '_' + this.lastName
  }
  // 设置器 =》负责设置数据（修改）
  set fullName(val) {
    console.log('setting...')
    let names = val.split('_')
    this.firstName = names[0]
    this.lastName = names[1]
  }
}
const person: Person = new Person('张', '三')
console.log(person)
// 获取该属性成员属性
console.log(person.fullName)
// 设置该属性的数据
person.fullName = '李_四'
console.log(person.fullName)
```

## 静态属性
非静态属性, 是类的实例对象的属性
静态属性, 是类对象的属性
```ts 
class Person {
  name1: string = 'A'
  static name2: string = 'B'
}
console.log(new Person().name1)
// console.log(Person.name1) // error
// console.log(new Person().name2) // error
console.log(Person.name2)
```

## 抽象类
抽象类不能被实例化，可以包含实例方法。作用是为了子类服务的。
```ts
abstract class Animal {
  abstract cry()
  run() {
    console.log('run()')
  }
}

class Dog extends Animal {
  cry() {
    console.log('Dog cry()')
  }
}

const dog = new Dog()
dog.cry()
dog.run()
```

# 函数重载
函数名相同, 而形参不同的多个函数
```ts
/* 
需求: 有一个add函数，它可以接收2个string类型的参数进行拼接，也可以接收2个number类型的参数进行相加 
*/

// 重载函数声明
function add(x: string, y: string): string
function add(x: number, y: number): number

// 定义函数实现
function add(x: string | number, y: string | number): string | number {
  // 在实现上我们要注意严格判断两个参数的类型是否相等，而不能简单的写一个 x + y
  if (typeof x === 'string' && typeof y === 'string') {
    return x + y
  } else if (typeof x === 'number' && typeof y === 'number') {
    return x + y
  }
}

console.log(add(1, 2))
console.log(add('a', 'b'))
// console.log(add(1, 'a')) // 若不写重载函数声明，不会报错
```

# 泛型
指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定具体类型的一种特性。
## 使用函数泛型
```ts
function createArray<T>(value: T, count: number): T[] {
  const arr: Array<T> = []
  for (let i = 0; i < count; i++) {
    arr.push(value)
  }
  return arr
}

const arr = createArray<number>(11.123, 3)
console.log(arr[0].toFixed())
const arr2 = createArray<string>('aa', 3)
console.log(arr2[0].split(''))
```

## 多个泛型参数的函数
一个函数可以定义多个泛型参数
```ts
function getMsg<K, V>(value1: K, value2: V): [K, V] {
  return [value1, value2]
}
const arr1 = getMsg<string, number>('Jack', 100.123)
console.log(arr1[0].split(''), arr1[1].toFixed(1))
```

## 泛型接口
在定义接口时, 为接口中的属性或方法定义泛型类型; 在使用接口时, 再指定具体的泛型类型。
```ts
interface baseCRUD<T> {
  data: T[]
  add: (t: T) => void
  getById: (id: number) => T
}

class User {
  id?: number
  name: string
  age: number
  constructor(name, age) {
    this.name = name
    this.age = age
  }
}

class UserCRUD implements baseCRUD<User> {
  data: User[] = []

  add(user: User): void {
    user = { ...user, id: Date.now() }
    this.data.push(user)
    console.log('保存user', user.id)
  }

  getById(id: number): User {
    return this.data.find(item => item.id === id)
  }
}


const userCRUD = new UserCRUD()
userCRUD.add(new User('tom', 12))
userCRUD.add(new User('tom2', 13))
console.log(userCRUD.data)
```

## 泛型类
在定义类时, 为类中的属性或方法定义泛型类型; 在创建类的实例时, 再指定特定的泛型类型
```ts
class GenericNumber<T> {
  zeroValue: T
  add: (x: T, y: T) => T
}

let myGenericNumber = new GenericNumber<number>()
myGenericNumber.zeroValue = 0
myGenericNumber.add = function (x, y) {
  return x + y
}

let myGenericString = new GenericNumber<string>()
myGenericString.zeroValue = 'abc'
myGenericString.add = function (x, y) {
  return x + y
}

console.log(myGenericString.add(myGenericString.zeroValue, 'test'))
console.log(myGenericNumber.add(myGenericNumber.zeroValue, 12))
```

## 泛型约束
```ts
// 如果直接对一个泛型参数取 length 属性会报错, 因为这个泛型根本就不知道它有这个属性
interface ILength {
  length: number
}
function getLength<T extends ILength>(x: T): number {
  return x.length
}
console.log(getLength<string>('Hello World'))
// console.log(getLength<number>(123)) // error
```

# 内置对象
JavaScript 中有很多内置对象，它们可以直接在 TypeScript 中当做定义好了的类型。
内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。
- ECMAScript 的内置对象
`Boolean  Number  String  Date  RegExp  Error`
```ts
let b: Boolean = new Boolean(1)
let n: Number = new Number(true)
let s: String = new String('abc')
let d: Date = new Date()
let r: RegExp = /^1/
let e: Error = new Error('error message')
// let bb: boolean = new Boolean(2)  // error
```
- BOM 和 DOM 的内置对象
`Window  Document  HTMLElement  DocumentFragment  Event  NodeList`
```ts
const div: HTMLElement = document.getElementById('test')
const divs: NodeList = document.querySelectorAll('div')
document.addEventListener('click', (event: MouseEvent) => {
  console.dir(event.target)
})
const fragment: DocumentFragment = document.createDocumentFragment()
```
