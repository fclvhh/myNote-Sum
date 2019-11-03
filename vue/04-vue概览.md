# Vue.js 学习指南

Vue.js知识量化

1. 认识Vue
2. Vue的基础语法
3. 组件化开发
4. Vue CLI 脚手架工具 详解
5. Vue-router
6. Vuex 详解
7. 网络封装  axios
8. 项目实战  
9. 项目部署



# 认识Vue

## 简单认识

- Vue是view 的法语发音,作用就是代替``mvc中``的`v`
- Vue是一个渐进式框架  什么是「渐进式」?
  - 项目老旧 用vue 重构时 , 不用推倒重来 , 把Vue页面作为项目的一部分嵌入 , **逐步迭代**掉老项目 
  - 举个例子:  项目有x个页面 ,  全部用jQuery , 可以用Vue一个页面一个页面的去重构, 同时不影响到整个项目

- Vue的特点和Web开发中的高级功能
  - 解耦视图和数据
    - 胡子语法 就是 数据和视图 解耦的一大体现
  - 可复用组件
  - 前端路由技术
  - 状态管理



## 安装Vue.js

- 方式一:  加快项目的速度 ? 
  - CDN 引入   [bootCDN](https://www.bootcdn.cn/)

- 方式二: 下载和引入
  - 官网下载
- NPM 安装



## 重要理念提升

#### 编程范式:

- 声明式编程    面向对象
- 命令式编程     面向过程

**学习框架的第一步就是这个思想的转变**

举例:

这是声明式编程

```javascript
// vue 
<div id = 'app'> 
    {{messege}}
</div>

new Veu({
    el:'#app',
    data{
    	message:'hello,my friend!'
	}
})
```

我们 声明一个变量 , 然后引入就可以用了

省去了 中间的实现过程 , 而这个过程是命令式代码, 别人写好的!



#### 响应式

数据驱动

当数据改变时 , 页面会跟着变  就叫做响应式



## 小结

![](assets\vue-小马哥-2.jpg)



# Vue中mvvm

![](assets\vue mvvm.jpg)

![](assets\mvvm-解读.jpg)



- ViewModel 层

  - 是View和 Model的桥梁
  - 干两件事
    - Data Binding    数据绑定   
      - 由于Vue响应式 , 意思就是Model层的改变会实时的反应到View 中
    - DOM  Listenner   文档监听
      - 监听事件 , 在需要时改变data   

  

 



# vue 小案例

[计数器案例](http://js.jirengu.com/kaxal/1)

![](C:\Users\lenovo\Documents\一“桶”前端\myNote\vue\assets\vue-小马哥-3.jpg)



