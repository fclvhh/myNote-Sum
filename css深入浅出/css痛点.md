# 为什么css难学?



**短回答:css不正交**



# 深入理解宽度和高度!

> 在实际布局的时候,宽度很少需要考虑的,基本上都是自适应,
>
> 所以我们的重点全部放到`Height`上



## 提到高度,就必须要知道是块高,还是内联元素的高!



==文档流:(Normal  Flow)==

> 我觉得翻译成**文档流**是不好的翻译,直接译成**普通流**,反而不会引起歧义



要点:(记忆)

- ''从左到右,从上到下''   
- 默认是自适应布局
- 关于Float
  - 发明出来就是为了实现文字围绕图片排版的,一开始根本就没想那么多
  - 造成后果就是'脱离文档流'
    - 所谓的脱离文档流,说破了就是==**算高度的时候别算上我**==
    - 文字会默认亲近我,会自动贴靠者我布局
  - 其他知识,等待补充
- 多个空格,默认替换成一个空格渲染





## 块级元素的高

> 了解了文档流之后,行内元素的宽高有内容决定,不可以自己设定值
>
> 块元素是可以设置的,但是我们为了更高的普适,最好不要这样做!



###从文字的基础知识谈起

![样图1](D:\一“桶”前端\myNote\css深入浅出\assets\样图1.jpg)

不同的字体,有不同的建议行高(一般是字体尺寸的倍数)

```css
.div{
    font-size:20px;
}
那么div高度就应该是20px,实际却是 20*1.4=28px ; 就是因为不同的字体有不同的默认行高
当然也可以我们自己指定行高,line-height:20px; 那么此时的20px就是div的高度
```





#### 文字居中

垂直水平居中:

```css
.class{
    line-height:height;
    text-align : center;
}
//小技巧,可以利用padding来帮助实现居中
```





## div里面套div,高度如何确定?

> 前面已经知道了div里面套文字,高度是如何确定的!
>
> 也知道了一些文字居中的技巧



出现的问题:

- 父子div的垂直外边距合并
- 子div如何居中







### 什么是垂直外边距重叠?

总结来说:

​	儿子带着爸爸跑路了! 

一旦给儿子垂直margin,就会带着爸爸一起跑路

![样图2](D:\一“桶”前端\myNote\css深入浅出\assets\样图2.jpg)

```css
.father{
    outline:1px solid red;
}
.son{
    outline:1px solid bule;
    height:100px;
    margin-top:100px;
}
//father 也会使用son的margin,从而被撑开,而son 的margin 超出了father
```

如何解决垂直外边距重叠的问题呢?

`给father加上border or paddding`就解决了

或者  给爸爸开启`BFC`!



### 如何居中?

- 内外div的高度确定

```css
//总体思路就是:上下左右都auto , 不就可以居中了吗!
但是,上下的auto不可以用! 烦人......(定位解决)
解决思路:
.son{
    margin: 0 auto;
    position:absolute;
    //上下左右都是0
    top:0;
    bottom:0;
    right:0;
    left:0;
}
```



- 高度不确定

```css
// 用flex 布局
.father{
    display:flex;
    justify-content:center;
    align-items:center;
}
```







## div里面既有内联,又有块级元素,高度怎么办?

**结论:div的`height`由它内部的文档流中的元素的高度总和==决定的==**



### 收尾细节:

内联元素的`height`仅由文字的高度决定

它的`padding`等元素,不可以决定 `height`,但是可以影响宽度



如何实现1;1比例自适应的div?

```css
.class{
    padding-top:100%;
}
```







# css的堆叠上下文

## 堆叠顺序

![堆叠上下文](D:\一“桶”前端\myNote\css深入浅出\assets\堆叠上下文.png)



小结:

**==bb快浮 内容定位   正负相夹==**



## 堆叠上下文

![堆叠上下文](D:\一“桶”前端\myNote\css深入浅出\assets\堆叠上下文-1567651112827.png)



小结:

开启定位 + html根 +opacity   ==> 触发堆叠上下文

注意:

- 同一个堆叠上下文的同一个层次,**后出现**的会覆盖前面的内容
  - 特别的:**浮动元素**里面的文字没有其他文字高
  - **块元素**里面的文字和其他文字在同一层
- html是根元素也开启堆叠上下文,所以所有的小的堆叠上下文都归他管,就和谐统一了



```html
<html>
    <div class="one"  style = "position:relative">
        块3
    </div>
    <div class = "two" style = "position:absolute">
        块4
    </div>
</html>
// 比较块三和块四的堆叠,要看同时包裹他们的html触发的堆叠上下文
// 由于块三和块四同级别,所以四覆盖三
```









# css布局

## 布局总览

![css布局总览](D:\一“桶”前端\myNote\css深入浅出\assets\css布局总览.jpg)



实际上必须要掌握的就是

1. float布局
2. flex布局



## flex布局(flexible:弹性的)

![](D:\一“桶”前端\myNote\css深入浅出\assets\flex布局.png)

要素;

- 主轴和侧轴
- 主尺寸和侧尺寸
- 起点和终点
- Container 和 item



### 优势

- flex布局与方向无关
  - 为什么这么说?
  - 因为块级元素的布局侧重于垂直方向
  - 行内元素的布局侧重于水平方向
- 可以实现
  - 空间的自动分配
  - 元素的自动对齐
- 适用简单的线性布局
  - 更复杂的布局交给grid布局



### Container 属性

- flex-direction:  方向
  - row
  - column
  - reverse 
- flex-wrap: 是否换行
  - 默认不换行
  - wrap 换行
- flex-flow : 上面的简写
- justify-content : 整理版面里面的内容,也就是主轴对齐
  - 决定item是如何对齐的
  - 比如:主轴中心两边占位,等等
- align-item : 侧轴对齐
  - 侧轴方向的
- align-content:等待摸索
  - 这个用的少,以后摸索







### item属性

- flex-grow:分配剩下来的空间给item
- flex-shrink:收缩空间
- flex-basis: 设置默认大小(一般不用)
- flex :上面的缩写
- order : 控制展现顺序 (代替双飞燕布局的)
- align-self;自身对齐方式







