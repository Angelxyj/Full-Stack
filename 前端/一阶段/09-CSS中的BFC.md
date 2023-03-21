# CSS 中的 BFC

## 什么是 BFC?

	   BFC（Block Formatting Context）格式化上下文，
	   是Web页面中盒模型布局的CSS渲染模式，指一个独立的渲染区域或者说是一个隔离的独立容器
	   
	   可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部

## BFC 的布局规则？

	1、内部的Box会在垂直方向，一个接一个地放置
	2、Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠（按照最大margin值设置）
	3、每个元素的margin box的左边， 与包含块border box的左边相接触，即使存在浮动也是如此
	4、BFC的区域不会与float box重叠
	5、BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
	6、计算BFC的高度时，浮动元素也参与计算

## 如何触发 BFC?

	1、body 根元素
	2、float的值 不是none
	3、position的值 不是static或者relative
	4、overflow的值 不是visible
	5、display的值 是inline-block、table-cell、flex、table-caption或者inline-flex...

## 适用解决哪些问题

### 解决浮动带来的高度塌陷问题

	方案一
		父元素添加 overflow:hidden
		
		优点：代码简洁
		缺点：内容增多的时候容易造成不会自动换行导致内容被隐藏掉，无法显示要溢出的元素
		
	方案二
		使用 after 伪元素清除浮动
			.clear::after { 
				content: '';
				display: block;
				height:0; 
				visbility: hidden;
				clear: both;
			}
			.clear {
				*zoom: 1;
				/* ie6 清除浮动的方式， * 号 只有在IE6 IE7执行，其他浏览器不执行 */
			}
			
			优点：符合闭合浮动思想，结构语义化正确
			缺点：ie6-7不支持伪元素：after，使用zoom:1触发hasLayout
			
	方案三
		使用 before 和 after 双伪元素清除浮动
			.clear::after, .clear::before {
				content: '';
				display: block;
			}
			.clear::after {
				clear: both;
			}
			.clear {
				*zoom: 1;
				/* ie6 清除浮动的方式， * 号 只有在IE6 IE7执行，其他浏览器不执行 */
			}
			
			优点：代码更简洁
			缺点：用zoom:1触发hasLayout

### 布局方案（自适应）

	窗口自适应：页面中的盒子大小会随着页面的大小的变化发生相应的变化
		设置方法 html,body{ height:100%; width:100%; }
	
	引入自动计算大小的函数：calc()
	用于动态计算值
	calc()函数支持 "+", "-", "*", "/" 运算
	calc()函数使用标准的数学运算优先级规则
	eg: width: calc(100% - 200px)
	
	方案一：
		一侧固定，一侧自适应
		
		通过浮动的方法
		通过定位的方法来设置元素的大小
	
	方案二
		左右固定，中间自适应
		
		通过浮动的方法
		通过定位的方法来设置元素的大小
	
	方案三
		圣杯布局（三栏布局）

