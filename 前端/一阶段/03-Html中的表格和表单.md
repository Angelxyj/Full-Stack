# HTML 中的表格和表单

## HTML 中的表格

### 创建一个表格

    <table border="" cellspacing="" cellpadding="" width="" height="">
    	<caption>表格标题</caption>
    	<tr>
    		<th>Header</th>
    	</tr>
    
    	<tr>
    		<td>Data</td>
    	</tr>
    </table>
    
    表格由 <table> 标签来定义。
    每个表格均有若干行（由 <tr> 标签定义）
    每行被分割为若干单元格（由 <td> 标签定义）
    字母 th 指表格列标题
    字母 td 指表格数据（table data），即数据单元格的内容
    
    表格的表头使用 <th> 标签进行定义 放在 <tr> 标签中下使用
    大多数浏览器会把表头显示为粗体居中的文本
    
    数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等
    
    table 的属性值
    	width : 表格的宽度
    	height : 表格的高度
    	border : 显示表格的边框
    	bordercolor ：边框的颜色
    	bgcolor ： 表格的背景色
    	cellspacing : 单元格外边距      设置为 0 变成单线表格
    	cellpadding : 单元格内边距
    
    tr 的属性值
    	height : 单元格的高度
    	align:   设置某一行文字的水平对齐方式  left  / center  / right
    	valign:  设置某一行文字的垂直对齐方式  top  / center  / bottom
    	bgcolor ： 表格的背景色
    
    td 的属性值
    	width : 单元格的宽度
    	height : 单元格的高度
    	align:  设置某一个单元格文字的水平对齐方式   left  / center  / right
        valign: 设置某一个单元格文字的垂直对齐方式   top  / middle  / bottom
        bgcolor ： 表格的背景色
    	colspan : 向右合并单元格(跨列合并)
    	rowspan : 向下合并单元格(跨行合并)

## HTML 中的表单

### 表单是什么

    对于用户而言是数据的录入和提交的界面
    对于网站而言获取用户信息的途径

### 创建一个表单

    <form action="" method="" name="">
    
    </form>
    
    form   !!! 单词不要记错了
    action: 定义表单最终提交的地址
    method:	设定数据提交方式，用于根据不同的数据需求来选择合适的提交方式 xxx
    	常见的提交方式有：GET,POST
    	面试题：GET 和 POST 的区别？
    	1. get请求通常是从服务器上获取数据，post请求通常表示向服务器提交数据
    	2. get请求发送的数据都写在地址栏上，用户可见。而post请求发送的数据用户不可见
    	3. get请求不能提交大量的数据，但post可以，因此不要混用
    建议：
    1、Get方式的安全性较Post方式要差些，包含机密信息的话，建议用Post数据提交方式
    2、在做数据查询时，建议用Get方式；而在做数据添加、修改或删除时，建议用Post方式
    
    name: 给表单一个名字，使其成为独一无二的表单 xxx

### 常用的表单元素

    <input type="" value="" name="" placeholder=""/>
    
    type : 类型 默认是 text 文本框 / password 密码框
    		/ button 常规按钮 / submit 提交按钮 / reset 重置按钮
    		/ checkbox 复选按钮 /radio 单选按钮
    value : 值
    placeholder : 提示文本 --- HTML5 新增属性
    
    <button><button/>
    
    当 value 和 placeholder 属性值 同时存在时，显示 value 的属性值

