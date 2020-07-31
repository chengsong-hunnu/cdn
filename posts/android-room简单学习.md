---
title: android room简单学习
date: 2020-07-07 16:42:17
tags: csdn room

---

### room

1.要使用room需要先声明依赖，[声明方法](https://developer.android.google.cn/jetpack/androidx/releases/room#declaring_dependencies)
**注意要在app模式下的build.gradle(module:app)里面添加依赖，不要在project里面的gradle里面添加。**

2.room由3个主要的组件，分别为
Database：数据库，
Entity：数据库中的表，
DAO：访问数据库的方法。

3.room可以使用一个Java类文件当做数据库的表，只需使用@Entity注解。
里面有几个主要的注解使用
**也可以在@Entity注解后面使用tablename声明表的名称**
 **@PrimaryKey(autoGenerate = true) // 设置主键，并且自动生长**
 **@ColumnInfo(name = "true_name") // 设置别名，如果不设置就默认为变量名**
 在其中可以定义需要的变量且对其中每一个变量都需要写出其set方法和get方法。

 4.之后我们使用DAO来对数据库进行访问
 同样我们需要使用一个Java接口类文件，同时使用@DAO 注解
 其中里面有4个主要的注解进行使用
 **@Insert 表示插入记录。
@Update 表示修改数据库中的记录。
@Delete 表示删除数据库中记录。
@Query(" ") 中可以写入SQL语句，来执行操作。**

5.创建一个Database，同样要使用@Database注解，且在其中需要指明entities和version
**Database 文件必须要继承 **androidx.room.RoomDatabase**，并且得是抽象类。**


之后就是建立一个布局，将数据库里面的数据显示出来，并与DAO的几个操作通过点击进行交互。

[主要代码在这里](https://github.com/chengsong-hunnu/local-github/tree/master/room)

主要借鉴了[萌宅鹿](https://blog.csdn.net/weixin_43734095/article/details/100182369?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1)和[android studio官方](https://developer.android.google.cn/training/data-storage/room)