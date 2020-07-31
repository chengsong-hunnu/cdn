---
 title: vue学习-三
date: 2020-07-13 17:52:29
tags: vue .vue 脚手架(vue-cli)

---

##### 单组件文件(.vue)

1.解决了**全局定义** **字符串模板** **不支持组件 CSS**  **没有构建步骤**这些缺点，使关注点分离，模块化开发。

2.vue-cli	vue.js的[标准开发工具](https://cli.vuejs.org/zh/guide/prototyping.html)

3.安装vue-cli按照官方文档的命令进行就可以了，不要其他的不加@符号的命令，在vscode终端里面目前是需要3版本以上。推荐使用git-bash进行npm下载或者cnpm下载。如果出现了什么命令没找到，注意添加安装路径到环境变量。如果安装好了，在bash里面vue --version没有问题，但在vscode终端里无法使用，将vscode设置成管理员打开。vue ui可以进行更简易化的创建配置操作。

4.在src目录里面开发，使用npm run serve看效果，npm run build打包到dist文件夹，可将dist文件夹部署到服务器，当做平常网站。注意绝对路径(/)，和相对路径(./)

5.将文件模块化，明确文件关系

**6.app.vue文件**。核心文件，在template里面写html代码，script里面写js，style里面写css。注意全局使用驼峰命名，防止一些奇怪错误。

```vue
<template>
  <div id="app">
    <chat-com></chat-com>
    <userlist-com></userlist-com>
  </div>
</template>

<script>
import chatCom from './components/chatcom'
import userlistCom from './components/userlistcom'

export default {
  name: 'App',
  components: {
    chatCom,
    userlistCom
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

使用import引入自定义组件，并在export default的component里面声明，里面的**数据data是返回一个对象**.





报错和解决方法：

1.Newline required at end of file but not found eol-last

在.vue文件的最后一行后面加一个空行，还只能加一个空行，多了不行

2.miss什么什么的，在miss的地方加空格。好吧，这是最考试创建的时候开启了eclint选项，是一个严格语法结构。不能使用tab，空格多了两个。

3.推荐使用vscode的格式化文档(代码格式化为：alt+shift+f)，再对照错误进行修改，字符串使用单引号。