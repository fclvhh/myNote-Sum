需求: 批量创建对象

[new全解](https://zhuanlan.zhihu.com/p/23987456)

prototype里面有一个属性construct , 用来表明构造函数是谁



new本质就是一个语法糖

帮我们做了三件事

1. this = { }     创建了一个空对象
2. this.prototype.- - proto - - = xxx.prototype
3. return this



this.prototype.- - proto - - = xxx.prototype 是其中最重要的代码,因为在生产环境中,我们不可以使用- - proto - -这个属性 ,所以 new 帮我们做好了

