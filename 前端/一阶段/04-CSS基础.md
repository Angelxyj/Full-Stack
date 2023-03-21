# CSS 基础

## 什么是 CSS

    CSS 指 层叠 样式 表 (Cascading Style Sheets)
    
    WEB标准中的表现标准语言
    简单说就是如何修饰网页信息的显示样式
    目前推荐遵循的是W3C发布的CSS3.0
    1998年5月21日由w3C正式推出的css2.1

## CSS 的语法

    	由两个主要的部分构成：选择器，以及一条或多条声明
    
    	选择器通常是需要改变样式的 HTML 元素
    
    	每条声明由一个属性和一个值组成
    		属性（property）是希望设置的样式属性（style attribute）
    		属性必须放在花括号中，属性与属性值用冒号连接
    		当一个属性有多个属性值的时候，属性值与属性值用空格隔开
    		每条声明用分号结束，最后一条声明可以不加分号，建议还是加上
    		在书写样式过程中，空格、换行等操作不影响属性显示
    
    	eg: 选择器 { 属性1: 属性值; 属性2 : 属性值 }
    
    	CSS 注释以 /* 开始, 以 */ 结束，快捷键 Ctrl + /
    
    特点：
    	层叠性：在同一个声明中，同一个css属性，多次去书写css属性值，以最后一个为准

## CSS 的单位

    1、颜色值：颜色名称、RGB数值、十六进制数值
    	颜色名称：black === 黑色、white === 亮白、
    			 red === 大红 ......
    
    	RGB数值：rgb(0~255, 0~255, 0~255)
    	RGBA数值：rgba(0~255, 0~255, 0~255,[0.0 ~ 1.0]) Alpha 透明度
    
    	十六进制数值：#000000、#ffffff
    		0、1、2、3、4、5、6、7、8、9和字母A、B、C、D、E、F（a、b、c、d、e、f）表示，
    		其中:A~F表示10~15
    
    2、相对单位：所设置的对象受屏幕分辨率、可视区域、浏览器设置以及相关元素的大小
    	px：根据屏幕像素点来确定的
    	em：表示元素的字体高度，能够根据字体的 font-size 属性值来确定单位的大小
    	百分比：通过另一个值来计算，一般参考父元素中的相同属性的值
    	URL:

## CSS 的分类

    	内联样式（行内样式，行间样式，嵌入式样式，内嵌样式）
    		直接在标签内部加入 style属性
    		<div style="属性名: 属性值; 属性名: 属性值;">
    		几乎不用
    
    	内部样式表
    		一般是在 <head></head> 标签中引入一个 <style></style>,在其内部给元素添加样式
    		也可以写在任意的位置
    
    		使用场景：适合案例或者短小的页面开发
    
    	外部样式表
    		在css文件夹中创建一个css文件，在html文件内把外部样式表引入
    		两种引用方式、两种引用外部样式表方法的区别
    			<link rel="stylesheet" href="css文件的路径"/>
    				rel（relation）：用于定义文档关联，表示关联样式表
    			<style>
    				@import url("css文件的路径");
    				@和import之间没有空格 url和小括号之间也没有空格；必须结尾以分号结束;
    			</style>
    
    		使用场景：适合整站或者比较长的页面开发
    
    		面试题：两者的区别是什么？
    		<link>	推荐使用
    		        1、属于XHTML
    		        2、优先加载CSS文件到页面
    				3、任何浏览器都能识别
    				4、能够使用javascript控制 dom 去改变样式的时候，只能使用link标签
    		@import
    		        1、属于CSS2.1
    		        2、先加载HTML结构在加载CSS文件
    				3、只有在IE5以上的才能识别
    
    	多重样式优先级：
    			一般情况下：内联样式 > 内部样式 > 外部样式 > 浏览器默认样式
    			需要注意的是：如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式
    			! important----优先级最高，加在修饰后面

## CSS 设置样式

### 选择器（设置样式的元素）

    基本选择器
    
    	1、通配符选择器
    		* {
    
    		}
    		* 选择的是当前页面中所有的元素
    
    	2、元素选择器
    		元素名称 {
    
    		}
    		选择的是 当前页面中的 某一类元素
    
    	3、id 选择器
    		# id {
    
    		}
    		id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式
    		id 是独一无二的
    
    	4、class 选择器
    		.class {
    
    		}
    		class 选择器用于描述一组元素的样式，
    		class 选择器有别于id选择器，class可以在多个元素中使用
    		一个元素可以设置多个类名
    
    	5、群组选择器
    		使用逗号（,）表示
    		把多个选择器写在一起，方便样式的组织管理，组内的选择器是任意类型的
    
    	6、伪类选择器
    		伪类，用于向某些选择器添加特殊的效果。
    		用伪类定义的样式并不是作用在标记上，而是作用在标记的状态上
    
    		设置 a标签 的四种状态
    		:link访问前的状态
    		:visited访问后的状态
    		:hover鼠标移入效果
    		:active激活状态(按下未松开)
    		l-v-h-a : love-hate按照顺序书写
    
    		需要注意的是：除了超链接的4种伪类选择器之外，其他伪类和伪对象选择器不被 IE6 及其以下版本浏览器支持
    
    	7、后代选择器
    		使用空格（ ）表示
    		前面的一个选择器表示包含框对象的选择器，而后面的选择器表示被包含的选择器
    
    	8、伪元素选择器
    		伪元素，是html中不存在的元素，仅在css中用来渲染的，伪元素创建了一个虚拟容器，
    		这个容器不包含任何DOM元素，但是可以包含内容
    
    		::after		在选择器后增加内容
    		::before	在选择器前增加内容
    		::first-letter	选择器的首字母
    		::first-line	选择器的首行
    		::selection		被用户选取的部分


    	优先级规则：
    		规则1：最近的祖先样式比其他祖先样式优先级高
    		规则2："直接样式"比"祖先样式"优先级高
    		规则3：优先级关系：内联样式 > ID 选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 元素选择器 = 伪元素选择器
    		规则4：权重原则	（内联） 1000、（id）  0100、（类，伪类，属性）  0010、（元素，伪元素）  0001、其他  0000、继承属性没有权重
    		规则5：属性后插有 !important 的属性拥有最高优先级,若同时存在，则再利用规则 3、4 判断优先级

### 声明（设置样式的语句）

#### 字体属性

    1、字体的系列：  font-family:
    	family-name   	字体名称
    
    	"微软雅黑",Arial,"arial narrow";
    
    	当字体是中文字体时，需加双引号；
    	可以把多个字体名称作为一个"回退"系统来保存。如果浏览器不支持第一个字体，则会尝试下一个
    	使用某种特定的字体系列（Geneva）完全取决于用户机器上该字体系列是否可用；这个属性没有指示任何字体下载
    	因此，强烈推荐使用一个通用字体系列名作为后路
    
    2、字体的大小：  font-size:
    	length			设置为一个固定的值
    	%				基于父元素的一个百分比值
    
    	谷歌浏览器默认字体大小16px,允许设置的最小字体12px
    	单位还可以是 pt，9pt = 12px;
    	为了减小系统间的字体显示差异，IE Netscape Mozilla的浏览器制作商于1999年召开会议，
    	共同确定在默认情况下16px/ppi为标准字体大小默认值,即 1em.
    
    3、字体的样式：  font-style:
    	normal			默认值,浏览器显示一个标准的字体样式
    	italic			显示一个斜体的字体样式
    	oblique			显示一个倾斜的字体样式
    
    4、字体的粗细：   font-weight:
    	normal			默认值,定义标准的字符
    	bold			粗体字符
    	lighter			更细的字符
    	100~900			由细到粗的字符,400 等同于 normal，而 700 等同于 bold
    
    字体属性的简写
    	font: font-size font-weight font-style font-family;

#### 文本属性

    1、文本的颜色：  color:
    	十六进制值(0-9A-F),
    	一个RGB值(0-255)
    	颜色的名称
    	eg: 纯白 #fff / rgb(255,255,255) / white
    		纯黑 #000 / rgb(0,0,0) / black
    
    2、文本的对齐方式：  text-align:
    	left  			排列到左边
    	right  			排列到右边
    	center  		排列到中间
    	justify			两端对齐 ???
    
    3、文本的行高：  line-height:
    	normal			默认，设置合理的行间距
    	number			设置数字，此数字会与当前的字体尺寸相乘来设置行间距
    
    	特殊用法：单行文本的垂直居中，将行高设置为和父元素高度一致
    
    	当单行文本的行高小于容器高时，可实现单行文本在容器中垂直中齐以上；
    	当单行文本的行高大于容器高时，可实现单行文本在容器中垂直中齐以下
    
    4、文本的线条修饰：  text-decoration:
    	none			默认，定义标准的文本
    	underline		文本下方的一条线
    	overline		文本上方的一条线
    	line-through	穿过文本的一条线
    
    5、缩进元素中文本的首行：  text-indent:
    	可以设置负数，但是不建议使用 !!!
    	text-indent: 2em;
    	em 是什么？
    	em 是相对长度单位，他会继承父级元素的字体大小，因此不是一个固定的值
    
    6、设置元素的垂直对齐：  vertical-align:
    	baseline		默认,元素放置在父元素的基线上
    	top				把元素的顶端与行中最高元素的顶端对齐
    	bottom			使元素及其后代元素的底部与整行的底部对齐
    	middle			把此元素放置在父元素的中部
    		-	需要配合行高line-height使用
    
    	面试题：解决img下方3-6像素间距问题 --- 行内块元素存在的问题
    	给img的父元素设置 font-size:0 
    	
    	img {
    		vertical-align: top;值不是默认的 baseline 即可 方案一
    		
    		float:left; 方案二
    		
    		display:block; 方案三
    	}
    
    7、设置字间距：	letter-spacing
    	normal			默认，定义单个字间的标准空间
    	length			定义单个字间的固定空间
    
    	每一个中文文字作为一个“字”，而每一个英文字母也作为一个“字”
    
    8、设置词间距：  word-spacing:
    	normal			默认，定义单词间的标准空间
    	length			定义单词间的固定空间
    
    	以空格为基准进行调节，如果多个单词被连在一起，则被word-spacing视为一个单词
    	如果汉字被空格分隔，则分隔的多个汉字就被视为不同的单词，word-spacing属性此时有效
    
    9、设置元素中空白的处理方式：	white-space:
    	normal			默认，空白会被浏览器忽略
    	nowrap			文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止
    
    10、元素中的字母：  text-transform:
    	none			默认，定义带有小写字母和大写字母的标准的文本
    	capitalize		文本中的每个单词以大写字母开头
    	uppercase		全部转换为大写字母
    	lowercase		全部转换为小写字母
    	验证码案例使用 --- js

#### 背景属性

    0、使用背景属性时，需要先设置一个空间 留存使用，设置宽高
    
    1、背景的颜色：  background-color
    	设置颜色透明度	rgba (255, 255, 255l, [0 ~ 1] )
    
    2、背景的图像：  background-image
    	URL			图像的URL
    
    3、背景图像是否及如何重复：	background-repeat
    	repeat		默认，背景图像将向垂直和水平方向重复
    	repeat-x	只有水平位置会重复背景图像
    	repeat-y	只有垂直位置会重复背景图像
    	no-repeat	background-image不会重复
    	inherit		从父元素继承 background-repeat 属性
    
    4、背景图像的起始位置：	background-position
    	background-position:0px 0px;
    	第一个值代表水平方向,正值代表向右移动
    	第二个值代表垂直方向,正值代表向下移动
    
    	background-position:right bottom;背景图在右下角
    	水平：left左、right右、center居中
    	垂直：top上、bottom下、center居中
    		也可以使用固定的数值
    	精灵图的使用 ---
    
    5、背景图像是否固定或者随着页面的其余部分滚动：  background-attachment
    	scroll		默认，背景图片随着页面的滚动而滚动
    	fixed		背景图片不会随着页面的滚动而滚动
    	知乎案例 ---
    
    背景属性的简写:
    	background: background-color background-image background-repeat background-attachment background-position[先是水平位置，再是垂直位置]
    	背景色 和 背景图的顺序可以进行对调
    
    	同时设置多张背景图 --- 需要注意的是 多张背景图的先后顺序，先写的在最上方显示
    	background:
    		background-image background-repeat background-position,
    		background-image background-repeat background-position,
    		background-image background-repeat background-position；
    	
    		背景大小单独去设置，按照多背景设置图片的顺序去设置背景大小
    		background-size: url 1(大小) , url 2(大小)
    	小黄人案例 ---

##### 精灵图

    什么是精灵图？
    	css精灵(CSS sprites),是一种网页图片应用处理技术
    	主要是指将网页中需要的零星的小图片集成到一个大的图片中
    为什么要用它？
    	1.减少对浏览器的请求次数，避免网页的延迟
    	2.方便小图标的统一管理
    如何使用？
    	利用CSS的“background-image”，“background- repeat”，“background-position”的组合进行背景定位
    	background-position可以用数字能精确的定位出背景图片的位置

#### 列表属性

    简写为：ul { list-style: none; }  去除列表项标志
    
    1、列表项标志的类型：  list-style-type:
    	none			无标记
    	disc			默认,标记是实心圆
    	circle			标记是空心圆
    	square			标记是实心方块
    	decimal			标记是数字
    			自定义文本
    
    2、列表中列表项标志的位置：  list-style-position
    	outside			默认值，保持标记位于文本的左侧。列表项目标记放置在文本以外，且环绕文本不根据标记对齐
    	inside			列表项目标记放置在文本以内，且环绕文本根据标记对齐
    
    3、将图像设置为列表项标志：  list-style-image
    	none			默认，无图形被显示
    	URL				图像的路径
    	设置之后，会直接覆盖 list-style-type的值
    
    4、特殊的伪类选择器： ::marker {  }

#### 边框属性

    1、边框的样式：  border-style:
    	none 			默认无边框
    	dotted			定义一个点线边框
    	dashed			定义一个虚线边框
    	solid			定义实线边框
    	double			定义两个边框,两个边框的宽度和 border-width 的值相同
    	
    2、边框的宽度：  border-width:
    
    3、边框的颜色：  border-color:
    	name			指定 颜色名称
    	RGB				指定 RGB 值
    	Hex 			指定 十六进制值
    
    利用边框制作一个三角形
    	.box{
    		width: 0px;
    		height:0px;
    		border-top: 100px solid transparent;
    		border-bottom: 100px solid yellow;
    		border-left: 100px solid transparent;
    		border-right: 100px solid transparent;
    		简写为：
    		border: 100px solid transparent;
    		border--bottom-color: yellow; // 设置单边上的颜色
    	}
    	- transparent代表透明色
    利用边框制作一个梯形
    	.box {
            width: 80px;
            height: 80px;
            border: 50px solid transparent;
            border-top-color: skyblue;
        }
    
    需要注意的是：border-color单独使用是不起作用的，必须得先使用border-style来设置边框样式
    
    可简写属性： border: 宽度，样式，颜色

#### 溢出属性

    	指定在元素的内容太大而无法放入指定区域时是剪裁内容还是添加滚动条
    	仅适用于具有指定高度的块元素
    
    	overflow:
    		visible:	默认。溢出没有被剪裁。内容在元素框外渲染
    		hidden:		溢出被剪裁，其余内容将不可见
    		scroll:		溢出被剪裁，同时添加滚动条以查看其余内容
    		auto:		与 scroll 类似，但仅在必要时添加滚动条
    
    	overflow-x 和 overflow-y
    		属性规定是仅水平还是垂直地（或同时）更改内容的溢出
    
    		overflow-x 指定如何处理内容的左/右边缘
    		overflow-y 指定如何处理内容的上/下边缘
    
    	用途：
    		面试题：文本溢出隐藏,超出部分显示...或者截取，前提必须有宽度
    
    		显示...	text-overflow: ellipsis;
    		截取		text-overflow: clip
    		单行文本：
    		CSS: {
    			width: ___px;
    			white-space: nowrap;
    			overflow: hidden;
    			text-overflow: ellipsis;
    		};
    
    		多行文本：
    		CSS: {
    			width: ___px;
    			overflow:hidden;
    			text-overflow:ellipsis;
    			-webkit-line-clamp:2;
    			display:-webkit-box;
    			-webkit-box-orient:vertical;
    		}
    		使用了WebKit的CSS扩展属性，该方法适用于WebKit浏览器及移动端
    
    		1.-webkit-line-clamp 用来限制在一个块元素显示的文本的行数
    		为了实现该效果，它需要组合其他的WebKit属性
    		常见结合属性：
    		2.display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示
    		3.-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式

## CSS 的特性

    1、层叠性  ---  长江后浪推前浪，前浪死在沙滩上
    	多种CSS样式的叠加，是浏览器处理冲突的一个能力,
    	如果一个属性通过两个相同选择器设置到同一个元素上，那么这个时候一个属性就会将另一个属性层叠掉
    
    	一般情况下，如果出现样式冲突，则会按照CSS书写的顺序，以最后的样式为准
    
    	1.样式冲突，遵循的原则是就近原则。 那个样式离着结构近，就执行那个样式
    	2.样式不冲突，不会层叠
    
    2、继承性  ---  龙生龙，凤生凤，老鼠生的孩子会打洞
    	指书写CSS样式表时，子标签会继承父标签的某些样式
    	想要设置一个可继承的属性，只需将它应用于父元素即可
    		可继承的属性有：
    			字体系列属性：font、font-family、font-weight、font-size、font-style
    			文本系列属性：color、text-align、line-height、text-decoration、text-indent......
    			列表系列属性: list-style
    			光标系列属性：cursor
    			元素可见属性：visibility
    3、优先级  --- 胜者为王，败者为寇
    	定义CSS样式时，经常出现两个或更多规则应用在同一元素上，这时就会出现优先级的问题
    
    	1.继承样式的权重为0
    		即在嵌套结构中，不管父元素样式的权重多大，被子元素继承时，他的权重都为0，也就是说子元素定义的样式会覆盖继承来的样式
    	2.行内样式优先
    		应用style属性的元素，其行内样式的权重非常高，可以理解为远大于100。总之，他拥有比上面提高的选择器都大的优先级
    	3.权重相同时，CSS遵循就近原则
    		也就是说靠近元素的样式具有最大的优先级，或者说排在最后的样式优先级最大
    	4.CSS定义了一个!important命令，该命令被赋予最大的优先级
    		也就是说不管权重如何以及样式位置的远近，!important都具有最大优先级
