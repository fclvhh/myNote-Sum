# HTTP为什么重要

1. HTTP是前后端合作的重要方式

99%的需求都是通过HTTP做到 , 小部分需求可以WebSockets做到。

2. HTTP能帮你从本质上解HTML、CSS、js、图片、json、jsonp等不同形式的资源
3. web性能优化基朩等价于对HTTP传输效率优化

4. 前端工程化需要你对HTTP缓存有深入了解





# 什么是HTTP

## 四个概念

1. server
2. client
3. request
4. response

用简单的工具了解这四个概念:

- curl作为client
- server.js作为server
- curl发送一个requset到server.js
- server.js返回一个response给curl



## curl

- 用户用来浏览网页的软件(GUI或命令行均可)
- 后端开发者的爬虫
- 搜索引擎的爬虫

client也可以叫做用户代理 , 因为它帮用户发送请求.

> 什么是代理?

「用户代理」的英文user agent  , 「代理」的英文是proxy , 所以不是完全一样的

通俗的来说 , proxy是为了满足特定功能的agent , 这些功能包括加速访问、内容缓存、内容过滤、信息加密等 ; 而agent 则功能比较广泛 , 如Chrome 就是一个user agent , 能满足广泛的上网需求 , curl和爬虫也是user agent , 一般不能叫做proxy



## server

你可以过很多方式建一个server

1. Apache+PHP/Java/Python/N0dejs
2. Nginx十PHP/Java/Python/Node.js
3. 只用Node.js

为了便于理解精，我们只用一就可以建一个server。
[FrankFang/nodejs-test]( https://github.com/FrankFang/nodejs-test/blob/master/server.js)
我们用curl来向个server发一个请求。

```shell
curl -v http://127.0.0.1
```

如果你不想用IP，可以设置一下hosts

```shell
127.0.0.1 wtf.com
curl -v http://wtf.com
```





## 请求

请求包括四郜分

1. 第一部分----动词  路径   协议/版本号

2. 第二部分–---一堆  key：value , 用回车分割 , 注意value前面有空格
3. 第三部分—-回车，作用只有一个     分隔开第二部分和第四部分
4. 第四部分一随便什么内容都可以，内容的格式必须要第二部分里用content-type说明



第二部分必会的key: value

```shell
content-Type: application/x-www-form-urlencoded
```

x:表示没有写道规范里面   form:表单



与**content-Type对应的第四部分的语法**:

```shell
key=value&key=value...
```

特殊字符变成  %unicode编码



## 响应

响应包括四部分

1. 第一部分   -协议/版本号 状态码  状态信息
2. 第二部分 -  一堆key: value，用回车分割
3. 第三部分- 回车，作用分隔开第二部分和第四部分
4. 第四部分。什么内容都可以．内容的格式必须要第二部分的content-Type说明

curl可以响应
chrome开发者工具也可以



# HTML、CSS、JS的本质

本质就是字符串，只不过Content-Type不同。 「也就是文件后缀不同」
注意url的后缀毫无意义 「浏览器又不认识」。

1. 浏览器通过i地址栏、iframe来请求html
2. 浏览器通过link获取取css然后渲染
3. 浏览器通过script标签获取js,然后执行
4. 浏览器通过image标签获取图片，然后展示





# JSON的本质

**本质就是字符串**，只不过content-Type为application/json

注意url的后缀毫无意义 「浏览器又不认识」。

**是符合 json语法的字符串**  



# JSONP的本质

本质是字符串，只不过

1. Content-Type :application/javascript    者text/Javascript
2. 内容格式为`functionName({"format:json"})`



# url小结

`url = 协议://主机地址/资源路径?查询参数`



所有的文件本质上都是字符串 , 关键是后缀不同!!!

> 如何告诉浏览器/服务器 字符串的后缀呢?

第二部分中非常关键的key: value

content-Type: value   获取后缀 , 浏览器就知道文件的类型了



> 如何告诉浏览器/服务器  文件里面的编码类型呢?「解决中文乱码问题」

同样是通过   content-Type: value1 ;  charset="utf-8"



这个键值对 , 同一样决定了第四部分**路径请求参数** 展示格式





一般来说后端写responese时候 , 常用代码

```js
response.setHeader("content-Type","text/html;charset=utf-8")
```





# 浏览器如何请求json/jsonp资源

前面已经知道了 , 浏览器请求资源 , 都已对应的方法

如: 

| 资源       | 对应的方法       |
| ---------- | ---------------- |
| html       | /资源路径        |
| css        | <link> 标签      |
| js         | <script>标签     |
| json/jsonp | ? 脑袋一下子懵了 |



响应的content-Tpye键值对 , 常用value

| 资源  | 对应的value                                      |
| ----- | ------------------------------------------------ |
| .html | text/html                                        |
| .css  | text/css                                         |
| .js   | text/javascript    or   application/javascript   |
| .json | text/json  or   appliaction/json                 |
| .xml  | text/xml  or   text/xml+html  or application/xml |
| .png  | image/png                                        |
| .jpg  | iimage/jpg                                       |
| .     |                                                  |



**ajax 就是获取json的一门技术**

**XMLHttpRequest  这个js对象 , 去获取json资源**

**依然靠script标签**









# 缓存

1. 使用Cache-Control是常见的缓存方式

```shell
Cache-Control:max-age=3600;
```

2. 想要更新缓存只需变更url
3. 如何让浏览器抛弃之前的缓存
   1. 给url                资源路径?md5
   2. 很象是动态路径的策略
   3. webpack的一个插件可以做到  给图片资源生成hash值
4. index.html 这样的入口文件不可以加缓存



# 并不是缓存

1. 使用Etag和304避免下载过程

2. 但是这并不是严恪且义的缓存









# Cookie

Cookie用于识别用户

[了解更多](https://zhuanlan.zhihu.com/p/22396872)

1. 现代前端几乎不会去碰Cookie….

2. 如果需要多页面共享信息，请用  localStorage或sessionStorage
3. Cookie的所有知识都可以用维基百科查到

简单逻辑:

​	服务器给浏览器reponse 添加一个cookie  ,  那么下次浏览器再次登录时 , 发送requset时这个cookie 会跟着发给服务器 , 服务器就不会执行再次验证的逻辑了

应用:

​	识别用户有没有曾经做过一件事 , 如果做过就响应一个cookie 

​	常用于登录验证



 



# Session

是艹艹为用户分配的一小段内存，户的识身依庭可以用0。准保存〔也可以不甲cookie)
反向代理是什么
sewer叾请丰中的信息，请求分发到不同的地方，此server即为反向悼
以№in区配首多嵫名网站为
2以“-小转诂求为俱