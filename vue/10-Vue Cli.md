

什么是 Vue Cli ?

![](assets\Vue-cli-1.jpg)

脚手架 依赖

1. node
2. webpack

# Vue cli 的使用

## 安装Vue cli

[安装Vue cli 3.x](https://cli.vuejs.org)

[拉取2.x模板](https://cli.vuejs.org/zh/guide/creating-a-project.html#%E4%BD%BF%E7%94%A8%E5%9B%BE%E5%BD%A2%E5%8C%96%E7%95%8C%E9%9D%A2)

![](assets\Vue-cli-3.jpg)



## Vue cli2 详解

![](assets\Vue-cli-4.jpg)



# 分析项目目录结构

- 先看 package.json文件
  - 搜索 script
  - 找  命令 dev 和build
    - dev 运行项目
    - build 打包项目

![](assets\目录分析.jpg)



如果不想使用ESlint 

可以在config目录下把index.js文件 

搜索 useEslint   ： false



# 👍runtime-compiler 和 runtime-only  的区别

## Vue程序运行过程

![](assets\Vue程序运行过程.jpg)



Vue的 模板 template  被 解析成  ast  (abstract syntax  tree)  「抽象语法树」, 抽象语法树又会被「编译 」成 render( )

reder() 函数  执行 出 一个 虚拟DOM树   

最后虚拟DOM树又会被渲染成真实 DOM 中,展示在页面中

​							==template - - >  ast  - - >  render - - >  vdom  - - > realdom==

## 总结:

 

区别:

1. runtime-compiler 的过程 
   1.  `template - - >  ast  - - >  render - - >  vdom  - - > realdom`
2. runtime-only  的过程
   1. `render - - >  vdom  - - > realdom`



结论:

1. only 性能更高

2. only 代码更少   render( )函数的原理

   ![](assets\Vue-1.jpg)

3.  ![](assets\Vue-2.jpg)

4. 这个h 就是createElement函数 ,  传入的VueComponent对象就是我们定义好的App组件
5. ==VueComponent对象 App组件 里面的 template的怎么处理呢？==
   1. 不用去考虑
   2. 因为 App组件被编译出来就是普通的对象， 这个普通的对象里， 已经把template全部渲染成render()了
   3. 问题又来了， .vue文件的template 是谁帮我们处理的呢？
      1. 是由vue-template-compiler  「08-vue终极使用方案中提过」 负责把.vue文件中的template 解析成render()函数的



参考

![](assets\Vue-3.jpg)





# build命令和dev命令

![](assets\build.jpg)



![](assets\dev.jpg)





# 认识VueCLI3

![](assets\Vue-cli3-1.jpg)

==public 文件夹和static文件夹的内容是原封不动的打包到dist文件下的==



## VueCLI3  初始化项目

![](assets\Vue-4.jpg)



## 目录结构分析

![](assets\Vue-5.jpg)



## 配置到底去那里了



经历webpack 和 vue cli2 的揉捏 , 我们对一大堆的配置文件都见怪不怪了

虽然很讨厌他 , 但是好久不见 , 甚是想念!

那么"他" 究竟去那里了呢?



### vue ui

启动本地服务器 , 查看可视化配置



### 文件目录

node_module -->@vue –> cli-server –>webpack.config.js

![](assets\vue-6.jpg)

根据  Service.js  

![](assets\vue-7.jpg)

找到对应配置的文件





## 如何修改配置

- 在项目根目录下  新建  vue.config.sj
- 在这个配置文件中配置选项
  - module.export = { }
  - 待后

![](assets\Vue-8.jpg)





