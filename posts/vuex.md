---
title: vuex
date: 2020-07-19 20:51:17
tags: vue vuex
---

Vuex 是一个专为 大型Vue.js 应用程序开发的**状态管理模式**

1.简单来说，vuex就是用来集中管理组件的数据的(多组件共享的数据)。

**数据驱动视图，动作(action)更改数据**，是一个环形单向数据流。

每一个 Vuex 应用的核心就是 store(数据仓库)。

2.vuex需要安装

```
npm install vuex -S
```

3.在vue应用的下面会多出来一个store的文件夹，在里面使用vuex。首先会在里面的index.js里面创建仓库对象。

```js
export default new Vuex.Store({ //data
state: {
    num:0
  },
 getters,
 //methods,在mutation里处理状态
  mutations: {
    addnum(state){
      state.num++;
    }
 //异步方法
 actions,
 //模块
 modules: {
  buyCar
 } })
```

在state里面添加全局数据，在mutation里面添加全局方法，修改数据。组件想要调用此方法，需要使用this.$store.commit('addnum');进行调用。通过$store.state.num获得数据。

**4.state**

在组件里的应用，一般放在computed里面当做一个函数，返回一个在state里面的数据，如

```js
msg:function(){return $store.state.num;}
```

​		4.1 mapstate

简化我们获取数据的方式，mapState的作用就是返回一个对象，这个对象可以直接丢给computed

```js
import { mapState } from 'vuex'//需要引用
   // computed: {
    //     userList: function() {
    //         return this.$store.state.userList;
    //     },
    //     goodsList: function() {
    //         return this.$store.state.goodsList;
    //     }
    // }
computed: mapState({
        title(state) {
            return state.title;
        },
        userList(state) {
            return state.userList;
        },
        goodsList(state) {
            return state.goodsList;
        }
    }),
```

如果state上有某个属性，可以直接赋值：

```js
computed: mapState({
    title: 'title',
    userList: 'userList',
    goodsList: 'goodsList'
}),
```

如果mapState属性的名字和state中属性的名字相同的话，就可以采用下面更简单的写法：

```js
computed: mapState(['title', 'goodsList', 'userList'])
```

有些复杂数据还是推荐使用第一种。

**5.getter**

相当于store的一个计算属性，就是对state的数据进行计算，当组件需要取到state的属性然后进行计算得到想要的结果的时候，计算的过程可以在`getters` 中进行，组件从getters中就可以直接拿到计算好的值。如果所有组件都需要这个计算的话，那就方便多了。

```js
getters: {
  // ...   ，可接受其他getter作为参数
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length;
    //return state.num++;
  }
}
```

Getter 会暴露为 `store.getters` 对象，你可以以属性的形式访问这些值

```js
computed: {
    ...mapState(['title', 'userList', 'goodsList']),
    // 直接调用getters的countAge属性即可
    allAge() {
        return this.$store.getters.countAge
    }
},
```

​	5.1 mapgetter	

如果你想将一个 getter 属性另取一个名字，使用对象形式：

```js
 ...mapGetters({ allAge: 'countAge' }),
    // allAge() {
    //     return this.$store.getters.countAge
    // }
```

**6.mutation**	更改 Vuex 的 store 中的状态的唯一方法是提交 mutation

当需要在store里面操作数据的增删的时候，需要使用mutation属性。

可以在mutation里面添加方法，进行修改数据

```js
mutations: {
        addUser(state) {
            state.userList.push({ userName: 'aaa', age: 10 			});
	}
}
//在组件里也使用进行调用。
methods: {
        addUser() {
            // 使用commit的方式调用
            this.$store.commit('addUser');
        }
    },
```

**我们在commit的第二个参数传递数据。这里有个专业的术语叫做【载荷】，只能有一个**

```js
 mutations: {
        // data接收commit的载荷
        addUser(state, data) {
            state.userList.push({
                userName: data.name,
                age: data.age
            });
        }
    }
    //组件里面
    methods: {
        addUser() {
            // commit的第二个参数填写传递的载荷
            this.$store.commit('addUser', {
                name: this.name,
                age: this.age
            });
        }
    },
```

​	整个应用程序，只有mutations才可以操作state状态。

​	但是注意：

​	**mutations中的属性，必须为纯函数，必须为同步代码。**

​	纯函数就是传入相同的参数，得到相同的结果。

​	同步代码就不能是异步的，比如ajax，比如setTimeout等。

**7.action**

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

*能操作state的只有mutations，actions也不行。只能调用mutations去操作state。*

在action里面可通过commit调用mutation你的数据操纵方法，在组件你的methods里面使用dispatch调用action里的方法

```js
addUserTimeout() {
        // 使用dispatch调用actions的属性
        this.$store.dispatch('addUserTimeout', {
            name: this.name,
            age: this.age
        });
    }
```

**总结mutations与actiosn的区别：**

​	1、commit方法用于调用mutation；dispatch 方法用于调用action；

​	2、mutation 函数必须是纯函数，而且不能有异步代码；action 可以不是纯函数，也可以有异步代码；

​	3、按照上述规则，可以用mutation完成的事情，可以直接调用mutation，mutation不能实现的事情丢给action来完成。

​	4、在action中，当完成异步操作，最终需要修改数据模型时，还是需要通过mutation来完成对数据模型的操作。action不允许直接操作数据模型。

**8.module**

Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter。

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}
//需要在index.js里导入，上面两个是一个各自单独的文件
const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

##### 	8.1命名空间

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名，使用的时候会使用这个命名

```js
export default {
    // 开启命名空间
    namespaced: true,
    state: {
        title: '我是首页',
    }
}
```

