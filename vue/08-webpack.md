# 什么是webpack?

![](C:\Users\lenovo\Documents\一“桶”前端\myNote\vue\assets\webpackV-1.jpg)

![](assets\webpackV-2.jpg)

![](assets\webpackV-3.jpg)

# webpack和gulp的区别

![](assets\webpackV-4.jpg)





# webpack 安装

![](assets\webpackV-5.jpg)



# webpack起步

## 文件目录

- dist
  - distribution : 发布
  - 这个目录是最终部署到服务器上文件夹

- src
  - mian.js    js的入口文件
  - js
    - xxx.sj
    - yyy.js
    - ....
  - 采用模块化的写法  ,  文件之间存在依赖关系
- index.html     项目的入口文件
  - 在里面 引入main.js是 不可以在浏览器运行的
  - 因为浏览器不认识 模块化写法 
  - 因此 需要 webpack 进行  ==模块化打包==  生成一个完整的浏览器可以认识的.js文件
  - 正确的是:
    - 引用 bundle.js 文件就好了
- 打包命令
  - `webpack  ./src/main.js  ./dist/bundle.js`
  - `webpack 需要打包的js文件地址   打包部署文件夹中的bundle.js`
  - webpack 内部处理模块之间的依赖
- 关键函数
  - `_webpack_require_()`
  - 负责处理依赖



## webpack 配置文件

1. 配置 入口 和 出口  ,  简化打包命令
   1. 新建webpack.config.js 文件  如下写代码

![](assets\webpackV-6.jpg)

​		注意:	

​				path 需要的是绝对路径 ,  需要用到nodejs中的path模块

​				`npm init` 安装 nodejs的模块 菜可以引入

![](assets\webpackV-7.jpg)

​    		  打包命令就简化成了`webpack`

​				

2. 完成npm的映射
   1.   真实项目: `npm run build`  就可以打包了   
   2. but why
      1. 打开package.json文件
      2. 找到 script 选项
      3. 输入键值对
         1. `"build":"webpack"`
         2. npm 运行 build 指令时  实际上实在运行webpack打包指令
   3. 通过 scirpt 选项的脚本命令映射完成的

![](assets\webpackV-8.jpg)



3. 局部的webpack 
   1. 安装局部webpack
   2. `npm install webpack@x.xx.x --save-dev`
      1. -dev : 安装的是开发时依赖
      2. 目前好像-dev命令废弃了
         1. 出问题再去解决吧
         2. 根本不慌呢!!!
   3. 我们再次打开 package.json 文件
      1. dev-   : development  开发
      2. devDependency : 开发时依赖
      3. 这是 npm自动为我们生成了 , 不需要手写

![](assets\webpackV-9.jpg)

4. ==npm run build 和 webpack 的区别==
   1. npm run build  默认是使用局部对应的webpack版本去打包
   2. webpack 则是使用全局webpack 去打包





# webpack loader

## 什么是 loader?

![](assets\webpackV-10.jpg)

## 使用loader

[和css相关的loader类型](https://webpack.docschina.org/loaders/#%E6%A0%B7%E5%BC%8F)

不同的loader负责处理不同的功能:

1. 以css-loader 和 style-loader 为例
   1. css-loader负责把css文件打包到 bundle.js文件中
      1. 不要问我是怎么去打包的 , 我也不太清楚!
      2. 但是我可以猜测一下
         1. 用js操作css的思路差不多
         2. 一个搬砖都还没弄明白的人 , 去操心 造火箭???
   2. style-loader负责把css文件渲染到DOM中 , 让样式真正的发挥效果



具体使用 可以去官方文档去看

![](assets\webpackV-11.jpg)



顺序错了, 就会直接报错!!!



## 具体案例

![](assets\loader-1.jpg)



## 真实开发

在main.js引入依赖

![](assets\loader-2.jpg)

安装loader

  				ps:     packega.json文件会自动生成对loader的依赖

![](assets\loader-3.jpg)



在webpack.config.js文件中配置 loader

![](assets\loader-4.jpg)





# less  sacc  等预处理文件的依赖

步骤和上面的真实开发一样 , 流程也是一样的

自己动手完成







# 图片等文件处理

- url-loader

![](assets\loader-5.jpg)

- 当文件小于限定时 , 会将文件编译成base64的字符串形式
- 一般开发中限制的多是 8kb
- 当文件>限定时  ,   打包会报错  

![](assets\loader-6.jpg)

- 安装一下 file-loader 就好了
- `npm install file-loader --save-dev`
- 不要高兴的太早  ,  还是会报错的
  - 因为file-loader生成的文件在dist目录下
  - 而css文件的url是没有dist目录的 , 也不可能有   否则就是影响我们的开发思路了
  - 那么怎么办? 
    - 配置 webpack.config.js文件 的  output 选项
    - 添加 键值对   `publicPath:"dis/"`
    - 解决url的路径问题

![](assets\loader-7.jpg)





- 新的问题:
  - 文件打包到dist目录中的文件名是一个hash值 , 可读性很差
    - 为什么是hash值?
    - 是为了给每个文件一个唯一标识符, 出于这个角度考虑的
    - 但是不人性化
  - 如何解决?
    - name+hash值 来命名 不就搞定了
    - hash值32位 也有点长了  可以hash:8  截取8位就好了
    - 后缀名　ｅｘｔ　占位 之后动态获取就后了   （extension）
    - 格式 : name.hash:8.ext
  - 实现
    - 在webpack.config.js文件下的  module选项里面的 rules数组  下的  负责配置｀ｕｒｌ－ｌｏａｄｅｒ｀的对象

![](assets\loader-8.jpg)



- 但是还是会报错
  - 原因就是 ; name 没有写到webpack 里面

![](assets\loader-9.jpg)









# ES6语法处理

## 安装对应的loader

![](assets\ES6-webpack-1.jpg)

问题:

​	我们打开webpack官网的话, 找到的babel-loader

![](assets\ES6-webpack-2.jpg)

上面的

`npm install -D babel-loader  @&babel-core babel-preset-es2015`

## 配置loader

![](assets\ES6-webpack-3.jpg)



文档上的配置

![](assets\ES6-webpack-4.jpg)















# webpack配置Vue

## 安装Vue.js

`npm install Vue --save`

![](assets\vue-webpck.jpg)



## 引入Vue 模块

`import Vue from "vue"`



## 打包项目

会报错: 因为Vue xxx-only 不可以有模板  要用xxx-conplie (编译 )

解决:

1. webpack.config.js 选项对象里面
2. 添加 键值对 
   1. resolve
      1. alias : 别名

![](assets\vue-webpck2.jpg)







## Vue的终极使用方案

==搞清楚的关键在于 搞懂el选项和template的关系==

### 原本代码

![](assets\vue-webpck3.jpg)



说明:

​	template里面的内容==打包编译==后最终会**替换**掉 el  挂载地方的元素

​	但是 temlate模板里面的东西太多了 , 看着不爽

### 进阶

把tempate以组件的方式提取出去  (自然模板用到的数据 ,方法之类的都要一起提取到组件里面去)

![](assets\vue-webpck4.jpg)

我们在跟组件里面注册一下就好

```javascript
components:{
    APP
}
```

 ![](assets\vue-webpck5.jpg)



### 模块化进化

把封装好的组件代码单独提取到一个js文件  比如就是 app.js

在app.js文件 最后写上 ES6**导出**代码

`export default App`

也可以这样组织代码:

![](assets\vue-webpck6.jpg)	



之后往 index.html 导入就好了:

![](assets\vue-webpck7.jpg)



​	

### Vue文件

上述使用之后 ，index.html 就相当清爽了

但是还存在问题  ；  app.js文件 看起来还是不够舒服

我们发明了 	专门写VueComponnet的文件     .vue

我们新建一个 App.vue的文件

文件默认三段

- template
- script
- style

我们把组件的代码抽到这里去 ， 一下就清爽了

![](assets\vue-webpck8.jpg)





### 最后

就在我们以为大功告成以后 ， 一打包   报错了。。。。。。。。。。。。。。

因为我们没有安装处理 .vue的文件 loader

处理问题： Vue官网

![](assets\vue-webpck9.jpg)

webpack.config.js 中的 module选项里面的rules数组里面 添加一个对象



你以为这就结束了?  就成功了?  屁!!!!



## 最终

我们 再次 npm run build 打包 , 依旧报错!!!!!!!!!!

![](assets\vue-w-1.jpg)



**解决方法1:**

修改 package.json文件

![](assets\Vue-w-2.jpg)

执行命令 ` npm install `安装修改过后的依赖



**解决方法2: 添加插件**







# webpack 配置文件后缀的简写

![](assets\Vue-w-3.jpg)

webpack.config.js文件中

这样以后 就不需要写`` ./App.vue`  直接 `./App `就可以