---
title: typescript学习
date: 2020-07-29 23:59:00
tags: typescript
---

typescript学习,下面简称ts

typescript是js的一个超集，添加了一些新的扩展.主要目的是为了开发大型应用。

- 类型批注和编译时类型检查
- 类型推断
- 类型擦除
- 接口
- 枚举
- Mixin
- 泛型编程
- 名字空间
- 元组
- Await

2.使用**npm install -g typescript**安装进行使用，**tsc**是使用的标识，**.ts**是typescript文件扩展名。执行以下命令将 TypeScript 转换为 JavaScript 代码：

```powershell
tsc test.ts
```

3.TypeScript 程序由以下几个部分组成：

- 模块
- 函数
- 变量
- 语句和表达式
- 注释

每一行指令都是一段语句，可使用分号进行语句隔开，使合理。

4.TypeScript 面向对象编程实例：

```js
class Site { 
   name():void { 
      console.log("Runoob") 
   } 
} 
var obj = new Site(); 
obj.name();
```

编译后生成的 JavaScript 代码如下：

```js
var Site = /** @class */ (function () {
    function Site() {
    }
    Site.prototype.name = function () {
        console.log("Runoob");
    };
    return Site;
}());
var obj = new Site();
obj.name();
```

ts有些特殊的变量类型，如**any**，一般用于用户的输入，定义存储各种类型的数组。

```js
let arrayList: any[] = [1, false, 'fine'];//ts的变量声明格式
let myts: number = 10;
var uname:string = "Runoob"; 
arrayList[1] = 100;
```

