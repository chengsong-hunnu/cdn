---
title: vue学习(二)
date: 2020-07-13 09:05:38
tags: vue 老陈
---

#### vue的B站老陈学习

那些基本的在前一篇大多都说了，这一篇记一些没说到的。

1.过渡动画，一般使用transition完成。例如

```html
<transition name = "nameoftransition">
   <div></div>
</transition>
```

需要指定这个动画的名字。

**重点是css里面的css类名和css动画效果**

一般是动画名(例如fade)再加一些特定标识，如

```css
.fade-enter-active, .fade-leave-active {
    transition: opacity 2s
}
.fade-enter, .fade-leave-to  {
    opacity: 0/*opacity表示透明效果*/
}
```

active表示动画的进行时间，enter和leave-to表示刚进入的效果和最终离开时的效果。还有enter-to和leave这两个表示动画最后的效果和离开开始的过渡状态。

[^菜鸟教程]: 来自菜鸟教程

<img src="https://www.runoob.com/wp-content/uploads/2018/06/transition.png" alt="img" style="zoom:50%;" />

还可以使用自定义的类名，可以和第三方css类插件库(如:animate.css)进行配合。

如：

```html
<transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
>
<div></div>
</transition>
```

此外还有enter-class,leave-class,enter-to-class,leave-to-class.与上面相对应。

过渡时间你也可以在transition里面是绑定duration进行设置。如：

```html
<transition :duration="1000">...</transition>
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```

2.生命周期

[^来自vue官网]: 来自vue官网

<img src="https://cn.vuejs.org/images/lifecycle.png" alt="Vue å®ä¾çå½å¨æ" style="zoom:33%;" />

我们能拿到的只有红色框里面的东西，中间的都是vue自动完成，我们无法控制。一般直接在vue实例里面添加这些东西进行控制。如：

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {//进行控制.
    console.log('a is: ' + this.a)
  }
})
```

在beforeCreate里面输出this和vue里面的data会有不同，this表示vue实例刚创建，但data里面的数据还没有绑定，会输出undefined，同时methods这些也没有绑定

在beforeCreate和created这中间就是绑定data和methods这些。

created之后就是渲染页面，在渲染之前绑定的一些数据在vue实例里面不会拿到，虽然在浏览器html里面有了。在渲染之后(mounted)就可以拿到这些dom对象。

当数据被修改，也会有两个阶段，在before阶段，虽然数据修改了，但内容还没渲染修改，在updated后，就会重新渲染，内容也改变了。

销毁一般很少调用。

3.组件感觉还有一些东西要写。

prop传递数据（子组件用来接受父组件传递过来的数据的一个自定义属性）：

```html
<div id="app">
    <child message="hello!"></child>
	<input v-model="parentMsg">//动态prop传递数据
      <br>
      <child v-bind:message1="parentMsg"></child>
</div>
 
<script>
// 注册
Vue.component('child', {
  // 声明 props
  props: ['message','message1'],
  // 同样也可以在 vm 实例中像 "this.message" 这样使用
  template: '<span>{{ message }}+{{message1}}</span>'
})
</script>
```

自定义事件（子组件要把数据传递回去，就需要使用自定义事件）

```html
<div id="app">
    <div id="counter-event-example">
      <p>{{ total }}</p>
      <button-counter v-on:increment="incrementTotal"></button-counter>
      <button-counter v-on:increment="incrementTotal"></button-counter>
    </div>
</div>
 
<script>
Vue.component('button-counter', {
  template: '<button v-on:click="incrementHandler">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementHandler: function () {
      this.counter += 1
      this.$emit('increment')//触发increment事件,在v-on监听里面触发，调用vue实例里面的函数
    }
  },
})
new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
</script>
```

component里面的data必须是一个函数，为了保证每个组件的数据不共用。注意驼峰命名法在html里面需要转换成横线格式。

同时有$parent,$children,$root属性，可以在组件中找到父元素的vue对象，子元素的，根(最外层对象)的这些，之后就可以当做vue对象进行调用方法之类的。

v-model也可以绑定到组件上，默认会利用名为 `value` 的 prop 和名为 `input` 的事件，需要在组件的template里面添加相应监听和$emit()调用。

**一般使用.vue分开使用组件的话**

**父组件给子组件传递数据：在父组件的子组件标签里面通过v-bind绑定一个<u>特定名</u>和数据。在子组件里使用props声明这个<u>特定名</u>，然后就可以使用这个特定名进行数据使用。**

**子组件给父组件传递数据和使用父组件的方法：**

​	**1.在父组件的子组件标签里面通过v-on监听一个<u>特定名</u>和一个在父组件里面的methods的方法名，在子组件里面自定义一个点击事件之类的，触发在子组件里面自定义的方法，在这个方法里面通过this.$emit('<u>特定名</u>','数据')，就会通过这个特定名调用父组件里面的方法，数据也作为参数传递了过去。**

​	**2.在父组件里通过v-bind绑定一个<u>特定名</u>和<u>父组件方法</u>，在子组件里通过props声明这个特定名，然后就可以把这个<u>特定名</u>当做一个方法进行使用**

4.插槽。整体结构不变，但要插入一些内容。<slot></slot>,新版本(2.6.0)可使用v-slot。一般来说，在自定义组件的标签里面添加内容，是不会被渲染到页面上去的，但slot解决了这个问题。<slot>在组件的template里面添加使用，添加之后，当在组件标签里面添加内容时，slot将会被替换掉，内容可被渲染到页面上。一个组件的template最外层只会有一个标签，一般可使用一个div将所有元素进行包裹。

**父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。**

5.动态组件。

```html
<component v-bind:is="currentTabComponent"></component>
```

可使用is属性切换不同的组件。

is用法。其中currentTabComponent是在vue实例里面进行数据定义，然后在局部定义组件和方法进行切换currentTabComponent，在this.$options.components[id]，this表示vue实例，获取到组件实例，之后将component标签替换成这个组件。

一般切换的时候，都会重新渲染组件。当我们不想让其重新渲染的时候，就可以使用<keep-alive>标签。如

```html
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

 keep-alive要求被切换到的组件都有自己的名字，不论是通过组件的 `name`选项还是局部/全局注册。

因为一些标签 有一些限制出现的地方，如<li>，为了避免这些情况，在组件中的template使用**单文件组件(.vue文件)**就不会存在这些限制，这也是重点。

6.**单文件组件(.vue文件)**

脚手架。.vue文件解决了**全局定义** **字符串模板** **不支持组件 CSS**  **没有构建步骤**这些缺点