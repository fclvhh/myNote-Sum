# Vuex是什么?

> 状态管理工具



## 状态管理

状态 -->变量

共享的状态-->共享变量

共享变量放哪里合适呢?    放哪一个组件,大家都不服气

找一个大管家来管理 共享变量   

大管家 : 一个对象    共享变量 :  对象属性

组件可以操作  对象中的属性 :  获取/修改

变量响应式 : 当管家对象中的属性改变, 相关组件会响应式更新



以上可知:

Vuex就是一个对象,为什么不可以自己去封装呢?

比如:

![](assets\Vuex-1.jpg)

但是有一个致命的问题-—-无法做到响应式



Vuex就出场了!!!

可以做到响应式



## 哪些东西需要管理



# 状态管理

## 单页面状态管理

![](assets\Vuex-2.jpg)

朴素的写法就是了

![](assets\Vuex-3.jpg)

​	



## 多状态管理

Vuex插件的安装和使用

![](C:\Users\lenovo\Documents\一“桶”前端\myNote\vue\assets\Vuex-5.jpg)

Vuex的简单了解

![](assets\Vuex-4.jpg)



Vuex进行多状态管理的流程分析

![](assets\Vuex-6.jpg)

Mutations 负责处理同步代码,修改state的状态

Actions负责处理异步代码,将异步结果交给Mutations, 然后它负责修改state

Devtools是一个浏览器插件,根据Mutation负责管理state的修改记录,

知道当出问题的时候,就知道是哪一个组件修改了state造成了错误, 方便我们寻找错误的!

