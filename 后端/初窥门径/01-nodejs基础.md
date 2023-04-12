---
typora-copy-images-to: media
---

# nodejs基础

## 一、nodejs介绍

### 1、简介

在我们此时的认知中，js是一门客户端语言，且是一门脚本语言，也就是说只能在浏览器上运行，且只能依赖HTML才能运行。

为了提高js的逼格，让js脱离浏览器和HTML也能运行，一代大神Ryan Dahl在2009年，通过分析将谷歌浏览器中V8引擎（即谷歌浏览器中的js解析器），分离出来，进行了二次封装，做出了一款软件，叫nodejs。

![1638328389587](media/1638328389587.png) 

这个软件中可以运行js代码，且不需要依赖浏览器和HTML。

所以说，nodejs本质上是一个js除了浏览器之外的运行环境，使得js除了是一门客户端语言外，还是一门和java、php、ruby平起平坐的服务器端语言。

### 2、nodejs使用注意事项

1. nodejs实现了js代码在浏览器外执行，所以在nodejs中无法运行DOM和BOM的代码
2. nodejs中除了ECMAScript代码外，还内置了很多其他API
3. nodejs让js在写前端代码的基础上，可以写后端代码，因为nodejs可以构建服务器

![1623980565201](media/1623980565201.png) 

相关网址：

- https://nodejs.org/zh-cn/
- http://nodejs.cn/

### 3、下载安装

下载安装：http://nodejs.cn/download/

![1638329291701](media/1638329291701.png) 

1. 安装.msi文件，傻瓜式下一步安装

   ![1638328843565](media/1638328843565.png) 

   ![1638328865745](media/1638328865745.png) 

   ![1638328892173](media/1638328892173.png) 

   ![1638328929119](media/1638328929119.png) 

   ![1638329063776](media/1638329063776.png) 

   ![1638329074821](media/1638329074821.png) 

   ![1638329100892](media/1638329100892.png) 

   安装完成后，没有桌面快捷方式，也不需要有。

2. 安装.zip文件，需要手动配置环境变量

   解压zip安装包

   ![1638329534003](media/1638329534003.png) 

   将解压后的文件夹改名，并放在指定的安装目录下：

   ![1638329641676](media/1638329641676.png) 

   复制当前文件夹路径，配置在系统环境变量中

   ![1638330111997](media/1638330111997.png) 

   安装完成。

安装完成测试，打开命令行，输入下列指令：

```shell
node -v
```

![1638329168022](./media/1638329168022.png)  



### 4、nodejs运行

nodejs是执行js代码的环境，执行方式有两种：

1. 在nodejs中直接执行代码（类似于在浏览器控制台执行写js代码并执行）

   在命令行中输入node，敲回车，就进入了nodejs的控制台，操作跟浏览器的控制台一样

   ```shell
   node
   console.log(111)
   1+2
   ```

   ![1638330321702](media/1638330321702.png) 

   退出这个控制台，通过两次`ctrl+c`或`.exit`命令。

   这种方式无法用来开发代码，因为我们也从来不在浏览器的控制台开发代码，这样的代码只能运行这么一次，下次重新打开就丢失了。

2. 使用nodejs执行js文件

   新建js文件，书写规范的js代码，通过命令行的node命令执行这个文件。

   执行的文件有两种方式：

   1. 当前命令行不在文件所在文件夹下，此时需要将文件的绝对路径 - 不常用

      ![1638330673904](media/1638330673904.png) 

   2. 当前命令行在文件所在的文件夹下，此时使用文件的相对路径即可 - 常用

      ![1638330713269](media/1638330713269.png) 



## 二、简单的命令

打开命令行窗口的方式：

- win+r
- 地址栏输入cmd
- shift+鼠标右击
- vscode中ctrl+反引号
- 开始菜单中输入cmd，点击`命令提示符`或`打开`。最万能是选择`以管理员身份运行`

```shell
盘符: # 进入某个盘符
cd 目录 # 进入某个目录  -  cd /
cd .. # 回到上一级目录 - 一样
dir # 查看当前文件夹中的文件和文件夹 - ls  
tree # 查看当前文件夹中的所有文件和文件夹包括子文件夹及其文件，树状结构
cls # 清屏 - clear
ipconfig # 查看当前电脑的网管信息 - ifconfig
systeminfo # 查看当前电脑的配置信息
md 文件夹名 # 新建文件夹 - mkdir   make directory
rd 文件夹名 # 删除文件夹 - rm -rf 文件夹   remove  directory
type nul> 文件名 # 新建文件 - touch  文件名
echo 内容 > 文件名 # 给文件中写入内容
ren 文件名 新的文件名 # 将文件重命名
del 文件名 # 删除一个文件 - rm -rf 文件名
```



## 三、模块化开发

### 1、介绍

因为node命令执行js文件的时候，只能指定一个js文件，所以我们在开发的时候只能将项目所有代码写在一个文件中，可是这样不利于我们进行分工合作，也不利于后期项目的维护，所以需要对js代码进行模块化管理。

所有前端的模块化，都是将一个js文件当做是一个模块，多个模块之间可以互相导入使用。

### 2、分类

其实在很早之前就有了模块化开发的概念了，前端的模块化开发规范共有以下几种：

1. IIFE（Immediately Invoked Function Expression）立即调用函数表达式

   通过自调用函数模拟模块化，解决了全局变量污染问题。

   例：b文件依赖a文件

   a文件

   ```js
   (function(){
       var a = 1
       window.moduleA = {
           a
       }
   })()
   ```

   b文件

   ```js
   (function(moduleA){
       var b = 2
       console.log("导入的模块数据是：", moduleA);
       console.log("当前模块数据是" + b);
   })(moduleA)
   ```

   最后在html文档中需要导入这两个文件：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Document</title>
   </head>
   <body>
   
   </body>
   <script src="a.js"></script>
   <script src="b.js"></script>
   </html>
   ```

   结果：

   ![1638368485225](media/1638368485225.png) 

2. AMD（Asynchronous Module Definition）异步模块定义

   由于这个规范不是原生`js`支持的，所以要先引入第三方库文件（RequireJS）才能实现。解决了多个`js`文件之间的依赖关系问题。也遗留了一个问题：`js`加载的时候浏览器会停止页面渲染，加载文件越多，页面失去响应时间。

   例：c文件依赖a文件/c文件依赖a文件和b文件/c文件依赖b文件，b文件依赖a文件

   a.js文件

   ```js
   define(function(){
       var a = 1
       return {a}
   })
   ```

   define函数用来定义模块，被别的模块导入。

   其中define的参数1表示当前模块的名称，别的模块导入当前模块的时候使用这个名称代表当前模块

   b.js文件

   ```js
   define(['./a.js'], function(moduleA){
       var b = 2
       return {b,moduleA}
   })
   ```

   define函数在定义模块的同时也可以依赖别的模块，如上代码，参数2表示依赖的模块。回调函数中的参数，是在回调函数中使用依赖模块中数据时对别的模块起的别名，或可理解为将别模块使用这个参数接收

   c.js文件

   ```js
   require(['./b.js'], function(moduleB){
       console.log(moduleB)
   })
   ```

   require函数作为最终的处理函数，参数1是依赖的模块，参数2是对所有逻辑的处理。

   最后在html中运行，首先引入require.js，然后引入c.js即可

   ```html
   <script src="https://cdn.bootcdn.net/ajax/libs/require.js/2.3.6/require.js"></script>
   <script src="c.js"></script>
   ```

   最终结果：

   ![1638409259479](media/1638409259479.png) 

   最终，c文件要运行，就必须先加载a文件和b文件。如果被依赖的文件中，需要很长时间才能运行结束，后面的文件也需要等待很长时间。所以首屏加载很慢。

3. CMD（Common Module Definition）通用模块定义

   同样不是原生js支持的，所以需要先引入第三方库文件（SeaJS）才能实现。他解决的问题跟AMD是一样的，但他的文件加载时机跟AMD是不一样的，AMD是所有依赖的文件提前加载，CMD是什么需要什么时候加载。

   例：

   a文件

   ```js
   define(function(require, exports, module){
       var a = 1
       exports.a = a
   })
   ```

   回调函数中的require是一个函数，用来引入别的模块，exports和module用来导出，其中exports就是module.exports。

   b文件

   ````js
   define(function(require, exports, module){
       let moduleA = require('./a.js')
       var b = 2
       module.exports = {
           moduleA,
           b
       }
   })
   ````

   require导入别的模块时，使用模块名称导入

   c文件

   ```js
   seajs.use(['./b.js'], function(module){
       console.log(module);
   })
   ```

   seajs.use方法表示要使用哪个模块，module是给使用的模块起的别名。

   最后在html中先导入sea.js，然后按照依赖顺序导入其他文件：

   ```html
   <script src="https://cdn.bootcdn.net/ajax/libs/seajs/3.0.3/sea.js"></script>
   <script src="c.js"></script>
   ```

   最终结果：

   ![1638412483079](media/1638412483079.png) 

   因为在seajs中提供了require，所以什么需要时，什么时候调用导入即可，实现了按需导入。

4. es6的模块化规范

   原生js支持，导出用export关键字，导入用import关键字。但必须在服务器环境中才能用，且html使用必须添加：`type="module"`属性。

   导出语法：

   ```js
   export 数据的定义
   // 或
   export default 数据
   ```

   导入语法：

   ```js
   import {解构} from '文件'
   import * as 对象 from '文件'
   import 对象 form '文件'
   import 对象, {解构} from '文件'
   ```

   例：

   a文件

   ```js
   var a = 1
   export default a
   export var a1 = 11
   ```

   导出时使用default关键字的，当导入时，直接接收到的就是这个数据，如果还有别导出数据，必须解构，否则接收不到。

   b文件

   ```js
   export var b = 2
   export var b1 = 22
   ```

   没有加default的，表示可以导出多个数据

   c文件

   ```js
   export var c1 = 3
   export var c2 = 33
   ```

   在html中的使用：

   ```html
   <script type="module">
   import moduleA,{a1} from './a.js'
   import {b, b1} from './b.js'
   import * as obj from './c.js'
   import {a1} from './a.js'
   
   console.log(moduleA, a1);
   console.log(b, b1);
   console.log(obj);
   console.log(a1);
   </script>
   ```

   导入可以直接将导出数据接收到，并起别名。也可以将导入的所有内容解构。也可以将所有数据组成一个对象

   最终结果：

   ![1638413050951](media/1638413050951.png) 

   总结：

   ```js
   export default {a: 1, b: 2} // 导入可以有 import obj from 文件/import * as obj from 文件
   多个export导出的 // 导入可以使用import {结构} from 文件/import * as obj from 文件
   export和export default一起导出的 // import XX和import {XX}都只能得到一个数据，要得到所有数据可以使用import * as obj或import obj,{b} from 文件
   ```

   

5. CommonJS - nodejs自带的模块化规范

   nodejs中自带的模块化规范，只能在nodejs中使用，导出使用`module.exports`关键字，导入使用`require`关键字。

### 3、CommonJS规范

使用commonjs规范导入导出模块：

导入：

```js
// 导入
require(被导入的模块)
```

导出：

```js
// 导出
module.exports = {数据}
module.exports.键 = 值
exports.键 = 值
```

一个文件就是独立的模块，可以理解为是一个独立的作用域，别的模块要用到这个作用域中的数据，就需要将这个作用域中的数据导出，可以理解为函数返回数据的意思。

a.js导出：

```js
let a = 10
module.exports = a // 数据a就被导出了 - 这种导出方式只能导出一个数据
module.exports = {a} // 对象被导出了 - 通常要导出多个数据就使用这种导出方式
module.exports.a = a // 导出了一个对象 - 这种导出方式可以在多个地方导出数据
```

为了让导出变的更简单，commonjs规范中，在每个模块中定义了一个变量exports，并将module.exports赋值给了exports，导出时可以使用exports：

```js
exports = {a}
exports.a = a
```

注意：使用exports导出的时候，千万不能直接给exports赋值，因为导出功能是由module.exports提供的，如果直接给exports赋值，就将exports的内存空间换成了另外一个数据空间了，将失去导出功能。

b.js导入：

```js
let data = require('./a.js')
```

导入的就是导出的数据，导出数据就接收到数据，导出对象就接收到对象。

注意：导入的文件是自定义的，那路径中的`./`是不能被省略的。导入文件的后缀可以省略，因为nodejs中能被导入的只能是js文件。

在nodejs中能被导入的模块分3种：

1. 自定义模块 - 文件路径中的`./`不能省略
2. 内置模块 - 只需要导入模块名称，不需要路径，因为默认会在系统中找这个文件
3. 第三方模块 - 别人写好的js文件，我们需要导入使用，所以必须提前下载好才能被导入使用

## 四、内置模块

### 1、fs

fs（file system）模块提供了用于与文件进行交互相关方法。

```js
const fs = require('fs')

// 写入数据（覆盖）
fs.writeFile(文件路径,待写入的数据,err => {}) // 异步写入文件
fs.writeFileSync(文件路径,待写入的数据) // 同步写入文件

// 读取文件中数据
fs.readFile(文件路径, 'utf-8’,(err,data) => {}) // 异步读取文件
fs.readFileSync(文件路径, 'utf-8’) // 同步读取文件
// 第二个参数是可选项，如果不加的话，默认读取出来的buffer(用16进制来描述2进制数据流的格式)数据，需要通过buffer.toString()才能转成字符串

// 判断文件是否存在
let ret = fs.existsSync(path)

// 获取文件信息（异步）
fs.stat(文件,(err,stats) => {
	stats.isDirectory() // 是否是目录
	stats.isFile()       // 是否为文件
	stats.size            // 文件大小(以字节为单位)
})

// 给文件中追加内容
fs.appendFile(文件路径,待写入的数据,err => {}) // 异步追加
fs.appendFileSync(文件路径,待写入的数据) // 同步追加

// 删除文件（异步）
fs.unlink(文件路径,err => {})

// 文件重命名
fs.renameSync(旧的文件路径, 新的文件路径)
```

### 2、os

os（`operation system`）模块提供了与操作系统相关的实用方法和属性。

```js
const os = require('os')
// 换行符
os.EOL  //根据操作系统生成对应的换行符 window \r\n，linux下面 \n

// cpu相关信息
os.cpus()
// 总内存大小 (单位 字节)
os.totalmem()
// 空余内存大小 (单位 字节)
os.freemem()
```

### 3、path

path模块用于处理文件和目录(文件夹)的路径。

```js
const path = require('path')

// 获取路径最后一部内容，一般用它来获取文件名称
path.basename('c:/a/b/c/d.html')  // d.html

// 获取目录名，路径最后分隔符部分被忽略
path.dirname('c:/a/b/c/d.html') // c:/a/b/c

// 获取路径中文件扩展名（后缀）
path.extname('c:/a/b/c/d.html') // .html

// 给定的路径连接在一起
path.join('/a', 'b', 'c') // /a/b/c
```

> __dirname：是nodejs提供的一个内置变量，代表当前文件所在位置的绝对路径
>
> 斜杠和反斜杠：通常用来分割路径，"某个文件夹下"的意思，在windows系统中，斜杠和反斜杠的意义是一样的，但是建议大家写斜杠，反斜杠用在代码中，有特殊含义，转义的意思，在服务器系统中，默认路径就使用斜杠

### 4、url

url的完整形态：

![1623983181391](media/1623983181391.png) 

URL字符串是结构化的字符串，包含多个含义不同的组成部分。 解析字符串后返回的 URL 对象，每个属性对应字符串的各个组成部分。

```js
const url = require('url');
const href = 'http://www.xxx.com:8080/pathname?id=100#bbb'
// 解析网址，返回Url对象
// 参2 如果为true 则 query获取得到的为对象形式
url.parse(href,true)

//以一种 Web 浏览器解析超链接的方式把一个目标 URL 解析成相对于一个基础 URL。
url.resolve('https://lynnn.cn/foo/bar',../'bar') // https://lynnn.cn/bar
```

例：

```js
const url = require('url')
let href = "http://xiaoming:123456@localhost:80/project/src/userinfo.html?id=5&cat=8#top";
let obj = url.parse(href)
// obj.query是我们比较常用的，因为我们经常会解析路径中的数据
console.log(obj);
```

![1624344004493](media/1624344004493.png) 

### 5、querystring

用于**解析和格式化** URL 查询字符串（URL地址的get形式传参）的实用工具。

```js
const querystring = require('querystring')

// query字符串转为对象
querystring.parse('foo=bar&abc=xyz')
querystring.decode('foo=bar&abc=xyz')

// 对象转为query字符串
querystring.stringify({ foo: 'bar',abc: 'xyz'})
querystring.encode({ foo: 'bar',abc: 'xyz'})
```

### 6、http

#### 6.1、服务器介绍

我们做好网页后，为了能让别人访问，需要在计算机上开启一个能被别人访问的服务，这个功能是由web服务器提供的。

web服务器，指的是能提供网站访问服务的软件或代码程序，让别人可以通过ip地址来访问自己计算内部的某些文件。目前最主流的三个Web服务器软件是Apache、 Nginx 、IIS、tomcat。 代码可以有nodejs代码、go语言代码。

软件其实也是由代码组成的，我们要学习的就是用nodejs的代码创建一个web服务器。

#### 6.2、前后端介绍

前端开发是面对用户的客户端上运行的代码，后端开发是面对数据库和前端的开发。

网页中所使用的数据都是存储在数据库中的，但前端无法直接操作到数据库，只有web服务器才能操作到数据库，web服务器中执行的代码开发叫后端开发。

在工作中通常是前端和后端一起开发，前端需要数据，就跟后端交互，后端连接数据库，将数据发送给前端。

#### 6.3、http介绍

nodejs可以通过代码创建web服务器，让js可以是前端语言，也可以是后端语言。

http模块就是nodejs封装在内部专门用于创建服务器的。

#### 6.4、创建服务器

```js
// 导入http模块    
const http = require('http')
// 创建web服务对象实例，并处理请求
const server = http.createServer(function(request, response) {
    response.end('this is my server!')
})
server.listen(8080, () => { // 这里的第二个参数回调函数可以省略
    console.log('服务已启动')
})
```

通常在访问服务器的时候，默认会有一个网站图标的请求，要求服务器提供网站图标。

简写：

```js
require('http').createServer((req, res) => {
    // 处理响应
}).listen(端口号)
```

#### 6.5、请求对象

通过请求对象req，可以获取请求的相关信息，例如：请求的客户端ip、请求方式、请求路径。。。

```js
require('http').createServer((req, res) => {
    // 获取客户端请求ip
    req.connection.remoteAddress
    // 获取请求路径
    req.url // 不包含协议、域名、端口号的路径
}).listen(端口号)
```

#### 6.6、响应对象

响应对象可以给客户端响应内容，结束响应、响应状态码、状态描述、响应头等信息

```js
require('http').createServer((req, res) => {
    // 设置响应内容
    res.write()
    // 结束响应
    res.end(字符串内容/buffer)
}).listen(端口号)
```

注意：每次响应一定要结束响应，否则客户端的请求会一直转，请求不到内容。响应结束后就不允许再响应内容了。

#### 6.7、静态资源服务器

假设我们已经写好了一套静态页面，但是需要让别人访问，就需要提供一个服务器，且别人来访问的时候还要能访问到对应的静态页面。这种服务器被称之为静态资源服务器，目的是只为了提供访问静态资源。

```js
const path = require('path')
let fs = require('fs')
require('http').createServer((req, res) => {
    // 处理响应
    // 通过请求路径获取到对应的文件路径
    let filePath = path.join(__dirname, 'public', path.basename(req.url))
    let ret = fs.existsSync(filePath) // 判断文件路径是否存在
    // 判断文件路径是否是一个文件
    fs.stat(filePath,(err,stats) => {
        if(stats.isFile()){
            // 读取文件并响应
            res.end(fs.readFileSync(filePath))
        }else{
            res.end('路径错误') // 结束响应，避免客户端一直转
        }
    })
}).listen(端口号)
```
