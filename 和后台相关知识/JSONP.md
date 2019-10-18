# 数据库是什么鬼?

1. 文件系统是一种数据库
2. MySQL是一种数据库

只要能==长久==的存储数据,就是数据库



## 引例

需求: 支付环节,当我在输入框输入金额,并且点击按钮时,我的账户余额,实时的减少,刷新页面后,我的余额还是减少的



问题的关键:

​		让我的余额随着操作永久的减少!这不是缓存级别的,而是外存级别的!(也就是,刷新浏览器页面,数据也回不去了)

![](D:\一“桶”前端\myNote\和后台相关知识\assets\2019-09-09_154119.png)

解决的思路:

我们单独创建一个文件,这个文件存储着我们的账户余额: 如  touch `db`  ,   在 `db` 文件里面 写代码  `var account=100`

我们读取`db` 文件里面的数据, 将这个数据展现到账户余额那里, 

当我们输入金额的时候,我们获取这个数字 `var number=8 `   

点击按钮时,触发一个事件: `account = account-number`   之后再把这个`account`写回到文件里面去就好了



其中最关键的是:

1. 获取文件的数据,然后展现到页面上
2. 把页面上的操作, 真实的在文件处完成



## 反思

上述过程.用户的体验是不好的,因为用户需要自己手动刷新页面,重新加载页面,执行读取文件操作,才可以获得账户金额

这个体验是相当差的!!!





==> 最终发明了一个srj方案  

也就是  server return javascript 的方案







# JSONP

解决了两个网站之间的交流

原理:

1. script 标签不受域名限制



---





## 细看来龙去脉

**数据库的减法为例**

通过前面的知识,我们知道了**数据库其实就是特殊格式的文件**

通过点击表单的提交按钮,向服务器提交了一个请求,服务器接收到请求,处理请求

也就是;把'’数据库'里面的数字取出来,减去1,然后把结果重新写到`数据库`里面



### 问题:表单会强制刷新跳转页面

用户需要回退,并且刷新页面,才能看到结果



### 优化:不用表单

手段一:

​	就用button按钮

​	给按钮绑定事件, function(){

​			创建一个img元素

​			==img.src = /xxx==    这种形式发送请求

​	}













# JSONP总结

> 用于请求方和服务方的跨域



## 步骤:

请求方; frank.com 的前端程序员(浏览器)

响应方:jack.com的后端程序员(服务器)



1. 请求方创建 script 的 `src`指向响应方,同时绑定一个查询参数`?callbackName=xxx`

2. 响应方根据查询参数`callbackName`,构造形如

   1. xxx.call(undefined,'你要的数据')
   2. 或者xxx(‘你要的数据’)

   这样的响应

3. 浏览器接收到响应,就会执行xxx.call(undefined.‘你要的数据’)

4. 请求方就知道了他要的数据

这样一个过程就被称为JSONP



## 约定

1. `callbackName` ->callback
2. xxx ->随机数  

```javascript
button.addEventListener('click', (e)=>{
    let script = document.createElement('script')
    let functionName = 'frank'+ parseInt(Math.random()*10000000 ,10)
    window[functionName] = function(){  // 每次请求之前搞出一个随机的函数
        amount.innerText = amount.innerText - 0 - 1
    }
    script.src = '/pay?callback=' + functionName
    document.body.appendChild(script)
    script.onload = function(e){ // 状态码是 200~299 则表示成功
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就干掉这个随机函数
    }
    script.onload = function(e){ // 状态码大于等于 400 则表示失败
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就干掉这个随机函数
    }
})
//后端代码
...
if (path === '/pay'){
    let amount = fs.readFileSync('./db', 'utf8')
    amount -= 1
    fs.writeFileSync('./db', amount)
    let callbackName = query.callback
    response.setHeader('Content-Type', 'application/javascript')
    response.write(`
        ${callbackName}.call(undefined, 'success')
    `)
    response.end()
}
...

```











# 面试题

## 什么是JSONP?

> 1. JSONP(JSON with Padding)是JSON的一种"使用模式"，可用于解决主流浏览器的==跨域数据访问==的问题。
>
>    1. 跨域数据访问: 自家网页和别人的服务器勾搭,原本是不行的
>
> 2. 由于==同源策略==，一般来说位于 server1.example.com 的网页无法与不是 server1.example.com的服务器沟通，而 HTML 的<script> 元素是一个例外。利用 <script> 元素的这个开放策略，网页可以得到从其他来源动态产生的 JSON 资料，而这种使用模式就是所谓的 JSONP。
>
>    1. <script>标签是默认跨域的
>
> 3. 用 JSONP 抓到的资料并不是 JSON，而是任意的JavaScript，用 JavaScript 直译器执行而不是用 JSON 解析器解析。
>
>    1. 上文看了话,应该明白为什么叫做JSONP了



## JSONP 为什么不支持 POST

> 因为用`jsonp`进行跨域，实际是在客户端动态添加了个<script></script>然后将`url`指向要请求的地址，script是没有同源策略的，用这种办法自然只能是get了。

