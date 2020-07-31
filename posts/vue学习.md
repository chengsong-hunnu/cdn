---
title: vue学习
date: 2020-07-08 14:47:58
tags: vue
---

#### 关于自己对Vue学习的一些重点和理解

学习的地方包括Vue自己的网站，菜鸟教程，B站等

##### 1.vue对象中会有两个重要的属性，el和data，表示绑定的属性和数据。

##### 2.模板语法

插值：数据绑定：文本：{{数据}}, v-once；原始html：v-html，将html文本转换为html样式；作用于html 的attribute：v-bind,

指令：指令 (Directives) 是带有 `v-` 前缀的特殊 attribute,一些指令可以带参数。

动态参数：可以用方括号括起来的 JavaScript 表达式作为一个指令的参数。可以使用null字符串显式的移除此属性。

**`v-bind` 指令可以用于响应式地更新 HTML attribute，**

**`v-on` 指令，它用于监听 DOM 事件,**进行事件处理

##### 3.计算属性和侦听器

​	1.对于任何复杂逻辑，你都应当使用**计算属性**computed。

​	2.我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。只要值没有发生变化，就不会再次计算属性，直接返回上一次的计算结果。而方法会每一次重新渲染时都重新计算。

​	3.Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：**侦听属性** 

##### 4.class和style绑定

​	1.对象语法，使用v-bind:class指令。

​	2.绑定内联样式，v-bind:style指令。是一个js对象

##### 5.条件渲染

​	1.v-if条件性的渲染一部分内容，使用<template>进行包裹，再使用v-if可以渲染多个内容。还有配套的 v-else,还有v-else-if进行连续使用。

​	2.v-show的使用与v-if差不多，但没有v-else这些。同时因为各自特性，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

##### 6.列表渲染

​	1.v-for，使用item in items的格式执行。其中items需要在vue里面的data里指定数据。也可使用value in object进行对象循环，也可使用(value, name, index)进行对象里面的数据，键名，索引访问。也可以使用n in 10进行循环。类似于 `v-if`，你也可以利用带有 `v-for` 的 `<template>` 来循环渲染一段包含多个元素的内容。

##### 7.事件处理

​	1.v-on监听dom事件，并执行一些js代码。还可以接收一个需要调用的方法名称。在vue的methods里面添加方法。

​	Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

​	使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

​	2.监听按键事件时，可以使用按键修饰符，如：v-on:keyup.enter，只有enter键可以触发事件。

##### 8.表单输入绑定

​	1.v-model，创建双向数据绑定，一处数据发生变化，另一处也会发生变化。

​	`v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 `value` property 和 `input` 事件；
- checkbox 和 radio 使用 `checked` property 和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件。

​    2.文本，多行文本，复选框，单选按钮，选择框。

​	3.值绑定

​	4.修饰符，.lazy;.number;.trim

##### 9.组件

​	1.组件可以扩展 HTML 元素，封装可重用的代码，是可复用的vue实例。组件系统让我们可以用独立可复用的小组件来构建大型应用，几乎任意类型的应用的界面都可以抽象为一个组件树。**每个组件都只有一个根元素**

​	2.Vue.component创建组件，第一个参数是组件名称，第二个参数是以对象的实例描述一个组件。通过将组件名作为标签进行调用。**一个组件的 `data` 选项必须是一个函数**，多个组件的使用，其数据都是分开的，不会互相影响。

​	3.组件需要注册，Vue.component全局注册，先在js里定义组件，再在vue实例中通过component定义想使用的组件局部注册。

​	4.组件里使用prop向子组件传递数据，当做自定义的attribute传递数据。也可使用v-bind动态传递prop，且是单向的。

​	5.监听子组件事件，父级组件可以像处理 native DOM 事件一样通过 `v-on` 监听子组件实例的任意事件：

```
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

同时子组件可以通过调用内建的 [**`$emit`** 方法](https://cn.vuejs.org/v2/api/#vm-emit)并传入事件名称来触发一个事件：

```
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```

​	6.<slot>内容</slot>，在组件里面通过vue自定义的slot可以实现插槽效果。

​	7.动态组件，通过 Vue 的 `<component>` 元素加一个特殊的 `is` attribute 来实现。

##### 10.自定义事件

​	1.Vue.directive自定义指令，第一个参数是名字，或在vue实例中通过directives局部注册。会有几个钩子函数bind,inserted,update,componentUpdated,unbind.

差不多这篇文章就不想写了，看其他的吧

