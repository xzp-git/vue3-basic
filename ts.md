# TS学习
## 强类型与弱类型
- 类型安全
> 强类型 语言层面限制函数的实参类型必须与形参类型相同  反之 就是弱类型
强类型语言中不允许任意的数据隐式类型转换，弱类型允许。
## 静态类型与动态类型
- 类型检查
> 动态类型的语言中变量是没有类型的，变量中存放的值是有类型的
## JavaScript自有类型的系统的问题
- 弱类型 且 动态类型
- 弱类型的问题
  1.只有运行时 才会暴露出代码的问题
- 强类型的优势
  1.错误更早暴露
  2.代码更智能，编码更准确
  3.重构更牢靠
  4.减少不必要的类型判断
## Flow静态类型检查方案
- JavaScript的类型检查器  Flow只是一个小工具
### Flow的使用
1.安装 yarn 
```js 
npm i yarn -g
```
2.初始化package文件
```js 
yarn init --yes
```
3.安装flow
```js 
yarn add flow-bin
```
4.初始化 flow配置文件
```js 
yarn flow init
```
5.执行 
```js 
yarn flow
```
6.完成编码后可以关闭这个flow服务 
```js 
yarn flow stop
```
```js
// @flow      需要在用到flow的文件中加入这个注释
function sum (a:number,b:number){
  return a + b 
}

sum (100 ,100)

sum('100','100')//控制台执行 yarn flow 会提示此处会报错
```

- 编码完成后移除 注解
一、方法一
1.安装 
```js 
yarn add flow-remove-types --dev
```
2.运行 
```js 
yarn flow-remove-types . -d dist 
```
- . 指当前文件  把当前文件移除注解后转化到 dist 目录
二、方法二
1.安装 babel 的相关依赖
```js 
yarn add @babel/core @babel/cli @babel/preset-flow --dev
```
2.在项目的根目录下创建 .babelrc文件添加配置
```js
  {
      "preset":["@babel/preset-flow"]
  }
```
3.运行 命令 
```js 
yarn babel src -d dist
```
把src项目下的注解移除生成到 dist文件下
- 使用vscode插件flow Language Support 
flow 官方的插件可以不用在控制台运行直接在编辑时提示错误，需要保存后才能提示
- 类型推断
有时候flow可以根据上下文自动的推断出数据类型
- 原始类型的使用
js中的原始数据类型都支持 
 *** 需要注意的是 在标记undefined的时候 *** 
 ```js 
 const a:void = undefined
 ```
- 数组类型的使用 需要一个泛型参数 有两种表示方式

  1.
```js 
 const arr:Array<number> = [1,2,3,4,5,6]
```
2.
```js 
const arr:number[] = [1,2,3,4,5,6]
```
3.固定长度的表示方式 
```js  
const foo:[string,number] = ["foo" , 66] 
//这种固定长度的表示方式又叫元组，如果一个函数中有多个返回值，则可以使用这种方式
```
- 对象类型
1.
```js 
const obj:{foo?:string,bar:number} = {foo:'string',bar:100}
```
加？表示这个值可有可无
2.
```js 
  const obj:{[string]:string} = {}
  obj.key1="value1"
  obj.key2="value2"      
```
这个对象可以加任意个的键值对，但是键与值都只能是string
- 函数类型
```js 
  function foo(callback:(string,number) => void){
     callback('string',100)
  }
  foo(function(str,n){

  })
 
```
- 特殊类型
1.字面量类型
```js 
const a:'foo' = "foo"
```
指a这个变量只能存放foo这个字符串
```js 
const type:'success'|'warning'|'danger' = "success" 
```
指type的值只能是这三个字符串中的一个
```js 
type StringOrNumber = string | number
const str:StringOrNumber= "success" 
```
可以用type关键字声明 一个数据类型
```js 
const num:?number= 66
const num:number | null | undefined = 66 
```
加？与它下面的那种表示方法一致
- Mixed&Any
Mixed代表所有的数据类型 强类型 必须先明确数据类型是什么
Any也能接受任意类型的数据 弱类型
```js
  function fn(value:mixed){
        if(typeof value == "string"){
           value.substring(0,10)
        }
  }
  function fn(value:any){
           value.substring(0,10)
  }


```
学习地址 https://www.saltycrane.com/cheat-sheets/flow-type/latest/
- 运行环境 Api 内置对象
例如浏览器环境
```js
 const ele:HTMLElement|null = document.getElementById('app')
```
## TypeScript语言规范与基本应用
- JavaScript的超集，最后将被编译为javascript
### 安装
```js 
yarn add typescript --dev
```
### 编译
```js 
yarn tsc 01.ts
```
将会生成一个同名的js文件
ts文件
```ts
const hello = (name:string) =>{
    console.log(`hello,${name}`)
}
hello("TypeScript")
```
生成的js文件
```js
var hello = function (name) {
    console.log("hello," + name);
};
hello("TypeScript");
```
### 配置文件
1.运行命令生成tsconfig.json的文件
```js
yarn tsc --init
```
### 数据类型
- 原始数据类型
```js  
 const a:string="foo"

 const b:number = 100 //NaN Infinity

 const c:boolean = true //false

//  const c:boolean = null   如果配置文件中的严格模式 为 false 则以上三种数据的值可以为null
 const c:void = undefined //标记函数的返回值类型  严格  undefined 非严格 null 与 undefined
 const c:undefined = undefined
 const c:null = null
 const h:symbol = Symbol()
```
- 标准库的声命
1.通过配置文件中的target来修改
2.通过修改配置文件中的"lib"选项
```js
"lib":["ES2015","DOM"]
```
- 中文的错误消息
编译时使用 
```js
yarn tsc --locale zh-CN
```
- 作用域问题
1.放在一个立即执行函数中
```js
(function(){
    const h:string="hello"
})()
```
2.在文件中添加一个 export {}
```js 
const h:string="hello"
export {}
```
- Object类型  
泛指 非原始数据类型
```js
const foo:object=function(){} //{} //[]
// Object类型可以是以上三种   object是小写的
```
```js
const obj:{foo:number,bar:string} = {foo:123,bar:"string"}
// 类似字面量的方式，更好的方式是用接口的方式
```
- 数组类型
1.泛指类型
```js
const arr1:Array<number> = [1,2,3]
```
2.
```js
const arr1:number[] = [1,2,3]
```
- 元组类型
```js
const tuple:[number,string] = [18,"zce"]
// const age = tuple[0]
// const age = tuple[1]
const [age,name] = tuple
```
- 枚举类型
```js
const post = {status:3} //0  //1 // 2

// 通过enum关键字来声明枚举值
enum PostStatus{
    Draft=0,
    Unpublished = 1,
    published = 2
}
const post = {status:PostStatus.published}
enum PostStatus{
    Draft,
    Unpublished ,
    published 
}
// 不给枚举变量指定值的话，默认会从0开始累加

enum PostStatus{
    Draft=6,
    Unpublished ,
    published 
}
// 假如给第一个变量指定一个值，后续的变量会在第一个值的基础上自增
// 枚举值也可以是字符串

const enum PostStatus{
    Draft=0,
    Unpublished = 1,
    published = 2
}
// 加 const 表示 常量枚举
```
- 函数类型
```js
function func1(a:number,b:number):string{
   return "func1"
}
func1(100,200)
// 实参 必须与形参相同
function func1(a:number,b?:number):string{
   return "func1"
}
function func1(a:number,b:number = 100,...rest:number[]):string{
   return "func1"
}
func1(100)
// 加？或者是默认值 表示参数可选  可选参数 必须出现在参数的最后
```
- 函数表达式类型
```js
const func2:(a:number,b?:number)=>string = function (a:number,b?:number):string{
   return "func1"
}
```
- 任意类型
```js
let foo:any = "string"
// any 类型是不安全的 轻易不使用
```
- 隐式类型推断
```js
let age = 18 //隐式推断为number

age = "string"  //语法报错

let foo
foo = 100
foo = "string"
// 隐式推断为  any类型
```

- 类型断言
```js
// 假定这个nums来自 一个明确的接口

const nums = [110,120,119,112]

const res = nums.find(i=>i>0)
// 此时 ts 并不知道 res 一定会返回一个 number 我们需要下面这儿样做 告诉ts 此处的   res 肯定会是个 number  即类型断言

const num1 = res as number
// 或者
const num2 = <number>res  //jsx中 不能使用 会冲突

```
- 接口
```js
// 接口定义对象

interface Post{
    title:string
    content:string
}
function printPost(post:Post){
    console.log(post.title)
    sonsole.log(post.content)
}

printPost({title:'hello typescript',content:"a javascript superset"})
```
 - 可选成员
```js

interface Post{
    title:string
    content:string
    subtitle?:string
}
```
 - 只读成员
 ```js
interface Post{
    title:string
    content:string
    subtitle?:string
    readonly summary:string
}
```
 - 动态用法
 ```js
 interface Cache{
     [key:string]:string
 }
 ```
- 类
描述一类具体事务的抽象
```js
class Person{
    name:string
    age:number
    constructor(name:string,age:number){
      this.name = "name"
      this.age = 23
    }
    sayHi(msg:string):void{
        console.log(`I am ${this.name},${msg}`)
    }
}
```
- 类的访问修饰符
控制类当中的成员的可访问级别
```js
class Person{
    public name:string //公有成员 默认是公有
    private age:number //私有变量
    protected gender:boolean
    constructor|(name:string,age:number){
        this.name = name
        this.age = age
        this.gender = true
    }
    sayHi(msg:string):void{
        console.log(`I am ${this.name},${msg}`)
        console.log(this.age)
    }
}
const tom = new Person('tom',18)
console.log(tom.name)
console.log(tom.age)  //报错 因为age是私有变量  private
console.log(tom.gender) //报错 因为 gender是受保护的变量  protected
// 两者的区别   
class Student extends Person {
    private constructor(name:string,age:number){
        super(name,age)
        console.log(this.gender)
    }
    static create(name:string,age:number){
        return new Students(name,age) 
    }
}
// protected 修饰的变量可以在子类中访问

// 构造函数的访问修饰符 默认是 public  如果设置成private则不能在外部被实例化 也不能被继 承   如果此时想要实例化的话，则要在类中创建一个静态的方法去实例化
const jack = Student.create('jack',18)
// 对于private protected 也是不能在外部实例的  但是可以被继承

```

- 类的只读属性
//使用 readonly 可以类的属性成员 设置为只读属性 如果这个属性已经有 访问修饰符了 则应该跟在 修饰符的后面 对于只读属性 可以在类型声明的时候 直接在 = 后面 初始化  或者在 构造函数中初始化   在初始化后 只读属性 是不可以被修改的
```js
class Person{
    public name:string //公有成员 默认是公有
    private age:number //私有变量
    protected readonly gender:boolean
    constructor|(name:string,age:number){
        this.name = name
        this.age = age
        this.gender = true
    }
    sayHi(msg:string):void{
        console.log(`I am ${this.name},${msg}`)
        console.log(this.age)
    }
}
```

- 类与接口
```js
interface Eat{
    eat (food:string):void

}
interface Run{
    run (distance : number):void
}
class Person implements Eat,Run{

    eat(food:string):void{
        console.log(`优雅的进餐:${food}`)
    }
    run(distance:number){
        console.log(`直立行走:${distance}`)
    }
}

class Animal implements Eat,Run{

    eat(food:string):void{
        console.log(`呼噜呼噜的吃:${food}`)
    }
    run(distance:number){
        console.log(`爬行:${distance}`)
    }
}

// 以上两个类都有共有的特性 这种情况就相当于不同的类型实现了相同的接口
// 使用接口定义公共的能力
// 需要注意 让每个接口更细化  在接口中定义的能力不一定会同时存在，因此一个接口对应一个能力
```
- 抽象类
//抽象类和接口在某种程度上有点相似，他也是用来约束子类中必须要有某个成员 
//不同的是 抽象类可以包含具体的实现  接口不包含具体的是实现
//较大的类目 使用抽象类
//定义方式 在class 前 加 abstract   抽象类只能被继承 不能被 new 去实例
```js
abstract class Animal implements Eat,Run{

    eat(food:string):void{
        console.log(`呼噜呼噜的吃:${food}`)
    }
    abstract run(distance:number):void   //抽象方法 不需要方法体
}
class Dog extends Animal{
     run(distance:number):void{
          console.log(`爬行:${distance}`)
     } 
}

const d = new Dog()
d.eat("谷歌")
d.run(500)
```
- 泛型
```js
function createNumberArray(length:number,value:number):number[]{
   const arr = Array<number>(length).fill(value)  //使用Array时再定义类型
   return arr
}
// 假如要定义一个string的数组 则上面的函数就不能用了  此时泛型就要上场了

function createArray<T>(length:number,value:T):T[]{
   const arr = Array<T>(length).fill(value)  
   return arr
}
const res  = createArray<string>(3,"TypeScript")   //使用泛型将所需要的类型传递进去 能更好的复用代码
```
- 类型声明
1.安装 yarn add lodash
``js
import { camelCase } from "lodash"
//安装第三方模块的时候没有类型声明需要进行类型的声明
declare function camelCase(input:string):string
//或者 有的模块 会提供 专门的类型声明  需要安装一下 yarn add @types/lodash --dev
//有的模块内部会集成类型声明 都不需要在单独安装类型声明的模块
const res  = camelCase("hello type")  
```


