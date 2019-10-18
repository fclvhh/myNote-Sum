# `css`布局原则

> 只有两个布局模式：
>
> 1. 横向布局
> 2. 纵向布局



## 不同布局div的嵌套原则



如下图：

![](D:\Typro图片库\布局理论—div-1565539538501.png)

上面解释了div的包裹原因



## 解决问题手段

当出现与预期不同的布局时，利用加border 的方法去测试

父子div 添加 border

---



# 不要万不得已不要轻易设置width和height

- bug1 
  - 当我们在文档流中微调元素的距离，设置好margin和padding之后，又让div脱离文档流，就会出现布局bug----块元素不再是独占一行，而是被内容撑开
  - 我们需要添加width：100% ；给div1   解决布局问题，让她继续独占一行
  - 此时，div.width+div微调时的padding >body.width     ==》宽度溢出
  - **解决思路**就是在div内层，套上一层div-inner,原本的保护原本在文档流中的微调
    - inner.width+inner.padding = div.width
    - 换句话说，就是把div.padding转嫁给了inner



---

**重要推论：**

> 盒子模型：
>
> 1. 盒子大小：width+padding      （padding是负责子元素位置微调）
> 2. margin负责兄弟盒子位置的微调
> 3. 父div.width = 子div.width+子div.padding
>    - 理解：父盒子的内容区域 = 子盒子的大小
>    - 这个公式可以解释上述方案
>      - div的爸爸是body   ==> body.width = div.width+div.padding 











## 如何减少使用呢？

- 设置span元素使其符合设计的大小 

```css
span原本为100，100
写法一
span{
    display：inline-block；
    height：200px;
    width：150px;
    line-height：200px;
    text—centr:center;
}
写法二
span{
    pddding:25px 50px;
    line-height:100px;
}
用pandding 避免
```





































# 常见布局之横向布局



## 诀窍

让希望横向布局的元素都浮动，并且给他们的爸爸清除浮动











## 微调结构的上下左右参数

- margin是负责兄弟之间的距离
- padding是父子之间的间距
- 看网页，查看他们距离body的参数，然后调整自此元素的位置，使得结构相似



上述技巧足够了

一般是先写完元素的样式，再去微调位置参数

应为实现css的效果可能会对原本的参数产生影响



- `css`的写作习惯
  - 先布局
  - 后样式
  - 最后微调参数







---





# 常见布局之左右布局

**诀窍：**

float套路 + 子元素.width:xx%;

调整两个子元素的大小 a.width:30%  和 b.width：70%

从而实现左右布局







# `css`理论

## css高度由其内部文档流的元素的高度总和决定



**文档流：**

- 文档内，元素流动的方向
  - 行内元素从左往右流
  - 块元素从上往下流（独占一行）

- 细节
  - 行内元素 ，如span,有一个属性 word-break，用来决定是否可以被打断
- 行内元素的**高度和宽度**由==<u>文本</u>==撑开
- 块元素**高度和宽度**由==<u>内容</u>==撑开



