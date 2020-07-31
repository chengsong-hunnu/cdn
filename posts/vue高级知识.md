---
title: vue高级知识
date: 2020-07-15 11:36:44
tags: vue socket less route
---

##### B站老陈

1.socket.io(套接字),用于浏览器和服务器进行实时，双向，基于事件(异步)的通信。

socket. emit('名','数据'),发送客户端数据。socket.on('名',function(data){})监听客户端或服务器发送的内容

服务器与客户端之间的连接以一个唯一的id进行标识，当一个新的用户浏览器进行与服务器的连接时，需要进行广播(sockets.emit())，将此连接的id广播给其他客户端，方便进行数据交流,使用socket.to(目标id).emit('名',{数据})

命名空间，用以区分。通过socket.join和leave可以加入和离开房间

2.vue实现原理

首先vue可以当做一个构造函数，传入一个参数为对象。

3.less。css扩展语言，预处理器。让其可重复使用，但浏览器不支持，需要编译转化为css。使用

```
lessc 源文件.less 目标文件.css
```

将less文件编译成css文件。

使用@变量名 = xxx,定义全局可使用的变量。可混合带参数使用，直接在一个class里面引入另一个class名，即可将样式复制过来，参数也可在样式里面当一个变量。参数需要@符号声明。参数可以使用一个默认值，当没有参数传入时，就使用默认值。@_匹配模式，相当于匹配一些class，再加上一些样式。还可对变量进行计算可进行样式直接嵌套(不建议使用嵌套)

可使用@argument将所有的默认变量传入。

```
.border_arg(@width: 1.75rem, @color: #e6e6e6, @style: solid) {
    border: @arguments
}
```

在vue里面使用less：

需要在sytle标签里面设置lang属性为less。lang="less"。

4.router路由

根据不同的路径显示不同的页面。在router里面的index.js里面配置不同的路径显示的不同的组件或者页面。根据选择的<router-link>替换<router-view/>来显示内容。在本质上还是一个页面。

所有的核心配置都在index.js里面

```js

const routes = [
  {
    path: '/',
    name: 'home',
    //component: Home
    components:{
      nav:navView,
      aside:asideView,
      default:Home
    }
  },
  {
    path:"/a",
    //redirect:"/about"
    redirect:(to)=>{
      console.log(to)
      if(to.query.go=='about'){
        return {name:"about"}
      }else{
        return {name:"news",params:{id:456789}}
      }
      
    }
  },]
```

通过path，name，component设置。

我们可以在任何组件内通过 `this.$router` 访问路由器，也可以通过 `this.$route` 访问当前路由

**动态路由**，把某种模式匹配到的所有路由，全都映射到同个组件。在path里面可使用通配符 `*`进行匹配任意路径，一般用于404页面。当使用一个通配符时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过通配符被匹配的部分

```js
 routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
  //通过$route.params.id可获得路径后的id参数值
  const User = {
  template: '<div>User {{ $route.params.id }}</div>'
	}
  {
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
  }	
```

可使用watch进行监听路由发生的变化

```js
watch: {
    $route(to, from) {
      // 对路由变化作出响应...
    }
```

**嵌套路由**

```js
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```

使用children属性进行嵌套的配置。还可以继续进行嵌套。

**编程式导航**(使用点击事件在js的mehtods里面使用)，除了使用 `<router-link>`，还可使用

`router.push(location, onComplete?, onAbort?)`进行导航。使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

```js
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

**注意：如果提供了 `path`，`params` 会被忽略，params将以path里面的为准**

router.go(n)这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步

**命名路由**

通过一个名称来标识一个路由

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})

<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
//这跟代码调用 router.push() 是一回事：
router.push({ name: 'user', params: { userId: 123 }})
```

**命名视图**

同时 (同级) 展示多个视图，而不是嵌套展示，你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。之前router-link时，只有一个router-view进行替换，只会有一个视图，现在可通过命名视图将几个视图何在一个页面上。

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。需要使用components进行配置，默认为default。也可配合嵌套路由，在childern里面使用命名视图。

```js
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>

const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

**重定向和别名**

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
重定向的目标也可以是一个命名的路由：
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
甚至是一个方法，动态返回重定向目标：
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```

​	`	/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。

**路由组件传参**

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。使用 `props` 将组件和路由解耦：

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})

//组件传参props
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

在组件里也使用props将传入的数据拿到，进行数据显示。

**路由守卫**

简单来说就是路由在跳转之前的验证，当满足条件时才会进行跳转。分为`全局守卫`，`路由守卫`和`组件守卫`

​	1.注册全局守卫应该在路由模块暴露出去之前定义，所有的路由跳转都会被调用使用`router.beforeEach(function(to,from,next){})`来注册一个全局守卫。

参数：

- to：代表目标路径对象

- from：来源路径对象

- next：用于决定是否继续进行跳转。

  当next()函数不传参数或者传入true的时候 则允许正常跳转；

  当next()函数传入false时 会中断跳转(阻止跳转)；

  当next()函数中**传入路径**时或者**对象**时(比如:{name:’xxx’})则会重定向到指定路径。

  2.路由守卫就是针对单个路由对象配置的守卫。

假如我在users组件配置路由守卫，那么只有跳转到users路由时才会触发该守卫，跳转到其他路由时不会触发该守卫。

路由守卫的注册写在路由匹配规则数组里面：

```js
let router = new Router({    
    routes: [        //...        
        {path: '/users',            
         component: Users,            
         name: 'u',            
         beforeEnter: (to, from, next) => {                					next(confirm('Entey Users?'));           
                                          }}]});
```

​	3.组件守卫是针对单个组件进行监听，在访问到该组件时才会触发。

​	写在组件里

```js
export let Home = {
    template: `
        <div>
            <h1>首页</h1>
            <router-view></router-view>
            <router-view name="b"></router-view>
        </div>
    `,
    // 在路由跳转时,如果会访问到当前组件,则会触发该守卫
    beforeRouteEnter(to, from, next) {
        next(confirm('Enter Home?'));
    },
    // 在路由跳转时,如果离开当前组件,则会触发该守卫
    beforeRouteLeave (to, from, next) {
        next(confirm('Leave Home?'));
    },
    // 在当前路径下,当路由的参数发生变化时，才会触发该路由守卫
    beforeRouteUpdate(to, from, next) {
        console.log(to.params.path);
    }
}
```

**路由元信息**

路由的特有属性，写在meta:{ }里面，为后面的组件提供一些特定信息进行逻辑判断。

**过渡动效**

用 `<transition>` 组件给它添加一些过渡效果

```js
<transition name="slide" mode="out-in" 
enter-active-class="animated bounceInLeft" 
leave-active-class="animated bounceOutRight">
      <keep-alive>
        <router-view/>
      </keep-alive>
      
    </transition>
```

**数据获取**



**滚动行为**

当创建一个 Router 实例，你可以提供一个 `scrollBehavior` 方法

```js
scrollBehavior (to, from, savedPosition) {
  if (savedPosition) {
    return savedPosition
  } else {
    return { x: 0, y: 0 }
  }
}
```

**路由懒加载**

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了