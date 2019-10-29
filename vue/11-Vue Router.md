# 什么是路由?

![](assets\什么是路由.jpg)



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

 ![](assets\Vue-router-1.jpg)

- 安装Vue-router 插件
- src文件夹
  - 新建一个router 文件夹
    - 新建一个  index.js
      - 在里面配置路由信息
  - 如何配置路由信息?
    - 导入路由插件  和 Vue的js文件 (后面要用到)
    - ![](assets\Vue-router-3.jpg)
    - 安装插件
      - `Vue.use(VueRouter)`
    - 创建`VueRouter`实例对象
      - 和创建Vue的实例  过程类似
    - 把实例对象传入到Vue的实例中去
      - 自然 , 自身要导出
      - Vue的实例要导入
- ![](assets\Vue-router-2.jpg)

- 我们在routers 数组中 , 配置映射关系
- 挂载到Vue的实例中去
  - ![](assets\Vue-router-4.jpg)





# 配置路由映射

- routers数组中的一个对象 , 就是一个映射关系
- 这个对象里面 两个键值对
  - path : “xxx”
  - component ; "xxx"
- ![](assets\Vue-router-5.jpg)

- 在Vue实例的根组件上 , 引入路由
  - VueRouter插件定义的全局组件`router-link-to `
  - 在根组件的template 里面使用
  - 这个`router-link-to`最终会被渲染成 a 标签的 
- `router-view` 占位符     决定 组件在什么位置显示



![](assets\Vue-router-6.jpg)







# 路由的默认路径

![](assets\Vue-router-7.jpg)



## 路径修改成history模式

-  在 index.js文件 `VurRouter`构造函数中 , 传入的选项对象中
  - 增加键值对
  - mode :  `history`
- ![](assets\Vue-router-8.jpg)







# router-link   知识补充

![](assets\Vue-router-9.jpg)

![](assets\Vue-router-10.jpg)

实在想批量修改 , 可以在 router文件夹下的 index.js文件中 添加  选项对象中的 选项   linkActiveClass







# 通过代码方式实现路由跳转

- $router  实例属性  
- 是VueRouter对象 给每一个组件 注入的属性

![](assets\Vue-router-11.jpg)





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