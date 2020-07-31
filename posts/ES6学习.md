---
title: ES6学习
date: 2020-07-24 16:34:10
tags: es6

---

js的一种版本标准，只是为了学一些重点的新语法。

<img src="https://www.runoob.com/wp-content/uploads/2018/12/es6-tutorial.jpg" alt="img" style="zoom: 50%;" />

1.箭头函数

**箭头函数的作用主要是定义匿名函数**

```js
let foo = function () {};
// 转换成箭头函数
let foo = () => {};
```

基本语法**（参数）**：

- 匿名函数没有参数：() 不能省略，占位作用。`let foo = () => {};`
- 只有一个参数：() 可以省略，也可以不省略。`let foo = a => {};`
- 多个参数，() 不能省略。`let foo = (a,b) => {};`

2.let 与 const

let 声明的变量只在 let 命令所在的代码块内有效。

const 声明一个只读的常量，一旦声明，常量的值就不能改变。

3.解构赋值

解构赋值是对赋值运算符的扩展，他是一种针对数组或者对象进行模式匹配，然后对其中的变量进行赋值。

4.展开符/三点运算符(...)

用来取代 arguments 但比 arguments 灵活.

![img](https://gitee.com/xxgw1997/Web-2/raw/master/12-ES6/images/9.png)

5.async 函数

async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。

```js
async function helloAsync(){
    return "helloAsync";
  }
console.log(helloAsync())  // Promise {<resolved>: "helloAsync"}
helloAsync().then(v=>{
   console.log(v);         // helloAsync
})
```

async 函数中可能会有 await 表达式，async 函数执行时，如果遇到 await 就会先暂停执行 ，等到触发的异步操作完成后，恢复 async 函数的执行并返回解析值。

