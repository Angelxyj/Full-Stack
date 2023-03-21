---
typora-copy-images-to: media
---

## 一、数学处理

### 1、Math常用API

- 圆周率

  ```js
  Math.PI // 3.1415926535
  ```

- 生成随机数

  ```js
  Math.random()
  ```

  生成的是0~1之间的随机小数，通常在实际项目中需要获取到一个范围内的随机整数，利用这个随机小数封装一个获取范围内的随机整数的函数：

  ```js
  function getRandom(a,b){
  	var max = a;
      var min = b;
      if(a<b){
      	max = b;
          min = a;
      }
      return parseInt(Math.random() * (max - min)) + min
  }
  ```

- 向上取整

  向上取整的含义是一个数字的小数部分不够1，将他处理成1。例如：10条数据每页展示3条，前3页都能放3条数据，但是第4页只能放1条数据，虽然占不满1页，但也要占1页

  ```js
  Math.ceil(3.3) // 4
  ```

- 向下取整

  向下取整跟`parseInt()`是一个意思，只要整数部分，舍掉小数部分得到整数

  ```js
  Math.floor(3.9) // 3
  ```

- 四舍五入

  ```js
  Math.round(3.3) // 3
  Math.round(3.9) // 4
  ```

- 求次方

  ```js
  Math.pow(2,3) // 2的3次方，参数1是底数，参数2是幂数
  ```

- 开平方根

  ```js
  Math.sqrt(9) // 3
  ```

  

- 绝对值

  ```js
  Math.abs(-6) // 6
  Math.abs(6) // 6
  ```

- 最大值

  ```js
  Math.max(9,5,1,3,4,8,2,6) // 9
  ```

- 最小值

  ```js
  Math.max(9,5,1,3,4,8,2,6) // 1
  ```

- 正弦

  ```js
  Math.sin(Math.PI*30/180) // 0.5
  ```

- 余弦

  ```js
  Math.cos(Math.PI*60/180) // 0.5
  ```

### 2、进制的转换

10进制转其他进制：`10进制数字.toString(进制数)`

```js
var x = 110;
x.toString(2) // 转为2进制
x.toString(8) // 转为8进制
x.toString(16) // 转为16进制
```

其他进制转10进制：`parseInt(数据,进制数)`

```js
var x = "110" // 这是一个二进制的字符串表示
parseInt(x, 2) // 把这个字符串当做二进制， 转为十进制

var x = "70" // 这是一个八进制的字符串表示
parseInt(x, 8) // 把这个字符串当做八进制， 转为十进制

var x = "ff" // 这是一个十六进制的字符串表示
parseInt(x, 16) // 把这个字符串当做十六进制， 转为十进制
```

## 二、时间日期处理

js提供了一个构造函数`Date`，用来创建时间日期对象。所以跟时间日期有关的操作都是通过时间日期对象来操作的。

### 1、时间日期对象创建

当前时间的时间日期对象

```js
var date = new Date()
console.log(date) // Tue Jul 30 2019 21:26:56 GMT+0800 (中国标准时间)
```

创建好的是一个对象，但是当输出的时候被浏览器自动转为字符串输出了。获取到的是当前本地的时间日期对象。如果把本地的时间日期改掉，获取到的时间日期对象会随着本地时间变化。

指定的时间日期对象

```js
var date = new Date("年-月-日 时:分:秒") // 也可以是英文版的时间字符串 - Sun May 13,2016
var date = new Date(年,月-1,日,时,分,秒)
var date = new Date(时间戳)
```

### 2、获取具体的时间日期

通过时间日期对象可以获取到具体的年月日时分秒，甚至毫秒和时间戳。

```js
date.getFullYear(); // 获取到完整的时间日期对象中的年份
date.getMonth(); // 获取到时间日期对象中的月份 - 这里的月份是通过0~11来描述1~12月的
date.getDate(); // 获取到时间日期对象中的日
date.getDay(); // 获取时间日期对象中的星期几
date.getHours(); // 获取到时间日期对象中的时
date.getMinutes(); // 获取到时间日期对象中分
date.getSeconds(); // 获取到时间日期对象中的秒
date.getMilliseconds(); // 获取到时间日期对象中的毫秒 - 1秒=1000毫秒
date.getTime(); // 获取到时间日期对象对应的时间戳
```

时间戳，指的是，格林尼治时间1970年1月1日0点0分0秒到现在走过的毫秒数。利用时间戳可以方便计算时间，例如：计算100天以前是几月几号。

将年月日时分秒放在页面中显示：

```js
var date = new Date();
var year = date.getFullYear(); 
var month = date.getMonth()+1;
var d = date.getDate();
var day = date.getDay();
var hour = date.getHours(); 
var minute = date.getMinutes(); 
var second = date.getSeconds(); 
document.write("现在是北京时间："+year+"年"+month+"月"+d+"日。星期"+day+"。"+hour+"时"+minute+"分"+second+"秒");
```

### 2、设置时间日期

通过时间日期对象，可以将其中的年月日时分秒进行设置，改变时间日期对象的时间。

```js
date.setFullYear(年份); // 设置时间日期对象中的年份
date.setMonth(当前月份-1); // 设置时间日期对象中的月份 - 这里的月份是通过0~11来描述1~12月的
date.setDate(日); // 设置时间日期对象中的日
date.setHours(时); // 设置时间日期对象中的时
date.setMinutes(分); // 设置时间日期对象中分
date.setSeconds(秒); // 设置时间日期对象中的秒
date.setMilliseconds(毫秒); // 设置时间日期对象中的毫秒
date.setTime(时间戳); // 设置时间日期对象对应的时间戳 - 直接用时间戳就能将时间日期对象中的年月日时分秒全部改变
```

星期几是不能设置的，是根据年月日生成的。

### 3、日期格式化

```js
date.toLocalString();//本地风格的日期格式
date.toLocaleDateString(); // 获取日期
date.toLocaleTimeString(); // 获取时间
```

### 4、时间戳的获取

格林威治时间/格林尼治时间

```js
Date.parse("2015-08-24") // 获取1970年到设定时间的毫秒数
new Date().getTime()
+new Date();
```

案例：

两个指定的日期相差多少天

```js
var date1=new Date(2010,10,3);
var date2=new Date(2017,9,24);
var day=(date2.getTime()-date1.getTime())/(1000*60*60*24);/*不用考虑闰年否*/
console.log("相差"+day+"天");
```

100天以后是哪年哪月哪日

```js
var date = +new Date()
date += 100 * 24 * 3600 * 1000
var newDate = new Date(date)
var year = newDate.getFullYear()
var month = newDate.getMonth() + 1;
var d = newDate.getDate()
console.log('100天以后是'+year+'年'+month+'月'+d+'日')
```