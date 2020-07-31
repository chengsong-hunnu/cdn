---
title: vue总结
date: 2020-07-27 21:06:30
tags: vue
---

先简单的学习了一些vue的基础知识，先做一个基础的总结，免得知识有些混乱和忘记，只是稍微提一下，并给一个例子

1.**基础常用语法**

v-bind,v-on,v-model,v-for,v-if/v-show,

2.在vue实例里面，需要el挂载，data数据，methods方法，computed计算属性，watch侦听器侦听数据变化，

3.核心组件，专门建一个文件夹放置组件这些可复用的页面，在需要使用的时候，需要使用import引入组件。

组件里面包括<template></template>里面放置html代码，<script></script>放置js代码，<style></style>放置css代码。

还有组件之间传递值的方式。

4.动画，<transition> 需要动画效果的html代码</transition>

5.路由，根据路径的不同显示不同地页面组件。路由守卫，就是对路由的传递进行验证看是否满足什么条件

6.css预编译less。

7.vuex，对大型vue应用的数据进行统一管理。