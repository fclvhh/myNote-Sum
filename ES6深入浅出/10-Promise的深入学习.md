参考资料:

- [阮一峰ES6-Promise](http://es6.ruanyifeng.com/#docs/promise#Promise-all)
- [MDN Prpmise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Promise面试-掘金](https://juejin.im/post/5b31a4b7f265da595725f322#heading-6)
- [八段代码解决Promsie](https://juejin.im/post/597724c26fb9a06bb75260e8#heading-7)
- [简单的Promise源码解析](https://juejin.im/post/5b32f552f265da59991155f0)
- [promised的简单实现](https://www.cnblogs.com/hustskyking/p/promise.html)

# promise究竟是什么?

> Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了`Promise`对象。
>
> 有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口，使得控制异步操作更加容易。

理解:

1. Promise 是一个对象, 里面装着一个异步操作,从这个对象可以获取异步操作的消息。因此一个promise对象就代表一个异步操作!
2. `Promise`对象代表一个异步操作，有三种状态：
   1. `pending`（进行中）
   2. `fulfilled`（已成功）
   3. `rejected`（已失败）
3. 一旦状态改变，就不会再变 !  Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。
4. 如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。
5. `Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。



# Promise怎么用?

`Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

`Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。