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



什么是回调?

在调用fn1时候,回调栈里面记住fn1,运行fn1的代码,发现要调用fn2(),

此时需要切换到fn2的运行环境里面去,然后调用f'n2,回调栈会记住fn2!这样一个过程就是回调!

==简而言之     B(A) 就是回调==



为什么要有回调?

- 代码异步,需要回调去处理
- 可以很简单的传递数据
  - 上述例子中B就很简单的把数据'’riven' 传递给了A 



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

​		这就被称为回调地狱!



# 解决回调的问题

## 回调的常见问题

1. 用回调判断异步操作结果的成功与否, 方法五花八门不好学习
   1. [js深入浅出-02]()
2. 回调地狱



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



## 感性认知

先看一个例子

```javascript
function timeout(ms) {
  return new Promise(
      (resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  }
  );
}

timeout(100).then((value) => {
  console.log(value);
});
```

我们一般是如何使用Promise的呢?

1. 写一个方法,方法返回一个promise对象,往`promsie`的构造函数传入一个函数,这个函数里面有我们想要的异步操作,同时玩这个函数里面传入两个形参 resolve,reject,这两个形参,都是PromsieA+规范里面的方法

2. 方法调用时,会返回一个Promise对象,这个Promise对象去调用then()   我们会往then()里面传入函数,这个函数就是平时写的回调函数,回调函数会接收一个参数,这个参数就是异步操作返回的结果,我们在回调函数里面处理这个结果

3. then()

   1. 为了满足链式调用,让结构更美,所以每次都要返回一个promsie对象,then()里面用到的函数也都需要返回一个promise对象
   2. 里面会有两个回调,第一个回调时成功回调,第二个回调是失败回调
   3. then().then().then()的操作逻辑
      1. 如果第一个then()执行是成功回调,那么就会执行第二个then()里面的成功回调
      2. 如果第一个then()执行的是失败回调,但是失败回调 解决的问题,而不是把失败抛出,那么依旧会执行下一个then()成功回调
      3. 如果第一个then()执行的是失败回调,并且失败回调 没有解决问题,反而是抛出了问题,那么就会执行下一个then()的失败回调
   4. 逻辑:
      1. 成功回调是执行事务,返回结果给下一个成功回调使用的
      2. 失败回调是处理错误的,如果处理成功,那么就会去执行成功回调,然后执行下一个成功回调;处理失败,把原因传给下一个失败回调,直到解决到错误,或者把最后的错误抛给调用者,也就是我们

   

   

   再看例子2

   ```javascript
   获取用户信息()
       .then(打印用户信息)
   	.then(获取另一个用户信息)
   	.then(打印另一个用户信息)
   //为了实现上述 优美的代码解构  (结构就很类似于队列)
   
   	// 实现 各个方法
   function 获取用户信息(){
       return new Promise(function(resolve,reject){
       执行操作,获取到用户信息  
   	resolve("用户:大鹏")
   })
   }
   function 获取用户信息(){
       return new Promise(function(resolve,reject){
   		//这个value就是上一个resolve信使 传递下来的
           console.log(value)
           resolve()
   })
   }
   function 获取另一个用户信息(){
       return new Promise(function(resolve,reject){
   		执行操作,获取到另一个用户信息
         resolve("用户:小白")  
   })
   }
   ```

   [Promise 运行code](http://js.jirengu.com/biney/1)

   

then()的两次回调的区别  例子3

[then的案例](http://js.jirengu.com/sikit/1)



Promise里面嵌套`Promsie`   例子4

也就是then里面的回调是一个promise , 队列中的函数有自己的子队列

[Promise嵌套](http://js.jirengu.com/dituf/1)



reject的作用是把 pending --> rejected

如果失败函数没有调用reject,就会执行第二个成功回调





# await/async

Promise返回的是promise对象,并且经常异步的,

如果我们希望使用Promise的数据,就要等待他完成, 这样写代码就很难受

所以发明了await语法,等待Promise完成

```javascript
let 用户信息 = await 获取用户信息()
console.log(用户信息)

如果 promise的结果错误 ,就会打印错误消息

```

try catch

```javascript
try{
	let 用户信息 = await 获取用户信息()
	console.log(用户信息)
}catch(error){
    console.log(error)
}
catch 相当于 .then(undefined,function(){})	

```



async例子

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

race([promise1,promise2]).then()  任意一个promise成功了就调用then()

catch() <==> then(undefined,function(){})  没有成功回调的then()

finally(function(){})   无论成功还是失败,回调函数都是一个(成功和失败回调合并)









