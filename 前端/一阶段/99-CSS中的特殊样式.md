# CSS 中的特殊样式设置

## 做一个三角形

    利用边框属性制作一个三角形
    	.box{
    		width: 0px;
    		border-top: 100px solid transparent;
    		border-bottom: 100px solid yellow;
    		border-left: 100px solid transparent;
    		border-right: 100px solid transparent;
    	}
    	- transparent代表透明色

    需要注意的是：border-color单独使用是不起作用的，必须得先使用border-style来设置边框样式

## 设置文本溢出

    文本溢出隐藏,超出部分显示...或者截取，前提必须有宽度

    	单行文本：
    	CSS: {
    		width: ___px;
    		white-space: nowrap;
    		overflow: hidden;
    		text-overflow: ellipsis;
    	}截取为clip;

    	多行文本：
    	CSS: {
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

## 设置元素隐藏

    display: none

    	设置元素的显示模式
    	1、该属性会让元素完全从DOM中消失，浏览器不渲染设置该属性的元素，不占据DOM树空间
    	2、无法进行事件监听，不可点击
    	3、动态修改该属性会造成重排，性能较差
    	4、非继承性属性！设置了该属性的元素的子元素即使设置display:block依然不会显示它的子元素
    	5、transition不支持display的显示隐藏动画效果，但是display:none不会影响animation的动画效果！

    visibility:hidden

    	设置元素是否可见
    	1、设置该属性的元素依然在DOM中，会被浏览器渲染，占据DOM树空间
    	2、但它无法被监听，因此不可点击
    	3、动态修改该属性会引成重绘，性能较display:none高
    	4、继承性属性！设置该元素的子元素如果修改visibility值是可以显示该子元素的！
    	5、transition支持该属性

    opacity:0

    	设置元素的透明度  从0.0（完全透明）到1.0（完全不透明）
    	1、占据空间，仅仅是设置透明度让该元素不可见
    	2、可以被监听，可以点击
    	3、动态修改不会造成重绘和重排，性能较高！
    	4、非继承性属性！设置该属性元素的子元素若设置opacity:1依然无法显示
    	5、可以配合transition显示淡入淡出效果

    transform:scale(0)

    height:0px;

    position:
    z-index: -1

## 设置鼠标手势

    cursor:

    	设置显示的光标的类型（形状）
    	default		默认光标（通常是一个箭头）
    	auto		默认浏览器设置的光标
    	crosshair	呈现为十字线
    	pointer		呈现为指示链接的指针（一只手）
    	move		此光标指示某对象可被移动

## 阻止:hover、:active 等鼠标行为状态的触发

    css 选择器
    	pointer-events: none;

## 设置带弧度的梯形

    .box {
    	width: 100px;
    	height: 25px;
    	text-align: center;
    	color: #fff;
    	position: relative;
    }

    .box::before {
    	content: "";
    	position: absolute;
    	top: 0;
    	left: 0;
    	right: 0;
    	bottom: 0;
    	z-index: -1;
    	transform-origin: left top;
    	background-color: brown;
    	transform: perspective(1em) rotateX(-7deg);
    	border-top-left-radius: 5px;
    	border-top-right-radius: 5px;
    }

## 纯 CSS 做 tab 功能
