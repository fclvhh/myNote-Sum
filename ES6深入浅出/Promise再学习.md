# 什么是Promise?

![](assets\Promise-1.jpg)



[setTIme回调地狱](http://js.jirengu.com/sawem/1)

[Promise解决回调地狱](http://js.jirengu.com/luduz/1)

![](assets\Promise-2.jpg)

- 涉及到的参数都是函数
- resolve() 执行后,会跳转到then() 执行
- then(A)  里面的参数A也是函数,最后会返回一个新的Promise对象