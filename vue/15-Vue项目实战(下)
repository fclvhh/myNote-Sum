p170对 –-> Vue项目上的回顾

# ㈥Better Scroll 的问题解决和思考



## bug1: scrollHeight

图片加载延时,导致scrollHeight计算失误,==>可滚动距离相对于实际过小

要监听图片加载,  回调执行 refresh()  重新计算scrollHeight

① 关于监听

```html
<item  :@onlaod="imgOnload"> </item>
```

问题:

​	这是在GoodsListItem 监听的 ,  回调需要获取scroll对象 执行refresh()方法

​	但是 scroll对象在home组件里面 , 直接隔了 GoodsLIst组件 , 是**爷孙关系**, 数据传递就存在麻烦

解决思路:

1. 两次父子组件通信完成爷孙通信    但是太麻烦了
2. Vuex 状态管理

```js	
this.$store.scroll
```

让组件共享 scroll这个对象 , 这样 GoodsListItem孙组件就可以直接调用了

3. 事件总线 $bus

$bus 就相当于 store 

```js
$bus.$emit("xxx")  //发送一个自定义事件xxx到 总线
$bus.$on("xxx",()=>{
    
})
// 在home组件的create hook中 执行上述代码 , 会从home组件创建时,就监听自定义事件xxx
// 完成了 item到 home的通信
```

小理解:

​	子传父 , $emit()    同一个屋子走着去就好

​	孙传爷 , 路太远 , 做bus 方便啊

怎么获得$bus:

​	在main.js 中

```js
Vue.prototype.$bus = new Vue()
```



## bug2: 组件加载快慢

refresh() 封装

```js
refresh(){
    this.scroll.refresh()
}
```

这样的封装存在 出现小概率bug的问题 , 就是 可能home组件创建比较慢 , this.$refs.scroll 没执行

拿不到scroll对象 , 那么this.scroll 为null对象 , 执行refresh() 自然报错



正确写法

```js
refresh() {
        this.scroll && this.scroll.refresh && this.scroll.refresh()
 }
```

用逻辑与 :  只要有一个错误 , 整体就不行

​	先判断this.scroll对象是否可以拿到 , 「拿到说明home组件加载完毕」

​	然后判断refresh 属性是否可以拿到 , 拿到说明 scroll组件加载完毕

​	最后自然是 执行属性方法





## bug3: refresh 频繁执行

前面代码的逻辑是为每一张图片都增加监听 , 但是一个page有30个img 

导致 refresh() 频繁执行 , 造成困扰

需求:

​	让refresh() 在1s中只执行一次好不好?

实现:

​	频繁刷新的防抖动函数





## 防抖和节流

先看一个开发中常见的 输入框案例:

需求:

1. 监听输入框的内容
2. 根据内容,向服务器发送请求, 实现 **搜索提示功能**

问题:

1. 每输入一个字符,都要监听 , 并且根据输入的字符串 向服务器发送请求 
2. 这样的方式 是不是太频繁了 , 造成及其大的性能损耗

> 因为,ajax 请求需要消耗服务器搜索数据库的计算资源
>
> 浏览器拿到数据需要js处理,展示到页面 ,消耗内存和cpu资源
>
> 开销很大 , 不适合频繁操作

3. 场景再现

用户输入时: 通常都是一口气输入很多内容  如: "rx5700"

但是浏览器是一个一个字符去监听的 , 导致会发送6次请求给服务器 , 实在是太多了 ,造成卡顿

很有必要等待1s,给用户时间去输入完整的信息 , 

再去服务器数据库检索 ,效率才高





**防抖:**

> 防止单位时间内,向服务器发送大量请求 ,  减轻服务器的压力!

单位时间内,事件被触发不是立即执行 , 而是等待一会

换句话说:

​	单位时间内 , 事件被触发了n次 , 等待一会后 ,执行最近的一次事件回调

**节流:**

> 单位时间内，只能触发一次函数。

单位事件内,事件只能被触发一次,



**小结:**

事件触发与回调函数

基本关系:

​	事件触发==>回调函数

①在源头打断 , 不让事件触发   称为节流

②在==>打断 ,  不然回调执行    称为防抖



### 防抖函数实现

debounce() 防抖函数实现

```js
debounce(fun,time-delay){
    // 用timer来记录setTimeout()返回的计时器对象
    let timer = null
    // 返回一个函数
    return function(...args){
        // 如果计时器存在 , 就清空他
        // 意思就是如果fun被调用过一次,肯定也delay了部分时间
        // 来到清除计时器这一步,说明这不是第一次 , 再次执行
        if(timer){
            clearTimeout(timer)
        }
        // 为再次执行的fun ,设置计时器 
        
        timer = setTimeout(()=>{
            fun.apply(this,args)
        },time-delay)
    }
}
```

第一个参数: 传入频繁执行,需要防抖的函数

第二个参数: 传入阻断时间



机制: 传入频繁执行的fun , 返回一个不会频繁执行的新的fun



理解:  

​	本来单位时间内 , refresh 要执行n次

​	但是debounce 只会让refresh 再delay时间内只执行一次, 其他次的执行会被取消

核心:

​	事件触发了 , 但是回调代码是空的 , 因为回调函数执行的函数被取消了











-----





清除refresh的抖动实操:

![](assets\Vue项目-7.jpg)



截图失误了 , 出bug

![](assets\Vue项目-8.jpg)



## bug4: tabControl吸顶失败

没有引入scroll做滚动时 , 用的是浏览器原生的滚动 

```css
.tabControl {
    position:sticky:
}
```



但是用了scroll , 用的第三方自定义的滚动 , 导致上述css失效



**实现思路:**

必须要知道滚动到多少,开始有吸附效果

① 获取到tabControl的offsetTop

只有元素才有offsetTop , 所以要去item组件的template模板中的元素才能拿到 数值



如何取到对应的元素?

所有组件都有一个$el 属性: 用于获取组件template里面的元素

```js
this.$refs.tabCotrol.$el.offsetTop
```

`$el ` 和`$refs`  的区别

1. `$refs` 可以拿到组件对象  
2. `$el` 可以拿到真实的展示在html文档中的元素



② offsetTop数值 问题

图片加载慢 , 导致这个值 计算不准确

监听加载最慢的轮播图,只要这个加载完成了 , 那么基本上tabControl 元素上面的所有图片都加载完毕

因此

1. 给home-swiper组件添加图片加载监听事件

```html
<img :src="item.image" @onload="imageLoad" alt="">
```

2. 实现回调

```js
methods:{
    imageLoad(){
        // 发送自定义事件给父组件 scroll
        this.$emit("swiperImageLoad")
    }
}
```

3. 触发home组件上的swiperImageLoad

```js
swiperImageLoad(){
     // 回调,拿到 tabControl元素在html中的高度
     // tabOffsetTop是data里的数据 , 用来存储
    this.tabOffsetTop = this.$refs.tabControl.$el.offsetTop
}
```



③吸顶判断

根据tabControl 滚动的距离 和 tabControl原本距离视口顶部的距离 做对比

一旦   滚动距离> 原本的距离  ==>说明tabControl 来到了视口顶部 , 可以考虑吸附了

```js
contentScroll(position) {
		    // 1.决定tabFixed是否显示
        this.isTabFixed = position.y < -this.tabOffsetTop

        // 2.决定backTop是否显示
        this.showBackTop = position.y < -BACKTOP_DISTANCE
}
```

④ 实现吸顶效果

⑴ 绑定class , 布尔值

```html
<tab-control class="{fixed:isTabFixed}" @itemClick="tabClick"
                 :titles="['流行', '新款', '精选']">
</tab-control>
```

```css
.fixed {
    position:fixed;
    top:44px;
    left:0;
    right:0;
}
```

结果:  丢失目标 , 因为scroll插件 导致的bug  

滚动元素的fixed 失效



⑵ 在home组件结构上部 添加一个tabControl 不参与滚动 , 根据 吸顶判断 , 决定是否展现就好

因为 , 反正参与滚动的tabControl会滚出去

```html
<tab-control v-show="isTabFixed" class="fixed" @itemClick="tabClick"
                 :titles="['流行', '新款', '精选']"></tab-control>
```



v-show 决定是否展示



## Better Scroll 的思考

本质: 

​	代替浏览器原生的滚动 , 自己模拟了一套滚动机制

步骤:

​	wrapper相当于　原生滚动的视窗　，　内容在视窗上进行滚动

结果：

​	很多原生的东西不能用，scroll有自己的一整套东西

​	自己的API

内部实现导致的 浏览器原生的

1. position:fixed ; position:sticky; 失效
2. 用自己内部的Api





# ㈦ 上拉加载更多 等

scroll组件的 __initScroll() 

```js
// 3.监听上拉到底部
        this.scroll.on('pullingUp', () => {
          console.log('上拉加载');
          this.$emit('pullingUp')
        })
```

scroll插件对象监听事件 ,发送自定义事件到home组件

```html
<scroll class="content"
            ref="scroll"
            @scroll="contentScroll"
           == @pullingUp="loadMore"==
            :data="showGoodsList"
            :pull-up-load="true"
            :probe-type="3">
```

home组件的自定义事件pullingUp 触发 ,执行loadMore回调

```js
loadMore() {
    this.getHomeProducts(this.currentType)
}
```

看一下getHmeProducts函数   也在home methods 中

```js
getHomeProducts(type) {
        getHomeData(type, this.goodsList[type].page).then(res => {
          const goodsList = res.data.list;
          this.goodsList[type].list.push(...goodsList)
          this.goodsList[type].page += 1

          this.$refs.scroll.finishPullUp()
        })
```

scroll 上的属性简单分析

pull-up-load

![](assets\Vue项目-9.jpg)

![](assets\Vue项目-10.jpg)

scroll pullUp事件的bug , 事件触发时, 只会执行一次回调 , 要用finishPullUp() 解决

![](assets\Vue项目-11.jpg)





## home保持原来的状态







# ㈧详情页面

跳转到详情页面:

①给 GoodsListItem 添加点击事件

```js
goToDetail: function () {
        // 1.获取iid
        let iid = this.goods.iid;

        // 2.跳转到详情页面
        this.$router.push({path: '/detail', query: {iid}})
      }
}
```



② 进行路由跳转

这里用方法 push, 因为很可能需要返回

③ 传递商品 id , 方便服务器查询数据库

1. 方式一： 动态路由

```js
this.$router.push（'/detail/'+:iid）
```

2. 方式二：query参数

```js
this.$router.push ({
    path:'/detial',
    query:{iid}
})
```

this.iid = goods.iid

④ detail组件拿到 根据查询参数 , 拿到商品id

```js
const iid = this.$route.query.iid
```

⑤ 在detail的create() hook 发送ajax请求 , iid 作为查询参数

得到数据 , 进行数据的分析 , 拿到详情展示需要的数据

⑥ 数据的分析

轮播图的图片资源: 放在一个数组里 , 很容易就拿出来

​	但是 img 过大 , 需要处理

```css
img {
    height:300px;
    overflow:hidden;
}
```



⑦ 复杂数据的处理

多个json对象,中的部分属性 取出来 , 放到一个类中,

**构造一个完美适合需求的对象**

```js
export class Goods {
  constructor(itemInfo, columns, services) {
    this.title = itemInfo.title;
    this.desc = itemInfo.desc;
    this.newPrice = itemInfo.price;
    this.oldPrice = itemInfo.oldPrice;
    this.discount = itemInfo.discountDesc;
    this.columns = columns;
    this.services = services;
    this.nowPrice = itemInfo.highNowPrice;
  }
}
```



上述代码, 集合了三个对象中有用的数据 , 集合到了一个class中

Goods 这个类 , 完美的符合项目的需求



数据整和:

​	数据整合之后 , 再展示





## 锚点效果

思路:

1. 点击 跳转   => scrollTo 做
2. 滚到对应位置  选项联动 =>和backTop 思路差不多

> 区别:
>
> ​	发送给Detail组件的自定义事件 , 改变导航的 currentIndex 就成了





# ㈨购物车功能

给购物车按钮 添加点击事件 , 绑定回调 addToCart

回调里发送自定义事件 addToCart 给 父组件 Detail , 触发绑定的回调方法  addToCart



addToCart方法

```js
addToCart() {
        // 1.创建对象
        const obj = {}
        // 2.对象信息
        obj.iid = this.iid;
        obj.imgURL = this.topImages[0]
        obj.title = this.goods.title
        obj.desc = this.goods.desc;
        obj.newPrice = this.goods.nowPrice;
        // 3.添加到Store中
        this.$store.commit('addCart', obj)
      }
```

将购物车 的item 所需要的数据封装到一个对象模型里面

方便我们通过这个对象容器 , 展示购物车item 的数据

同时这个对象也会添加到store , 方便不同的组件交流



Vuex最大的应用 差不多就是购物车功能的实现 , 各个组件都可以通过store 共享数据





Vuex 里的代码



```js 
carlist=[]  // 里面放state对象
```



mutation的 addCart()

需要进行对象的判断:

1. 该商品是否已经加入了购物车
2. 加入了就修改  对象的count属性
3. 没加入 , 把info对象 push 到carlist 数组



```css
.box {
    height:100vh;
    display:grid;
    align-items:center;
    justify-items:center;
}
```



​	

