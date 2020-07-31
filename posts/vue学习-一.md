---
title: vue学习(一)
date: 2020-07-11 17:12:23
tags: vue
---

##### vue的黑马学习

1.vue作用范围，只有在vue实例的el挂载点里面及其后代元素才会进行这个vue实例的操作。不仅可以使用id选择器，class选择器也行，使用.class进行el挂载。其他的选择器也都能行。可以使用其他的双标签，但不能是body和html标签。

2.Vue中用到的数据定义在data中，data中可以写复杂类型的数据，渲染复杂类型数据时,遵守js的语法即可

3.dom操作是获取元素，操作它们。vue使用指令操作

4.v-text指令。直接替换标签里面的所有文字和数据。跟两个大括号没什么差别。

5.v-html，设置标签的innerHTML，就是将纯html文本解析成html效果。

6.v-on，为元素绑定事件，类似onclick等。v-on:click="函数名",函数在vue实例的methods里面定义，methods:{click:function(){ }, other:{ } }。同时，在函数名后面可以传递参数，相应在定义函数时也要添加这个参数。还可以使用(.)表示的指令后缀调用修饰符。如@click.once="",只会调用一次，@keyup.enter=""这些。

7.v-show,根据表达值的真假，切换元素的显示和隐藏。本质就是切换display属性。

8.v-if,v-if="值",根据值的真假，判断是否显示，但不是操纵display，而是直接操纵dom元素。

9.v-bind,设置元素的属性。v-bind:属性名="属性值"，一般属性值在data里面进行定义。

10.v-for,生成列表。格式v-for="item in items"，在data里面对items进行数据设置，一般是数组或者对象或者对象数组。使用对象时，可使用(value,key,index) in object 对对象进行数据访问。

11.v-model，获取和设置表单元素的值(进行双向数据绑定)。就是表单中的数据发生改变时，另一个绑定的数据元素也会发生改变。一般用来获取输入文本框中的输入数据.

12.网络请求(axios).一般使用get和post

例如：

```js
axios.get("https://autumnfish.cn/api/joke/list?num=1")
					.then(function(response){
						that.joke=response.data.jokes;//这里不能通过this访问joke，this的范围已经发生了变化
						console.log(response.data.jokes);
					},function(err){
						console.log(err);
					})
```

