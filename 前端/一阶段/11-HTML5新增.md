# HTML5 新增

## 什么是 HTML5?

    HTML5 是下一代 HTML 标准
    HTML , HTML 4.01 的上一个版本诞生于 1999 年。自从那以后，Web 世界已经经历了巨变
    HTML5 仍处于完善之中。然而，大部分现代浏览器已经具备了某些 HTML5 支持

    <!doctype> 声明必须位于 HTML5 文档中的第一行,使用非常简单

    对于中文网页需要使用 <meta charset="utf-8"> 声明编码，否则会出现乱码

## HTML5 的语法

    内容类型（ContentType）
    	HTML5 的文件扩展符与内容类型保持不变，仍然为".html"或".htm"

    DOCTYPE 声明
    	不区分大小写

    指定字符集编码
    	meta charset="UTF-8"

    可省略标记的元素
    	不允许写结束标记的元素：br、col、embed、hr、img、input、、link、meta
    	可以省略结束标记的元素：li、dt、dd、p、option、colgroup、thead、tbody、tfoot、tr、td、th
    	可以省略全部标记的元素：html、head、body、colgroup、tbody

    省略引号
    	属性值可以使用双引号，也可以使用单引号

## HTML5 中的语义化标签

HTML5 提供了新的语义元素来明确一个 Web 页面的不同部分
![Html5-layout](images/Html5-layout.jpg)

```html
<!-- 定义文档的头部区域 -->
<header></header>
<!-- 定义导航链接的部分 -->
<nav></nav>
<!-- 定义独立的内容 -->
<article></article>
<!-- 定义文档中的节（section、区段） -->
<section></section>
<!-- 定义页面主区域内容之外的内容（比如侧边栏） -->
<aside></aside>
<!-- 定义文档的底部区域 -->
<footer></footer>
<!-- 定义独立的流内容（图像、图表、照片、代码等等） -->
<figure></figure>
<!-- 定义被置于 "figure" 元素的第一个或最后一个子元素的位置 -->
<figcaption></figcaption>
<!-- 定义带有记号的文本 -->
<mark></mark>
```

## 额外的表单元素

```html
<!-- <datalist> 元素规定输入域的选项列表
    使用 <input> 元素的列表属性与 <datalist> 元素绑定 -->
<input list="check" />
<datalist id="check">
  <option value="男"></option>
  <option value="女"></option>
  <option value="未知"></option>
</datalist>
```

## HTML5 中的 input type 类型

### 验证类型

```html
<!-- 验证是否是一个邮箱地址 -->
<input type="email" />
<!-- 验证是否是一个手机号 -->
<input type="tel" />
<!-- 验证是否是一个url地址 -->
<input type="url" />
```

### 取值类型

```html
<!-- 取一个数字（滑块的方式） -->
<input type="range" />
<!-- 取一个数字（微调的方式） -->
<input type="number" />
<!-- 取一个颜色 -->
<input type="color" />
<!-- 取一个日期 -->
<input type="date" />
<!-- 取一个月份 -->
<input type="month" />
<!-- 取一个周数 -->
<input type="week" />
<!-- 取一个时间 -->
<input type="time" />
<!-- 取一个本地时间 -->
<input type="datetime-local" />
<!-- 产生一个搜索意义的表单 -->
<input type="search" />
```

## HTML5 中的表单属性

### form 新属性

```html
<!-- autocomplete 属性规定 form 或 input 域应该拥有自动完成功能 -->
<form autocomplete="on"></form>

<!--  novalidate 属性规定在提交表单时不应该验证 form 或 input 域 -->
<form novalidate></form>
```

### input 新属性

```html
<!-- autocomplete属性-->
<input autocomplete="on" />

<!-- placeholder 属性提供一种提示文本（hint），描述输入域所期待的值 -->
<input type="text" placeholder="请输入您的账号" />
<input type="password" placeholder="请输入您的密码" />

<!-- required 属性规定必须在提交之前填写输入域（不能为空） 必填项-->
<input type="text" required />

<!--  autofocus 属性规定在页面加载时，域自动地获得焦点 需要注意的是：一个页面只能有一个-->
<input type="text" autofocus />

<!--  min、max 和 step 属性 
    适用于以下类型的 <input> 标签：date、number 以及 range-->
<input type="number" name="quantity" min="1" max="5" />

<!-- multiple 属性规定<input> 元素中可选择多个值，在选择过程中
    用于以下类型的<input> 标签：email 和 file -->
<input type="file" name="img" multiple />

<!-- pattern 属性描述了一个正则表达式用于验证<input> 元素的值
    适用于以下类型的<input> 标签: text, search, url, tel, email, 和 password -->
<input type="text" pattern="[A-Za-z]{3}" />
```

## HTML5 中的关联文本

    点击文本时也可以控制表单元素的选择情况

    第一种方案
      <label>
      <input type="radio" name="agree" value="agree" />
      我同意此条款
      </label>

    第二种方案
      <input type="checkbox" name="agree" value="agree" id="box" />
      <label for="box">我同意此条款</label>

## HTML5 中的音频和视频

    1、音频 <audio></audio>
      src 音频文件的 URL
      controls 向用户显示音频控件（比如播放/暂停按钮）
      autoplay 音频在就绪后马上播放
      loop 每当音频结束时重新开始播放
      muted 音频输出为静音
      preload 当网页加载时，音频是否默认被加载以及如何被加载

    2、视频 <video></video>
      src 音频文件的 URL
      controls 向用户显示音频控件（比如播放/暂停按钮）
      autoplay 音频在就绪后马上播放
      loop 每当音频结束时重新开始播放
      muted 音频输出为静音
      preload 当网页加载时，音频是否默认被加载以及如何被加载
      poster 视频正在下载时显示的图像，直到用户点击播放按钮
      width 视频播放器的宽度
      height 视频播放器的高度

## HTML5 中的Canvas

### 什么是 canvas?

    HTML5 <canvas> 元素用于图形的绘制，通过脚本 (通常是 JavaScript)来完成
    <canvas> 标签只是图形容器，您必须使用 脚本 来绘制图形
    你可以通过多种方法使用 canvas 绘制路径,盒、圆、字符以及添加图像

## HTML5 中的内联 SVG

### 什么是 SVG？

    SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
    SVG 用于定义用于网络的基于矢量的图形
    SVG 使用 XML 格式定义图形
    SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
    SVG 是万维网联盟的标准

### SVG 优势

    与其他图像格式相比（比如 JPEG 和 GIF），使用 SVG 的优势在于：

    SVG 图像可通过文本编辑器来创建和修改
    SVG 图像可被搜索、索引、脚本化或压缩
    SVG 是可伸缩的
    SVG 图像可在任何的分辨率下被高质量地打印
    SVG 可在图像质量不下降的情况下被放大

## SVG 与 Canvas 区别

    SVG 适用于描述 XML 中的 2D 图形的语言
    Canvas 随时随地绘制 2D 图形（使用 javaScript）
    SVG 是基于 XML 的，意味这可以操作 DOM，渲染速度较慢
    在 SVG 中每个形状都被当做是一个对象，如果 SVG 发生改变，页面就会发生重绘
    Canvas 是一像素一像素地渲染，如果改变某一个位置，整个画布会重绘

| Canvas                             | SVG                        |
| ---------------------------------- | -------------------------- |
| 依赖分辨率                         | 不依赖分辨率               |
| 不支持事件处理器                   | 支持事件处理器             |
| 能够以.png 或.jpg 格式保存结果图像 | 复杂度会减慢搞渲染速度     |
| 文字呈现功能比较简单               | 适合大型渲染区域的应用程序 |
| 最合适图像密集的游戏               | 不适合游戏应用             |
