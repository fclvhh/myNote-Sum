# 起源

说到前端框架 , 就总会谈论到「双向绑定」和「单向绑定」这些概念

但是要理解这些概念 , 最好从其最原始的形态入手 , 也就是自己搞出双向绑定和单向绑定

这就要说到mvc了

Backbone.js是基于MVC思想的



# 意大利面条式的代码

> 实现没有规划 , 就直接完成业务需求
>
> 导致代码组织的很烂 , 不从从头开始''吸'' 面条会 , 不知道代码什么意思
>
> 可读性很差

[意大利面条](https://jsbin.com/noraye/8/edit?html,js,output)

**要点:**

```js
 fetchDb() {}    //从数据库拿数据
saveDb(newData) {}  //把数据保存到数据库
template = `{{xxx}}` // html模板
$(xxx).html(template.replace('{{xxx}}',)) //字符串替换
// 绑定事件和回调 , 操作数据
```



# 一些程序员想出了解决办法

> 业务做多了 , 自然进行页面代码的总结 , 发现这些代码可以分成三类:

1. 专门操作远程数据的代码
2. 专门呈现页面元素的代码
3. 其他控制逻辑的代码



> 为什么分成这三类?=== 抄袭了后端的分类思想

- 专门操作Mysql数据库的代码
- 专门渲染html的代码
- 其他控制逻辑的代码



> 这就是经典的「Web三层架构」, 这些架构慢慢的演化 , 最终被广大程序员完善为MVC思想

1. M对象专门负责 数据
2. V对象掌门负责表现
3. C对象专门负责其他逻辑



> 如果反思一下 , 会发现这个分类是 , 无懈可击的!

1. 每个网页都有数据
2. 每个网页都有表现
3. 每个网页都有其他逻辑

于是乎 , MVC成了经久不衰的设计模式(设计模式就是「套路」的意思)



> [来看MVC思想改写](https://jsbin.com/yuwopuf/3/edit?html,js,output)

改进了一下几点:

1. 代码变成三块有结构有组织的对象 : model,view 和 controller
2. model只负责存储数据 , 请求数据 , 更新数据
3. view只负责渲染HTML(可接收一个data来定制数据)
4. controll负责调度model和view



**代码要点:**

```js
m = {
    data:{} // 用户数据缓存
    fetch(id){} //拿数据
	update()  //更新数据
}
v = {
    el:''  //文档容器
    template:``  //html代码模板字符串
    render(data) {
           //字符串replace 替换拿到的数据
    }                     
}

c = {
    init(options) {
        //负责操作m和v
        //自然需要传入m和v两个对象
    }
    bindEvnets() {
        //事件绑定
    }
}
```



# 模板代码(也就是类)

> 太多的对象

一个页面或模块只需要  model view controller 三个对象

第二个页面就需要再来  model2 view2 controller2 三个对象

第三个页面就需要再来  model3 view3 controller3 三个对象

...….

第N个页面就需要再来  modelN  viewN  controllerN 三个对象



> 太多的重复代码

每写一个model , 都要写很类似的代码

每写一个view 都要写很类似的代码

每写一个controller 都要写很类似的代码



> 为什么不用模板代码(也就是面向对象)把重复的代码写到一个类呢?

[模板代码优化](https://jsbin.com/sodojac/5/edit?js,output)





# 烦人的地方

一般来说 , 如果代码有重复或类似 , 就能优化

上面的「模板代码优化」也有这样的重复代码 , 一个一个来解决



1. 每次model获取数据之后 , 还要「手动」调用this.view.render(this.model.data) ,大约有四个地方调用了update Model

> 解决思路:给Model加上事件机制 
>
> 也就是 添加发布订阅设计模式的代码 

[优化后的代码](https://jsbin.com/sodojac/10/edit?js,output)



2. 有一个bug : 每次render都会更新#appd的innerHTML , 这可能会丢失用户写在页面某一个input的里面的数据

> 解决的思路主要有两个:
>
> 1. 用户只要输入了什么 , 就记录在js的data里面(数据绑定的初步思想)
> 2. 不要粗暴的操作innerHtml , 而是只更新需要更新的部分(虚拟DOM的初步思想就出现了)

Angular 就是基于第一个思想发明的 , 而React则是基于第二个思想发明的



体现在代码上就是:

view 对象多了一个data属性

Model的data是用户数据「从数据库里取出来的」

view里面的数据是 UI 数据 , 「用户操作页面临时产生的」







3. events能不能写到html上 , 而不是写到js里面

> 这样写 ,  更加的直观
>
> react和angular 都采纳了



