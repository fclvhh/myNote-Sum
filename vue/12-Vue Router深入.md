# 动态路由

〖动态路径〗

①让router数组的对象里的 path属性是可变的

②思路: ⅰ 在路由中,配置路径值时,搞一个占位符  ⅱ 在router-link组件中 为to属性动态绑定

![](assets\Vue-router-路由表的实现8.jpg)

③「$router」和    「route」的区别 : router代表routes数组,         route属性代表数组里面的**当前对象**,也就是当前组件

![](assets\Vue-router-路由表的实现9.jpg)







# 路由懒加载

〖路由懒加载的起因〗

①SPA的思路就是 「资源url==>获得所有的静态资源」 

② 但是 **静态资源是非常多的** 包含了   根组件和所有的子组件   下载到浏览器的缓存中太费时间了, 用户体验不好

③静态资源在webpack打包时,打包成了一个  「庞大」js文件     也就是**每个组件都打包到了一起**

④因此 进行了拆分    〔根组件.js〕和 〔其他组件,js〕



## 路由懒加载机制

1. 只加载  **root组件**所对应的 js文件 和   首页对应的 **home组件**
2. 当 页面发出了指令 ,再去加载对应的 页面组件
3. 这就是懒加载



## 懒加载代码实现

1. 懒加载  加载的是什么?   「 是组件」
2. 现在就是要告诉  浏览器   「哪些组件是懒加载的?」
3. 在router文件夹下的index.js中      **导入时声明子组件是否懒加载**
4. 声明格式   `const 组件名 = ()=>import("组件路径")`



**根组件里面就不再需要导入组件了,只需要导入路由组件,路由组件在配置映射时,导入相应的组件就好了**

![](assets\Vue-router-路由表的实现10.jpg)





# 路由嵌套

〖概念解释〗

①学习到现在,我们在「根组件」通过 ⒒ router-link 完成了组件页面的跳转    ⒓通过router-view 确定了页面的展示位置

②背后是 路由 routes数组 和 数组里的对象支持的

③这个体现到「浏览器的地址栏」里  : /xxx –>页面a  ;  /yyy –>页面b

④**根组件和子组件的关系 是可以复制的**      形成的「树型关系」就称为 路由嵌套





## 路由嵌套实现

- ①路由映射关系的嵌套
  - children属性   属性值是一个数组 「等价于 routes」
  - 映射对象的path属性值   必须纯粹「不可以加 '/'」
- ②子组件充当根组件的角色   写router-link组件  和 router-view组件
- ③孙子组件 template的书写
- ④依然可以默认路由哦  「战场由 routes ==>children」



**路由配置的实现**![](assets\Vue-router-路由嵌套1.jpg)



## 子组件模板书写

①router 的index.js 导入模板「一般懒加载导入」 , 孙子组件导出模板

②配置路由

③子组件 导入 router组件, 并且用 router-link 和router-view 标签完成template







# 组件之间url信息传递



〖信息传递方式〗

①动态路由方式传递 params

- 动态路由   路径格式   /home/:xx
- 传递的方式 : 在path后面跟上对应的值
  - **对应的值获取**
  - 核心 :  属性绑定获取组建的data
  - ⑴ router标签的 to 属性 绑定data
  - ⑵ 根据「 :xx 」路由占位语法 ,完成数据传递, 路由映射生效

②query的key传递

- 本质 : to属性绑定对象语法的升级
- query选项 来自于「url的query参数 」
- query属性的属性值是一个对象



## url的query参数绑定

㈠**点击 router-link跳转时, url的query参数 就由 to属性绑定的对象中的query属性获取**

![](assets\组件参数传递.jpg)



**㈡依然要提前配置好路由映射 : ur是路径  但是映射的组件也要配置好**

![](assets\路由.jpg)



## url的path绑定

就是动态路由知识

  



# $route 和   router

是路由功能的底层实现

用这两个router对象,就可以用js代码,手动实现上述功能

比如动态路由中 第③步实现

**配合事件绑定机制,调用回调函数,执行js代码大概实现部分功能**





# 导航守卫

> 监听组件的跳转  在跳转时,完成部分功能
>
> 如 : 让title 变成 对应组件的名字



〖**业务实现**〗

![](assets\路由2.jpg)

![](assets\路由3.jpg)



## 路由守卫细节分析

① beforeEach()

② mata 「路由对象属性」

![](assets\路由4.jpg)



- 需要传入一个回调函数
- 回调函数有三个参数  
  - to : **待跳转路由对象**
  - from : **当前路由对象** 
  - next : 表示下一步 , 必须被执行调用





## 路由对象的声明周期

前置钩子函数   beforeEach

后置钩子函数 afterEach()  不用next  

![](assets\路由5.jpg)

![](assets\路由6.jpg)







# keep-alive

> 作用: 缓存Vue的「组件对象」

〖解释〗:

实际开发中, 用户操作页面时 , 会「频繁」的进行页面跳转 !

① 从Vue的组件对象来  思考页面跳转的流程

- 点击  跳转去别的页面组件
  - 如: home - ->about
  - home组件的Vue对象会被destroy
- 点击  跳转会home页面组件
  - 会创建一个新的home组件的Vue对象

- 这样感觉消耗性能比较多

② keep-alive是一个组件  , 用法如下

- 在App.vue中 包裹 router-view 组件
- 之后所有的在对应位置展示的组件的Vue对象都会被缓存



## 判断Vue对象是否被反复创建

〖原理〗:

Vue对象的声明周期函数  

重难点如下 :

1. 组件的创建  〔组件在Vue中就是一个Vue的对象〕   「created()」 

2. 模板挂载到DOM中   〔template模板挂载到页面相应位置〕 「mounted()」
3. 页面的刷新   〔模板的双向绑定  **响应式** 的数据改变〕会导致刷新   「updated()」

我们可以在组件构造器中, 添加created选项, 打印i信息,用来判断是组件对象是否被重新调用了



## 具体实现

![](assets\路由7.jpg)

![](assets\路由8.jpg)

![](assets\路由9.jpg)



## keep-alive细节

![](assets\路由10.jpg)



**小结:**

1. 只有在使用了keep-alive组件的情况下, 才可以使用 activated  和 deactivated 属性
2. exclude 用的比较多 , 用来去除不需要被缓存Vue对象的组件
   1. 用法: excuted="xx,yy,zz"
   2. 作为keep-alive标签的属性
   3. xx,yy,zz为组件的name
   4. 中间不可以出现 空格





# TabBar实现

## 基础试探   

⒈![](assets\Tab-bar1.jpg)

⒉![](assets\Tab-bar2.jpg)

⒊![](assets\Tab-bar3.jpg)



## 抽离子组件

⒋![](assets\Tab-bar4.jpg)

⒌![](assets\Tab-bar5.jpg)

⒍![](assets\Tab-bar6.jpg)



## 子组件slot

⒎![](assets\Tab-bar7.jpg)

⒏![](assets\Tab-bar8.jpg)



## 🎈组件效果

① 活跃效果和没点击的效果

- 类似于 超链接的的状态, 不同的状态有不能的样式
- 活跃效果展示图片1 , 非活跃展示图片2 

解决思路: 

在设计组件时,就考虑到这一点

- 所有可能用到的资源都应该用插槽占位
  - 而不是用到的时候,再去搞
- 在组件内部用代码决定什么时候展示

![](assets\Tab-bar9.jpg)



p121-p124 四节 感受到基础不牢

小结:

目前来说,架子算是搭建好了,就是用Vue在各个组件实现功能的不熟练

基本知道怎么回事,但是记忆不住,不会写



但是时间问题,把下面的知识学完,之后一点一滴的逐步解决!!!





## 路由功能

### 前置

```shell
npm install vue-router --save
```

Vue.use() 使用插件

touter文件夹  index.js



### 目录

view 文件夹装页面组件

home子文件夹, 用来装 首页相关的页面组件



配置路由映射对象  path  componnet

注意懒加载 + /路径的 重定向





### 🎈跳转

main-tabber 组件 作为 tanbar 和 tabbar-item的根组件

抽离的意义是为了简化App根组件

我们在 main-tabbar 组件 具体实现tabbar功能



①点击跳转

事件触发的回调函数内部的代码

```javascript
this.$router.replace(path)
```

执行这句代码可以让改变path, 路由根据映射跳转到对应的组件页面



如何拿到path?

main-tabbar 作为一个内容根组件   传递path 给子组件

自然想到:

子组件 props 拿到path 

父组件  通过属性  传递path

由于path是写死的, 不用绑定属性从data里拿 , 直接属性写死 , 就可以拿到了

```
<tab-bar-item link="/xx" >
//个性代码
</tab-bar-item>
```





## tab-bar-item 的颜色控制

选中高亮功能

思路:

点击一个item时候,跳转时 , item变色

⒈icon变色

两个img,控制展示那一张

采用v-show 命令   true 展示 , false不展示

⒉text变色

绑定style



解决问题的核心关键在于,**如何判断跳转完成了**

```javascript
this.$route.path.indexOf(this.link) !== -1
```

拿到当前路由的 路径

和父组件传下来的 路径比较  就可以得到最重要的布尔值了



用计算属性



⒊v-show所需要的布尔值拿到了

style计算属性实现

```javascript
return this.isActive ? {'color': 'red'} : {}
```

如果用methods

则是动态插入类命到class中, 让对应的类选择器生效的套路

而style 可知赋值样式 , 并且优先级很高 , 用计算属性确实更加的合适



## 提高封装

![](assets\Tab-bar13.jpg)

![](assets\Tab-bar12.jpg)

![](assets\Tab-bar11.jpg)

