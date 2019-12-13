# 闲聊回调函数

形式一：

```javascript
function fn1(){
	console.log("hello!")
}
function fn2(){
	fn1()
}
fn2() 
-------------------------------------------------
hello!

// 在调用XXX时，又会去调用YYY，把yyy称为回调函数

```

形式二:

上述代码的严格模式

```javascript
function fn1(){
	console.log("hello!")
}
function fn2(fn){
	fn()
}
fn2(fn1) 
-------------------------------------------------
hello!
// 严格来说,是需要传参,这才是函数严谨的调用
// 把fn1作为参数传给fn2使用,就称为回调
// 只不过方式一 写起来更简单,所以就用了方式一
```



## 什么是回调?

在调用fn1时候,回调栈里面记住fn1,运行fn1的代码,发现要调用fn2(),

此时需要切换到fn2的运行环境里面去,然后调用f'n2,回调栈会记住fn2!这样一个过程就是回调!

**🎈〖核心总结〗**

⒈函数A作为参数 传入到函数B

⒉函数B 使用了作为参数的函数 A

本质 : 对一个现象的描述

现象 :  使用参数,导致了函数上下文的切换   =>参数为函数

**把这个现象描述成回调!!!** 





## 为什么要有回调?

①来看一个例子

```javascript
// DOM ready之后执行
$(document).ready(function(){
    // 获取模板
    $.get(url, function(tpl){
        // 获取数据
        $.get(url2, function(data){
            // 构建 DOMString
            makeHtml(tpl, data, function(str){
                // 插入到 DOM 中
                $(obj).html(str);
            });
        });
    });
});
```

②代码分析

⑴上述代码功能: 组成一个DOM ,然后渲染到html文档中去

⑵功能实现:

​		⒈得到模板  html

​		⒉得到data   

​		⒊拼接

​		⒋渲染

关系:  基本上每一步都依赖上一步的结果,每一步都需要时间

⑶ j s 单线程 

1. 上述功能大体需要分四步,每一步都**需要时间**来处理
2. j s 由于是单线程的,所以**不可以等待**
3. 产生了一个 矛盾 –- 异步的问题
4. **特殊机制** : js是不等待的,所有的回调js函数都是交给浏览器监听异步操作是否完成,再通知js引起回来完成函数代码 

⑷矛盾的解决

利用函数回调设计了一种函数   

```javascript
fn(a,b){
    // ①关于a参数的操作,得到结果
    a=a+1
    // ②b是函数作为参数被调用, 使用到操作1的结果
     b(a)
}
```

分析:

1. 首先完成自己的异步操作
2. 通过回调把**异步操作的结果**传递给函数去执行第二步的异步操作

通过get()这个函数的嵌套调用可以实现上述需求

⑸ 回调函数设计的好处

结果传递 : 方便的将异步操作结果,传递给下一步的操作

剩下的问题:

- 匿名函数简化代码书写
  - 不用先声明,再作为参数
  - 匿名函数就直接作为参数
- 多级嵌套实现功能
- 如果功能过于负责起怎么办????
- 异步回调失败了怎么表示
  - 计算机根据 01,肯定知道成功还是失败了
  - 但是人怎么知道呢?
  - 是异步操作太负责,需要时间太多?
  - 还是异步失败了呢?

# 回调地狱

```javascript
// 形态1:
function 获取用户信息 (fn){
    fn('riven')
}
function 打印用户信息(用户信息){
    console.log(用户信息)
}

获取用户信息(打印用户信息)  

// 变形2:函数匿名调用 B(A) 中的A变成匿名函数
// 如果函数A的目的就是为了被B调用,那么A采用匿名函数更高效
function 获取用户信息 (fn){
    fn('riven')
}
获取用户信息(function(用户信息){
    console.log(用户信息)
})  

//变形3:回调地狱
//B(A(C(D(E(F)))))  这样嵌套的方式实在会很复杂, 如果再采用匿名函数简化那简直就是地狱了啊
获取用户信息(function(用户信息){
    console.log(用户信息)
    保存用户信息(用户信息,function(){
        获取更多(xxx,function(){.....})
    })
})

```

小结:

​		匿名函数作为函数调用的参数,如果多层嵌套,就会导致代码无法理解!

​		并且 代码结构难看 ,是个">” 形式 , 我们希望是链式形式

​		这就被称为回调地狱!

==匿名回调的嵌套,是">"字形,结构看不清,导致逻辑理不清==

解决问题的核心===> **让结构清晰**

**思路: 让回调把自己的逻辑代码执行完毕,再去执行传入的回调**

​			**也就是清晰将两部分 分割**

一个Promise先执行自己的逻辑代码, 用resolve和reject 改变状态, 并且弹出 操作结果

状态改变的Promise会自动调用自身的then()or catch() Api , 添加回调,返回新的Promise对象, 并根据

之前Promise状态,决定新的Promise 执行 哪一个回调

# 解决回调的问题

## 回调的常见问题

1. 用回调判断异步操作结果的成功与否, 方法五花八门不好学习
2. 我们希望统一格式,明确判断异步操作是否成功,不用付出额外成本
3. 回调地狱



## 异步编程的解决方案-Promise

Promise 是异步编程的一种解决方案，比传统的异步解决方案【回调函数】和【事件】更合理、更强大。现已被 ES6 纳入进规范中。

可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口(也就是A+规范)，使得控制异步操作更加容易。

- 统一了判断异步结果的格式
- 部分解决了回调地狱

看一个例子

```javascript
fs.readFile('./l.txt', (error, content)=>{
      if(error){
      //失畋
      console.log("呜呜")
}else{
        //成功
     console.log("哈哈")
     }
})
//采用promise 优化上述代码
// 执行异步操作,然后执行then()  如果异步操作成功,执行第一个回调函数,反之执行第二个函数
var readFilePromise = new Promise (function(resolve,reject){
    readFile('./l.txt')
})
readFilePromise('./l.txt').then(function(){},function(){})
```



`promiss`语法

```javascript
// 对于 形如 A(B(C(D)))  也就是 A(fun(fun(fun()))) 这样的形式
//采用Promise 优化

A-Promise(function(){},function(){}) // 这是B 
	.then(function(){},function(){}) // 这是C
	.then(function(){},functioon(){}) // 这就是D
//A-Promiss成功的话,调用第一个fn,否则调用第二个fn,然后再执行then,then成功话就执行第一个fn,否则第二个fn
```

**小结:**

- 通过then()解决回调地狱
- 通过传入两个函数参数,解决不统一的问题



## 🎈感性认知

先看一个简单的例子[setTIme回调地狱](http://js.jirengu.com/sawem/1)

[Promise解决回调地狱](http://js.jirengu.com/luduz/1)



我们一般是如何使用Promise的呢?

1. 创建一个Promise对象   new Promise((resolve,reject)=>{ })
2. Promise函数会传入一个箭头函数作为参数,该箭头函数有两个固定的参数   resolve 和 reject
  
   1. resolve和reject 参数也是函数
3. resolve( ) 函数调用  会转到 then() ;  reject( error) 函数调用会跳转到catch( )
4. then(  ()={ }  )  

   1. then函数 会把一个箭头函数作为参数
   2. 箭头函数每次都要返回一个promise对象 
   3. Promise 函数的用法一致

5. catch((error)=>{ console.log(error) })

   1. then函数 会把一个箭头函数作为参数
   2. 一般箭头函数会接收一个参数, 然后把错误打出来

   

   

# Promise深入

## promise逻辑

![](assets\Promise-5.jpg)

⑴创建Promise函数

```javascript
  new Promise((resolve,reject)=>{
      异步操作
      在异步操作的回调函数代码中调用resolve
  })
```

⑵所谓包裹

把异步操作代码,放到Promise的回调函数中

⑶异步操作的三种状态

1. 等待结果
2. 成功
3. 失败

⑷ reject和resolve回调函数根据异步操作的状态执行

1. 这部分的逻辑Promise封装好了,我们不用写if-else ,直接用就好了
2. resolve()回调负责把异步结果带到 then()函数
3. reject()回调负责把异步结果带到catch()函数

## 细节深入

㈠如何得到Promise对象

1. 往Promise() 里面塞  executor

Promise( executor) 构造函数   的 executor

```js
const promise = new Promise(executor)
```

executor是一个箭头函数

```js
(resolve,reject)=>{
    resolve()
}
```

箭头函数有两个参数, 这两个参数又都是回调函数

自己写判断逻辑,true 就执行 resolve回调  , false就执行reject回调

2. Promise.resolve(executor)

```
promise.resolve((resolve,reject)=>{
    resolve()
})
```

㈡ Promise对象原型

方法

1. then()

当Promise的状态变成fulfilled状态, 也就是包裹的异步操作成功了

变成fulfilled状态Promise会执行这个方法

**then()的作用:**

-  添加下一步操作 的成功回调和失败回调给「当前Promise 对象」,返回一个新的Promise对象
- 新的Promise对象会执行其中一个回调,并将回调的结果 resolve或reject 出去

```js
promise.then(res=>{
    console.log("成功!")
    res = res+1
    resolve(res)
})
```

2. catch()

添加一个拒绝(rejection) 回调到当前 promise, 返回一个新的promise。当这个回调函数被调用，新 promise 将以它的返回值来resolve，否则如果当前promise 进入fulfilled状态，则以当前promise的完成结果作为新promise的完成结果.

3. finally()

添加一个回调给当前对象,在当前对象执行完毕时,返回一个promise对象,

不管这个返回的promise状态是什么, 都执行添加的回调

㈢执行细节

reject和resolve的功能

1. 返回异步操作的结果 / 返回异步错误的原因
2. 改变当前的Promise的状态

| Promise状态 改变前                                | 改变后                          |
| ------------------------------------------------- | ------------------------------- |
| *pending*: 初始状态，既不是成功，也不是失败状态。 | *fulfilled*: 意味着操作成功完成 |
| *pending*                                         | *rejected*: 意味着操作失败。    |

then()和catch()的功能

1. 添加回调给当前的promise对象 
2. 根据resolve和reject确定当前对象的状态 , 返回同样的状态的对象

fuifilled状态的promise对象会执行添加的成功回调 , rejected状态的会执行添加的失败回调



想要某个函数?拥有promise功能，只需让其返回一个promise即可。

```js
requset(config){
    return new	Promise((reject,resolve)=>{
        const instance = axios.create(config)
        instance.then(res=>{
            console.log(res)
            resolve(res)
        })
    })
}
```

这样 requset函数就具备了Promsie的功能!!!



## 关于then()

then的另外一种用法

```js
then(()=>{},()=>{})
```

有两个回调作为参数, 第一个回调是成功回调,第二个回调是失败回调, 取代了catch()

```js
.then((res)=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})
```



这样的处理方式存在bug

看一个例子

```js
axios({
    url:"xxx"
}).then(s1,e1)  //第一责任人
.then(s2,e2)    // 第二责任人
.then(s3,e3)    // 第三责任人
```

第一责任人,给axios添加回调,并且会返回新的Promise对象

不管怎么样,axios的状态一定会发生改变 , 这样的新的Promise,就必然会执行添加的回调中的一个

同理 , 这样思考,就可以理清楚关系了

----

---

---

---



# 自己写Promise  

> 实在理解不来,没关系,记住下面的模板逻辑,先背下来, 一边用 , 一边理解

```js
function ajax (){
    return new Promise((resolve,reject)=>{
        做事
        成功:resolve(res) //改变当前Promise状态,并且返回res 处理结果
        失败:reject(err)
    })
}
var promise = ajax()
promise.then(res=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})
or
promsie.then(successFn,errFn)
```



# await/async

Promise返回的是promise对象,并且经常异步的,

如果我们希望使用Promise的数据,就要等待他完成, 这样写代码就很难受

所以发明了await语法,等待Promise完成

```javascript
let 用户信息 = await 获取用户信息()
console.log(用户信息)

如果 promise的结果错误 ,就会打印错误消息

```

## await

解决js单线程的异步报错  ==> **不等待报错**

在异步操作前面,加上 await 关键字 , 告诉 js引擎, 相关代码暂且不执行

进一步包含 await语句的函数也需要    **不明显异步函数声明**





## try  catch

```javascript
try{
	let 用户信息 = await 获取用户信息()
	console.log(用户信息)
}catch(error){
    console.log(error)
}
catch 相当于 .then(undefined,function(){})	

```



## async例子

```javascript
function fn (){
	let 用户信息 = await 获取用户信息()
	console.log(用户信息)
    return 用户信息
}
var s = fn()
console.log(s)
// 上述代码会报错
// 因为 浏览器认为fn()是同步函数,但是他里面有await异步操作 所以fn是一个异步函数,浏览器那里直到啊,必然报错
// 解决办法: 异步函数声明
async function fn (){
	let 用户信息 = await 获取用户信息()
	console.log(用户信息)
    return 用户信息
}
var s = await fn()
console.log(s)
```



# 常用API

1. *Promise.all()*
2. [`Promise.prototype.allSettled()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)
3. [`Promise.prototype.catch()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)
4. [`Promise.prototype.finally()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)
5. [`Promise.prototype.then()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)
6. [`Promise.race()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)
7. [`Promise.reject()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)
8. [`Promise.resolve()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)



all ([promise1,promise2]).then() 所有的promise 都成功了才调用 then()

**race([promise1,promise2]).then()  任意一个promise成功了就调用then()**

catch() <==> then(undefined,function(){})  没有成功回调的then()

finally(function(){})   无论成功还是失败,回调函数都是一个(成功和失败回调合并)









