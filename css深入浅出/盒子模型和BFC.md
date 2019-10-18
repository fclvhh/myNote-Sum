# 什么是BFC?

![CSS-bfc](D:\一“桶”前端\myNote\css深入浅出\assets\CSS-bfc-1567666161854.png)

**小结如下:**

- 是一个`格式化上下文` (可以类比如堆叠上下文)
- 开启条件`浮动,绝对定位,不是block的块容器,overflow`



-----



![MDN-BFC](D:\一“桶”前端\myNote\css深入浅出\assets\MDN-BFC.png)

**要点如下:**

- display:flow-root  
  - 新的属性:支持BFC
- 其他方式开启BFC,都是存在隐患的,实际工作中几乎不用



----



![real-BFC](D:\一“桶”前端\myNote\css深入浅出\assets\real-BFC.png)

**小结:**

- 没有定义,只有功能和特性





# BFC的功能和特性

## 特性

> BFC负责一个区域:
>
> 	 1. 这个区域里面的元素不准许出去
>   	 2. 这个区域里面的元素,与不准予被骚扰
>          	 1. 如层叠啊,等等带来的骚扰
> 	 3. 只负责管理儿子,孙子交给开启BFC的儿子管理



**小结:**

- BFC就像是一个标准的`女儿奴` 
  - 不准女儿回家晚点
  - 不准别人勾搭女儿
- BFC就负责包裹住内部的元素,不影响别人,也不受别人影响



## 功能

- **爸爸管儿子**
  - 不然孩子出去
  - 这个特性可以解决:垂直外面距合并的问题
    - 垂直外边距的实质就是:儿子把爸爸拐跑了
    - 现在给爸爸开启BFC,那么儿子就出不去了
- 兄弟之间划清界限
  - BFC之间互相不干扰
  - 这个特性可以解决:布局层叠带来的困扰
  - 实例:float实现左右布局



```html css
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
  <style>
.gege{
  height:600px;
  width:100px;
  border:1px solid yellow;
  float:left;
  margin-right:20px;
}
.didi{
  min-height:600px;
  width:100%;(这句不可以写)
  border:3px solid pink;
  overflow:hidden;
}
//要点,didi的宽度不可以写死,否则didi就为了满足宽度,就会自动下去
  </style>
</head>
<body>
  <div class="gege"></div>
  <div class="didi">222</div>
</body>
</html>


```

![](D:\一“桶”前端\myNote\css深入浅出\assets\两列布局效果图.jpg)

