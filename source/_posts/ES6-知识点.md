---
title: ES6 学习笔记总结
date: 2017-11-23 14:10:30
categories: "ECMAScript 6" #文章分类目录
tags: [ES6, StudyNote] #文章标签 可以省略
description: #你對本頁的描述 可以省略
---

### 前言
  ECMAScript 6（以下简称ES6）是JavaScript语言的下一代标准。因为当前版本的ES6是在2015年发布的，所以又称ECMAScript 2015。

----------
### Babel转码器
[Babel](https://babeljs.io/) 
Babel是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。大家可以选择自己习惯的工具来使用使用Babel，具体过程可直接在Babel官网查看：

----------
### let和const命令

#### 基本用法

用来声明变量，但是所有声明的变量只在let命令所在的代码块中有效;
```js
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```
变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。

#### 不存在变量提升

let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。
```js
// let 的情况
console.log(error); // 报错ReferenceError
let error = 2;
```

#### 块级作用域
```js
function fun() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```
上面的函数有两个代码块，都声明了变量n，运行后输出 5。这表示外层代码块不受内层代码块的影响。

#### 不能重复声明变量
let不允许在相同作用域内，重复声明同一个变量。
```js
function func() {
  let a = 10;
  let a = 1;
  console.log(a); // 报错
}
function func(a) {
  let a; // 报错
}
function func(a) {
  {
    let a; // 不报错
  }
}
```

#### const
const也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变。
const命令的常量 也是不提升，同样存在暂时性死区，只在声明所在的块级作用域内有效.
const声明的常量，也与let一样不可重复声明。
```js
var message = "Hello!";
let age = 25;
const message = "Goodbye!";// 报错
const age = 30;// 报错
和let一样不可重复声明
```
```js
if (true) {
  const fun = 5;
}
fun // Uncaught ReferenceError: fun is not defined
```
块级作用域
### 变量的解构赋值
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构
```js
let { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
----------------------
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```
### Symbol
ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。
它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

注意，Symbol函数前不能使用new命令，否则会报错。这是因为生成的Symbol是一个原始类型的值，不是对象
Symbol函数可以接受一个字符串作为参数，表示对Symbol实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
```js
// 没有参数的情况
var s1 = Symbol();
var s2 = Symbol();
s1 === s2 // false
// 有参数的情况
var s1 = Symbol("foo");
var s2 = Symbol("foo");
s1 === s2 // false
```
s1和s2都是Symbol函数的返回值，而且参数相同，但是它们是不相等的。


```js
let sym = Symbol('My symbol');

"your symbol is " + sym
// TypeError: can't convert symbol to string
`your symbol is ${sym}`
// TypeError: can't convert symbol to string
```
Symbol 值不能与其他类型的值进行运算，会报错

### 箭头函数
ES6 允许使用“箭头”（=>）定义函数，支持expression 和 statement 两种形式。同时一点很重要的是它拥有词法作用域的this值，帮你很好的解决this的指向问题，这是一个很酷的方式，可以帮你减少一些代码的编写。
注意
（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。
```js
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);
  // 普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0
```
Timer函数内部设置了两个定时器，分别使用了箭头函数和普通函数。前者的this绑定定义时所在的作用域（即Timer函数），后者的this指向运行时所在的作用域（即全局对象）。所以，3100 毫秒之后，timer.s1被更新了 3 次，而timer.s2一次都没更新。

```js
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout( () => {
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}
 var animal = new Animal()
 animal.says('hi')  //animal says hi
```
当我们使用箭头函数时，函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，它的this是继承外面的，因此内部的this就是外层代码块的this。
### Module语法
在 ES6 之前，社区制定了一些模块加载方案，最主要的有 CommonJS 和 AMD 两种。前者用于服务器，后者用于浏览器。ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。

ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。
#### export
ES6将一个文件视为一个模块，上面的模块通过 export 向外输出了一个变量。一个模块也可以同时往外面输出多个变量。

```js
// profile.js 
//写法一：
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
//写法二：
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
export {firstName, lastName, year};
```
#### import命令
定义好模块的输出以后就可以在另外一个模块通过import引用。
```js
  //index.js
 import {name, age} from './profile.js'
```
#### default
不用关系模块输出了什么，通过 export default 指令就能加载到默认模块，不需要通过 花括号来指定输出的模块,一个模块只能使用 export default 一次
```js
 // default 导出
  export default class { ... }
  // 导入的时候不需要花括号
  import test from './test.js';
```
#### import
### 参考
link： http://es6.ruanyifeng.com/