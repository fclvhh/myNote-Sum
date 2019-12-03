# 什么是Promise?

![](assets\Promise-1.jpg)



[setTIme回调地狱](http://js.jirengu.com/sawem/1)

[Promise解决回调地狱](http://js.jirengu.com/luduz/1)

![](assets\Promise-2.jpg)

- 涉及到的参数都是函数
- resolve() 执行后,会跳转到then() 执行
- then(A)  里面的参数A也是函数,最后会返回一个新的Promise对象

什么时候使用Promise

![](assets\Promise-3.jpg)



Promise的三种状态![](assets\Promise-5.jpg)

![](assets\Promise-6.jpg)

Promise的第二种写法![](assets\Promise-7.jpg)





# Promise的链式调用

![](assets\Promise-8.jpg)

new Promise的优化

![](assets\Promise-9.jpg)

进一步省略

![](assets\Promise-10.jpg)



reject() 省略也是同理, 只不过不是来到then()去执行,而是转到catch()去执行!!!

简化1![](assets\Promise-11.jpg)

最终简化![](assets\Promise-12.jpg)

