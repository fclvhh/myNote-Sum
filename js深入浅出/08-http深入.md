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









# http缓存 「外存」

1. 使用Cache-Control是常见的缓存方式

设置响应头 Cache-Control

```shell
Cache-Control:max-age=3600;
```

2. 想要更新缓存只需变更url
3. 如何让浏览器抛弃之前的缓存
   1. 给url                资源路径?md5
   2. 很像是动态路径的策略
   3. webpack的一个插件可以做到  给图片资源生成hash值
4. index.html 这样的入口文件不可以加缓存



## 缓存原理

1. 浏览器 向 服务器发送了一个url , 请求一个main.js资源
2. 服务器响应请求 , 并且设置了响应头 Cache-Control : max-age = xxx
3. 一段时间后, 当浏览器再次像浏览器发送同一个url请求时
   1. 浏览器会根据 Cache-Control  ,  查看时间是否到了max-age
   2. 如果没有 , 那么浏览器不会发送这次请求 , 会拦截下来 , 自己从自身的缓存中 , 拿main.js的内容   「缓存在用户的硬盘上」
   3. 注意: 相同的url请求没有发送出去 , 被浏览器拦截了



## 为什么首页不能设置缓存?

反证: 

​	如果首页设置了缓存 , 那么用户在访问过页面的时候 , 再次刷新/访问

​	浏览器根本就不会去发送url请求 , 完全就和服务器失联了

​	因此 : 首页并不能设置缓存

当后台更新了代码 ，访问首页 ， 那么首页用到的新的资源都会去下载

**首页就是代码的入口:** 

- 所有url最终都是通过首页这个入口发出的



## max-age一般设置多大？

答: 

​	max-age一般设置为10年!!!

惊呼! 那么久!!! 如果我想更新代码怎么办??



回顾 : 

​	缓存的原理 : 是同一个url , 浏览器不会去发送请求, 会进行拦截

方法 : 

​	当我想更新代码的时候 , 手动**更改url** , 不就好了!

方案 : 

- 让url里面含有一个随机数占位符
- 每次打包代码的时候 , 自动生成一个随机数 , 替换url的占位地方
- 这样就又缓存 , 就保持了更新的能了

工具 : 

- webpack打包工具
- webpack插件可以完成



## expires

老版本的http响应头 , 功能和Cache-Control 类似





# 并不是缓存

1. 使用Etag和304避免下载过程

2. 但是这并不是严恪且义的缓存



## md5算法

用来判断自己下载的文件是否正确

一个文件内容对应一个字符串

可以用md5 , 生成的字符串 , 判断文件的内容是否一致



**npm** md5 库

```shell
npm xxx
```

引入

```js
const md5 = require("md5")
```

使用

```js
let fileMd5 = md5("文件地址") // 返回一个字符串
setHeader("ETag",fileMd5)
```





## Etag响应头

语法

```js
setHeader("ETag",md5String)
```

下次请求的时候 , 会将**ETag响应头**内容放到 **if-None-Match 请求头**里面

如果一样 , 那就说明不需要重新下载资源 

此时资源还在**浏览器内存**里面

注意 : 

1. 和缓存原理不同 
   1. 缓存用到了文件 , Etag用的是内存
   2. 缓存浏览器不发送url请求 , Etag是发送请求 , 但是不下载资源





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



## 注册与登录

**注册 :** 

​	用户输入用户名和密码

前端 : 收集用户的输入 , 进行格式匹配, 之后发送给对应的路由

后端 : 拿到url传来的数据 , 进行字符串给解析 , 拿到数据库做对比 , 如果不存在

​		就在数据库添加用户信息 , 如果存在 , 则提示用户已注册



**登录 :** 

​	用户输入用户名和密码 , 点击提交

后端 : 接收到数据 , 到数据库里查 , 如果存在 , 就予许登录 , 不存在 就不行

前端 : 当用户登录成功后, 拿到用户的基本信息 , 进行页面的友好设计提示



**问题 :** 

​	前端怎么拿到用户的信息呢?

答 :  通过cookie , cookie储存用户信息



##  流程

1. 登录成功 , 后端

```js
response.setHeader('Set-Cookie', `sign_in_email=${email}`)
```

 通过set-Cookie 响应头设置

2. 浏览器拿到响应

3. 以后浏览器向同一个服务器发送请求的时候 , 在请求里面都有cookie请求头
4. 这样的向服务请发送请求的时候 , 就不用重复登录了 
   1. 因为cookie就是登录成功 , 后端发给浏览器的
   2. 所有浏览器有就待变登录成功了 , 自然就不用重复登录了



## Cookie特点

1. 服务器 : Set-Cookie响应头设置Cookie
2. 浏览器 : 每次向服务器请求 , 都会带上请求头Cookie
3. 服务器读取Cookie就知道登录用户的信息了



注意 : 

1. Cookie是跟着浏览器走的
2. Cookie存在哪里  ?  一个文件里面
3. Cookie能作假嘛?  可以 ,所以存在安全问题
   1. 用户可以篡改Cookie , 如何解决?
   2. session 可以搞定
4. Cookie 有效期嘛?  默认有效期20分钟 , 可以手动设置?
   1. 后端可以强制设置 ,  Cookie的有效期
5. 利用cookie , 前端可以做  "欢迎xxx登录 , 你好"







 



# Session

是浏览器为用户分配的一小段内存，用户的识身依庭可以用0。准保存〔也可以不甲cookie)
反向代理是什么
sewer叾请丰中的信息，请求分发到不同的地方，此server即为反向悼
以№in区配首多嵫名网站为
2以“-小转诂求为俱



## Cookie的问题

用户可以篡改Cookie , 从而得到伪装成其他用户 , 得到其他用户的信息

从而造成安全性问题!!!



Cookie**流程** : 

1. 浏览器发送请求
2. 获取到Cookie
3. 用Cookie的值作为key , 去缓存里拿value

```js
let url = xxx?cookie="1@qq.com" 
let cache = {
	1@qq.com:222,
    2@qq.com:bbb
}
let cookie = 1@qq.com
// 读取密码 , 
let value = cache[cookie]

// 如果我们修改cookie
let cookie = 2@qq.com
// 就可以拿到用户2的密码 , 就可以做很多事
let value = cache[cookie]
```



那么如何避免上述流程的问题呢?



## Sesstion实现简单策略

**分析:**

​	上述问题出现的核心 ,  cache[cookie] 

​	cookie值直接作为key, 不安全 , 我们进行一层加密来试试



```js
 // cookie值变成一个随机数
let url = xxx?cookie="1@qq.com" 
let cache = {
	1@qq.com:222,
    2@qq.com:bbb
}
// 用户名加密表 sessions
let sessions = {
    sessionId:1@qq.com,
    3xxx.4xxx: 2@qq.com,
    
}
// 生成随机数sessionId
if(url){
    sessionId = random()*1000000 
    let cookie = sessionId
    // 添加数据到sessions
    sessions[sessionId] = 1@qq.com
}
// 拿数据
let userName = session[cookike]
let value = cache[userName]

// 这样用户就不能修改cookie了 , 因为cookie是一个随机数
// 修改没有意义 , 还修改个毛啊


```



## 小结:什么是Session?

1. 当用户注册成功之后 , 服务器会生成一个sessionId,并且为其分配一小块内存
2. sessionId里面对应用户的关键信息
3. 服务器在设置响应头set-Cookie , 会把sessionId的值 , 赋给set-Cookie
4. 这样就既实现的Cookie的功能 , 又避免了Cookie带来的安全性问题



**问题:**

1. sessionId会存储吗?

存储在服务器的内存sessions里面 , 用户必须要带着sessionId去访问

如果用户手贱修改了Cookie , 也就是sessionId , 那么就访问不了那块内存了







# localStorage

**本质: **

​	浏览器提供的一个api对象

​	本质是浏览器生成的一个文件 , hash结构 , 一个对象

**常用api**

- localStorage.getItem()   查Item
- localStorage.setItem()   写Item  (增改)
- localStorage.removeItem()  删除item



## 使用

把ui数据 ,  持久化到c盘的一个文件里面

这样的就算刷新的页面 , ui数据也会存在



> 应用

记录下 , 网站是否已经提示了用户





## localStorage特点

1. LocalStorage 根http 无关
2. http不会带上 localStorage的值
3. localStorage是支持同源策略的
4. 每个域名 localStorage 最大的存数量为5m左右
5. 常用场景 : 记录有没有提示过用户
6. 理论上 : 永久有效 , 除非用户清理缓存



## sessionStorage

和localStorage的用法几乎完全一样



**区别:**

1. 在用户关闭页面后 , 一个很短的时间自动清空

