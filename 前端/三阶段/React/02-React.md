# React

# 一、概述

官网：https://reactjs.org/

中文网（个人翻译站）：https://react.docschina.org/



## 1、介绍

**React起源于Facebook的内部项目**，它是一个用于构建用户界面的javascript库，Facebook用它来架设公司的Instagram网站，并于2013年5月开源。

React拥有较高的性能，代码逻辑非常简单，越来越多的人已开始关注和使用它。认为它可能是将来Web开发的主流工具**之一**。



## 2、特点

- 声明式

你只需要描述UI看起来是什么样式，**就跟写HTML一样**，React负责渲染UI

- 基于组件

组件是React最重要的内容，组件表示页面中的部分内容

- 学习一次，随处使用
  - 使用React可以开发Web应用—ReactJs
  - 使用React可以开发移动端—react-native
  - 可以开发VR应用—react 360



## 3、React与传统MVC的关系

React用于**构建用户界面**的JavaScript 库，它不是一个完整的MVC框架，最多可以认为是MVC中的V（View）。可以简单地理解为：React将界面分成了各个独立的小块，每一个块就是组件，这些组件之间可以**组合、嵌套**，就成了我们的页面。

Vue是一个框架。

**React可以说它不是框架，它只是一个构建页面的JavaScript库，但是外面也认为其是一个框架。**



## 4、开发工具的安装

- Chrome 浏览器扩展：https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=zh-CN

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/c1407e83f4b0a56beca4baadaac2de471975a092.png?sign=6007a47eebf330bd919ee70a42746fe7&t=5f8ee1ad)

- vscode安装react开发扩展

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/4d2506168810f2fb7195cd6b9f766ed998dc365d.png?sign=54df8319890c5709784d4eff96570639&t=5f8ee2a8)



## 5、React初识

React开发需要引入多个依赖文件，其中react.js、react-dom.js这两个文件是我们创建react应用程序必须要引入的依赖文件。

- react.js
  - 核心，提供创建元素，组件等功能

- react-dom.js
  -  提供DOM相关功能

> 注意：关于react外部js文件
>
> - 回头如果需要其他版本自己再去下
>   - https://reactjs.org/docs/add-react-to-a-website.html
> - 网上找的（包扩我们发的）==可能==会出现文件下载不完整的情况，如果出现这种情况建议重新去下载

下载对应的react.js和react-dom.js的开发版本的js类库文件到本机中后，通过HTML的`script`标签引入到当前的网页中，如下（**在引入对应的JavaScript文件时需要注意先后顺序的问题，先引核心文件，再引其他文件**）：

~~~html
// React 的核心库
<script src="js/react.development.js"></script>
//DOM 相关的功能
<script src="js/react-dom.development.js"></script>
~~~

在HTML中定义reactjs渲染容器id和进行React实例化相关操作,完成helloworld显示：

~~~html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
        <title>Examples</title>
        <meta
            name="viewport"
            content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no"
        />
        <meta name="description" content="" />
        <meta name="keywords" content="" />
        <link href="" rel="stylesheet" />
    </head>
    <body>
        <div id="app"></div>
        <!-- 引入react相关的文件 -->
        <script src="./js/react.development.js"></script>
        <script src="./js/react-dom.development.js"></script>
        <script>
            // 创建虚拟dom
            // React.createElement(标签名称,对象形式的DOM属性信息,DOM中的内容/子DOM)
            // React.createElement(标签名称,对象形式的DOM属性信息,DOM中的内容/子DOM,DOM中的内容/子DOM,...)
            // React.createElement(标签名称,对象形式的DOM属性信息,[DOM中的内容/子DOM,DOM中的内容/子DOM,...])
            const vNode = React.createElement("div", {}, "hello world");
            // 获取挂载点
            const el = document.getElementById("app");
            // const el = document.querySelector("#app")
            // ReactDOM.render(虚拟DOM,挂载点)
            ReactDOM.render(vNode, el);
        </script>
    </body>
</html>
~~~

> 需要注意，在react中，JavaScript代码部分里面如果涉及到DOM的class属性操作，请不要直接使用`class`，原因是这个`class`是es6里面的关键词，react里面需要使用`className`进行替换。所以，举例如下：
>
> ~~~javascript
> const vNode = React.createElement("div", {id: "hi",className: "cls"}, "hello world");
> ~~~



# 二、JSX语法

## 1、概述

由于通过React.createElement()方法创建的React元素有一些问题：代码比较繁琐，结构不直观，无法一眼看出描述的结构，不优雅，开发时写代码很不友好。

React使用JSX来替代常规的JavaScript，JSX可以理解为的JavaScript语法扩展，它里面的标签申明要符合XML规范要求（**必须需要有一个唯一的根元素**）。React不一定非要使用JSX，但它有以下优点：

- JSX执行**更快**，因为它在编译为JavaScript代码后进行了优化

- 它是类型安全的，在编译过程中就能发现错误

- 声明式语法更加直观，**与HTML结构相同**，**降低了学习成本**，提升开发效率速

- jsx语法中一定要有一个**顶级元素包裹（XML一大特点）**，否则编译报错，程序不能运行



## 2、JSX重构Hello world

在项目中尝试JSX最快的方法是在页面中添加这个 `<script>` 标签，引入解析jsx语法的babel类库，注意后续的`<script>`标签块中使用了JSX语法，则一定要申明类型`type="text/babel"`，否则babel将不进行解析jsx语法。

~~~html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
~~~

重构hello world代码：

~~~jsx
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
        <title>Examples</title>
        <meta
            name="viewport"
            content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no"
        />
        <meta name="description" content="" />
        <meta name="keywords" content="" />
        <link href="" rel="stylesheet" />
    </head>
    <body>
        <div id="app"></div>
        <!-- 引入react相关的文件 -->
        <script src="./js/babel.min.js"></script>
        <script src="./js/react.development.js"></script>
        <script src="./js/react-dom.development.js"></script>
        <!-- script标签上一定要写上 type="text/babel" -->
        <script type="text/babel">
            // 创建虚拟dom
            const vNode = <div>hello world</div>;
            // const vNode = (<div>hello world</div>);
            // 获取挂载点
            const el = document.getElementById("app");
            // 页面渲染
            ReactDOM.render(vNode, el);
        </script>
    </body>
</html>
~~~

在写jsx语法的时候需要注意，如果对应的dom有多个层次，建议给整体添加小括号，这样的话允许通过格式化插件将代码格式化成多行，这样的好处，我们可以清晰的看清dom的层次结构。【建议】



## 3、JSX语法基础

### 3.1、插值表达式

在JSX语法中，要把JS代码写到`{ }`中，所有标签必须要闭合。

~~~jsx
let num = 100;
let bool = false;

// JSX 语法
var vNode = (
    <div>
        {/* 我是注释 */}
        {num}
        <hr />
        {/* 三目运算 */}
        {bool ? "条件为真" : "条件为假"}
    </div>
);

ReactDOM.render(vNode, document.getElementById("app"));
~~~

注意：在jsx语法中不支持“//”注释形式以及“<!---->”注释形式，只能使用“{/* */}”注释形式。



### 3.2、属性绑定

> 对标：Vue2的`v-bind`指令

~~~jsx
const src = "http://www.mobiletrain.org/images/index/new_logo.png";
const style = { fontSize: "20px", color: "red" };
const html = "<a href='http://www.baidu.com'>百度一下</a>";

// 获取元素
const app = document.getElementById("app");
// 创建虚拟DOM
const vNode = (
    <div>
        { /*标签的属性如果需要被JSX解析，则属性的值不能加引号*/ }
        <img src={src} />
        <hr/>
        <p style={style}>北川3次地震为汶川地震余震</p>
        <hr/>
        <p className="cl1">iPhone12开售排队</p>
        { /*
             输出HTML字符串（了解）
             注意点：react默认不解析html字符串
             原因是：安全问题
             如果真要输出解析的html字符串请按照以下的语法
        */ }
        <p dangerouslySetInnerHTML={{__html:html}}></p>
    </div>
);
// 渲染页面
ReactDOM.render(vNode, app);
~~~



### 3.3、数组渲染

#### 3.3.1、直接渲染

~~~jsx
let arr = ["张三", "李四", "王五", "罗翔"];
// 获取挂载点
const el = document.getElementById("app");
// 创建虚拟DOM
const vNode = (
    <div>
        {/* 直接输出数据 */}
        {arr}
    </div>
);
// 渲染
ReactDOM.render(vNode, el);
~~~

上述输出的结果如下：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/300444482c3a5c11e3fdf15003ba6142ba0b951d.png?sign=14d92baf0356eaf1441e41093edb4065&t=5f92987b)

#### 3.3.2、处理并渲染

~~~jsx
let arr = ["张三", "李四", "王五", "罗翔"];
// 获取挂载点
const el = document.getElementById("app");
// 创建虚拟DOM
const vNode = (
    <div>
        {/* 处理并渲染 */}
        <ul>
            {/* 给循环体包裹一层{}，不包就错 */}
            {
                arr.map((item, index) => {
                    return <li key={index}>{item}</li>;
                })
            }
            <hr />
            {
                /* 
                给循环体包裹一层{}，不包就错，如果循环体就1行
            	{}与return可以被省略（箭头函数）
            	*/
            }
            {
                arr.map((item, index) => (
                    <li key={index}>{item}</li>
                ))
            }
        </ul>
    </div>
);
// 渲染
ReactDOM.render(vNode, el);
~~~



# 三、项目构建

React团队推荐使用create-react-app（相当于vue的`vue-cli`）来创建React新的单页应用项目，它提供了一个**零配置**的现代构建设置。

React脚手架（create-react-app）意义：

- 脚手架是官方提供，零配置，无需手动配置繁琐的工具即可使用

- 充分利用Webpack，Babel，ESLint等工具辅助项目开发

- 关注业务，而不是工具配置

create-react-app会配置我们的开发环境，以便使我们能够使用最新的 JavaScript特性，提供良好的开发体验，并为生产环境优化你的应用程序。为了能够顺利的使用create-react-app脚手架，我们需要在我们的机器上安装Node >= 8.10 和 npm >= 5.6。

## 1、初始化项目

在终端中使用以下命令来构建react项目：

~~~shell
# 免安装形式
npx create-react-app my-app
# 上面这种安装方式不需要全局安装create-react-app,如果需要全局安装，则可以执行下面的命令
# npm i -g create-react-app
# create-react-app your-app
~~~

项目创建需要消耗的时间可能会有点久，在项目创建完毕后可以执行以下指令：

~~~shell
# 进入项目目录
cd my-app
# 启动项目
npm start
~~~

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/7e386902e80b9ef5ba3b68333fe7a44d4bd3eb33.png?sign=ef7d37b07b0a7dbae3f3f22912b5c9ed&t=5f93d66a)

> 提示：如果本机安装了`yarn`（一款Facebook自家的包管理工具，类似npm），则安装好给予的项目启动命令提示是`yarn start`。



## 2、目录结构

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/d7961534de022d85cb6692d86bcd12573ab86e2b.png?sign=c6f3956be4f2b83e38b7ded996d9a923&t=5f93d80d)

- public目录下
  - manifest.json：清单文件（说明性文件），谷歌要求有这个文件，但是这个文件对开发者来讲没什么用。
  - robots.txt：用于声明当前项目哪些路径、目录允许搜索引擎抓取。
- src目录下
  - `*.css`：样式文件
  - App.js：类似于App.vue，就是react里面的根组件（**在react中，组件后缀是js，但是以后写react组件的时候后缀请使用jsx，为了便于区分组件与封装的js文件**）
  - App.test.js：测试文件（可以删除）
  - index.js：类似于main.js，是项目执行的入口文件（打包入口）
  - reportWebVitals.js：谷歌新增的性能优化库文件（可以不要）
  - setupTests.js：针对项目index.js的一个单元测试文件（可以不要）

> 了解了react的目录结构后，可以对初始化的项目进行文件清理。**此处将`src`与`public`目录中的内容全部删除即可，后期如果需要自己往里面写内容。**



# 四、组件

![组件](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/09/279e26e948d53d33a2a05e10e7c29aa736fe80a1.png?sign=9f63925b42a763bb945ad25cda41928f&t=5f5073bf)

组件允许我们将UI拆分为独立可复用的代码片段，并对每个片段进行独立构思。从概念上类似于JavaScript函数，它接受任意的入参（props），并返回用于描述页面展示内容的React元素（JSX）。

在react中，组件的形式有2种：

- 函数组件（拥抱函数式开发方式，面向过程）
  - 无状态（函数组件也被称之为无状态组件）
  - 无生命周期
- 类组件（面向对象）
  - 有状态
  - 有生命周期



## 1、组件的创建方式

> 在react17之后，允许在项目不用“import React from "react";”，但是在之前的版本是不行的。建议写，肯定不会错。



### 1.1、函数创建组件

通过函数创建的组件有以下特点：

- 函数组件（无状态组件）：使用JS的函数创建组件
- 函数名称以大写字母开头（建议）
- 函数组件**必须有返回值**，表示该组件的结构（虚拟DOM），且内容必须有顶级元素
- 函数组件是没有生命周期的

例如，新建组件文件`src/App.jsx`：

> 约定：组件后缀可以是`.js`也可以是`.jsx`，为了方便区分组件与项目的业务代码，建议组件用`.jsx`，业务代码后缀用`.js`。

~~~jsx
import React from 'react'
// 函数名首字母必须大写
function App() {
    return (
        <div>这是第一个函数组件</div>
    )
}

export default App;
~~~

要想输出效果，可以再创建项目入口文件`src/index.js`：

~~~javascript
import React from "react";
import ReactDOM from "react-dom";

import App from "./App";

ReactDOM.render(<App></App>, document.getElementById("root"));
~~~

> 注意，由于之前清理了项目，当前项目中现在是没有挂载点的，所以需要在`public/`下创建一个html文件`index.html`，在其body中设置一个挂载位置：
>
> ~~~html
> <div id="root">
>     
> </div>
> ~~~



### 1.2、类组件

类组件有以下特点：

- 使用ES6语法的class创建的组件（有状态组件）

- 类名称为大写字母开头（建议）

- 类组件要继承React.Component父类，从而可以使用父类中提供的方法或者属性

- 类组件**必须**提供render方法，用于页面结构渲染，结构必须要有顶级元素，且必须return去返回

~~~jsx
import React from "react";
// 创建class类，继承React.Component，在里面提供render方法，在return里面返回内容
class App extends React.Component {
    render() {
        return <div>这是第一个类组件</div>;
    }
}

export default App;
~~~

除了上述的写法以外，还可以对`React.Component`进行按需导入，写成以下形式：

~~~jsx
// 引入react和Component
import React,{Component} from "react";

// 类组件
class App extends Component {
    render() {
        return <div>这是第一个类组件</div>;
    }
}

// 导出
export default App;
~~~



## 2、组件传值（父-子）

组件间传值，在React中是通过**只读**属性props来完成数据传递的。

props：接受任意的入参，并返回用于描述页面展示内容的React元素。

### 2.1、函数组件

函数组件传值使用props：以形参的形式给函数传递`props`参数。（与vue的思想是一样的）

例如，有子组件`src/Components/Item.jsx`，代码如下：

~~~jsx
import React from "react";

const Item = (props) => {
    return (
        <div>
            海纳百川有容乃大，{props.next}。 -- {props.name}
        </div>
    );
};

export default Item;
~~~

要想在父组件中给其传递`name`和`next`值，则父组件`src/App.jsx`可以写成：

~~~jsx
import React from "react";

import Item from "./Components/Item";

class App extends React.Component {
    render() {
        return <Item name="林则徐" next="壁立千仞无欲则刚"></Item>;
    }
}

export default App;
~~~

> React的父传子的方式与Vue类似，都是通过调用子组件给子组件传递自定义属性方式进行传值的。



### 2.2、类组件

在父组件中通过自定义属性向子组件传值后，如何在子级类组件中获取传递过来的值呢？

我们可以在子级类组件中通过`this.props`属性来获取传递到子组件的值，如下：

~~~jsx
import React, { Component } from "react";

class Item extends Component {
    render() {
        return (
            <div>
                 海纳百川有容乃大，{this.props.next}。 -- {this.props.name}
            </div>
        );
    }
}

export default Item;
~~~



# 五、 事件处理

## 1、事件绑定

React元素的事件处理和DOM元素的很相似，但是有一点语法上的不同。React元素的事件绑定采用`on+事件名`的方式来绑定一个事件，注意，这里和原生的事件是有区别的，**原生的事件全是小写**，如`onclick`, React里的事件是驼峰如`onClick`，**React的事件并不是原生事件，而是合成事件**。

在React里，类组件与函数组件绑定事件是差不多的，只是在类组件中绑定事件函数的时候需要用到`this`，代表指向当前的类的引用，在函数中不需要调用`this`关键词。

**函数组件事件绑定**

~~~jsx
import React from "react";

const clickHandler = () => {
    console.log("海纳百川有容乃大，壁立千仞无欲则刚。");
};

const App = () => {
    return <button onClick={clickHandler}>老林说</button>;
};

export default App;
~~~

**类组件事件绑定**

~~~jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return (
            <div>
                // 使用JSX语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。
                <button onClick={this.clickHandler}>老林说</button>
            </div>
        );
    }
    clickHandler() {
        console.log("海纳百川有容乃大，壁立千仞无欲则刚。");
    }
}

export default App;
~~~

> 注意点：
>
> - 在写事件的时候，调用时如果不涉及传参的话，一定不要加`()`，加了就错。
> - 在类组件中写事件处理程序的时候，不能写标准的`function xxx () {}`，写了就报错，一定要用简化的写法或者箭头函数形式
> - 事件处理属性名称（事件绑定时用的属性）一定要使用符合react的小驼峰写法



## 2、事件对象

React中可以通过事件处理函数的参数获取到事件对象，它的事件对象叫做：合成事件，**即兼容所有浏览器**，无需担心跨浏览器兼容问题。这个对象和之前学习的事件对象所包含的方法和属性都基本一致，不同的是React中的事件对象并不是浏览器提供的，而是它自己内部所构建的。此事件对象拥有和浏览器原生事件相同的接口，包括`stopPropagation()`和 `preventDefault()`，如果我们想获取到原生事件对象，可以通过`e.nativeEvent`属性来进行获取。

~~~jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>老林说</button>
            </div>
        );
    }
    clickHandler(e) {
        console.log("海纳百川有容乃大，壁立千仞无欲则刚。");
        // react构建的事件对象
        console.log(e);
        // 浏览器原生的事件对象
        console.log(e.nativeEvent);
        // 事件对应的DOM对象
        console.log(e.target);
        // 事件对应的DOM对象的内嵌HTML
    	console.log(e.target.innerHTML);
    }
}

export default App;
~~~



## 3、事件方法传参

React中对于事件方法传参的方式有着非常灵活的用法。以传递参数`username`值为`zhangsan`为例，常见的有以下几种方式：

- 类组件特有的，通过`this.事件方法.bind`（bind为绑定数据）方式进行传参（**推荐**），例如：
  - `onClick={this.clickHandler.bind(this,'zhangsan')}`
    - 对应的形参接收：`clickHandler(username)`
  - `onClick={this.clickHandler.bind(this,'zhangsan')}`
    - 对应的形参接收：`clickHandler(username,event)`

- 两种组件类型都可以使用，使用箭头函数传参，例如：
  - `onClick={() => this.事件方法('zhangsan')}`
    - 对应的形参接收：`clickHandler(username)`
  - `onClick={(e) => this.事件方法('zhangsan',e)}`
    - 对应的形参接收：`clickHandler(username,event)`

> 关于this：指的是当前的这个组件对象，需要注意其指向问题。具体可以看下一节。



## 4、this指向问题

在JSX事件函数方法中的this，默认不会绑定this指向。如果我们忘记绑定，当我们调用这个函数的时候this的值为undefined。所以使用时一定要绑定好this的指向！

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/000bb708d55935b83a611f37318ee1d89c32bb26.png?sign=a0d08668cfee717e94664b0682dfaa23&t=5f95992c)

例如，像下面这段代码回调函数中的`this`输出为`undefined`：

~~~jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>
                    老林说
                </button>
            </div>
        );
    }
    clickHandler() {
        console.log(this);
    }
}

export default App;
~~~

解决上面出现的`this`指向问题的方式有以下几种：

- 通过类组件的构造函数进行绑定

~~~jsx
import React, { Component } from "react";

class App extends Component {
    constructor(props) {
        super(props)
        // 解决this指向问题
        this.clickHandler = this.clickHandler.bind(this)
    }
    
    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>
                    老林说
                </button>
            </div>
        );
    }
    
    clickHandler() {
        console.log(this);
    }
}

export default App;
~~~

- 使用bind绑定

~~~jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return (
            <div>
                <button onClick={this.clickHandler.bind(this)}>
                    老林说
                </button>
            </div>
        );
    }

    clickHandler() {
        console.log(this);
    }
}

export default App;
~~~

- 使用箭头函数：方式1

~~~jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return (
            <div>
                <button onClick={() => this.clickHandler()}>老林说</button>
            </div>
        );
    }

    clickHandler() {
        console.log(this);
    }
}
~~~

- 使用箭头函数：方式2

~~~jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>老林说</button>
            </div>
        );
    }

    clickHandler = () => {
        console.log(this);
    }
}

export default App;
~~~

> 后续使用this（特别是在自定义事件处理程序中），一定要注意绑定绑定this指向。



# 六、State状态

> 如果将state与vue中的某个点做类比的话，则其相当于vue组件中的`data`，作用就是用于存储当前组件中需要用到的数据。

状态就是组件描述某种显示情况的数据，由组件自己设置和更改，也就是说由组件自己维护，使用状态的目的就是为了在不同的状态下使组件的显示不同。

`state`状态只在class类组件才有，**函数组件没有此功能！**

## 1、基本使用

- 状态（state）即数据，是组件内部的私有数据，只能在组件内部使用
- state的值是对象，表示一个组件中可以有多个数据
- 通过this.state.xxx来获取状态（对应vue的this.xxxx）
- state数据值**只能**通过this.setState()来修改（与vue不同）
- Do not mutate state directly. Use setState()
- state可以定义在类的构造方法中也可以写在类的成员属性中

**第一种设置方式**

~~~jsx
import React, { Component } from "react";

class App extends Component {
    // 构造函数初始state
    constructor(props) {
        super(props);
        this.state = {
            count: 0,
        };
    }
    render() {
        return <div>{this.state.count}</div>;
    }
}

export default App;
~~~

**第二种设置方式（推荐）**

~~~jsx
import React, { Component } from "react";

class App extends Component {
    // 常规初始化
    state = {
        count: 0,
    };
    render() {
        return <div>{this.state.count}</div>;
    }
}

export default App;
~~~

> 切记：不要直接通过`this.state.xxx = xxxx`形式去更改state的值。否则会包警告，警告如下：Do not mutate state directly. Use setState()。



## 2、修改状态

在vue中，data属性是利用`Object.defineProperty`处理过的，更改data的数据的时候会触发数据的`getter`和`setter`，但是React中没有做这样的处理，如果直接更改的话，react是无法得知的，所以，需要使用特殊的更改状态的方法`setState`。

`setState`接受2个参数，第一个参数负责对state自身进行修改（必须的），我们称之为`updater`；第二个参数是一个回调函数（可选），因为`setState`方法是异步的，如果想在更新好状态后做进一步处理，此时就可以用到第二个参数了。

语法：**this.setState(updater[,callback])**

`updater`参数传递的时候支持两种形式：**对象形式**与**==函数形式==**

**对象形式**

~~~jsx
this.setState({
    count: this.state.count + 1
})
~~~

**函数形式**

~~~jsx
this.setState(state => {
    return {
        count: state.count + 1
    }
})
~~~

上述两种参数形式的`updater`建议使用**函数形式**。因为对象形式在批量使用的时候会存在问题，因此建议使用函数形式。



## 3、props与state的区别

> props = vue中的props
>
> state = vue中的data

- props中存储的数据，都是外界传递到组件中的

- props 中的数据，都是只读的

- state 中的数据，都是可读可写的（写的时候得用setState()）

- props 在函数声明或类申明的组件中都有

- state 只有类声明的组件中才有

`state`的主要作用是用于组件保存、控制、修改自己的可变状态。`state`在组件内部初始化，可以被组件自身修改，而外部不能访问也不能修改。你可以认为`state`是一个局部的、只能被组件自身控制的数据源。`state` 中状态可以通过`this.setState`方法进行更新，`setState`会导致组件的重新渲染。

`props`的主要作用是让使用该组件的父组件可以传入参数来配置该组件。它是外部传进来的配置参数，组件内部无法控制也无法修改。除非外部组件主动传入新的`props`，否则组件的`props`永远保持不变。



# 七、Props进阶

## 1、children属性

`children`属性表示组件标签的**子**节点，**当组件标签有子节点时**，接受传值的子组件中的props里就会有该属性，与普通的props一样，其值可以使任意类型。

例如，在父组件中有如下代码：

~~~jsx
import React, { Component } from "react";
import Cmp from './Components/Cmp'

class App extends Component {
    state = {
        content: "蚂蚁集团A股发行价确定",
    };
    render() {
        return <Cmp>{this.state.content}</Cmp>;
    }
}

export default App;
~~~

上述代码中的`{this.state.content}`即为子组件标签中的子节点，那么在子组件的props属性里就存在一个children属性可以被我们使用。如下：

~~~jsx
import React, { Component } from "react";

class Cmp extends Component {
    render() {
        return <div>{this.props.children}</div>;
    }
}

export default Cmp;
~~~

> 需要注意，**如果子组件标签里只存在一个子节点，则children属性值为一个字符串；如果子组件标签里存在多个子节点，那么children属性的值为一个索引数组。**

简而言之，上述写法形式有点像Vue里的插槽，但是不是，children这一小结所讲的内容简单来说就是父传子的另外一种写法而已：原先父传子是将值写在了**组件标签的属性中**，只不过现在写在了**组件标签里**而已。



## 2、props-type

> 关于JavaScript的class中的静态成员与常规成员
>
> ~~~javascript
> class App {
> 	static uname = 'zhangsan'
> 	name = 'lisi'
> }
> console.log((new App).name);
> console.log(App.uname);		
> // 常规的属性是在对象里的，如果要用得先实例化
> // 静态属性是类里面的，使用的时候不要实例化
> // 静态成员要优先于常规的成员
> ~~~

React是为了构建大型应用程序而生，在一个大型应用开发过程中会进行多人协作，往往可能根本不知道别人使用你写的组件的时候会传入什么样的参数，这样就有可能会造成应用程序运行不了但是又不报错的情况。所以必须要对于`props`设传入的数据类型进行校验。

为了解决这个问题，React提供了一种机制，让写组件的人可以给组件的`props`设定参数检查，需要安装和使用[prop-types](<https://www.npmjs.com/package/prop-types>)：

~~~shell
npm i -S prop-types
~~~

在使用时，无论是函数组件还是类组件，都需要对`prop-types`进行导入：

~~~jsx
import PropTypes from "prop-types"
~~~

随后依据使用的组件类型选择对应的应用方式，如果是函数组件则按照下面的方式应用：

~~~jsx
function App(props){
    // 函数组件声明过程
    return "";
}
// 为App方法组件挂上验证规则
App.propTypes = {
    // 待验证的属性名：PropTypes.类型规则[.isRequired]
    prop-name:PropTypes.string,
    // ... 
}
~~~

如果是类组件则按照下面的方式应用：

~~~jsx
class App extends Component{
    // 类内部完成检查
    static propTypes = {
       // 待验证的属性名：PropTypes.类型规则[.isRequired]
       prop-name:PropTypes.string
    }
	// 渲染
	render() {
        return "";
    }
}
~~~

本质上来上面类组件的写法与方法组件的写法是一样的，只不过考虑到类组件的代码完整性，我们把规则的验证做成了类的静态成员属性的方式，如果还原成最初的代码则如下：

~~~jsx
class App extends Component {
    render() {
         return <div>{this.props.flag}--{this.props.num}</div>;
    }
}

App.propTypes = {
    flag: PropTypes.string,
    num: PropTypes.number.isRequired,
};
~~~

需要注意，`isRequired`规则必须放在最后且不能独立于其他规则存在。更多的验证规则，可以参考[React官网](https://reactjs.org/docs/typechecking-with-proptypes.html)。



## 3、默认值

如果`props`有属性没有传来数据，为了不让程序异常，我们可以依据业务情况给对应的属性设置默认值。

依据组件类型的不同选择对应的操作方式，如果是函数组件则采用如下方式：

~~~jsx
function App(props){
    // 函数组件声明过程
    return "";
}
// 为App方法组件挂上默认值
App.defaultProps = {
    // 属性名：默认值
    title: "标题",
    // ...
}
~~~

如果使用的是类组件则使用如下方式：

~~~jsx
class App extends Component {
    // 添加静态成员属性`defaultProps`设置props的默认值
    static defaultProps = {
        // 属性名：默认值
        title: "标题"
        // ...
    }
}
~~~



# 八、生命周期（重点）

**函数组件无生命周期一说。**后续提到的生命周期钩子函数都是针对类组件的。但是在函数组件中有其它的办法去实现类似于类组件中的生命周期的效果。（后期再说：hooks）

生命周期函数指在某一时刻组件会自动调用并执行的函数。React每个类组件都包含`生命周期方法`，我们可以重写这些方法，以便于在运行过程中特定的阶段执行这些方法。例如：

我们希望在第一次将其呈现到DOM时设置一个计时器`Clock`。这在React中称为“安装”（挂载）。

我们也想在删除由产生的DOM时清除该计时器`Clock`。这在React中称为“卸载”。

参考：https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

**完整的生命周期图**

![完整的生命周期图](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/d96aceebbcf9bf3dccc95b3afc7b2f75ef3e59ec.png?sign=81ab7fb514b47c66446ba45f557afe0f&t=5f96fd9b)



**常用的生命周期图**

![常用的生命周期图](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/06b9e675773182f54d91647cbf48ec2b5aa43de9.png?sign=2119db1b051cbdc67293ac5351be7884&t=5f96fddc)

**1.constructor(props)**

React组件的构造函数在挂载之前被调用。在实现`React.Component`构造函数时，需要先在添加其他内容前，==调用`super(props)`==，用来将父组件传来的`props`绑定到这个类中，使用`this.props`将会得到。

**2.static getDerivedStateFromProps(nextProps, prevState)**

`getDerivedStateFromProps` 是react16.3之后新增，在组件实例化后，和接受新的`props`后被调用。他必须返回一个对象来更新状态，或者返回null表示新的props不需要任何state的更新。

作用：==从父组件中接收props属性，写入当前子组件的state中==

~~~vue
// vue的data初始值
data() {
	return {
		a: 0
	}
}
props: ['num'],

// 生命周期函数  当前类似于getDerivedStateFromProps
mounted(){
	this.a = this.num;
}
~~~

~~~react
// getDerivedStateFromProps 作用是接收父传递的属性并且更新给自己的state
state = {
    num: 0,
    username: "zhangsan",
    age: 18,
};

render() {
    return <div>这是子组件</div>;
}

// 生命周期getDerivedStateFromProps
// 参数1：父传递过来的props对象
// 参数2：当前子组件自身的state对象
static getDerivedStateFromProps(Props, State) {
    // console.log(Props, State);
    // 这个生命周期函数必须有返回值：
    // 情况1：如果想要更新组件自身的state，则返回一个state对象
    // 情况2：如果不需要跟新自身的state，则返回null
    // 更新与否的条件是，需求的props数据是否与state的数据项的值相等
    // 判断的时候要用全等符号
    if (Props.newnum === State.num) {
        return null;
    } else {
        // 返回的这个对象会与原先的state对象合并（不是覆盖）
        return { num: Props.newnum };
        // State.num = Props.newnum;
        // return false;
    }
}
~~~

如果是由于父组件的`props`更改，所带来的重新渲染，也会触发此方法。

调用`setState()`不会触发`getDerivedStateFromProps()`。【**react16.3及以前，在此以后setState()调用后以及强制更新的时候都会触发该周期。**】

之前这里都是使用`constructor`+`componentWillRecieveProps`完成相同的功能的。

**~~3. componentWillMount() / UNSAFE_componentWillMount()~~**

已被弃用。

**4.render()**

render()方法是必需的，它主要负责组件的渲染，会被重复调用若干次，不建议在此处写异步代码（大概率会死循环）。

在该组件生命周期函数中不要写对于页面上渲染的数据的更新操作，如果这么做，可能出现死循环。

**5. componentDidMount**

==类似于vue的mounted==

`componentDidMount`在组件被装配后立即调用。初始化使得DOM节点应该进行到这里。

==**`通常在这里进行ajax请求`**==

如果要初始化第三方的dom库，也在这里进行初始化。只有到这里才能获取到真实的dom.

**~~6.componentWillReceiveProps()/UNSAFE_componentWillReceiveProps(nextProps)~~**

已被弃用。

**7.shouldComponentUpdate(nextProps, nextState)**

调用`shouldComponentUpdate`使React知道，组件的输出是否受`state`和`props`的影响。默认每个状态的更改都会重新渲染，大多数情况下应该保持这个默认行为。

在渲染新的`props`或`state`前，`shouldComponentUpdate`会被调用。默认为`true`。这个方法不会在初始化时被调用，也不会在`forceUpdate()`时被调用。返回`false`不会阻止子组件在`state`更改时重新渲染。

如果`shouldComponentUpdate()`返回`false`，`componentWillUpdate`,`render`和`componentDidUpdate`不会被调用。

> 在渲染新的`props`或`state`前，`shouldComponentUpdate`会被调用。来确定组件是否需要被重新渲染。其作用是用于实现组件渲染的性能优化。其返回值是bool，true则表示该组件需要被重新渲染，false则表示该组件不用被重新渲染。

~~~react
class App22Child1 extends Component {
    render() {
        console.log("走了Child1的渲染生命周期");
        return <div>父组件传来的值是{this.props.c1}</div>;
    }

    // 决定当前组件是否渲染
    // true=渲染，false=不渲染
    // 参数1：是父传递过来的props
    // 参数2：是当前组件自身的state对象
    shouldComponentUpdate(Props, State) {
        // console.log(Props, State);
        // 问题：什么时候要更新当前这个组件？？？
        // 答：当前收到的值是否与之前的值一样，如果一样则不更新
        if (Props.c1 === this.props.c1) {
            return false;
        } else {
            return true;
        }
    }
}

// 对于刚才上述的这个写法虽然本着提升性能目的的，但是写起来非常费劲，而且如果有很多数据的话，则判断起来也非常麻烦，因此react给我们做了简化操作。
// 使用方式：在导入成员的时候导入`PureComponent`组件，将继承关系改成继承`PureComponent`即可，原先的`Component`继承就不用了。
// 其会自动帮我们判断组件是否需要进行下一次渲染。

import React, { PureComponent } from "react";

class App22Child1 extends PureComponent {
    render() {
        console.log("走了Child1的渲染生命周期");
        return <div>父组件传来的值是{this.props.c1}</div>;
    }

    // 决定当前组件是否渲染
    // true=渲染，false=不渲染
    // 参数1：是父传递过来的props
    // 参数2：是当前组件自身的state对象
    // shouldComponentUpdate(Props, State) {
    //     // console.log(Props, State);
    //     // 问题：什么时候要更新当前这个组件？？？
    //     // 答：当前收到的值是否与之前的值一样，如果一样则不更新
    //     if (Props.c1 === this.props.c1) {
    //         return false;
    //     } else {
    //         return true;
    //     }
    // }
}

export default App22Child1;

~~~

**~~8.UNSAFE_componentWillUpdate(nextProps, nextState)~~**

已被弃用。

**9.getSnapshotBeforeUpdate()**

在react `render()`后的输出被渲染到DOM之前被调用，用于获取渲染之前的DOM信息，**需要配合componentDidUpdate()一起使用。**

在vue中，类似于`beforeUpdate()`

**10.componentDidUpdate(prevProps, prevState, snapshot)**

==类似于vue的updated生命周期==

在更新发生后立即调用`componentDidUpdate()`。

**11.componentWillUnmount()**

==类似于vue的beforeDestory==

在组件被卸载并销毁之前立即被调用。在此方法中执行任何必要的清理，例如使定时器无效，取消网络请求或清理在`componentDidMount`中创建的任何监听。

**12.componentDidCatch(error, info)**

错误边界是React组件，可以在其子组件树中的任何位置捕获JavaScript错误。



> 常用的生命周期：
>
> - **constructor**
> - getDerivedStateFromProps
> - **render**
> - **componentDidMount**
> - **componentWillUnmount**



# 九、表单处理

## 1、特别说明

有以下示例代码：

~~~jsx
import React, { Component } from "react";

class App extends Component {
    state = {
        msg: "hello world",
    };
    render() {
        return (
            <div>
                <input type="text" value={this.state.msg} />
            </div>
        );
    }
}

export default App;
~~~

通过运行后我们可以在浏览器的`consle`控制台找到React给予我们的提示：

> **Warning**: You provided a `value` prop to a form field without an `onChange` handler. This will render a read-only field. If the field should be mutable use `defaultValue`. Otherwise, set either `onChange` or `readOnly`.

通过上述的警告提示，我们可以得知，在React中并不存在类似于Vue的双向数据绑定操作。此处需要注意以下几点：

- Vue中的`v-model`是语法糖

- 在React里使用的是**单向**数据流

由于在React里数据流是单向的，所以我们就必须得考虑一个问题：怎么获取用户在表单中输入的数据呢？解决办法：

- 给表单项添加`onChange`事件【受控组件】
- 给表单项的value/checked，设置成defaultValue/defaultChecked【非受控组件】

React推荐我们使用受控组件。



## 2、受控组件

将`state`与表单项中的`value`值绑定在一起，由`state`的值来控制表单元素的值，称为受控组件。

绑定步骤：

- 在state中添加一个状态，作为表单元素的value值

- 给表单元素绑定change事件，将表单元素的值设置为state的值

~~~jsx
// 表单解决方案：受控组件

// 注意点：
// 1. 受控组件的定义：所谓受控组件，顾名思义就是受React控制的组件。其特点是将state在表单中的更新，通过onChange事件来进行操作。
// 2. 受控组件必须要求表单项中要有俩个组合的情况：
//  针对普通的文本框、多行文本框等：value属性+onChange事件类型
//  针对单选按钮组/复选框：checked属性+onChange事件类型
// 3. 每次onChange事件的触发都会重新渲染页面

// 案例需要：state中约定一个用户的基本信息，要求将信息展示在表单中，允许被用户修改，点击提交按钮获取用户修改后的最新的信息（state中的）

import React, { Component } from "react";

class App extends Component {
    // 初始化用户信息（项目中的话信息是来自于ajax）
    state = {
        userInfo: {
            uname: "zhangsan",
            mail: "zhangsan@1000phone.com",
            // 社交属性性别
            gender: "女",
            hobbies: ["1", "2"],
        },
    };
    render() {
        // 先结构用户信息，用起来方便
        let { uname, mail, gender, hobbies } = this.state.userInfo;
        return (
            <div>
                <div>
                    昵称：
                    <input type="text" name="uname" value={uname} onChange={this.vModel.bind(this)} />
                </div>
                <div>
                    邮箱：
                    <input type="text" name="mail" value={mail} onChange={this.vModel.bind(this)} />
                </div>
                <div>
                    性别：
                    <input type="radio" name="gender" value="男" checked={gender === "男" ? true : false} onChange={this.vModel.bind(this)} />男
                    <input type="radio" name="gender" value="女" checked={gender === "女" ? true : false} onChange={this.vModel.bind(this)} />女
                </div>
                <div>
                    爱好：
                    <input type="checkbox" name="hobbies" value="1" checked={hobbies.includes("1") ? true : false} onChange={this.vModel.bind(this)} />
                    吃饭
                    <input type="checkbox" name="hobbies" value="2" checked={hobbies.includes("2") ? true : false} onChange={this.vModel.bind(this)} />
                    睡觉
                    <input type="checkbox" name="hobbies" value="3" checked={hobbies.includes("3") ? true : false} onChange={this.vModel.bind(this)} />
                    撸代码
                </div>
                <button onClick={this.submit.bind(this)}>提交</button>
            </div>
        );
    }

    // 修改用户名
    // chgUname(e) {
    //     // 修改state中的数据
    //     // 获取最新的数据
    //     let val = e.target.value;
    //     // 获取要改的属性名
    //     let name = e.target.name;
    //     // 修改数据
    //     this.setState((state) => {
    //         // 对象有一个特点：引用传值
    //         state.userInfo[name] = val;
    //         return state;
    //     });
    // }

    // // 修改邮箱
    // chgMail(e) {
    //     let val = e.target.value;
    //     // 获取要改的属性名
    //     let name = e.target.name;
    //     // 修改数据
    //     this.setState((state) => {
    //         // 对象有一个特点：引用传值
    //         state.userInfo[name] = val;
    //         return state;
    //     });
    // }

    // 封装受控组件的表单数据与state的绑定关系
    vModel(e) {
        // 修改state中的数据
        // 获取最新的数据
        let val = e.target.value;
        // 获取要改的属性名
        let name = e.target.name;
        // 修改数据
        this.setState((state) => {
            // 判断是否是复选框的处理
            if (e.target.type === "checkbox") {
                // 多选
                // console.log('走了多选');
                // 思路：先去判断当前的值在原来的数据中是否存在，存在则移除，不存在则添加
                let index = state.userInfo[name].indexOf(val);
                if (index > -1) {
                    state.userInfo[name].splice(index, 1);
                } else {
                    state.userInfo[name].push(val);
                }
            } else {
                // 对象有一个特点：引用传值
                state.userInfo[name] = val;
            }
            return state;
        });
    }

    // 提交表单
    submit() {
        console.log(this.state.userInfo);
    }
}

export default App;
~~~

> 注意点：
>
> - 如果需要将原本应该分别处理的事件合在一起去写的话，一定需要多传递一个参数用于记录当前修改的是state中的哪一个值，此时可以选择以下方案中的任意一种：
>   - 通过相同`name`属性值去辨别，给每个表单项设置与state中相同的key名的name值，然后通过事件对象去获取
>   - 可以直接在事件绑定的位置传递标记，例如：`onChange={this.changeHandler.bind(this,'username')}`
> - 表单项中表单项类型为`checkbox`的比较特殊，与其他类型的不同，需要特殊处理（取反操作，而其余的表单项是来什么值用什么值）



## 3、非受控组件

没有和state数据源进行关联的表单项，而是**借助ref**，使用元素DOM方式获取表单元素值

使用步骤

- 调用React.createRef()方法创建ref对象

- 将创建好的ref对象添加到文本框中

- 通过ref对象获取到文本框的值

> 提示：一般表单项少的时候可以考虑使用非受控组件。
>
> 非受控组件不能给表单项加`value`属性，一旦加了它就不是非受控组件了，它就成了受控组件了。如果有默认值请通过`defaultValue`属性进行输出。

~~~jsx
// 表单处理：非受控组件

// 注意点：
// 1. 含义：非受控组件指的是不受react控制的组件
// 2. 非受控组件需要借助ref对象来实现对表单项的操作
// 3. 关于ref：是react中用户获取组件/元素对象的一个对象，在使用的时候需要通过createRef()进行创建，然后将创建好可变数据绑定到组件/元素上。后续根据元素的类型获取到对应信息：
//      如果元素是html标签的话，则获取到的是dom信息
//      如果元素是组件标签的话，则获取到的是该组件对应的组件实例（整个实例，是一个对象）
// 4. 非受控组件在实际表单处理应用中，获取到的都是dom对象，也就是说非受控组件基本使用的都是dom操作
// 5. 非受控组件操作的时候需要配合defaultValue和defaultChecked属性来实现，该操作不会像受控组件一样，每次改变数据导致页面组件重新渲染
// 6. 在使用ref的时候如果一个ref要对应多个表单项（如单选按钮组/复选框），则不能给每个元素都加ref，需要给其公共父去加

// 案例需要：约定一个用户的基本信息，要求将信息展示在表单中，允许被用户修改，点击提交按钮获取用户修改后的最新的信息
import React, { Component, createRef } from "react";

class App extends Component {
    constructor(props) {
        super(props);
        // 创建ref对象
        this.ref_uname = createRef();
        this.ref_mail = createRef();
        this.ref_gender = createRef();
        this.ref_hobbies = createRef();
        // ref对象是后续获取表单最新数据的唯一途径
        // 建议性操作，将所有的ref丢到一个数组中（后续可以循环取值）
        this.refArr = [this.ref_uname, this.ref_mail, this.ref_gender, this.ref_hobbies];
    }
    // 初始化用户信息（项目中的话信息是来自于ajax）
    state = {
        userInfo: {
            uname: "zhangsan",
            mail: "zhangsan@1000phone.com",
            // 社交属性性别
            gender: "女",
            hobbies: ["1", "2"],
        },
    };
    render() {
        console.log("走了render");
        // 先结构用户信息，用起来方便
        let { uname, mail, gender, hobbies } = this.state.userInfo;
        return (
            <div>
                <div>
                    昵称：
                    <input type="text" ref={this.ref_uname} name="uname" defaultValue={uname} />
                </div>
                <div>
                    邮箱：
                    <input type="text" ref={this.ref_mail} name="mail" defaultValue={mail} />
                </div>
                <div ref={this.ref_gender}>
                    性别：
                    <input type="radio" name="gender" value="男" defaultChecked={gender === "男" ? true : false} />男
                    <input type="radio" name="gender" value="女" defaultChecked={gender === "女" ? true : false} />女
                </div>
                <div ref={this.ref_hobbies}>
                    爱好：
                    <input type="checkbox" name="hobbies" value="1" defaultChecked={hobbies.includes("1") ? true : false} />
                    吃饭
                    <input type="checkbox" name="hobbies" value="2" defaultChecked={hobbies.includes("2") ? true : false} />
                    睡觉
                    <input type="checkbox" name="hobbies" value="3" defaultChecked={hobbies.includes("3") ? true : false} />
                    撸代码
                </div>
                <button onClick={this.submit.bind(this)}>提交</button>
            </div>
        );
    }

    // 提交：在点击之后获取最新的表单数据
    submit() {
        // 用于保存所有的数据，此处不能使用state及setState（一旦使用就是受控组件）
        let data = { userInfo: {} };
        // console.log(this.ref_uname.current);
        // 循环所有的ref逐个进行处理
        this.refArr.forEach((ref) => {
            // 获取到当前的dom对象
            let obj = ref.current;
            // 判断是否存在多个子
            if (obj.children.length > 0) {
                // 多个元素（还需要判断是单选还是复选）
                for (var i = 0; i < obj.children.length; i++) {
                    if (obj.children[i].type === "radio" && obj.children[i].checked) {
                        // 将单选按钮组中选中的那个值赋给data
                        data.userInfo[obj.children[i].name] = obj.children[i].value;
                    }
                    if (obj.children[i].type === "checkbox" && obj.children[i].checked) {
                        // 将多选框中选中的值保留
                        if (!data.userInfo[obj.children[i].name]) {
                            data.userInfo[obj.children[i].name] = [];
                        }
                        data.userInfo[obj.children[i].name].push(obj.children[i].value);
                    }
                }
            } else {
                // 单个元素
                let name = obj.name;
                let val = obj.value;
                data.userInfo[name] = val;
            }
        });
        // 最终的data就是需要提交的数据
        console.log(data);
    }
}

export default App;
~~~



# 十、组件通信

在React中，除了props可以实现数据传递以外，还支持两种形式数据传递：

- 通过ref对象形式进行传递
  - 父-子
  - 子-父
- 通过属性传递方法
  - 父-子
  - 子-父

## 1、父→子

该传值的实现可以分为两种，思想大致如下：

- 父通过`ref`标记子组件，通过`ref`获取子组件实例对象，父将自己的状态或数据以实参形式传递给子组件中预设的方法，在子组件中的预设方法以形参形式接收父组件传递来的数据，并保存到子组件自身的状态（父主动把数据给子）
- 在父组件中定义一个获取父组件自身状态或数据的方法，将该方法以`props`属性的形式传递给子组件，子组件收到后执行该方法即可获取到父组件的状态或数据（子去要数据）

~~~jsx
// 父传子的操作
// 1. 需要有父子关系，因此需要只少2个组件
// 2. 本次俩个组件不考虑用2个文件，换一种书写形式
// 方法有两种：
//      方式1：通过ref对象，将ref对象给子组件标签绑定上去，后续可以通过ref对象获取子组件整个实例。随后可以在子组件中定义一个方法，用于被ref对象调用，接收父数据
//      方式2：通过props，传递一个父中的方法（该方法用于返回父的数据），方法传递给子之后，子在需要父数据的时候执行即可（子去要数据）

import React, { Component, createRef } from "react";

// 父组件
class Father extends Component {
    // 构造函数
    constructor(props) {
        super(props);
        // 创建一个给子用的ref对象
        this.child = createRef();
    }
    // 父的数据
    state = {
        msg: "最开始玩股票的时候，只有跌的时候亏钱",
    };
    render() {
        return (
            <div>
                <Son ref={this.child} fun={this.returnMsgToChild.bind(this)}></Son>
                {/* 方法1：父主动把数据给子 */}
                <button onClick={this.sendMsgToSon.bind(this)}>点击按钮把消息发给子</button>
            </div>
        );
    }
    // 方法1：主动将数据给子
    sendMsgToSon() {
        // 由于打印输出方法在子组件中，如果要走打印则必须要去调用子组件中的方法
        this.child.current.getMsgFromFather(this.state.msg);
    }

    // 方法2：将数据准备好，等着子来要
    returnMsgToChild() {
        return this.state.msg;
    }
}

// 子组件
class Son extends Component {
    render() {
        return (
            <div>
                <div>这是子组件</div>
                <button onClick={this.giveMeMsg.bind(this)}>爸： 钱 儿</button>
            </div>
        );
    }

    // 方法1：接收父亲给的数据
    getMsgFromFather(msg) {
        console.log("子收到的消息是：" + msg);
    }

    // 方法2：向父组件要数据
    giveMeMsg() {
        // 执行父给的方法
        console.log("子向父要的数据是：" + this.props.fun());
    }
}

export default Father;
~~~



## 2、子→父

该传值的实现可以分为两种，思想大致如下：

- （父主动获取子的数据）父通过`ref`标记子组件，随后通过子组件实例对象获取子组件的数据
- 在父组件中预埋一个修改父组件自身的方法，将该方法以`props`的形式传递给子组件，子组件收到方法时去调用，并且将自己需要给父的数据以实参的形式给这个方法

~~~jsx
// 组件通信：子-父传值
// 1. 方式有两种，思路与前面基本一致
//      方法1：通过ref对象，给子组件绑定ref对象以获取整个子组件实例，也就获取到了子的state，进而可以获取state中的数据（被动，父去取的）
//      方法2：同时属性传递方法，方法依旧需要子去执行，但是子需要将自己的数据传递给该方法，由父接收并且可以打印输出（主动，子给父的）

import React, { Component, createRef } from "react";

// 父组件
class Father extends Component {
    // 构造函数
    constructor(props) {
        super(props);
        // 创建子组件的ref对象
        this.child = createRef();
    }
    render() {
        return (
            <div>
                <Son ref={this.child} fun={this.SonSetMsg.bind(this)}></Son>
                {/* 方法1：父主动去获取子的数据 */}
                <button onClick={this.getMsgFromSon.bind(this)}>子： 还钱！爸</button>
            </div>
        );
    }

    // 方法1：父主动去获取子的数据
    getMsgFromSon() {
        console.log(this.child.current.state.msg);
    }

    // 方法2：父亲准备一个篮子，空的，需要子往里放东西
    SonSetMsg(msg) {
        console.log(msg);
    }
}

// 子组件
class Son extends Component {
    state = {
        msg: "后来学会了做空，股票涨的时候也会赔钱",
    };
    render() {
        return (
            <div>
                <div>子组件</div>
                <button onClick={this.setMsg.bind(this)}>子把数据放篮子里</button>
            </div>
        );
    }
    // 方法2：子存放数据
    setMsg() {
        this.props.fun(this.state.msg);
    }
}

export default Father;
~~~



## 3、跨组件通信（了解）

网址：https://zh-hans.reactjs.org/docs/context.html

在react没有类似vue中的事件总线来解决这个问题。在实际的项目中，当需要组件间跨级访问信息时，如果还使用组件层层传递props，此时代码显得不那么优雅，甚至有些冗余。在react中，我们还可以使用context来实现跨级父子组件间的通信。

~~~javascript
import React, { Component, createContext } from "react"

const {
	Provider,
	Consumer
} = createContext()
~~~

> 提示：在React的context中，数据被看成了商品，发布数据的组件会用provider身份（卖方），接收数据的组件使用consumer身份（卖方）。

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/6e6a35f08f8620236171af7b7df86c6e497fad47.png?sign=75f71781d763be66f76b1ff2a37791d1&t=5f98f468)

- 创建Context对象

当React渲染一个订阅了这个Context对象的组件，这个组件会从组件树中离自身最近的那个匹配的Provider中读取到当前的context值。

~~~javascript
// 定义全局context
// 由于这个操作后期可能被复用，建议独立文件去创建。此处以`src/Context/index.js`为例
import { createContext } from "react"

export default createContext()
~~~

- 发布消息

在App.jsx组件中发布消息，这样所有的组件都可以消费它的消息。

~~~jsx
import React, { Component } from "react";
import Cmp1 from "./Components/Cmp1";
import Cmp2 from "./Components/Cmp2";
// 导入context对象
import ContextObj from "./Context/index";
let { Provider } = context;


class App extends Component {
    state = {
        count: 12345,
    };

    render() {
        return (
            <div>
                <Provider value={this.state.count}>
                    <Cmp6></Cmp6>
                    <Cmp7></Cmp7>
                </Provider>
            </div>
        );
    }
}

export default App;
~~~

- 组件消费

在子组件中通过Api完成消费动作，从而实现消息通信。消费的方式有2种：

方式1：通过组件消费

~~~jsx
import React, { Component } from "react";

import ContextObj from "../Context/index";
let { Consumer } = ContextObj;

class Cmp1 extends Component {
    render() {
        return (
            <div>
                <Consumer>
                    {(value) => {
                        return <div>获取到的值是：{value}</div>;
                    }}
                </Consumer>
            </div>
        );
    }
}

export default Cmp1;
~~~

方式2：通过绑定静成属性来消费

~~~jsx
import React, { Component } from "react";
import ContextObj from "../Context/index";

class Cmp2 extends Component {
    static contextType = ContextObj;
    render() {
        return <div>{this.context}</div>;
    }
}

export default Cmp2;
~~~



# 十一、HOC  - 高阶组件

高阶函数。

Higher - Order Components，**其实就是一个函数，传给它一个组件，它返回一个新的组件**。形如：

~~~jsx
const NewComponent = HOC(YourComponent)
~~~

通俗的来讲，`高阶组件`就相当于手机壳，通过包装组件，增强组件功能。例如最近成热点的华为Mate 40`环删保护壳`：

![](https://storage.lynnn.cn/assets/markdown/91147/documents/2020/10/4f524eb810cea9ceacbe324e73a770343a267593.jpeg?sign=ec3e937178bb72dea4305f755f7d8092&t=5f9993ca)

HOC实现步骤：

- 创建一个函数

- 指定函数参数，**参数应该以大写字母开头**

- 在函数内部创建一个类组件，提供**复用**的状态（如有）及逻辑代码，并返回

- 在该组件中，渲染参数组件，同时将状态通过prop传递给参数组件（可选，如有）

- 调用该高阶组件，传入要增强的组件，通过返回值拿到增强后的组件，并将其渲染到页面

比如，我们想要我们的组件通过自动注入一个版权信息（文件位置`src/Hoc/Hoc_copyright.js`）：

~~~jsx
import React, { Component, Fragment } from "react";

const withCopyright = (Cmp) => {
    return class Hoc extends Component {
        render() {
            return (
                <Fragment>
                    <Cmp></Cmp>
                    <div>&copy; 2020 千峰教育</div>
                </Fragment>
            );
        }
    };
};

export default withCopyright;

// Fragment是一个伪标签，渲染的时候是不会显示在页面中的，因此也不会影响视图显示
~~~

使用方式：

~~~jsx
import React, { Component } from "react";
// 引入HOC
import Hoc from './Hoc/Hoc_copyright'

class App extends Component {
    render() {
        return (
            <div>
                <h1>网站首页</h1>
            </div>
        );
    }
}

export default Hoc(App);
~~~

这样只要我们有需要用到版权信息的组件，都可以直接使用withCopyright这个高阶组件包裹即可。

提示：高阶组件是强化组件的一种方式，一般是具备复用的，如果只是某个组件需要强化，则没有必要写成高阶组件的形式，直接在需要强化的组件中写强化的代码即可。



# 十二、Redux（难）

## 1、简介

2013年Facebook提出了Flux架构的思想，引发了很多的实现。2015年，Redux出现，将Flux与函数式编程结合一起，很短时间内就成为了最热门的前端架构。

简单说，如果你的UI层非常简单，没有很多互动，Redux就是不必要的，用了反而增加复杂性。

如果你的项目的迭代变得越来越复杂，组件的数量和层级也变得越来越多，越来越深，此时组件间的数据通信就变得异常的复杂和低效，为了解决这个问题，引入了状态管理（**redux**）从而很好的解决多组件之间的通信问题。

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/a6087b34b9f365ef23b0a53fb6ca225099d5cfb6.png?sign=fbbd37dae9446bbcef3b79c60c2d31e3&t=5f99a237)

如果需要使用Redux请先进行安装：

网址：https://redux.js.org/introduction/getting-started

~~~shell
npm i -S redux
~~~

> 与vuex的区别：
>
> - 代码书写上vuex的代码会比redux的感觉简单一些
> - 两者在模块化上的实现也有区别，redux的模块化分的文件会更多，但是redux在命名空间层面的操作比vuex简单



## 2、三大原则（重点）

- 单一数据源
  - 整个应用的`state`（这个state不是组件中的state，请不要混淆）被储存在一棵对象结构树中，并且这个对象结构只存在于唯一一个store中
- State是只读的
  - 唯一改变state的方法就是触发dispatch+action，action是一个用于描述已发生事件的**普通对象**（action普通对象必须要有`type`属性，值是什么无所谓，其余属性也无所谓）。
- （最终修改数据的方法）使用**纯函数**（一个函数的返回结果只受到其形参的影响，则其就是纯函数）来执行修改
  - 为了描述action如何改变state tree ，我们需要编写reducer，==reducer必须是纯函数==，它接收先前的state和action，并返回**新的**state（不会合并的，自行注意这个坑）

**操作原理图**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/c2b874cdd9bc6f0579afebeedc5f958131a78040.png?sign=ec995f5c878596b98a9e1b3827aa4c4c&t=5f99b163)

a. store通过reducer创建了初始状态

b. view通过store.getState()获取到了store中保存的state挂载在了自己的状态上

c. 用户产生了操作（事件），调用了actions 的方法

d. actions的方法被调用，创建了带有标示性信息的action（描述对象，描述如何修改数据）

e. actions将action通过调用store.dispatch方法发送到了reducer中

f. reducer接收到action并根据标识信息判断之后返回了新的state（自己注意合并的问题）

g. store的state被reducer更改为新state的时候，store.subscribe方法里的回调函数会执行，此时就可以通知view去重新获取state

- store.getState()：用于获取仓库中初始的数据（一次性）
- store.dispatch()：用于派发修改数据的任务，参数是action普通对象
- store.subscribe(callback)：视图组件用于订阅新数据的方法（二次及以后的数据更新，使之产生类似于vue的响应式store数据）

> 纯函数是函数式编程的概念，必须遵守以下一些约束。
>
> - 不得改写参数
>
> - 不能调用系统 I/O 的API
>
> - 不能调用Date.now()或者Math.random()等不纯的方法，因为每次会得到不一样的结果

请注意：由于reducer被要求是纯函数，所以reducer函数里面不能改变State，必须返回一个全新的数据（不会自动合并原始数据的，因此一定要注意：别把原始数据搞丢了）。



## 3、redux的使用

**案例：在组件中展示一个按钮，点按钮后给redux中的数字+9，数字初始为0。实现一个计数器的效果**

步骤：

- 创建store
- 创建视图组件（展示store中的数据）
- 修改
- 回显数据到视图组件



**实现步骤**

a. 创建默认数据源：

~~~js
// 1. 这是仓库store

// a. 导入需要使用的成员
// createStore方法，作用用于产生仓库
import { createStore } from "redux";

// b. 创建数据源
// 默认数据源是一个普通对象，可以有很多的数据
const defaultState = {
    // 定义初始化的数据
    count: 0,
};

// c. 创建纯函数reducer（方法名叫什么无所谓）
// 作用：负责返回state（可能是直接返回state，也可能是返回修改后的state）
// 语法：reducer(state = defaultState,actions)
function reducer(state = defaultState, actions) {
    // 在返回之前写修改数据源的操作
    return state;
}

// d. 产生仓库
// 产生仓库的时候需要往仓库中存放数据源，因此需要传递reducer过去
const store = createStore(reducer);

// e. 导出仓库
export default store;

~~~

为了方便调试redux（可选安装），建议去谷歌商店安装`redux dev tools`，在使用的时候需要参考其[说明页面](https://github.com/zalmoxisus/redux-devtools-extension#usage)

> redux工具条在安装好之后不能直接使用，需要配置仓库代码，然后才能使用。

~~~js
// d. 产生仓库
// 产生仓库的时候需要往仓库中存放数据源，因此需要传递reducer过去
const store = createStore(
    reducer,
    // 必须要加上一段插件的配置工具，才能在浏览器中使用redux扩展
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
~~~

显示效果：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/01/8a98ff5a63061eb039045fe1c42cd955d518311c.png?sign=7f00fc5ce078fb8192e4014f51649cb6&t=600a9550)



b. 建立视图组件并且展示数据源

~~~react
import React, { Component } from "react";
// 需要导入store
import store from "../store/index";
class Counter extends Component {
    // 在constructor中获取store中的数据
    constructor(props) {
        super(props);
        // 获取store数据（一次性，不具备响应式）
        this.state = store.getState();
        // 订阅数据的更新
        store.subscribe(() => this.setState(() => store.getState()));
    }
    render() {
        console.log(this.state);
        return (
            <div>
                <div>当前Store中的数据是：{this.state.reducer.count}</div>
                <button onClick={this.addCount.bind(this)}>点击+9</button>
                <hr />
                <div>当前Store中的数据是：{this.state.reducer2.age}</div>
                <button onClick={this.addAge.bind(this)}>点击+1</button>
            </div>
        );
    }

    // 点击+9
    addCount() {
        // 描述数据如何更改的对象，其必须有type属性
        let action = { type: "mod_count", payload: 9 };
        // 通过store.dispatch去派发action（会将该action派发给所有的reducer，每个reducer都会被执行，因此一定要注意type的取值）
        store.dispatch(action);
    }

    // 点击+1
    addAge() {
        let action = { type: "mod_age", payload: 1 };
        store.dispatch(action);
    }
}

export default Counter;
~~~

c. 修改操作

视图组件中的代码：

~~~js
handler() {
    // 3. +9这个修改操作需要通过普通对象去描述（actions）
    const action = {
        // type是用于在reducer方法中做条件判断用的
        type: "add",
        // 另一个属性用于声明本次修改具体的值是多少
        payload: 9,
    };
    // 派发修改任务
    store.dispatch(action);
}
~~~

仓库文件的代码：

~~~js
function reducer(state = defaultState, actions) {
    console.log(actions);
    //判断是否是加法操作
    if (actions.type === "add") {
        return { count: state.count + actions.payload };
    }
    // 在返回之前写修改数据源的操作
    return state;
}
~~~

d. 回显新的数据

~~~js
// 构造函数
constructor(props) {
    super(props);
    // 2. 在视图组件中获取初始的仓库数据
    // getState()方法是store对象内置的方法
    this.state = store.getState();
    // 4. 订阅新的数据
    store.subscribe(() => {
        // 获取新数据修改当前的state
        this.setState(() => store.getState());
    });
}
~~~

如果有多个reducer，需要通过`combineReducers`方法进行合并，代码如下：

~~~js
// 合并reducer
// 有点类似于vuex的命名空间
const reducers = combineReducers({ reducer, reducer2 });

// c. 创建store对象（通过createStore方法），目前（后续有变）其参数就是reducer
const store = createStore(reducers, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());
~~~



## 4、模块化

针对redux的模块化，在一个常规项目中会将其代码拆分成以下几个部分：

- States：建立同名目录，存放模块化之后的state（默认数据源）

- Reducers，建立同名目录，存放模块化之后的reducer
- Actions：建立同名目录，存放模块化之后的action
- Type（可选）：建立同名目录，存放独立的type声明
  - 注意：在整个项目中，**对于不同数据源的更改时使用的type名称不能重复**，这个一定要注意。（原因redux在做修改数据的时候，**其原理是依据type的值去循环每个reducer，找到匹配的去执行**，为了避免出现同名，建议type集中书写）

具体实现，以项目的代码为准。

> 由于代码已经经过模块化，在获取redux中的数据的时候需要更改获取方式，比如说之前获取count是写成：this.state.count，模块化之后需要写成：this.state.counter.count，比之前多了一个模块化的模块名称（等同于vuex中命名空间）



## 5、react-redux

网址：https://react-redux.js.org/

React-Redux是Redux的官方针对React开发的扩展库，默认没有在React项目中安装，需要手动来安装。react-redux是依赖于redux，所以必须先安装redux。

我们可以理解为react-redux就是redux给我们提供一些高阶组件。

~~~shell
npm i -S react-redux
~~~

React-redux所能解决的问题是：

- 使用它以后我们不需要在每个组件中再去 手动订阅数据的更新了。
- 没有了数据的初始化state赋值，当前组件自身state和这个redux不冲突了
- 使用本节的react-redux与下一节的redux-thunk并不是为了简化代码的，它们存在的意义是解决前面所遇到的问题

**使用步骤**

- 在项目入口文件中定义Provider

  - 该步骤的操作有点类似于之前组件通信中的context那块的操作

  - 将整个仓库作为商品提供给App根组件，后续的所有的组件都可以获取到仓库store中的数据

  - 注意：与context不一样，这里绑定数据使用的属性是“store”

  - src/index.js文件中的示例代码：

  - ~~~js
    // 导入
    import React from "react";
    import ReactDOM from "react-dom";
    // 导入provider
    import { Provider } from "react-redux";
    import store from "./store/index";
    
    // 导入需要展示的组件
    import App from "./Login";
    
    // 渲染视图
    // 在展示app组件的时候需要按照组件的形式进行操作
    ReactDOM.render(
        <Provider store={store}>
            <App></App>
        </Provider>,
        document.getElementById("root")
    );
    ~~~

- 在需要使用redux的组件中使用

  - 这个步骤与vuex中map系列函数（mapState，mapMutations，mapActions、mapGetters）的思想是一样的

  - 思想：将仓库中的属性和方法映射成当前组件自身的属性和方法

  - 在实际使用的时候组件中不再需要使用store对象了（包括初始的获取数据：store.getState()、store.dispatch(）、store.subscribe()）

  - 步骤

    - 在需要使用reudx的组件前面导入react-redux提供的高阶组件：connect

    - 编写映射方法（请注意，这个方法映射不是类组件的方法，而是在类组件外写的方法）

      - mapStateToProps(state)
        - 作用：将仓库中的state数据源映射成本组件的属性props，返回一个props对象
        - 参数：仓库中的state
      - mapDispatchToProps(dispatch)
        - 作用：将派发action的方法映射成当前组件自身的属性，该方法也要求返回一个对象，该对象中存放的就是派发action的方法集合
        - 参数：dispatch如同之前的store.dispatch()
      - 编写时，可以写箭头函数，也可以写常规函数

    - 应用高阶组件connect，写法是固定的

      - ~~~js
        // 在组件最后导出的时候改写成如下：
        export default connect(mapStateToProps,mapDispatchToProps)(ComponentName)
        ~~~

  - 组件中实际使用时的参考代码：以jsx为例

  - ~~~react
    import React, { Component } from "react";
    // 需要导入store
    // import store from "../store/index";
    // 导入action创建模块（导出里面全部的方法）
    import * as actionCreate from "../store/actions/index";
    // 导入type
    import { MOD_COUNT, MOD_AGE } from "../store/types/index";
    // import * as types from "../store/types/index";
    
    // 第一步：在需要使用redux组件中导入一个由react-redux提供的hoc
    import { connect } from "react-redux";
    class Counter extends Component {
        // 在constructor中获取store中的数据
        constructor(props) {
            super(props);
            // 获取store数据（一次性，不具备响应式）
            // this.state = store.getState();
            // 订阅数据的更新
            // store.subscribe(() => this.setState(() => store.getState()));
        }
        render() {
            console.log(this.state);
            return (
                <div>
                    <div>当前Store中的数据是：{this.props.tool.count}</div>
                    <button onClick={this.props.addCount}>点击+9</button>
                    <hr />
                    <div>当前Store中的数据是：{this.props.user.age}</div>
                    <button onClick={this.props.addAge}>点击+1</button>
                </div>
            );
        }
    }
    
    // 第二步：在类外面定义俩个映射方法
    // 将redux中的state数据源映射到本组件自身的props中
    function mapStateToProps(state) {
        // return state.user;
        // return state.tool;
        return state;
    }
    // 将dispatch映射成自身组件的props
    function mapDispatchToProps(dispatch) {
        // 该方法返回一个对象，对象中都是方法
        return {
            addCount() {
                dispatch(actionCreate.createAction(MOD_COUNT, 9));
            },
            addAge() {
                dispatch(actionCreate.createAction(MOD_AGE, 1));
            },
        };
    }
    
    // 第三步：应用HOC
    // connect函数的俩个参数顺序不能颠倒
    export default connect(mapStateToProps, mapDispatchToProps)(Counter);
    ~~~
  
  

## 6、redux-thunk（中间件）

通常情况下，action只是一个对象，不能包含异步操作，这导致了很多创建action的逻辑只能写在组件中，代码量较多也不便于复用，同时对该部分代码测试的时候也比较困难，**组件的业务逻辑也不清晰**，使用中间件了之后，可以通过actionCreator异步编写action，这样代码就会拆分到actionCreator中，可维护性大大提高，可以方便于测试、复用，同时actionCreator还集成了异步操作中不同的action派发机制，减少编码过程中的代码量。

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/01/f602da4624bbd2d6a94f60659d3425c6c718a225.png?sign=c2ad92a84b02dd359e3433318d1e6317&t=600e8805)

常见的异步库：

- **Redux-thunk**
- Redux-saga
- Redux-effects
- Redux-side-effects
- Redux-loop
- Redux-observable
- …

基于Promise的异步库：

- Redux-promise
- Redux-promises
- Redux-simple-promise
- Redux-promise-middleware
- …

这里我们使用一个Redux官方出品的中间件库：**redux-thunk**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/19a9bd4441389b0de7378dbc88bd09cd43df515d.png?sign=befc0a76a6388904b5ec3cfb90add605&t=5f99b565)



在使用前需要先安装这个中间件：

~~~shell
npm i -S redux-thunk
~~~



步骤：

- 在仓库的创建文件`store/index.js`文件中导入中间件的应用方法，再去导入redux-thunk，并且应用

  - 导入redux提供的中间件使用的方法：applyMiddleware

- 会产生报错（浏览器的redux调试工具的报错）需要解决

  - 解决思路：查看

    [插件的项目主页]: https://github.com/zalmoxisus/redux-devtools-extension#usage

    ，找解决办法

    ![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/01/6e50a16108fda29d18af71d2cc157474c33df8de.png?sign=2532a8ce08844760f0e65ef26eabe329&t=600e8b12)

    修改为的配置如下：

    ~~~js
    // 解决插件报错的操作
    const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
    
    const store = createStore(
        // 合并多个reducer（整合数据源）,不合并会报错
        combineReducers({ counter, global }),
        // 应用中间件
        composeEnhancers(applyMiddleware(thunk))
        // 必须要加上一段插件的配置工具，才能在浏览器中使用redux扩展
        // window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
    );
    ~~~

- 去需要做异步处理的action的位置去使用异步实现（通过dispatch派发action）

  - ~~~js
    // - 异步方法（载荷可能是异步获取的数据）
    export const createActionAsync = (type, payload) => {
        // 异步代码先不写（暂时没有异步中间件）
        // return { type, payload };
        // setTimeout(() => {
        //     return { type, payload };
        // }, 1000);
    
        // 异步写法
        return (dispatch) => {
            setTimeout(() => {
                dispatch({ type, payload });
            }, 3000);
        };
    };
    ~~~



## 7、面试题：redux优化

问题：redux是用于大规模数据管理的，一个项目中可能会有很多的数据，这就导致模块化后会产生若干个reducer需要在store/index.js中进行导入，也就会出现以下情况（举例）：

~~~js
import search from "./Reducers/Reducer1";
import search from "./Reducers/Reducer2";
import search from "./Reducers/Reducer3";
import search from "./Reducers/Reducer4";
import search from "./Reducers/Reducer5";
import search from "./Reducers/Reducer6";
import search from "./Reducers/Reducer7";
import search from "./Reducers/Reducer8";
import search from "./Reducers/Reducer9";
import search from "./Reducers/Reducer10";
// ...
~~~

如何对其进行优化？



解决思路：通过编写一个方法，实现指定文件夹的遍历，实现自动导入。

核心方法：require.context()

> 该方法接受3个参数：
>
> 参数1：目录
>
> 参数2：是否递归遍历，布尔值
>
> 参数3：正则表达式

代码实现：

==注意：写该优化代码的实现必须要写在所有的import语句之后==

~~~js
// 代码优化，批量导入
const files = require.context("./reducers", false, /\.js$/);
// 比较固定的处理代码
let members = {}; // 组合成员用的
files.keys().forEach((element) => {
    // element是对应的模块文件的路径
    console.log(element);
    // 依据路径获取导出的成员
    let member = files(element).default;
    // 获取文件名充当对象的属性名
    let filename = element.replace(/(.*\/)*([^.]+).*/gi, "$2");
    // 组合成员
    members[filename] = member;
});
~~~



作业：vue综合案例的时候不是有nodejs，请使用当时的nodejs代码，结合受控/非受控组件，实现react的登录页面（不用管样式），登录成功之后将token保存到redux中。



# 十三、路由

## 1、介绍

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/cea272f101176b948ac627959723ae3a93a0e4b9.png?sign=a44bce7e29d23549df61fd1be8bbc8ea&t=5f9abd07)



React Router官网：https://reactrouter.com/

使用用React Router前需要先进行安装：

~~~shell
npm i -S react-router-dom
~~~

React Router现在的主版本是5，于2019年3月21日搞笑的发布，[搞笑的官网链接](<https://reacttraining.com/blog/react-router-v5/>)， 本来是要发布4.4的版本的，结果成了5。从4开始，使用方式相对于之前版本的思想有所不同。之前版本的思想是传统的思想：**路由应该统一在一处渲染**， Router 4之后是这样的思想：**一切皆组件**。



## 2、路由的使用

### 2.1、相关组件

如前面介绍里说的，自Router 4之后的思想是`一切皆组件`，所以在正式开始学习React路由前需要先对几个组件要有所掌握：

- Router组件（别名，真实是不存在的，为了简写路由模式的组件名称）：**包裹整个应用**（单个具体的组件/**根组件**），一个React应用只需要使用一次
  - 注意：**在react中，不存在类似于vue的路由配置文件，对于前端路由模式的选择，我们可以通过该组件完成**
  - Router类型： HashRouter和BrowserRouter
    - HashRouter： 使用URL的哈希值实现 （localhost:3000/#/first）
    - BrowserRouter：使用H5的history API实现（localhost3000/first）
- **Link**/NavLink组件：用于指定导航链接（a标签）就是做声明式导航的（类似于vue中的`router-link组件`）
  - 最终Link会编译成a标签，而to属性会被编译成 a标签的href属性
- Route组件：指定路由展示组件相关信息（组件渲染）【**路由规则**】{path: xx,component:xxx}
  - path属性：路由规则，这里需要跟Link组件里面to属性的值一致
  - component属性：展示的组件
  - 语法：<Route path="路径" component={组件}></Route>
  - 该组件除了具备定义路由规则功能外，还有类似于vue中`router-view`的功能

**各个组件之间的关系**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/10/2bdaba50f309c0a59921c7e188597c744931f703.png?sign=6e90bed85333f028465d58ca7faae7c7&t=5f9abe65)

> 注意：`Link`和`Route`组件必须被`Router`组件给包裹，否则报错。



### 2.2、声明式导航

- 在`src/index.js`文件中定义一个路由模式（可选，也可以在具体的某个组件中使用Router）

~~~jsx
import React from "react";
import ReactDOM from "react-dom";

// 设置路由模式
import {HashRouter as Router} from 'react-router-dom'

// 定义 provider
import { Provider } from "react-redux";
import store from "./Store/index";

import App from "./App";

ReactDOM.render(
    <Provider store={store}>
        {/* 使用Router包裹根组件 */}
        <Router>
            <App></App>
        </Router>
    </Provider>,
    document.getElementById("root")
);
~~~

- 在根组件`src/App.js`中引入路由相关组件（根据自身需要选择路由模式），并使用

~~~jsx
import React, { Component } from "react";
import { HashRouter as Router, Route, Link } from "react-router-dom";

import Cmp10 from "./Components/Cmp10";
import Cmp11 from "./Components/Cmp11";

class App extends Component {
    render() {
        return (
            <Router>
                <div>
                    <h1>导航区域</h1>
                    <div>
                        <ul>
                            <li>
                                <Link to="/home">首页</Link>
                            </li>
                            <li>
                                <Link to="/news">新闻</Link>
                            </li>
                        </ul>
                    </div>
                </div>
                <Route path="/home" component={Cmp10}></Route>
                <Route path="/news" component={Cmp11}></Route>
            </Router>
        );
    }
}

export default App;
~~~

在写上述代码时注意，路由自带组件的顺序嵌套关系，组件`<Link></Link>`和组件`<Route></Route>`必须被组件`<Router></Router>`给包裹着。

> 需要注意：
>
> 刨除样式的影响，`Route`组件在HTML代码中的位置决定了渲染后其在页面中显示的位置。如果`Route`放在最后，则其显示的时候也在最后；若其放在渲染内容的最前面，相应的显示也会在最开始。



### 2.3、编程式导航

react-router-dom中通过history对象中的push/go等方法实现编程式导航功能，这一点与之前的vue路由还是很相似的。

形如：

~~~jsx
this.props.history.push({
  pathname: "/home",
  search: "from=404",	// 表示传递查询字符串
  state: {				// 隐式传参，地址栏不体现
    username: "admin",
  },
});

// 给定给定的数字（正数或负数）决定去往历史栈中的哪个地址，正数往未来，负数往过去
this.props.history.go(-1)
~~~

> 请勿在根组件中写编程式导航，因为根组件默认是没有props对象，解决办法见后续。



## 3、路由参数

路由参数：在Route定义渲染组件时给定动态绑定的参数。

React路由传参方式有三种：

- ==动态路由参数（param）==
  - 以“/film/detail/:id”形式传递的数据
  - 在目标页面路由中传递
  - 在落地组件中通过`this.props.match.params`得到
  - 一般用于restful规范下的开发
- 查询字符串（query）
  - 通过地址栏中的 `?key=value&key=value`传递
  - 在落地组件中通过`this.props.location.search`得到
  - 由于得到的数据是带“?”的，还需要进一步加工处理之后才能使用，因此建议少用或者不用
- **隐**式传参（state），通过地址栏是观察不到的
  - 不合适写在声明式导航中，写在编程式导航中更加合适
  - 一般数用于**埋点**数据
    - 简单的讲，埋点是将部分标记隐藏起来，等待用户去触发，因为这个事情不想让用户看到（需要做一些数据的收集，后续做分析），因此会使用隐式传参的方式
  - 在落地组件中通过`this.props.location.state`得到

接收示例：

~~~jsx
constructor(props){
    super(props)
    this.state = {
        // 接收动态路由参数
        news_id: this.props.match.params.id,
        // 接收查询字符串并处理
        query: querystring.parse(this.props.location.search.slice(1)),
        // 接收state
        state: this.props.location.state
    };
}
~~~



## 4、嵌套路由

在有一些功能中，往往请求地址的前缀是相同的，不同的只是后面一部份，此时就可以使用多级路由（路由嵌套）来实现此路由的定义实现。

例如，路由规则如下

````
/admin/index
/admin/user
/admin/goods
/admin/add
````

它们路由前缀的admin是相同的，不同的只是后面一部份。

**思想：**

- 借助react路由默认是**非严格匹配模式**的便利
  - 例如。上述路由都有`/admin`开头，那么我们可以在路由定义时定义一个组件的路由规则“/admin”。如果这样做，则上述4个路由都会匹配上这个路由规则。匹配的这个组件我们称之为父组件
  - 扩展：如果某个路由需要使用严格模式，请在这个路由上加上一个属性：exact
- 再在父组件中写嵌套的子路由的匹配规则

**实现方式**

- 先需要定义个组件，用于负责匹配同一前缀的路由，将匹配到的路由指向到具体的模块

~~~jsx
<Route path="/admin" component={Admin}></Route>
~~~

- 创建模块路由组件（父）负责指定各个子路由的去向

~~~jsx
render() {
    // 获取前缀，供后续地址做路由拼接
    let prefix = this.props.match.path;
    return (
        <div>
            <h1>欢迎使用后台管理程序</h1>
            <Route path={`${prefix}/user`} component={User}></Route>
            <Route path={`${prefix}/goods`} component={Goods}></Route>
        </div>
    );
}
~~~

- 创建父路由中的子路由需要的组件



## 5、重定向与404路由

### 5.1、重定向路由

React的重定向路由有以下写法：

> 在重定向的时候需要知道，从哪里来，到哪里去，因此该组件需要使用2个属性：
>
> - from：匹配需要重定向的路由
> - to：需要去往的路由

~~~react
import { Redirect } from "react-router-dom"

<Redirect from="/from" to="/to"></Redirect>
~~~



### 5.2、404路由

项目中少不了404页面的配置，在React里面配置404页面需要注意：

- 需要用到Switch组件，让其去包裹路由的`Route`组件（Switch组件保证只渲染其中一个子路由）

  ~~~jsx
  import NotFound from "./Components/404";
  
  <Route>
      <NotFound></NotFound>
  </Route>
  // 或
   <Route component={NotFound}></Route>
  ~~~

> 注意：在404路由的位置，不需要给定具体的路由匹配规则，不给`path`表示匹配`*`，即所有的路由都会匹配，因此用404路由一定要加`Switch`匹配一个路由。
>
> 注意：并不会因为当前是404路由/重定向路由而改变状态码，因为当前写的是前端的内容，状态码是后端提供的，只有等后期上线以后才能有状态码。

例如：

~~~jsx
<div>
    <Link to="/home">家</Link> &emsp;
    <Link to="/news">新闻</Link>&emsp;
    <Link to="/about">关于</Link>&emsp;
    <Redirect from="/" to="/home"></Redirect>
    <Switch>
        <Route path="/home" component={Cmp11}></Route>
        <Route path="/news" component={Cmp12}></Route>
        <Route path="/about" component={Cmp13}></Route>
        <Route component={NotFound}></Route>
    </Switch>
</div>
~~~



## 6、三种路由渲染方式

Route路由渲染组件是用于路由规则匹配成功后组件渲染容器，此组件提供了3种方式组件渲染方式：

- component属性（**组件对象**/函数）

  - ~~~jsx
    <Route path="/home" component={Home} />
    ~~~

  - ~~~jsx
    <Route path="/home" component={() => <Home />} />
    ~~~

- render属性（函数）

  - ~~~jsx
    <Route path="/home" render={() => <Home />} />
    ~~~

- children属性（组件/**函数**）

  - ~~~jsx
    <Route path="/about" children={() => {
    	if(props.match){
            return <div>children渲染</div>
        }
    }} />
    ~~~

  - ~~~jsx
    <Route path="/about" children={<About />} />
    ~~~

**注意点**

- component可以使用组件类渲染和函数方式渲染，render只能使用函数，children使用函数或直接使用组件
- 当children的值是一个函数时，无论当前地址和path路径匹不匹配，都将会执行children对应的函数，当children的值为一个组件时，当前地址和path不匹配时，路由组件不渲染
- 除组件对象方式外，其余方式会使得渲染的组件丢失props
- 函数式渲染方式支持传递传递参数，其有一个形参“props”
- children函数方式渲染，会在形参中接受到一个对象，对象中match属性如果当前地址匹配成功返回对象，否则null



## 7、withRouter高阶组件

**作用：把不是通过路由切换过来的组件中，将react-router 的 history、location、match 三个对象传入props对象上**

默认情况下，必须是经过路由匹配渲染的组件才存在this.props，才拥有路由参数，才能使用编程式导航的写法，才能执行this.props.history.push('/uri')跳转到对应路由的页面。然而不是所有组件都直接与路由相连的，当这些组件需要路由参数时，使用withRouter就可以给此组件传入路由参数，此时就可以使用this.props。

~~~jsx
// 引入withRouter
import { withRouter} from 'react-router-dom'

// 执行一下withRouter
export default withRouter(Cmp)
~~~

> 该高阶组件是路由包自带的东西，因此只需要引入+使用就可以了，不需要自己定义。



## 8、封装自定义组件

意义：

- **尝试去封装一个组件，开启封装的新世界**
- 希望在react中也能够使用类似于vue的自定义标签名的导航组件
- 综合练习今天的内容
- 解释children渲染方式函数式的好处

需求：请自行封装一个组件，组件名叫做`Nav`，要求实现可以自定义`tag`属性，来决定最终被渲染成的标签，最好还可以给个样式，比如说，选中的菜单给个红色字。

**封装的组件：Nav**

~~~jsx
// 自定义封装导航组件
// 使用children的函数式渲染方式来解决组件的渲染问题

import React, { Component } from "react";
// 导入需要用的组件
import { withRouter, Route } from "react-router-dom";
class Nav extends Component {
    render() {
        // 先获取菜单需要的数据（插槽）
        let text = this.props.children;
        // 声明需要用的标签类型，如果没传递则默认为a标签（坑）
        // 变量首字母需要大写
        let Tag = this.props.tag ? this.props.tag : "a";
        // 获取要去的地址
        let url = this.props.to;
        return (
            <div>
                <Route
                    exact
                    path={url}
                    children={(props) => {
                        // 匹配上的，match为对象；否则为null
                        let style = {};
                        if (props.match) {
                            // 激活
                            style = { color: "red" };
                        }
                        // console.log(props);
                        return (
                            // 样式绑定+事件处理
                            <Tag onClick={this.go.bind(this, url)} style={style}>
                                {text}
                            </Tag>
                        );
                    }}
                ></Route>
            </div>
        );
    }
    // 事件处理程序
    go(url) {
        // 编程式导航
        this.props.history.push(url);
    }
}

export default withRouter(Nav);

~~~

**封装组件的应用**

~~~jsx
// 封装后组件需要被应用，测试组件

import React, { Component } from "react";
import Nav from "./Nav";
class App extends Component {
    render() {
        return (
            <div>
                <ul>
                    <li>
                        <Nav to="/" tag="a">
                            首页
                        </Nav>
                    </li>
                    <li>
                        <Nav to="/news" tag="div">
                            新闻
                        </Nav>
                    </li>
                    <li>
                        <Nav to="/about" tag="h2">
                            关于
                        </Nav>
                    </li>
                </ul>
            </div>
        );
    }
}

export default App;
~~~



# 十四、三方组件

## 1、过渡动画组件

### 1.1、简介

在项目中可能会有一些动画效果展示或是页面切换效果，使用css动画的方式虽然可以实现，但比较局限，涉及到一些js动画的时候没法处理了。`react-transition-group`是react的第三方模块，借助这个模块可以更方便的实现更加复杂的动画效果。

地址：https://reactcommunity.org/react-transition-group/css-transition

该三方包的安装命令如下：

~~~shell
npm i -S react-transition-group
~~~



### 1.2、基本使用

步骤：

- 需要定义好css动画的样式
- 按照transition动画组件的语法去使用css样式

~~~css
/* app.css，来自于官网 */
.alert-enter {
    opacity: 0;
    transform: scale(0.9);
}
.alert-enter-active {
    opacity: 1;
    transform: translateX(0);
    transition: opacity 300ms, transform 300ms;
}
.alert-exit {
    opacity: 1;
}
.alert-exit-active {
    opacity: 0;
    transform: scale(0.9);
    transition: opacity 300ms, transform 300ms;
}

.alert-exit-done {
    display: none;
}
~~~

~~~jsx
import React, { Component, Fragment } from "react";
import { CSSTransition } from "react-transition-group";

import "./assets/app.css";

class App extends Component {
    state = {
        // 是否显示
        isShow: true,
    };
    handleToggole = () => {
        this.setState(() => {
            return {
                isShow: !this.state.isShow,
            };
        });
    };
    render() {
        return (
            <Fragment>
                <CSSTransition
                    {/* in：表示控制默认是否显示 */}
                    in={this.state.isShow}
                    {/* timeout：动画所持续的时间，单位：毫秒 */}
                    timeout={ 300 }
                    {/*  classNames：class样式类名 */}
                    classNames={{
                        enter: "alert-enter",
                        enterActive: "alert-enter-active",
                        exit: "alert-exit",
                        exitActive: "alert-exit-active",
                    }}
                    {/* unmountOnExit：在元素退出的时候删除对应的DOM */}
                    unmountOnExit
                >
                    <div>玩转React Transition</div>
                </CSSTransition>
                <button onClick={this.handleToggole}>
                    触发动画
                </button>
            </Fragment>
        );
    }
}

export default App;
~~~

在上述demo中，我们的动画样式来自于官网的案例，但是以后写项目的时候样式肯定不能用刚才的，那此时就需要我们自己写样式了，这是一个耗时而且费力的工作。为了便于高效开发，对于常见的动画效果前辈们已经给我们造好轮子了，我们只需要拿过来直接使用即可。因此推荐使用常见的一些动画库，比如：[animate.css](https://animate.style/)。

在`animate.css`官网找到`CDN`外链地址，打开后将`animate.min.css`文件保存到本地：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/3f924521014c9e455a286396ab10f976dddc4b52.png?sign=caf9b48531e9420a948f51cac724c3f0&t=5fa011fd)

随后将`animate.min.css`文件放到项目中，假定放置到`./src/assets/css/animate.min.css`，则随后在需要使用`animate`动画的组件中按照以下方式进行导入：

~~~jsx
import "./assets/css/animate.min.css";
~~~

导入好以后，根据`animate`官网的演示效果复制所需要的动画样式类名（==切勿使用浏览器自带的翻译功能翻译页面，否则会导致样式演示效果无法预览==），替换`react-transition-group`配置部分的对应`classNames`样式值名即可，例如：

~~~jsx
render() {
    return (
        <Fragment>
            <CSSTransition
                in={this.state.isShow}
                timeout={1000}
                classNames={{
                    enter: "animate__animated",
                    enterActive: "animate__fadeInDown",
                    exit: "animate__animated",
                    exitActive: "animate__fadeOutDown",
                }}
                unmountOnExit
                >
                <div>玩转React Transition</div>
            </CSSTransition>
            <button onClick={this.handleToggole}>触发动画</button>
        </Fragment>
    );
}
~~~



### 1.3、列表过渡动画

**核心代码**

> 此处单独靠`CSSTransition`是不能实现列表的动画的，**因为`CSSTransition`只针对单个元素进行动画效果**

~~~jsx
<TransitionGroup>
    <CSSTransition>
        <li>aaaa</li>
    </CSSTransition>
</TransitionGroup>
~~~

**案例目标**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/973166ca8710446781ddd9d05ae5850023ec6147.gif?sign=432aae3ddc51f15a75b01f90e685ffe3&t=5fa030cd)

~~~react
import React, { Component } from "react";
// 导入动画组件包中的内容
import { TransitionGroup, CSSTransition } from "react-transition-group";
import "./assets/css/animate.min.css";

// 实现步骤：
// 1. 按照两步去走（任务分解）
//      - 先实现没有动画的效果
//      - 实现上一步后再去考虑加上动画效果
// 2. 以当前案例为例，有有少个h5标签，最终其实就有多少个CSSTransition组件

class App extends Component {
    state = {
        username: ["张三"],
    };

    render() {
        return (
            <div>
                <div>
                    <button onClick={this.handler.bind(this)}>添加</button>
                    <hr />
                    {/* 展示列表的数据 */}
                    <TransitionGroup>
                        {this.state.username.map((item, index) => {
                            return (
                                <CSSTransition
                                    timeout={1000}
                                    classNames={{
                                        enter: "animate__animated",
                                        enterActive:
                                            "animate__animated animate__fadeInUp"
                                    }}
                                    key={index}
                                >
                                    <h5 key={index}>{item}</h5>
                                </CSSTransition>
                            );
                        })}
                    </TransitionGroup>
                </div>
            </div>
        );
    }

    handler() {
        this.setState((state) => {
            // 产生新的数据（不重复的名字）
            let tmp = "李四" + Math.random();
            return { username: [...state.username, tmp] };
        });
    }
}

export default App;
~~~



### 1.4、路由过渡动画

路由过渡动画，即路由切换时为其添加动画效果。

其实现思路与列表的过渡动画类似。

**案例效果**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/1142508302ec6516107a3e67dd45bc439a085e37.gif?sign=1b4ceedc05b3583d5d4055b2c2747a30&t=5fa0463a)

~~~react
import React, { Component } from "react";

// 思想步骤：先去实现不带动画效果的，再实现动画效果
// 这里有一个坑：

import { Route, Link, Switch, withRouter } from "react-router-dom";

import About from "./Components/About";
import News from "./Components/News";
// 导入动画组件
import { TransitionGroup,CSSTransition } from "react-transition-group";

import "./assets/css/animate.min.css"

class App extends Component {
    render() {
        return (
            <div>
                <ul>
                    <li>
                        <Link to="/about">关于</Link>
                    </li>
                    <li>
                        <Link to="/news">新闻</Link>
                    </li>
                </ul>

                <TransitionGroup>
                    {/* 因为switch只会匹配一个路由，因此可以直接使用CSSTransition包裹Switch */}
                    <CSSTransition
                        timeout={1000}
                        classNames={{
                            enter: "animate__animated",
                            enterActive:
                                "animate__animated animate__fadeInDown",
                            exit: "animate__animated",
                            exitActive: "animate__animated animate__fadeOutUp",
                        }}
                        // 加key让CSSTransition知道自己的内容发生了变化，要求key值不重复
                        // 此处的key并不是为了提供效率，而是为了让框架强制重新渲染CSSTransition
                        key={this.props.location.pathname}
                    >
                        <Switch>
                            <Route path="/about" component={About}></Route>
                            <Route path="/news" component={News}></Route>
                        </Switch>
                    </CSSTransition>
                </TransitionGroup>
            </div>
        );
    }
}

export default withRouter(App);
~~~



## 2、css-in-js技术

### 2.1、简介

CSS-in-JS是一种技术，而不是一个具体的库实现。简单来说CSS-in-JS就是将应用的CSS样式写在JavaScript文件里面，而不是独立为一些css，scss或less之类的文件，这样你就可以在CSS中使用一些属于JS的诸如模块声明，变量定义，函数调用和条件判断等语言特性来提供灵活的可扩展的样式定义。CSS-in-JS在React社区的热度是最高的，这是因为React本身不会管用户怎么去为组件定义样式的问题，而Vue有属于框架自己的一套定义样式的方案。

- 在js文件中写css就是css-in-js技术
- 好处：
  - 支持一些js的特性
    - 继承
    - 变量
    - 函数
  - 支持框架的特性
    - 传值特性

`styled-components` 应该是CSS-in-JS最热门的一个库，通过`styled-components`，你可以使用ES6的标签模板字符串语法，为需要styled的Component定义一系列CSS属性，当该组件的JS代码被解析执行的时候，styled-components会动态生成一个CSS选择器（比较随意的），并把对应的CSS样式通过style标签的形式插入到head标签里面。动态生成的CSS选择器会有一小段哈希值来保证**全局唯一性**来避免样式发生冲突。

- 通过ES6里面的模版字符串形式写css样式（遵循之前css样式代码的写法）
- 每个样式选择器都会在编译之后自动被添加上一个hash值（全局唯一）

使用`styled-components`前需要安装，安装的命令如下：

~~~shell
npm i -S styled-components
~~~

由于css后期会在模版字符串中编写，默认情况下vscode是没有css样式代码片段的（写样式的时候是没有代码提示的），为了提高css代码在模版字符串中编写的效率，此处强烈建议安装一个vscode的扩展：vscode-styled-components



### 2.2、定义样式与使用

**定义**

~~~javascript
import styled from "styled-components";
const Title = styled.div`
    font-size: 110px;
    color: pink;
    font-family: 华文行楷;
    background-color: black;
`;
export { Title };
~~~

**使用**

在使用的时候成员会被当作组件去使用（首字母大写）

~~~jsx
import React, { Component, Fragment } from "react";
// 就像使用常规 React 组件一样使用 Title
import { Title } from "./assets/style/style";

class App extends Component {
    render() {
        return (
            <Fragment>
                <Title>桃之夭夭，灼灼其华。</Title>
            </Fragment>
        );
    }
}

export default App;
~~~

**效果**

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/0d46a4677c99b01d7caaecc08ec7349d21962da9.png?sign=d576889f41bac254c738e300e5de8c23&t=5fa04de3)



### 2.3、样式继承

在`styled-components`中也可以使用样式的继承，其继承思想与`react`的组件继承相似：

- 继承父的样式：父有，子没有的样式，一旦继承则子也有了
- 重载父的样式：父有，子也有的样式，一旦继承则子覆盖父的样式

**样式继承**

~~~javascript
import styled from "styled-components";

const Button = styled.button`
    font-size: 20px;
    border: 1px solid red;
    border-radius: 3px;
`;
// 一个继承 Button 的新组件, 重载了一部分样式
// 继承会合并与父的样式，但是如果遇到样式冲突（相同），会以自己的为准
const Button2 = styled(Button)`
    color: blue;
    border-color: yellow;
`;

export { Button, Button2 };
~~~

**使用**

~~~jsx
import React, { Component, Fragment } from "react";

import { Button, Button2 } from "./assets/style/style";

class App extends Component {
    render() {
        return (
            <Fragment>
                <Button>我是第1个按钮</Button>
                <Button2>我是第2个按钮</Button2>
            </Fragment>
        );
    }
}

export default App;
~~~



### 2.4、属性传递

属性传递：样式值的动态传参（组件传值）

基于`css-in-js`的特性，在`styled-components`中也允许我们使用`props`（父传子），这样一来，我们可以对部分需要的样式进行传参，很方便的动态控制样式的改变。

**属性传递（JS中接收）**

~~~javascript
import styled from "styled-components";

// 参数传递
const Input = styled.input`
    color: ${(props) => props.inputColor || "red"};
`;

export { Input };
~~~

**动态传递参数**

~~~jsx
import React, { Component, Fragment } from "react";

import { Input } from "./assets/style/style";

class App extends Component {
    render() {
        return (
            <Fragment>
                <Input defaultValue="are you ok？" inputColor="blue"></Input>
            </Fragment>
        );
    }
}

export default App;
~~~



## 3、immutable.js

学习这个知识点并不是为了简化代码的写法，用它的原因是为了解决问题：解决项目当中状态大规模管理的深拷贝的问题（防止对象/数组因为引用传递而在使用过程中出现的各种问题）。

### 3.1、JS中数据修改问题

我们先来看一段熟悉的代码：

~~~jsx
import React, { Component } from "react";

class App extends Component {
    state = {
        str: "千锋教育",
        obj: {
            y: 1,
        },
        arr: [1, 2, 3],
    };
    componentDidMount() {
        const newState = this.state;
        console.log(newState === this.state);
    }
    render() {
        return <div></div>;
    }
}

export default App;
~~~

由于js的**对象和数组都是引用类型**。所以newState的state实际上是指向于同一块内存地址的, 所以结果是newState和state是相等的。

此时，我们尝试修改一下数据：

~~~jsx
componentDidMount() {
    const newState = this.state;
    newState.str = "千锋教育H5学院";
    console.log(newState,this.state);
}
~~~

可以看到，newState的修改也会引起state的修改（对象是引用传递）。要解决这个问题，js中提供了另一种修改数据的方式，要修改一个数据之前先制作一份数据的拷贝，像这样：

~~~jsx
componentDidMount() {
    const newState = Object.assign({}, this.state);
    newState.str = "千锋教育H5学院";
    console.log(newState,this.state);
}
~~~

我们可以使用很多方式在js中复制数据，比如`…`,  `Object.assign`, `Object.freeze`, `slice`, `concat`, `map`, `filter`,  `reduce`等方式进行复制，但这些都是浅拷贝，就是只拷贝第一层数据，更深层的数据还是同一个引用，比如：

~~~jsx
componentDidMount() {
    const newState = Object.assign({}, this.state);
    newState.str = "千锋教育H5学院";
    newState.obj.y = 2;
    newState.arr.push(4);
    console.log(newState,this.state);
}
~~~

可以看到，当在更改`newState`更深层次的数据的时候，还是会影响到`state`的值。如果要深层复制，就得一层一层的做**递归拷贝**，这是一个复杂的问题。虽然有些第三方的库已经帮我们做好了，但是深层复制是非常消耗性能的。那么这个问题如何解决呢？这就需要用到`immutable.js`了。



### 3.2、介绍

项目地址：https://immutable-js.github.io/immutable-js/

immutable.js出自Facebook，是最流行的**不可变数据**结构的实现之一。

不可变数据 (Immutable Data)就是一旦创建，就不能再被更改的数据。**对Immutable对象的任何修改或添加删除操作都会返回一个新的Immutable对象**。Immutable实现的原理是持久化数据结构（Persistent Data Structure），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免深层拷贝把所有节点都复制一遍带来的s性能损耗，Immutable使用了 **结构共享**（Structural Sharing），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享（不变）。

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/c806baf0dfe2be8bec988a411b2a6bad68c5685b.png?sign=643b01d5ca7626c46d98cea307809ff6&t=5fa1828b)

如果上面这张图不能直观的表现出变化，我们可以看下面这张图：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/b3d75c9b8adeafc3dd5e11383a545c21cfc18a2c.gif?sign=537807d5a25d0fc0e7800003a9114e1b&t=5fa2321b)



**使用immutable.js的优缺点**

- 优点
  - 降低mutable带来的复杂度
  - 节省内存
  - 历史追溯性（版本控制的感觉）。每时每刻的值都被保留了，想回退到哪一步只要简单的将数据取出就行。
  - **拥抱函数式编程**。immutable本来就是函数式编程的概念，纯函数式编程的特点就是，只要输入一致，输出必然一致，相比于面向对象，这样开发组件和调试更方便。
- 缺点
  - 需要重新学习api（我们需要去查阅文档，增加了学习成本）
  - 资源包大小增加（源码大约5000行左右）
  - 容易与原生对象混淆：由于api与原生不同，混用的话容易出错

与其他包一样，`immutable.js`包默认没有被安装到react中，因此学习使用前需要先进行安装到项目：

~~~shell
npm i -S immutable
~~~



### 3.3、常用API

immutable.js提供了许多永久不可变数据结构，包括： `List`，`Stack`，`Map`，`OrderedMap`，`Set`，`OrderedSet`和`Record`。为了满足工作开发需求，至少需要掌握以下API（API信息全部来自官网，除了以下体现到的API外，更多API请访问官网）：在使用的时候我们只是将其格式在两种之间来回转化，转化之后其就可以帮助我们解决深层拷贝的问题。（序列化与反序列化的感觉）

注意：当前还没有将immutable整合到框架，因此以下api测试的代码不要写在组件中。此处可以另起js文件，让这个js文件被入口文件包含即可。

#### 3.3.1、object转Map对象

~~~jsx
import { Map, is } from "immutable";

const state = {
    id: 1,
    name: "张三",
    age: 22,
    mobile: { public: "1300000000", private: "13333333333" },
};
const map1 = Map(state);
const map2 = Map(state);

// 比较上的差异
console.log(map1 === map2);
console.log(map1.equals(map2));
console.log(is(map1, map2));

// 获取上的差异
console.log(state.id);
console.log(state.mobile.private);
console.log(map1.get("id"));
console.log(map1.getIn(["mobile","private"]));

// 修改上的差异
state.name = "王二麻";
console.log(state);
const newMap1 = map1.set("name", "王二麻");
console.log(newMap1.get("name"));
console.log(newMap1.setIn(["mobile", "private"], "13800138000").getIn(["mobile", "private"]));
// 重点Update
console.log(newMap1.update("age", (val) => val + 1).get("age"));
console.log(newMap1.updateIn(["mobile", "private"], () => "13888888888").getIn(["mobile", "private"]));
~~~



#### 3.3.2、array转List对象

~~~jsx
import { List } from "immutable";

const state = ["灞波儿奔", "奔波儿灞"];
let list = List(state);

// 获取
console.log(list.get(0));

// 合并
list = list.concat(["黑鱼精", "鲇鱼怪"]);
console.log(list.get(3));

// 追加
list = list.push("乱石山碧波潭");
console.log(list.get(4));

// 把List对象转成js数组
console.log(list.toArray());
~~~



#### 3.3.3、JS转immutable（Map）

~~~jsx
import { Map, is, fromJS } from "immutable";

const state = {
	id: 1,
	name: "张三",
	age: 22,
	mobile: { public: "1300000000", private: "13333333333" },
};

// 转immutable对象（重点）
const immutable = fromJS(state);
console.log(immutable);
// 获取
console.log(immutable.get("name"));
console.log(immutable.getIn(["mobile","private"]));
~~~



#### 3.3.4、immutable转JS

~~~jsx
import { Map, is, fromJS } from "immutable";

const state = {
	id: 1,
	name: "张三",
	age: 22,
	mobiles: { public: "1300000000", private: "13333333333" },
};

// 转immutable对象（重点）
const immutable = fromJS(state);
console.log(immutable.toJS());
~~~

注意：toJS方法不需要导入（本身就在map对象的原型上），导入了实际也不会被使用。



### 3.4、Redux中集成

[redux官网](<https://redux.js.org/recipes/using-immutablejs-with-redux>)推荐使用[redux-immutable](<https://www.npmjs.com/package/redux-immutable>)进行redux和immutable的集成。`redux-immutable`包的安装命令如下：

~~~shell
npm i -S redux-immutable
~~~

以登录案例为例，步骤如下：

- （固定）将redux仓库创建入口文件中合并reducer的方法改成redux-immutable提供的合并方法（新方法与之前的是同名的combineReducers）

~~~js
// 模块化操作

// combineReducers方法是redux中内置的，作用是合并多个reducer和数据源，参数是一个对象，对象中属性值是单个reducer，而其属性名有点类似于vuex模块化时的命名空间的概念
import { createStore, applyMiddleware, compose } from "redux";
// 导入redux-thunk
import thunk from "redux-thunk";
import counter from "./Reducers/counter";
import global from "./Reducers/global";
// 导入redux-immutable提供的合并reducer的方法
import { combineReducers } from 'redux-immutable'

// 解决插件报错的操作
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(
    // 合并多个reducer（整合数据源）,不合并会报错
    combineReducers({ counter, global }),
    // 应用中间件
    composeEnhancers(applyMiddleware(thunk))
    // 必须要加上一段插件的配置工具，才能在浏览器中使用redux扩展
    // window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

export default store;
~~~

- 将数据源设置成immutable对象

~~~js
// 功能模块公用的数据源
// 注意，api都是immutable中提供的，不要导入错了包
import { fromJS } from "immutable";
const state = fromJS({
    _token: "",
});

export default state;
~~~

- 在组件中使用的时候将其转成js对象

~~~js
// 将数据源的数据映射成当前组件自身的props属性
function mapStateToProps(state) {
    // console.log(state);
    // 返回props对象
    // 这里可以获取整个仓库的数据：state
    // 也可以只要特定模块的数据，例如：state.global
    return state.toJS().global;
}
~~~

如果说这里没有使用react-redux，而是使用的之前的订阅的方式，则需要在订阅那个位置做数据格式的转化。

- 在修改的时候使用immutable的api实现修改

~~~js
// jwt的reducer
import defaultState from "../States/global";

function reducer(state = defaultState, actions) {
    // 判断是否是设置token的操作
    if (actions.type === "set_token") {
        // return { ...state, _token: actions.payload };
        console.log(actions);
        // 修改之后返回新值，可以直接ruturn或者用变量接收再返回
        return state.update("_token", () => actions.payload);
    }
    // 在返回之前写修改数据源的操作
    return state;
}

export default reducer;
~~~

