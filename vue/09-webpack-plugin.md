# 认识 plugin

![](assets\wp-p-1.jpg)



# 版权声明插件

webpack自带的插件

![](assets\wp-p-3.jpg)



# 打包html的插件

![](assets\wp-p-4.jpg)

 ![](assets\wp-p-5.jpg)



**使用html-webpack 插件带来的问题**:

1. 我们在图片资源loader那部分内容 ,  留下了一个问题
   1. 当时我们没有把indexhtml文件打包到dist 文件中
   2. 之前引用图片资源时 ,  是没有 /dist  目录的
   3. 打包过后却是 存在dist 目录  造成路径问题
   4. 但是在output 选项下 设置了 publicPath  解决了问题
2. 现在index.html 在插件的帮助下打包到了 dist目录
   1. 这个publicPath 反而引发了错误
   2. 真是牵一发 , 而动全身啊
   3. 难啊!!!
3. 打包生成的 在dist 目录的index.html 文件还是存在问题
   1. 文件缺少  `<div id=app>  </div>` 给Vue 挂载
      1. 解决: 在plugins 的配置对象中 
      2. tenplate : “index.html”    这里的index.html 是 src 的  因为此时dist 的还在打包呢
   2. src 下的index.html 可以不写 引入bundle.js  文件的script 
      1. 因为 打包插件会在dist 生成的 index.html 中自动引入 打包好的 bundle.js文件



![](assets\wp-p-6.jpg)





# js压缩plugin插件

把bundle.js压缩 , 删除空格的

![](assets\wp-p-7.jpg)

开发阶段: 不建议使用这个插件

项目上线时, 再加上

# 搭建本地服务器

webpack-dev-server

数据保存在内存里面 , 最终还是需要npm run  build  把打包数据保存到硬盘中

![](assets\wp-p-8.jpg)



最终测试结束后

npm run build  执行打包命令   是数据真正的保存到磁盘里





# 配置文件分离

- webpack.config.js    完整的配置
  - base.config.js    开发和上线都需要的公共配置
  - prod.config.js    上线需要的配置
  - dev.config.js       开发需要的配置



实战步骤:

1. 在项目根目录下新建 build目录
   1. build
      1. base.config.js 
      2. prod.config.js
      3. dev.config.js
2. 安装   webpack-merge
   1. npm install webpack-merge  --save-dev
   2. 在 base.config.js  prod.config.js dev.config.js 中导入
3. 写代码

![](assets\wp-p-9.jpg)



4. 依次做好这些之后 , 就可以把src 下的webpack.config.js文件删除了
   1. 因为已经没有必要使用了
   2. 这会造成问题
      1. 原本package.json文件;里面的 脚本失效了
      2. 因为之前 是根据webpack.config.js 映射的
      3. 解决办法
         1. 在后面配置对对应配置文件的地址
         2. `--config  映射的配置文件的路径`

![](assets\wp-p.jpg)

5. 做好这些还是会 文件目录的问题
   1. 因为从 webpack.config.js 文件抽离出来的  base.config.js 
   2. 设置 output 选项 , 获取 绝对路径时 , 处于项目根目录下 , 自然输出在dist
   3. 由于没有修改 output 配置  ,   同时 base.config在build文件夹下,导致了   build - - - dist 的不符合预期的后果
   4. 修改 /dist ==> ../dist   
   5. 重新 npm run  build

前后图对比:

前

![](assets\wp-p-a.jpg)

后

![](assets\wp-p-b.jpg)

6. 这个过程本身就是 模块化思想的体现





