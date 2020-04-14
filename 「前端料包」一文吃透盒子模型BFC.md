## 前言
接触写博客有一段时间了，都是边学边学着写，但总感觉写的凌乱，想起啥写啥。这几天在刷红宝书，收获还是蛮多的，决定结合自己的学习，写一个系列，我叫它 **「前端料包」**，旨在巩固前端基础，努力提升自己，同时也乐于做一个分享者。这个系列包括但不限于下面脑图中的内容，目录和发文顺序暂且如下图，今天带来的是第二篇。
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd2de222f7?w=1016&h=1620&f=png&s=94107)
## 1、什么是BFC
在讲BFC之前，先说一下文档流。我们常见的文档流有三种：浮动流、定位流和普通流，而BFC中的FC就是其中的普通流。

**FC（Formatting context）**
`Formatting context` 是 `W3C CSS2.1` 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。最常见的 `Formatting context` 有 `Block fomatting context` (简称BFC)和 `Inline formatting context` (简称IFC)。此外还有CSS3中新增的GFC（`grid formatting context`）和FFC（`flex formatting context`），此处不做展开。


**BFC 定义**
`BFC(Block formatting context)`直译为"块级格式化上下文"。它是一个独立的渲染区域，只有`Block-level box`参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

> Box
>
> Box 是 CSS 布局的对象和基本单位， 直观点来说，就是一个页面是由很多个 Box 组成的。元素的类型和 display属性，决定了这个 Box 的类型。 不同类型的 Box， 会参与不同的 Formatting Context（一个决定如何渲染文档的容器），因此Box内的元素会以不同的方式渲染。让我们看看有哪些盒子：
> 
> `block-level box:`display 属性为 block, list-item, table 的元素，会生成block-level box。并且参与 block fomatting context； 
> `inline-level box`:display属性为 inline, inline-block, inline-table 的元素，会生成 inline-level box。并且参与
> inline formatting context；
>  `run-in box:` css3 中才有。

## 2、BFC有什么特点

 1.  内部的Box会在垂直方向，一个接一个地放置；
 2.  属于同一个BFC的两个相邻Box的垂直方向margin会发生叠加；
 3. BFC的区域不会与float box叠加；
 4. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然；
 5. 计算BFC的高度时，浮动元素也参与计算；
 6. 每个元素的`margin box`的左边， 与包含块`border box`的左边相接触(对于从左往右的格式化，否则相反)，即使存在浮动也是如此。

## 3、如何触发BFC
满足下列条件之一就可触发BFC
 - 根元素，即HTML元素；
 - 浮动元素：`float` 除 `none` 以外的值；
 - 定位元素：`position (absolute、fixed)`；
 - `display` 为 `inline-block, table-cell, table-caption, flex, inline-flex`；
 - `overflow` 除了 `visible` 以外的值 (`hidden、auto、scroll`)。

## 4、BFC使用场景
**（1）解决margin重叠问题**

由于浏览器解析页面是由上而下的，上下外边距会取最大值，就是所谓的边距重叠问题（对应上面提到的特性2），于是出现如下的布局问题

```html
<head>
<meta charset="UTF-8">
<title>BFC</title>
<style>
    .d1 {
		width: 200px;
		height: 200px;
		background-color: chartreuse;
		margin-bottom: 20px;
		}

    .d2 {
	   	width: 200px;
	  	height: 200px;
		background-color: red;
	 	margin-top: 30px;
	    }
</style>
</head>
<body>
	<div class="d1"></div>
	<div class="d2"></div>
</body>
```

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd2e395c16?w=264&h=546&f=png&s=14204)
我们在网页设计时如果想要垂直方向上的外边距不重叠，可以通过给其中一个元素加一个父元素，并触发父元素的BFC，这样两个div就不属于同一个BFC，垂直方向上的外边距就不会重叠，看代码：
```html
<head>
<meta charset="UTF-8">
<title>BFC</title>
<style>
	.d1 {
		width: 200px;
		height: 200px;
		background-color: chartreuse;
		margin-bottom: 20px;
		}

	.d2 {
	   	width: 200px;
	  	height: 200px;
		background-color: red;
	 	 margin-top: 30px;
		}
	.parent{
		overflow: hidden;	// 触发BFC
		   }
</style>
</head>
<body>
	<div class="parent">
		<div class="d1"></div>
		</div>
	<div class="d2"></div>
</body>
```
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd33d368b6?w=376&h=578&f=png&s=22072)
**（2）用于自适应两栏布局**

```html
<head>
	<meta charset="UTF-8">
	<title></title>
	<style>
        .left{
            width: 100px;
            height: 150px;
            float:left;
            background: greenyellow;
        }
        .right{
            height: 200px;
            background: palevioletred;
        }
    </style>
</head>
<body>
	<div class="left"></div>
	<div class="right"></div>
</body>
```
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd342acfa8?w=337&h=259&f=png&s=8457)
从效果图可以看出，由于`.left`设置了浮动，产生了浮动流（产生了浮动流的元素不能被块级元素看到，只能被设置了inline、文本元素看到），两个元素叠在了一起，但这并不是我们想要的结果，可通过上面提到的BFC特性之3，触发`.right`的BFC来解决这个问题。

```css
.right{
        height: 200px;
        background: palevioletred;
        overflow: hidden;/*触发BFC*/
        }
```

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd2e9c6251?w=562&h=245&f=gif&s=47986)
**（3）清除浮动**

先看一个例子

```html
<head>
	<meta charset="UTF-8">
	<title></title>
	<style>
		#clearFloat{
			background: green;
			border: 2px solid blue;
				}
		#clearFloat .box{
			margin: 5px;
			float: left;
			width: 100px;
			height: 100px;
			background: red;
				}
	</style>
</head>
<body>
	<div id="clearFloat">
		<div class="box">sun1</div>
		<div class="box">sun2</div>
	</div>
```
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd3493248d?w=313&h=189&f=png&s=2956)
从图中可以看到，在父元素没有设置`height`，子元素又`float`的情况下，父元素不会被子元素撑开。这是因为设置了`float`的元素脱离了文档流，飘了起来，不在原来的父元素里了（被掏空了，自然就瘪了）。同样我们可以利用上文提到的BFC特性之5，触发父元素的BFC就可以解决这个问题。

```css
#clearFloat{
		background: green;
		border: 2px solid blue;
		overflow: hidden;/*触发BFC，清除浮动 即使父元素不设宽高，也能被撑开*/
			}
```
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd5a6d05af?w=562&h=245&f=gif&s=78397)
**(4)解决高度塌陷**

先看一个例子

```html
<head>
	<meta charset="UTF-8">
	<title></title>
	<style type="text/css">
		.p-box {
			width: 300px;
			height: 300px;
			background-color: green;
				
			}
			
		.s-box {
			width: 100px;
			height: 100px;
			background-color: cornflowerblue;
			margin: 20px;
			}
	</style>
</head>

<body>
	<div class="p-box">
	<div class="s-box"></div>
	</div>
</body>
```
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd5b8e6b28?w=328&h=328&f=png&s=8402)
样式中给s-box设置了`margin：20px，`但发现上边距没有出来，这时也可以通过触发父元素的BFC来解决。
	![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/11/18/16e7d2fd5f88e024?w=562&h=327&f=gif&s=101309)

## 后话
以上我自己对BFC一点总结，BFC的概念比较抽象，但通过实例分析还是很好理解的，其实上面提到的很多规则我们平时布局中就经常用到，只是没有总结出来。另外，小生乃前端小白一枚，写文章只是为了让自己对该知识点有更深刻的印象和理解，写的东西也很小白，文中如有不对，欢迎指正~ 然后就是希望看完的朋友点个喜欢，也可以关注一波~ 我会持续输出！

[个人博客链接](http://jkvin.cn)

[CSDN个人主页](https://blog.csdn.net/qq_39735040)

[掘金个人主页](https://juejin.im/user/5d999147f265da5b9764b996)

[简书个人主页](https://www.jianshu.com/u/370e80e9707d)
## 参考文章
[10 分钟理解 BFC 原理](https://juejin.im/post/5bc33d0d6fb9a05d1658afc7)

[BFC 神奇背后的原理](https://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html)

[史上最全面、最透彻的BFC原理剖析](https://github.com/zuopf769/notebook/blob/master/fe/BFC%E5%8E%9F%E7%90%86%E5%89%96%E6%9E%90/README.md)
