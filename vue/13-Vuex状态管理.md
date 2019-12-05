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

## 案例实现

⒈![](assets\Vuex-7.jpg)

⒉![](assets\Vuex-8.jpg)

⒊![](assets\Vuex-9.jpg)





# Vuex核心概念

## State单一状态树

说人话: 就是指创建一个store对象 , 换句话说就是单例

好处 : 

 多个系统分管信息,虽然信息得到了有效的分发 , 但是当想把信息整合时,就会发现要花费大量的时间,

去每一个小系统下载信息

这太麻烦了

单一系统, 没有这样的问题

![](assets\Vuex-10.jpg)



## Getters基本使用

相当于Vuex的计算属性

![](assets\Vuex-11.jpg)

![](assets\Vuex-12.jpg)

Getters用法深入

![](assets\Vuex-13.jpg)



动态设置过滤条件

![](assets\Vuex-14.jpg)

![](assets\Vuex-15.jpg)



## mutations使用

①基本使用

![](assets\Vuex-16.jpg)



②传递参数

![](assets\Vuex-20.jpg)

案例实现

⒈![](assets\Vuex-17.jpg)

⒉![](assets\Vuex-18.jpg)

⒊![](assets\Vuex-19.jpg)



③提交风格

![](assets\Vuex-21.jpg)

![](assets\Vuex-22.jpg)



④响应规则

state选项中的所有内容都会被加入到响应式系统,会被watcher监听是否发生了变化

带来的问题:

通过代码去添加内容时,由于添加的属性不是在state里面初始化的

故而是 做不到响应式的



解决:

Vue.set( ) 实现响应式

数组下标修改,不是响应式

对象代码**添加属性**,也不是响应式

都可以利用Vue.set解决

![](assets\Vuex-232.jpg)



删除对象属性也做不到响应式

Vue.delete()





⑤mutations的type类型定义成常量

将所有的type 抽离到一个文件中去, 用const 定义成常量

在store的index.js 或者App.vue 中使用时 , 就可以代码提示, 避免出错了

最好常量是大写的



⑥Mutation 要是同步函数

如果是异步函数, 那么Mutation对应Vuex浏览器工具无法监听



## Actions

①基本用法

理清 组件分发-->action 提交-->mutation调用     这样的设计顺序

![](assets\Vuex-23.jpg)

![](assets\Vuex-24.jpg)



逻 辑 :  异步代码需要在actions里面写,等异步时间到,需要回调时, 执行commit操作

​			跳转到mutation里面的代码 , 去执行实现

在App.vue绑定的methods的里面的方式时 ,  组件先提交给actions   dispatch

  

②Promise包裹异步

![](assets\Vuex-26.jpg)



![](assets\Vuex-25.jpg)



代码的核心思想

组件 dispath给action 

action执行异步代码, 内部用commit  让js引擎跳转到 mutations

执行mutations对应的 类型函数

用resolve() 跳转到then() 向组件传递 一些结果信息





actions是mutation处理异步的中转 , 所以他们的代码很想!!!





## module

解决单一状态树臃肿的问题

Vuex 准许将store分割成模块 :  依然单一状态树 , 因为每一个store都有唯一的state

每个module-xx都是一个完整的store



 ```javascript
const moduleA = {
    state:{
        
    },
    ...
}
 ```



store里面的module 负责

```javascript
module:{
    a:moduleA
}
```

a就变成了state里面的一个属性





我们在使用小store 或者说moduleA 里面的东西时,直接调用state/getters …. 就好了



因为在大的store,大家都是一家人

分割出来的小store只是为了解决大store 代码过长带来的臃肿问题



小store 会被挂载到大store中





# 项目结构

![](assets\Vuex-项目结果.jpg)