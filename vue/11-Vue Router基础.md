# 什么是路由?

**参考**![](assets\什么是路由.jpg)



## 首先搞明白什么是路由器

- 路由器: 接收数据包,转发数据包的机器
  - 两个核心功能
  - ①路由功能: 决定数据包  转发的路径
  - ②转发功能 : 把数据包转发给下一个处理者「可以是路由器,也可使服务器」
- 路由 :  决定信息传递的路径
  - 如何决定的?
    - 路由表 !    映射数据包和转发路径的一个**映射表**
- **路由的本质就是  映射关系**



## 编程领域为什么要借用路由的概念?

![](assets\引入路由概念.png)

**我们发现 : url和页面存在映射关系, 这种关系恰好和路由器中路由的概念一样** 

凡是对数据的处理都可以称之为 render :

- 服务器对数据的拼接 –-->后端渲染
- 浏览器对把内存的数据展示到视窗 –-->引擎渲染
- 前端通过js在浏览器的中完成数据拼接--->前端渲染

数据处理:

1. 拼接
2. 展示





# 后端路由

## 后端渲染

- 客户端发送请求到服务器
- 服务器解析url请求
  - 用java等后端技术  , 操作数据库 , 读取数据
  - 把数据 动态的 拼接到  css +html5的页面中 , 渲染成一个完整的页面
  - 这个过程就是后端渲染过程



## 后端路由

- 后端处理URL 和 页面之间的映射关系 ,  就称为 后端 路由
  - 前面可知 
  - 路由本质就是一个映射表
    - 网路中 , 映射` ip `和 mac 主机  ,   决定数据包传递给谁
    - 编程中 , 映射`url `和  数据 「页面」
      - 后端处理了数据 , 渲染成了页面
      - 把页面传递给客户端
-   ![](assets\后端路由.jpg)



- 前后端分离阶段
  - 数据通过ajax 请求api 接口获得数据
    - 后端只负责提供数据
  - 浏览器显示的大部分代码 , 都是前端写的`js`代码在浏览器中执行,最终渲染出来的页面
  - 后端处理![](assets\前后端分离.jpg)
  - 阶段分析![](assets\前后端分离2.jpg)

- 单页面富应用阶段「SPA : single page web application」

  - 在前后端分离的基础上,又加上了一层前端路由
  - SPA 
    - 整个网页只有一个html页面
    - 也就是说 , 静态资源服务器里面 , 一个页面包含的所有的静态资源

  - 和前后端分离阶段的区别

    - 前后端分离阶段 , 每个`url`对应一个页面 , 一个完整web项目 , 有多个url , 对应多个页面
    - SPA 阶段, 一个web项目 , 只有一个页面, 对应一个url
      - 这个页面一口气包含了所有的资源
      - 采用了懒加载 的策略 渲染数据

    

  - 前端路由
    - 负责从SPA 的所有资源中 , 抽取出对应部分资源 , 渲染到页面上
    - 在 Vue 中就是一个一个组件
    - 本质上:  组件的映射  [组件也可以是一个页面]   `url–>组件`
      - 就相当于把静态服务器给下载到了页面
      - 用户改变url 向页面 请求对应的组件
    - 管理这种映射的技术 - - - - - 前端路由

  

  [前端路由发展史mind](https://mm.edrawsoft.cn/map.html?obj=qqD062E720CDD5EFDAC9AE87117B50AC48/Personal/Vue/%E7%BB%84%E4%BB%B6/%E5%89%8D%E7%AB%AF%E8%B7%AF%E7%94%B1%E5%8F%91%E5%B1%95%E5%8F%B2.emmx)

# 前端路由

- 核心

  - 改变URL , 页面不会进行整体刷新
  - 具体 看  后端路由 SPA等内容

- 如何实现

  - url的hash
    - url的hash也就是锚点(#), 本质上改变window.location的href属性
    - 我们可以直接赋值loaction.hash来改变href, 但是页面不刷新

  

  ![](assets\hash.jpg)



- 
  - HTML5的history模式： pushState
  - 等等 ， 但是平时用的少  
  - 里面还有很多 api   可以自行   MDN history







# 安装和使用Vue-router

**参考**

 ![](assets\Vue-router-1.jpg)

## 总体思路

〖在模块化工程中使用〗    注重理解

- ① 创建router对象, 导出并挂载到Vue实例
- ② 导入组件, 用选项route , 配置路径和组件的映射
- ③ 在Vue根组件「App.vue」的 template模板中, 使用对应的跳转

〖实际开发中使用Vue-router〗

- ![](assets\Vue-router-21.jpg)

## router对象相关

- 安装Vue-router 插件
  - vue-cli 那里勾选就直接安装了
  - 下面的内容都是为了方便理解,人工演示的
  - 正常情况下,不需要自己弄
- 来到src文件夹
  - 新建一个router 文件夹
    - 新建一个  index.js
    - 在里面「配置路由信息」
  - 如何配置路由信息?
    - 一:导入VueRouter插件  和 Vue的js文件 (后面要用到)
    - ![](assets\Vue-router-3.jpg)
    - 安装插件
      - `Vue.use(VueRouter)`
    - 创建`VueRouter`实例对象
      - 和创建Vue的实例  过程类似
    - **把实例对象传入到Vue的实例中去**
      - 自然 , 自身要导出
      - Vue的实例要导入

==**图片过程**==

〖核心总结:〗: ①创建Vue-router对象  「在自身文件夹下的js文件创建」   ②导出   

​						③挂载: 把router对象导入,挂载到Vue实例中去

**前面的实现效果 1**![](assets\Vue-router-路由表的实现1.jpg)

**把router数组抽取出来** ![](assets\Vue-router-路由表的实现3.jpg)

**完成Vue实例挂载router对象**![](assets\Vue-router-路由表的实现2.jpg)

- 我们在routers 数组中 , 配置映射关系

  





## 配置路由映射关系

- routers数组中的一个对象 , 就是一个映射关系**「一个对象就是一个映射关系」**

- 这个对象里面 两个键值对  **「对象内部属性」**
  - path : “xxx”      「url太长,path表示相对地址」
  - component ; "xxx"
  - **两个属性完成路径和组件的映射**
  
- 具体完成过程图示![](C:\Users\lenovo\Documents\一“桶”前端\myNote\vue\assets\Vue-router-路由表的实现4.jpg)

  

  - VueRouter插件定义的全局组件`router-link-to `
  - 在根组件的template 里面使用
  - 这个`router-link-to`最终会被渲染成 a 标签的 

- `router-view` 占位符     决定 组件在什么位置显示



## 实现点击跳转功能

〖核心思想〗: 

①模拟a标签的功能,点击就根据src属性进行跳转

②「router-link 全局组件」代替了a标签

③to 属性代替了 src属性 ,跳转的也不是页面而是组件  「跳转到组件」

④「router-view全局标签」决定〔**组件展示**〕在什么位置

**图示**![](assets\Vue-router-路由表的实现5.jpg)





# 设置首页

**参考**![](assets\Vue-router-7.jpg)

〖如何设置首页〗

① 新建路由映射  ==>router数组里面添加一个对象

② 对象的属性: path 属性+ 「redirect属性」

③ path:"/"  默认首页路径, **重定向到home组件**



## 路径修改成history模式

-  在 index.js文件 `VueRouter`构造函数中 , 传入的选项对象中
  - 增加属性 「mode :  ''history’'」
  - mode :  `history`
- ![](assets\Vue-router-8.jpg)







# router-link   知识补充

![](assets\Vue-router-9.jpg)

![](assets\Vue-router-10.jpg)

实在想批量修改 , 可以在 router文件夹下的 index.js文件中 添加  选项对象中的 选项   linkActiveClass







# 通过代码方式实现路由跳转

〖代码实现跳转〗

①不再是用router-link组件

②利用js代码模拟 组件  「$router属性」

![](assets\Vue-router-路由表的实现6.jpg)









# 动态路由

- 创建一个组件 User 
- 在routers数组中 , 配置路由
  - ![](assets\动态获取.jpg)
  - ==格式:   `:xxx`   表示动态路径==, 后面的字符串 由属性绑定获取
- 动态绑定属性
  - ![](assets\动态获取2.jpg)
- 进一步 , 如果我还想要把 动态路由的路径 获取到页面中去, 该怎么办?
  - 通过`$route` 实例属性获取 路由对象 , 根据这个对象的属性 params 属性 获取参数
  - `$route` 和`$router` 的区别   有没有`r`的区别还是很大的, 注意
    - `$route`指的是 routes数组里面的一个路由对象
    - `$router`  值的是 routers数组里面的所有路由对象 
  - ![](assets\动态获取3.jpg)







# 认识路由的懒加载

- 打包文件结构
  - ![](assets\打包文件.jpg)
- 路由懒加载
  - 用到时 再加载
  - ![](assets\懒加载.jpg)
  - 小结:
    - app.hash.js的js文件过大 , 请求时间 可能过长 , 导致用户体验不佳
    - 我们分割这个js文件
      - 以组件的为模块 , 进行文件的分割
      - 需要时 , 再去请求加载 
      - 这样用户体验会好很多

- 懒加载语法 和 代码对比

![](assets\懒加载2.jpg)





## 路由懒加载方式

![](assets\懒加载方式.jpg)

- 最常用的就是第三种
- 原来的代码
  - 导入
  - `import  xxz  from    “./yyy”`
- 懒加载的代码
  - 动态导入
  - const Home = () => import (“../xxx/zzz.vue”)
  - 在路由 routers 数组 里  修改路由对象的  conponent 
  - ![](assets\动态导入.jpg)





# 路由嵌套

![](assets\路由嵌套.jpg)

配置对应的子路由

![](assets\默认路由.jpg)

- 黄色的是动态路由 ， 懒加载的写法

在组件内部使用  路由的标签

注意前面的`/` 不能省

![](assets\路由嵌套3.jpg)





# `Vue-router `的参数传递



- 传递参数主要有两种类型
  - `params`
    - 配置动态路由   格式 : /router/:id 
    - 传递的方式 : 在path 后面跟上对应的值
    - 传递后形成的路径 : /router/xxx
  - `query`
    - 配置路由格式 : /router , 也就是普通配置
    - 传递的方式 : 对象中使用query的key作为传递方式
    - 传递后形成的路径 : /router?id=xxx
- 关于 query
  - URL : 协议:// host 端口(默认80, 浏览器一般省了)/ path? query参数