---
title: html基础
date: '2012-12-29'
description: html5和html不是一个概念，学习html5和学习html不是一个概念。
categories:
- 'html'
tags:
- html5
- html
- css
---

如果你一直从事程序相关的工作，比如说程序员，但是并没有深入研究过前端。可能你会和我有一样的困惑。`html+css`或者前几年比较流行的说法`div+css`，好像很简单，html不就是几个标签，css不就是几个渲染效果，为什么别人用得那么溜，为什么在我手里就这么不对，到处都是错位。

那么，你或许应该和我一起熟悉下以下这几个概念

###文档流

什么叫做`文档流`？
将窗体自上而下分成一行行，并在每行中按从左至右的顺序排列元素，即为文档流。

有三种情况使得元素脱离文档流而存在，分别是`浮动`，`绝对定位`，`固定定位`。

没错，这就是文档流，就像写信一样，从左至右，自上而下，就这么简单，互联网一开始就是为了浏览这些文档。

但是，这样不利于排版，于是，早期的互联网专家们为了更方便的写出更直观的文档，他们制定了HTML并且规定了一些基本原则，比如，所有的元素分为三类：`块级元素(block)`，`内联元素(inline)`，`可变元素(这个其实可以不算进去)`。

####块级元素(block)
每个非浮动块级元素都独占一行，浮动元素按规定负载行的一端。若当前行容不下，则另起新行浮动。

块元素（block element）

+ address - 地址
+ blockquote - 块引用
+ center - 举中对齐块
+ dir - 目录列表
+ div - 常用块级容易，也是css layout的主要标签
+ dl - 定义列表
+ fieldset - form控制组
+ form - 交互表单
+ h1 - 大标题
+ h2 - 副标题
+ h3 - 3级标题
+ h4 - 4级标题
+ h5 - 5级标题
+ h6 - 6级标题
+ hr - 水平分隔线
+ isindex - input prompt
+ menu - 菜单列表
+ noframes - frames可选内容（对于不支持frame的浏览器显示此区块内容）
+ noscript - 可选脚本内容（对于不支持script的浏览器显示此内容）
+ ol - 排序列表
+ p - 段落
+ pre - 格式化文本
+ table - 表格
+ ul - 非排序列表

####内联元素(inline)
任何不适块级元素的课件元素都是内联元素。其表现的特性是`行布局`，这里的行布局就是说其表现形式始终以行进行。

内联元素（inline element）

+ a - 锚点
+ abbr - 缩写
+ acronym - 首字
+ b - 粗体（不推荐）
+ bdo - bidi override
+ big - 大字体
+ br - 换行
+ cite - 引用
+ code - 计算机代码（在引用源码的时候需要）
+ dfn - 定义字段
+ em - 强调
+ font - 字体设定（不推荐）
+ i - 斜体
+ img - 图片
+ input - 输入框
+ kbd - 定义键盘文本
+ label - 表格标签
+ q - 短引用
+ s - 中划线（不推荐）
+ samp - 定义范例计算机代码
+ select - 项目选择
+ small - 小字体文本
+ span - 常用内联容器，定义文本内区块
+ strike - 中划线
+ strong - 粗体强调
+ sub - 下标
+ sup - 上标
+ textarea - 多行文本输入框
+ tt - 电传文本
+ u - 下划线
+ var - 定义变量

####可变元素
可变元素为根据上下文语境决定该元素为块元素或者内联元素。
不用纠结这个概念，它就是块元素或者内联元素。

可变元素
+ applet - java applet
+ button - 按钮
+ del - 删除文本
+ iframe - inline frame
+ ins - 插入的文本
+ map - 图片区块（map）
+ object - object对象
+ script - 客户端脚本

####块元素与内联元素及其它

+ 不管是块元素还是内联元素都是长方形的盒子，这就是下面要讲的盒子模型。
+ 两者之间可以通过`display:inline;`或者`display:block;`互相转换。
+ 浮动元素不占任何正常文档流空间，而浮动元素的定位还是基于正常的文档流，然后从文档流中抽出并尽可能远的移动至左侧或者右侧。文字内容会围绕在浮动元素周围。当一个元素从正常文档流中抽出后，仍然在文档流中的其他元素将忽略该元素并填补他原先的空间。(浏览器的兼容性里浮动是个大问题是因为各个浏览器对浮动理论的理解不同，这里写得应该是IE的理解)
+ 基于文档流，我们可以很容易理解以下定位模式：

1. `相对定位(relative)`，即相对于元素在文档中的位置进行偏移。但保留原占位符。
2. `绝对定位(absolute)`,即完全脱离文档流，原占位不保留，相对于position属性非static值的最近父级元素进行偏移。
3. `固定定位(fixed)`，即完全脱离文档流，相对于浏览器的内容窗口进行偏移。

###盒子模型

关于`盒子模型(box model)`，必须要了解的就是IE模型和W3C模型的差异，这也是造成我们需要解决各浏览器兼容性的主要方面！下面这张图说明了这种差异性的本质。

<img src="{{urls.media}}/htmlbasic/box_models.png" alt="盒子模型的差异性">

其实也就是说，在w3c盒子模型中，width和height定义的仅仅是content的宽高，但在IE模型中定义的是包括border和padding在内的真正的`盒子`的宽高，很明显IE模型反而更为厚道一些，在w3c模型中，一个布局都已定义好的页面，若是想更改某个box的padding，你必须同时更改这个box的content的width和height，而在IE模型中你只需改变padding。这确实无疑是w3c的一个失误，不过它已在CSS3中有所修正。

另外，我之前没有注意到的一点是，原来各个盒子之间的margin并不是相加，而是看谁的大就用谁的。
可是有个问题我仍在疑惑，并没有实验取证：
嵌套的盒子模型之间，里面盒子的margin和外面盒子的padding是相加还是怎么个处理方式？

然后，就可以说到关于解决IE模型和W3C模型差异的方法，以下是我收集的各种方法，可供参考：

####1.当然是神奇的CSS HACK.

此处可参考[博客园] (http://www.cnblogs.com/WuQiang/archive/2011/08/23/2150240.html)，虽然我它应该也是抄的，而且就是抄的百度词条的。

	<!DOCTYPE html>  
	<html>  
	<head>  
	    <title>Css Hack</title>  
	    <style>  
	    #test   
	    {   
	        width:300px;   
	        height:300px;   
	        background-color:blue;      /*firefox*/
	        background-color:red\9;      /*all ie*/
	        background-color:yellow\0;    /*ie8*/
	        +background-color:pink;        /*ie7*/
	        _background-color:orange;       /*ie6*/
	    }  
	    :root #test { background-color:purple\9; }  /*ie9*/
	    @media all and (min-width:0px){ #test {background-color:black\0;} }  /*opera*/
	    @media screen and (-webkit-min-device-pixel-ratio:0){ #test {background-color:gray;} }  /*chrome and safari*/
	    </style>  
	</head>  
	<body>  
		<div id="wrapper-test">
	    <div id="test">test</div>  
		</div>
	</body>  
	</html>
我对CSS HACK也不是很了解，此处只是作为一个笔记留些示例笔记在此处。

####2.!important：

在IE7还没出现之前，曾有过这样的处理方式，比如上面的#test
我们这样处理就可以解决在不同的浏览器实现不同效果。

	#test{
			background-color:blue !important;/*for firefox*/
			background-color:red;/*for ie6*/
			}

原理很简单，IE6不识别!important。

####3.终极解决方案：

本来只有一个content层，比如#test，再外面加一个wrapper层，比如#wrapper-test
div#wrapper-test用来实现padding,margin和border,不定义width和height
div#test用来定义height和width,padding和margin，border都定义为0

	#wrapper-test{
			padding: 20px;
			margin: 20px;
			border: 1px solid red;
			}
	#test {
			margin:0;
			padding:0;
			border:none;
			height:100px;
			width:100px;
			background-color:blue;
			}

以上这一段在任何浏览器应该都有你期望的显示了！

####4.未来解决方案：
在css3中，有一个属性box-sizing,它有两个可选值：content-box和border-box，选用后者，则会按IE模型处理盒子。所以你可以这样：
	
	textarea { 
		-webkit-box-sizing: border-box; /* Safari/Chrome, other WebKit */
		-moz-box-sizing: border-box;    /* Firefox, other Gecko */
		box-sizing: border-box;         /* Opera/IE 8+ */
	}
	
在css3中，关于盒子的大小控制，排列，布局有很多有意思的新东西。

-------------------------

关于box model还有另外一个方面是需要注意的，我们初学者容易忽视的，width:auto的情况以及一些例外。

+ 当position为static(默认及无定义)或者relative时，box会自动延伸至上级box的内部空间宽。
+ 当position为absolute或者fixed时，box会自动收缩至盒内content所撑开的宽度。
+ 关于float的element，还在实验取证中，不过解决方案是凡是浮动元素一律加上固定的宽高。

####关于浮动闭合
几种解决方案：

	.clear{clear:both:height:0;overflow:hidden;}
	/*需要清除的地方加div.clear*/


	.clearfix:after{content:".";display:block;height:0;clear:both;visibilty:hidden}
	.clearfix{*+height:1%;};
	/*浮动元素的父元素添加class="clearfix"*/

	.clearfix{overflow:auto;_height:1%;}

	.clearfix{overflow:hidden;_zoom:1;}

盒子模型大概是CSS最难搞定的地方了吧，网上的文档乱得一塌糊涂，哪位同学或者高人有介绍盒子模型的资料推荐么，中文英文都OK，网上的或者纸质书本也都OK。

###布局 流动布局vs固定布局vs弹性布局vs响应式布局

+ 固定布局(Fixed Layout) 采用px作为block的单位。
+ 流动布局(Fluid Layout) 采用%百分比作为block的单位，指定min-width和max-width。
+ 弹性布局(Elastic Layout) 采用em作为block的单位。
+ 响应式布局(Responsive Layout) 其实和前三者没有必然关系，主要是通过mediaqueries兼容不同的屏幕大小。

其实混合使用，才会有最佳的解决方案，响应式布局将屏幕大小分为几个大块，380～720，760～1280，更大等等，流动布局确定某个block的大小，更好的将黄金分割率及三等分割原则用于设计，em用于一些LOGO或者ICO的大小指定，使得其不失去提醒的功能，固定布局则可用于留白等等，当然，这只是我的以为，具体设计还得积累经验。


###层叠
即z-index,需要注意的是z-index仅仅在设置了position属性的元素上起左右，即设置了position为absolute,fixed或者relative，在static上不起作用。

###样式优先级
几个原则：
!important的用户样式>!important的作者样式>作者样式>用户样式>浏览器定义的样式
作者样式中：元素内定样式>内嵌样式表>导入样式表(@import)>外链样式表
`用户样式`即用户在浏览器里自己写配置文件定义的样式。

具体到每条规则遵循以下规则：

	对于规则中的每个ID选择符，特殊性加0，1，0，0 
	对于规则中每个类选择符和属性选择符以及伪类，特殊性加0，0，1，0 
	对于规则中的每个元素名或者伪元素，特殊性加0，0，0，1 
	对于通配符，特殊性加0，0，0，0. 
	对于内联规则，特殊性加 1，0，0，0 
	
比方说：

	<!DOCTYPE HTML>
	<html lang="en">
	<head>
	        <meta charset="UTF-8">
	        <title></title>
			<style type="text/css">
			p{
					color:red;
					}
			/*特殊性为0,0,0,1*/
			
			div p{
					color:blue;
					}
			/*特殊性为0,0,0,2*/
			#test {
					color:yellow;
					}
			/*特殊性为0,1,0,0*/

			</style>
	</head>
	<body>
	       <div>
	               <p class="test"></p>
	       </div> 
	</body>
	</html>

很容易理解，0,1,0,0>0,0,0,2>0,0,0,1，所以字体颜色为黄色。

另外，如果同等特殊性的规则你写两次的话，后面那个优先级大于前面那个。
比如

	:active{color:Red;}
	:hover{color:Blue;}
	:visited{color:Purple;}
	:link{color:Green;} 

传说是不会出现红色和蓝色的。
你最好这样写：

	:link{color:Green;}  
	:visited{color:Purple;}
	:hover{color:Blue;}
	:active{color:Red;}

不过，这个问题我倒从来没遇到过，也许现在浏览器智能了。
	

###继承
+ 通配符的特殊性比继承得高。
+ border,margin,padding等不会被继承（好像关乎box-model的不会被继承）。

本人第一次发文，请轻拍！
