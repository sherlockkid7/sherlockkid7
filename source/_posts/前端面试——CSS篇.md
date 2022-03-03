---
title: 前端面试——CSS篇
date: 2020-07-06 15:40:20
tags: [CSS]
categories: 面试
---

### 1.盒子水平垂直居中的五大方案

```css
.container{
        width: 400px;
        height: 400px;
        margin: 50px auto;
        background-color: #f5f5f5;
    }
.box{
        width: 100px;
        height: 100px;
        background-color: #000;
    }
```

定位（三种）    前提条件：盒子的父元素设置相对定位

> 1.box盒子设置绝对定位，`left：50%;top:50%;`,再使用margin移动自身宽高的一半（已知box盒子的宽高）

```css
 .container{
        position: relative;
    }
    .box{
        position: absolute;
        top:50%;
        left:50%;
        margin-top: -50px;
        margin-left: -50px;  
    }
```

> 2.box盒子设置绝对定位，并且left,right,top,bottom设置为0,margin:auto（未知box盒子的宽高）

```css
.container{
    position: relative;
}
.box{
    position: absolute;
    top:0;
    left:0;
    right:0;
    bottom: 0;
    margin:auto; 
}
```

> 3.box盒子设置绝对定位，`left：50%;top:50%;`，再使用css3的transform属性，设置为`translate（-50%,-50%）;`移动自身宽高的一半，有兼容性问题（未知box盒子的宽高）

```css
.container{
    position: relative;
}
.box{
    position: absolute;
    top:50%;
    left:50%;
    transform: translate(-50%,-50%);
}
```

> 4.给居中盒子的父元素使用display:flex，再设置水平居中`justify-content:center；`垂直居中`align-items:center;`（可以不用设置box盒子的宽高，让盒子内容撑开宽高，有兼容性问题）

```css
 .container{
        display: flex;
        justify-content: center;
        align-items: center;
    }
```

5.使用js,需要给container设置相对定位，并且给container和box设置id(js可以根据id获取该元素)

```js
<script>
        let cWidth = container.offsetWidth,
            cHeight = container.offsetHeight,
            bWidth = box.offsetWidth,
            bHeight = box.offsetHeight;
        box.style.position = "absolute";
        box.style.left = (cWidth-bWidth)/2 + 'px';
        box.style.top = (cHeight-bHeight)/2 +'px';
    </script>
```

### 2.盒模型

盒模型是元素在网页中的实际占位，浏览器把元素（标签）看成一个形象的盒子，每个盒子（即标签）都会有内容(width,height)，边框(border)，以及内容和边框之间的缝隙（即内间距padding），还有盒子与盒子之间的间距（即外边距margin）,

盒模型包括两种：IE盒模型和标准(W3C)盒模型

#### **标准(W3C)**盒模型

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200531130423268.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

> **标准(W3C)盒子模型：**内容content+内边距padding+边框border+外边距margin
>
> 在标准盒子模型中，width 和 height 指的是内容区域的宽度和高度。
> CSS如何设置标准盒模型       box-sizing:content-box;

写页面时经常遇到，当增加内边距、边框和外边距时，不会影响内容区域的尺寸，但是会增加元素框的总尺寸。
解决方法：box-sizing:border-box;

#### IE盒模型

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200531130514396.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

> **IE盒子模型：**内容（content+padding+border）+ 外边距margin
>
> IE盒子模型中，width 和 height 指的是内容区域+border+padding的宽度和高度。
> CSS如何设置IE盒子模型   box-sizing:border-box;

### 3.清除浮动的方法

> 清除浮动是为了解决子元素浮动而导致父元素高度塌陷的问题

清除浮动的常用方法：

1. 父元素定义`height`

2. 额外标签法

   在有浮动的子元素末尾插入一个没有内容的块级元素， 并添加样式`clear:both`

3. 给父级元素使用伪元素

   `.clearfix::after { content:"";display: block;clear:both;}`

4. 触发父元素BFC

    给父元素添加样式`overflow`为`hidden`或`auto`，通过触发BFC方式，实现清除浮动

### 4.BFC

BFC（ Block Formatting Context ）即`块级格式上下文`，简单的说，BFC是页面上的一个隔离的独立容器，其内部元素和外部元素不会相互影响

**如何触发BFC？**

- 根元素(`<html>`)
- 浮动元素（元素的 float 不是 none）
- 绝对定位/固定定位元素（元素的 position 为 absolute 或 fixed）
- overflow 值不为 visible 的块元素
- 元素`display`的值为 inline-block 或 table-cell 或 table-caption 或 grid

BFC的应用场景

- **清除浮动**：BFC内部的浮动元素会参与高度计算

- **避免某元素被浮动元素覆盖**：BFC的区域不会与浮动元素的区域重叠
- **阻止外边距重叠**：属于同一个BFC的两个相邻Box的margin会发生折叠，不同BFC不会发生折叠

### 5.position属性

| 值       | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| relative | 生成相对定位的元素，相对于其正常位置进行定位，不脱离文档流。 |
| absolute | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。 |
| fixed    | 生成固定定位的元素，相对于浏览器窗口进行定位。               |
| static   | 默认值。没有定位，元素出现在正常的文档流中                   |
| sticky   | 粘性定位,元素在跨越特定阈值前为相对定位，之后为固定定位      |

### 6.CSS选择器

- 通用选择器(*)
- 类选择器(.wrap)
- id选择器（#wrap）
- 标签选择器（div, h1, p）
- 相邻选择器(h1 + p)
- 子选择器（ul > li）
- 后代选择器（li a）
- 伪类选择器（li:first-child，a:hover）

> 优先级（同权重情况下样式定义最近者为准）：!important > [ id > class > tag ]
> !important 比内联优先级高
>
> 元素选择符的权值：元素标签（派生选择器）：1，class选择符：10，id选择符：100，内联样式权值最大，为1000

### 7.rem和em

rem和em都是相对单位，主要参考的标签不同：

- rem是相对于根节点`<html>`的字体大小`font-size`实现的，浏览器默认字号是font-size:16px
- em是相对于父元素的`font-size`，和百分比%类似，%也是相对于父级的，只不过是%相对于父级宽高，而em相对于父级字号

### 8.vw和vh

vw：viewpoint width，视窗宽度，1vw相当于视窗宽度的1%

vh：viewpoint height，视窗高度，1vh等于视窗高度的1%。

vw、vh 与 % 百分比的区别：

- % 是相对于父元素的大小设定的比率，vw、vh 是视窗大小决定的。
- vw、vh 优势在于能够直接获取高度，而用 % 在没有设置 body 高度的情况下，无法正确获得可视区域的高度

### 9.手机端如何做适配的

目前前端主要做适配的方法有：百分比，em,rem,媒体查询(即media query),flex布局（即弹性盒），vw,vh等

我在项目中主要用的是rem，flex布局，有时会用到媒体查询

主要操作：

1.在页面头部设置meta声明的viewport属性。

```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
//width、height、initial-scale、maximum-scale、minimum-scale、user-scalable这些属性，分别表示宽度、高度、初始缩放比例、最大缩放比例、最小缩放比例、是否允许用户缩放
```

2.主要使用了一个手淘的js库[flexible.js](http://caibaojian.com/t/flexible-js)，在页面变化时,动态获取当前视口宽度width,除以一个固定的数n,得到rem的值,赋值给font-size

```js
//设置根元素字体大小。此时为宽的100等分
document.documentElement.style.fontSize = ocument.documentElement.clientWidth / 100 + 'px';
```

但是如果每次都要手动去计算(px=>rem)有点太过麻烦了，我们可以通过cssrem插件（在VScode中）在编辑的时候直接计算转成rem。所以在开发的时候直接按照设计稿的尺寸写px，编译后会直接转化成rem

### 10.display: none和 visibility:hidden的区别

都是隐藏对应的元素，不同点：

- display: none 

  不占据空间，会触发reflow（回流），进行渲染。元素及其子元素都会消失。

- visibility:hidden 

  占据空间，只会触发repaint（重绘），因为没有发现位置变化，不进行渲染。

  visibility是继承属性，若子元素使用了visibility:visible，则不继承，这个子孙元素又会显现出来。

### 11.CSS预处理器

平时写css用过less、stylus，可以定义变量，嵌套，混入等功能，方便开发人员快速高效写css

最流行的CSS预处理器

- less
- sass
- stylus
- postcss

### 12.CSS 页面常用布局

中间自适应，两边定宽

#### 圣杯布局

- 三者都设置向左浮动
- 设置中间部分宽度为100%;
- 设置负边距， left设置负左边距为100%, right设置负左边距为负的自身宽度
- 设置container的padding值给左右两个子面板留出空间
- 设置两个子面板为相对定位，`left面板`的left值为负的`left面板`宽度，`right面板`的right值为负的`right面板`的值

但是圣杯布局有个问题：**当面板的部分比两边的子面板宽度小的时候，布局就会乱掉**。因此也就有了双飞翼布局来克服这个问题。如果不增加任何标签，想实现更完美的布局非常困难，因此双飞翼布局在主面板上选择了加一个标签

```html
<div class="container clearfix">
        <div class="center"></div>
        <div class="left"></div>
        <div class="right"></div>
</div>
```

```css
	/* 解决浮动引起的高度塌陷 */
	.clearfix::after{
        content: "";
        clear: both;
        display: block;
    }
	.container{
        padding: 0 200px;
    }
    .center,.left,.right{
        float: left;
    }
    /* 中间部分宽度自适应 */
    .center{
        width: 100%;
        min-height: 400px;
        background-color: #34495e;
    }
    .left,.right{
        width: 200px;
        min-height: 200px;
    }
    /* 左边栏使用margin-left移动中间部分的宽度，再使用相对定位向左移动自身的宽度 */
    .left{
        margin-left: -100%;
        background-color: #2ecc71;
        position: relative;
        left:-200px;
    }
    .right{
        margin-left: -200px;
        background-color: #3498db;
        position: relative;
        right:-200px;
    }
```

#### 双飞翼布局

 - left、container、right都设置左浮动 
 - 设置container宽度为100%
 - 设置负边距，left设置margin-left为-100%，right设置margin-left为负的自身宽度
 - 设置center的margin值为left和right宽度，为左右两个侧栏留出空间

```html
<div class=" clearfix">
        <div class="container">
            <div class="center"></div>
        </div>
        <div class="left"></div>
        <div class="right"></div>
</div>
```

```css
.clearfix::after{
        content: "";
        clear: both;
        display: block;
    }
    .container,.left,.right{
        float: left;
    }
    .container{
        width: 100%;
    }
    
    .center{
        margin: 0 200px;
        min-height: 400px; 
        background-color: #34495e; 
    }
    .left,.right{
        width: 200px;
        min-height: 200px;
    }
    .left{
        margin-left: -100%;
        background-color: #2ecc71;
    }
    .right{
        margin-left: -200px;
        background-color: #3498db;
    }
```

双飞翼布局与圣杯布局的主要差别在于：

- 双飞翼布局给主面板（中间元素）添加了一个父标签,用来通过margin给子面板腾出空间

- 圣杯布局采用的是padding,而双飞翼布局采用的margin, 解决了圣杯布局的问题
- 双飞翼布局不用设置相对定位，以及对应的left和right值

#### Flex弹性布局

缺点：仅支持 IE9 以上浏览器

```html
<div class="container">
        <div class="left"></div>
        <div class="center"></div>
        <div class="right"></div>
</div>
```

```css
.container{
        display: flex;
        min-height: 400px;
    }
    .left,.right{
        background-color: #95a5a6;
        width: 200px;
        /* flex: 0 0 200px; */ 与使用宽度能够实现同样的效果
    }
    .center{
        flex:1;
        background-color: #000;
    }
```

#### Grid 布局(网格布局)

grid-template-columns属性定义每一列的列宽
grid-template-rows属性定义每一行的行高

```html
<div class="container">
        <div class="left"></div>
        <div class="center"></div>
        <div class="right"></div>
</div>
```

```css
.container{
        display: grid;
        grid-template-rows: 400px;
        grid-template-columns: 200px auto 200px;/* 设置列宽，也可使用百分比 */
    }
     .left,.right{
        background-color: #95a5a6;  
    } 
```

### 13.CSS3动画

CSS3动画大致上包括两种：

- 过渡动画：主要通过`transition`来实现，通过设置过渡属性，运动时间，延迟时间和运动速度实现。
- 关键帧动画：主要通过`animation`配合`@keyframes`实现

transition动画和animation动画的主要区别：

- transition动画需要有触发条件，animation不需要
- transition只有开始结束两种状态，而animation可以实现多种状态，并且animation是可以做循环运动甚至是无限运动

### 14.CSS3新增特性有哪些

边框 => 圆角：border-radius, 盒阴影：box-shadow

动画 => transition(过渡动画),animation（关键帧动画）

背景 => background-clip（背景图的绘制区域）, background-size(背景图的尺寸),background-origin(背景图的定位区域)

2D(3D)转换 => transform（是指变换、变形，是 css3 的一个属性，和 width，height 属性一样）,其属性值有scale | translate（是指元素进行 2D(3D)维度上位移或范围变换）|rotate| perspective（3D转换）

线性渐变 => linear-gradient