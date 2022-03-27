---
title: 200515TypeScript
comments: true
tags:
  - TypeScript
  - Js
categories: Js
date: 2020-05-15 15:27:35
---
<!--more-->
# ts中的类型
* boolean
* number
* string
* array
```ts
// 第一种写法
let arr1:string[] = ['a','b']
// 第二种写法
let arr2:Array<number> = [1,2]
```
* tuple(元组):数组的一种,可以指定数组里面的类型
```let tuple:[string,number] = ['a',1]```
* enum(枚举)
```ts
enum Flag {success=1 , error = 0}
let flag:Flag = Flag.error;
// 默认值为索引值
enum Flag {success ,  error }
console.log(Flag.success) //0
console.log(Flag.error)   //1
// 默认值为前面一个的下一个值
enum Flag {success = 6 ,  error }
console.log(Flag.success)  //6
console.log(Flag.error)    //7
```
* any
* null和undefined (never其他类型的子类型)
* void
```ts
// 用来表示函数的返回类型为空,不可用null或者undefined
function test():void{
  console.log(1)
}
test()
```
* never  代表从不会出现的值

# 函数
## 参数类型和返回类型
```ts
let add = (num1:number , num2:number):number =>{
  return num1 + num2
}
```
## 可选参数
?
```ts
let add = (num1:number , num2 ?:number):number =>{
  if (num2) 
    return num1 + num2
  else
    return num1
}
```

## 默认参数
:<类型> = <值>
```ts
let add = (num1:number , num2 : number = 2):number =>{
  return num1 + num2
}
```

## 剩余参数
使用展开运算符
```ts
let loop = (...result:number[]):void=>{
  result.forEach((ele:number)=>{
    console.log(ele);
  })
}
loop(1,2,3)
```

## 函数重载
js设计出来就没有想过重载,所以ts只能手动实现重载
```ts
function showAge(num:number):void;
function showAge(num:string):void;
function showAge(num:any):void{
  if (typeof num === 'string') {
    console.log('传入的是字符串') ;
  }
  else{
    console.log('传入的是数字') ;
  }
}

showAge(1)   //传入的是数字
showAge('2')  //传入的是字符串
showAge(true)  // error
```

# 类
## 定义
```ts
class Person {
  name: string; //省略pubilc
  constructor(name: string) {
    this.name = name;
  }
  setName(name: string): void {
    this.name = name;
  }
  getName(): string {
    return this.name
  }
}
```

## 继承
extends 和 super
```ts
class Stu extends Person {
  constructor(name: string){
    super(name);
  }
}

let a = new Stu('涨三倍');
console.log(a.getName())
```

## 修饰符
* public   类里面,类外,子类都能用 (默认)
* private   在类里面和子类可以用,类外部不能用
* protected 只能在类里面用

## 静态属性 静态方法
不需要实例化就能使用的属性和方法
```ts
class Person {
  static sex: string = "男";
  static print(){
    console.log(Person.sex)
  }
}
Person.print()
```

## 抽象类 继承多态
### 继承多态
```ts
class Animal{
  name: string;
  constructor(name: string){
    this.name = name;
  }
  eat(){}
}
class Dog extends Animal{
  constructor(name: string){
    super(name)
  }
  eat(){
    console.log(`${this.name}`)
  }
}
class Cat extends Animal{
  constructor(name: string){
    super(name)
  }
  eat(){
    console.log(`${this.name}`)
  }
}

let dog = new Dog('狗');
let cat = new Cat('猫')
dog.eat()
cat.eat()
```
### 抽象类
1. 抽象方法只能定义在抽象类中
2. 继承了抽象类的类必须实现抽象方法
3. 抽象类不能实现
```ts
abstract class Animal{
  name: string;
  constructor(name: string){
    this.name = name;
  }

  abstract eat():void
}

class Dog extends Animal{
  constructor(name: string){super(name)}

  eat():void{
    console.log('我是狗的eat')
  }
}

let a = new Dog('n');
a.eat()
```

# 接口
## 属性类接口
对 json 的约束
```ts
interface Fullname{
  firstName: string,
  lastName: string
}
function printName(name: Fullname){
  // console.log(name.age)  会报错 因为是接口中不存在的属性
  console.log(`${name.firstName}-${name.lastName}`)
}

let obj = {
  age: 15,
  firstName:'zhang',
  lastName: 'yin'
}

printName(obj);
printName({
  firstName: 123,  //error
  lastName: 'yin'
})
```
## 函数类型接口
**对方法传入的参数以及返回值进行约束**
```ts
interface ecrypt {
  (key: string, value: string):string
}

let test:ecrypt = function(key: string, value: string){
  return key + value;
}

console.log(test('a','b'))
```

## 可索引接口
**数组,对象的约束**
```ts
interface objRule {
  [index:string]: string
}
let a: objRule = {
  name : '111',
  age : '2222',
  set: 3  // error
}
```

## 类类型接口
**对类的约束**
```ts
interface Animal {
  name: string ;
  eat(food: string):void
}

class Dog implements Animal{
  name: string;
  constructor(name: string){
    this.name = name;
  }
  eat(food: string): void {
    console.log(this.name + food)
  }
}

let dog = new Dog('a');
dog.eat('b');
```


## 接口拓展
1. 接口继承接口
2. 类继承类实现接口

# 泛型
**可以支持不特定的数据类型**
## 泛型的定义
```ts
function getData<T>(data: T): T{
  return data;
}

console.log(getData<number>(123))
console.log(getData<string>('123'))
```

## 泛型可以用来拓展函数的未来数据
```ts
class MinList<T> {
  private list:T[];
  constructor(){
    this.list = [];
  }
  
  add(data: T){
    this.list.push(data)
  }

  min(): T{
    let min: T = this.list[0];
    for (const iterator  of this.list) {
      min =  iterator < min ? iterator : min 
    }
    return min;
  }
}
// 泛型初始化
let test1 = new MinList<string>();
test1.add('b');
test1.add('d');
test1.add('e');
console.log(test1.min())
```
该类用来需要最小值堆,在初始化时决定是一个number堆还是一个string堆

## 泛型接口
```ts
// 第一种写法
interface ecrypt{
  <T>(data: T): T
}

let getData: ecrypt = function<T>(data: T): T{
  return data;
}

getData<string>('123');
getData<string>(123);// error

```


```ts
// 第二种写法
interface ecrypt<T> {
  (value: T):T
}

function getData<T>(value: T): T{
  return value;
}

let test: ecrypt<string> = getData;
console.log(test('123'))
```

## 类泛型
```ts
class User {
  name: string;
  pwd: string;
  constructor(name: string, pwd: string){
    this.name = name ;
    this.pwd = pwd;
  }
}

class Rec {
  des: string;
  constructor(des: string){
    this.des = des;
  } 
}

class MySql<T>{
  list : T[] = [];
  add(one: T){
    console.log(one)
  }
}
/*
User { name: '123', pwd: '123' }
Rec { des: 'rec' }
*/
let user = new User('123','123');
let mysql1 = new MySql<User>();
mysql1.add(user);
let mysql2 = new MySql<Rec>();
mysql2.add(new Rec('rec'));
```

# 装饰器
## 普通传参
**默认参数为params,就是那个class,且无法传入新的参数**
```ts
function addName(params: any){
  params.prototype.addProperty = 'aaa'
}

@addName
class Person{
  constructor(){}
}

let a:any = new Person();
console.log(a.addProperty) // aaa
```

## 类装饰器
可以传参,第一个params是传入的参数,返回函数的参数target就是目标类
```ts
function addName(params: string){
  return function(target: any){
    console.log(target);
    console.log(params)
  }
}

@addName('1')
class Person{
  constructor(){}
}
/*
[Function: Person]
1
*/
let a:any = new Person();

```

## 属性装饰器
```ts
function addName(params: string){
  // params  传入参数
  // target  目标类
  // attr 目标属性
  return function(target: any,attr: any){
    console.log(target);
    console.log(attr)
    target[attr] = params;
  }
}

class Person{
  @addName('属性装饰器')
  name: string | undefined;
  constructor(){};
  getName(): void{
    console.log(this.name)
  }
}

let a:any = new Person();
a.getName()
```

## 方法装饰器
1. target 对于静态成员来说是构造函数,对于普通方法是原型对象
2. methodName 方法名字
3. desc 属性描述符

```ts
function addName(params: string){
  return function(target: any,methodName: any,desc:any){
    console.log(target);
    console.log(methodName)
    console.log(desc);
  }
}


class Person{
  
  name: string | undefined;
  constructor(){};
  @addName('属性装饰器')
  getName(): void{
    console.log(this.name)
  }
}

let a:any = new Person();
/*
Person { getName: [Function] }
getName
{
  value: [Function],
  writable: true,
  enumerable: true,
  configurable: true
}
*/

```

## 方法参数装饰器
1. target 对于静态成员来说是构造函数,对于普通方法是原型对象
2. methodName 参数名字
3. desc 参数在函数列表中的索引

```ts
function addName(params: string){
  return function(target: any,paramsName: any,paramsIndex:any){
    console.log(target);
    console.log(paramsName)
    console.log(paramsIndex);
  }
}


class Person{
  
  name: string | undefined;
  constructor(){};
 
  getName( @addName('属性装饰器') num:number ): void{
    console.log(this.name)
  }
}

let a:any = new Person();
/*
Person { getName: [Function] }
getName
0
*/

```

## 执行顺序
后入先出,压栈