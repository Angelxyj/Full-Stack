## 一.props进阶

### 1.children属性

在vue中对应的是插槽Slot（匿名插槽）。

`children`属性表示组件标签的子节点，**当组件标签有子节点时**，接受传值的子组件中的props里就会有该属性，与普通的props一样，其值可以使任意类型 字符串/数组/对象...。

例如，在父组件中有如下代码：

~~~
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

~~~
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

### 2.props-type

React是为了构建大型应用程序而生，在一个大型应用开发过程中会进行多人协作，往往可能根本不知道别人使用你写的组件的时候会传入什么样的参数，这样就有可能会造成应用程序运行不了但是又不报错的情况。所以必须要对于`props`设传入的数据类型进行校验。

为了解决这个问题，React提供了一种机制，让写组件的人可以给组件的`props`设定参数检查，需要安装和使用[prop-types](<https://www.npmjs.com/package/prop-types>)：

现在的react的版本已经直接将其作为依赖，在node_modules中已经有了，因此可以免于安装

npm i -S prop-types

在使用时，无论是函数组件还是类组件，都需要对`prop-types`进行导入：

~~~
import Pts from "prop-types"
~~~

随后依据使用的组件类型选择对应的应用方式，如果是函数组件则按照下面的方式应用：

~~~
function App(props){
    // 函数组件声明过程
    return "";
}
// 为App方法组件挂上验证规则
App.propTypes = {
    // 待验证的属性名：PropTypes.类型规则[.isRequired] 
    // isRequired不可以单独使用,否则报错
    propname:Pts.string,
    // ... 
}
~~~

如果是类组件则按照下面的方式应用：

本质上来上面类组件的写法与方法组件的写法是一样的，只不过考虑到类组件的代码完整性，我们把规则的验证做成了类的静态成员属性的方式，如果还原成最初的代码则如下：

~~~
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

需要注意，`isRequired`规则必须放在最后且不能独立于其他规则存在。更多的验证规则，可以参考[React官网](https://reactjs.org/docs/typechecking-with-proptypes.html)

### 3.默认值(defaultProps)

如果`props`有属性没有传来数据，为了不让程序异常，我们可以依据业务情况给对应的属性设置默认值。

依据组件类型的不同选择对应的操作方式，如果是函数组件则采用如下方式：

~~~
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

~~~
class App extends Component {
    // 添加静态成员属性`defaultProps`设置props的默认值
    static defaultProps = {
        // 属性名：默认值
        title: "标题"
        // ...
    }
}

// 方式2 将静态属性定义在类外
App.defaultProps ={
    title: '标题'
}
~~~

### 4.render Props

术语“render prop”是指一种在[react](https://so.csdn.net/so/search?q=react&spm=1001.2101.3001.7020)组件之间使用一个值为函数的prop共享代码的简单技术。

具有 render prop 的组件接受一个函数，该函数返回一个 React 元素并调用它而不是实现自己的渲染逻辑。

ShowHello组件

~~~
import React from 'react';
 
class ShowHello extends React.Component {
  constructor(props) {
	super(props);
    this.state = {
      greet:"Hello World"
    }
  }
  render(){
    return (
      <div>
        {/* <Page greet={this.state.greet} /> */}   //将prop方式换成render prop的方式
        {this.props.render(this.state.greet)}    //不同的组件共享同一个状态，且不需要创建            
                                                 //一个新的ShowHello组件
      </div>
    )
  }
}
 
export default ShowHello

~~~

Home组件

~~~
import React from 'react';
import ShowHello from './ShowHello';
import Page from './Page';
 
class Home extends React.Component {
  render(){
    return (
      <div>
        <ShowHello render={greet => (   //通过render prop的方式决定哪个组件渲染使用此状态
          <Page greet={greet}/>
        )}/>
      </div>
    )
  }
}
 
export default Home

~~~



## 二.函数组件

### 1. 组件的创建方式

- 函数组件（无状态组件,类似于vue中的data,即没有数据）：使用JS的函数创建组件
- 函数名称以大写字母开头（建议）
- 函数组件**必须有返回值**，表示该组件的结构（虚拟DOM），且内容必须有顶级元素
- 函数组件是没有生命周期的

例如，新建组件文件`src/App.jsx`：

> 约定：组件后缀可以是`.js`也可以是`.jsx`，为了方便区分组件与项目的业务代码，建议组件用`.jsx`，业务代码后缀用`.js`。

~~~
// 使用快捷键 rfc 即可创建函数组件模板
// 17.0后的版本react的导入可有可无,不使用React变量
// 函数名首字母必须大写
// 函数组件必须有返回值,返回值为一个jsx语法的dom模板结构
// 函数组件无状态,无生命周期,
// 函数组件有hooks
// 组件根元素还可以使用 <></> 或<React.Fragment></React.Fragment> 但是这种写法一般很少用
import React from 'react' 
function Hello() {
    return (
        <div>这是第一个函数组件</div>
    )
}
export default Hello
~~~

要想输出效果，可以再创建项目入口文件`src/index.js`：

~~~
import React from "react";
import ReactDOM from 'react-dom/client';
import App from "./你创建的组件路径";
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  // React.StrictMode 在开发时,使用严格模式,对于打包后的生产环境代码没有任何影响,主要用来在控制台做一些提示和警告的
  // 例如 react 中有一些语法已经废弃,没有再维护,并没有删除,就会出一些提示,建议可以不用设置严格模式,因为当你使用第三方包的时候,
  // 第三方包中可能使用了废弃的语法,这时候控制台就会出现提示和警告.
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
~~~

箭头函数写法:

~~~
import React from 'react'
const Hello = () => {
    return (
        <div>这是第一个函数组件</div>
    )
}
export default Hello
~~~

### 2.Hooks（重点）

#### 2.1简介

React中组件有函数组件与类组件，在 React Hooks 出现之前，我们可以使用函数和类组件来进行项目开发，但是如果组件中需要进行状态管理，函数组件就显得无能为力。React在**v16.8** 的版本中推出了 React Hooks 新特性，Hook是**一套工具函数的集合**,它增强了函数组件的功能，**hook不等于函数组件，所有的hook函数都是以use开头。**【以use开头的**特殊**的函数集合，就称之为Hooks】

Hook的定义：是react函数组件中使用的一些以“use”开头的特殊函数。

使用 React Hooks 相比于从前的类组件有以下几点好处：

- **代码可读性更强**，原本同一块功能的代码逻辑被拆分在了不同的生命周期函数中，容易使开发者不利于维护和迭代，通过 React Hooks 可以将功能代码聚合，方便阅读维护
- **组件树层级变浅，代码的跳跃性会变小**，在原本的代码中，我们经常使用 HOC/render/Props 等方式来复用组件的状态，增强功能等，无疑增加了组件树层数及渲染，而在 React Hooks 中，这些功能都可以通过强大的自定义的 Hooks 来实现
- **hook使用比使用类组件简单许多**（仁者见仁智者见智，但是Redux使用时 ，绝对是函数组件简单）

#### 2.2.hook的使用限制

- hook**只能**用在函数组件中或其他hook函数中使用, 类组件不能使用.
- **普通（非组件导出函数，且不以use开头的函数）**函数不能使用hook（hook不能在组件函数外去使用）
- **hook不能被有条件的调用**，因此不能放在if/for中（如果真有有条件调用的需求，请把条件写在hook函数内）

#### 2.3.常用的hook函数

官网文档：https://reactjs.org/docs/hooks-reference.html

hook的使用步骤：

- 自定义hook（自己用自己写的）
- 导入hook成员
- 使用hook成员

##### 2.3.1useState

作用：保存组件的状态（用于解决函数组件无状态问题）

语法：

```js
import React, { useState } from 'react';
const [state, setstate] = useState(initialState);
```

案例：

~~~
/**
 * hook：useState
 * 作用：实现函数组件中的所谓的“状态”
 * 提供方：react
 * 语法：const [访问变量名,设置数据方法名] = useState(初始默认值)
 * 参数含义：
 *      访问变量名：可以通过该变量名获取state的值
 *      设置数据方法名：只能通过调用该方法修改state的值，其只接受一个参数，参数要么是直接表示其值，要么以函数返回形式表示其值(必须有返回值)。如果前面的变量名叫a，那么此处的函数名一般叫做setA
 *      初始默认值：给state的初始值，支持字面量，也支持数组和对象等复杂数据类型
 * 注意点：
 *      a. 在修改字面量时，直接将最终的字面量的值给到修改函数即可
 *      b. 在修改对象或数组的时候，如果只是修改其中的一部分数据，记得最后返回的数据需要携带没有被修改的数据，如果不携带则丢失
 */

import React, { useState } from 'react';

const Index = () => {

    // 基础的字面量
    const [count, setCount] = useState(0)
    // 对象类型
    const [user, setUser] = useState({ uname: "张三", age: 22 })

    // 修改用户的信息的事件处理程序
    const modUser = () => {
         //1.直接赋值 setUser({ ...user, age: 33 })
         //2. 函数返回的形式 
         setUser((m) => {
             // m为当前要修改的数据 user { uname: "张三", age: 22 }
            return {
                ...user, 
                age: user.age + 5
            }
        })
    }
    
    // 修改数组信息 注意: 再修改对象或数组时,如果只是修改其中一部分数据,记得要将剩余数据返回,否则元数据丢失
    const [usernameArr, setUsernameArr] = useState(['刘','关','张']);
    const editArr=()=>{
        usernameArr.push('赵云')
        console.log(usernameArr);
        // setUsernameArr(usernameArr)// 该种方式修改不了,useState会认为是同一个数组,不能修改
        // 第一种方式:
        setUsernameArr([...usernameArr])
        // 第二种方式
        // setUsernameArr(()=>{
        //     return  [...usernameArr,'赵云']  
        // })
    }
    return (
        <div>
            <div>计数器的值是：{count}</div>
            {/*点击事件直接写函数表达式 一步到位*/}
            <div><button onClick={() => setCount(count + 1)}>+1</button></div>
            <hr />
            <div>
                <div>用户信息是：</div>
                <div>
                    用户名是：{user.uname}
                </div>
                <div>
                    年龄是：{user.age}
                </div>
                <button onClick={() => modUser()}>修改用户年龄</button>
            </div>
            <hr/>
            <p>{usernameArr.map((item,index)=>{
                return <span key={index}>{item}</span>
            })}</p>
            <p><button onClick={editArr}>修改数组</button></p>
        </div>
    );
}

export default Index;
~~~

##### 2.3.2useEffect

作用：模拟类组件中的生命周期的

函数组件对于在一些生命周期中操作还是无能为力，所以 React提供了 useEffect 来帮助开发者处理函数组件，来帮助模拟完成一部分开发中**非常常用的生命周期方法（并不是全部的生命周期）**。常被称为：**副作用处理函数**。此函数的操作是异步的。

useEffect 相当类组件中的3个生命周期 （但含义上不完全等同）

- componentDidMount
- componentDidUpdate
- componetWillUnMount

语法：

~~~
 useEffect(() => {
     effect... 副作用的操作,（类似于组件挂载完毕后、更新完毕后的操作）
     return () => {
         cleanup... 清理副作用
         （类似于组件的解除挂载的周期，可选）
     }
 },[INPUT,....])
~~~

案例

~~~
/**
 * hook：useEffect
 * 作用：用于操作作用及副作用代码的（用于控制函数组件生命周期的）
 * 语法：
 *      useEffect(callback[,array]) 数组array可选
 * 提供方：react
 * 细致语法：
 *      a. 在首次渲染及后续每次更新之后会执行callback的代码（模拟首次componentDidMount及componentDidUpdate）
 *          useEffect(没有返回值的callback)
 *      b. 被返回的函数是用于清理副作用的，会在执行下一次副作用前执行
 *          useEffect(带有返回值的cakkbacl)     //返回值必须是一个函数
 *      c. 有第二个参数，并且第二参数为空数组（仅仅会让副作用代码在首次挂载完毕后执行一次）
 *          useEffect(callback,[])
 *      d. 有第二个参数，且第二个参数中有具体的元素，元素个数不限，元素为useState的访问变量名。关注指定元素的更新，当被指定的元素更新后，将会再去执行副作用代码，及清理副作用的代码（如有）。
 *          useEffect(callback,[el1,el2,....])
 * 
 * 清理副作用很有意义：
 * 假设给页面绑定了点击事件，如果不清理事件，可能会导致事件越绑越多
 */
import React, { useEffect, useState } from 'react';

const Index = () => {
    const [count, setCount] = useState(0);
    const [position, setPosition] = useState({ x: 0, y: 0 })

    // 情形1: 没有返回值的callback
    // useEffect(() => {
    //     console.log(1)  //副作用代码 1.页面初始化执行,2.数据更新后执行 
    // })
    // 情形2: 带有返回值得callback函数
    // useEffect(() => {
    //     console.log(1)  //副作用代码  1.页面初始化执行;2.数据更新后执行
    //     return () => {
    //         console.log(2)  //清除副作用代码 1.下一次数据更新前执行
    //     }
    // })
    // 情形3: 第2个参数为空数组
    // useEffect(() => {
    //     console.log(1)  //副作用代码  1.页面初始化执行
    //     return () => {
    //         console.log(2)  //清除副作用代码 1.该函数不执行,return 没有意义了,相当于不写return
    //     }
    // }, [])
    // 情形4:第二个参数有数据
    // useEffect(() => {
    //     console.log(1)  //副作用代码  1.页面初始化执行,2.数据更新后执行
    //     return () => {
    //         console.log(2)  //清除副作用代码 1.下一次数据更新前执行
    //     }
    // }, [count]) // 此处第二个参数为被监听数据,可以写多个数据

    // 使用demo案例:
    const handler = (e) => {
        console.log('页面点击了');
        setPosition({ x: e.pageX, y: e.pageY })
    }
    useEffect(() => {
        // 给页面绑定点击事件
        document.addEventListener('click', handler)
        return () => {
            document.removeEventListener('click', handler) //清除副作用代码 1.下一次事件绑定前																清除,否则事件绑定对此叠加
        }
    })
    return (
        <div>
            <p>count:{count}</p>
            <p><button onClick={() => setCount(count + 1)}>+1</button></p>
            <hr></hr>
            <p>页面点击位置的坐标:x:{position.x},y:{position.y}</p>
        </div>
    );
}

export default Index;
~~~

##### 2.3.3useRef 

作用：用来生成对 DOM 对象的引用（类似于类组件中的createRef方法）

获取元素节点或者 子组件上的数据和方法

效果: 与类组件createRef相比实现相同的功能,函数组件使用useRef 相对复杂麻烦.

案例：

- 父组件Index.jsx

  ~~~
  /**
   * useRef的注意点：
   * 1. 使用肯定得导入useRef
   * 2. 通过执行该useRef函数获取ref对象
   * 3. 将ref对象挂载到需要获取对象的标签上面，例如：ref={ref}
   * 4. 后续我们可以通过ref对象获取对应的标签的对象
   * 5. 特别需要注意：默认情况下，普通的html标签是可以直接使用ref对象的（获取到的是dom对象），而函数组件标签默认不能使用ref对象！
   * 6. 如果需要解决函数组件标签使用ref就报错的问题，需要借助React.forwardRef()【让子具备转发ref的能力，子去给父ref】，该方法不是hook函数，而是hoc强化函数；还需要借助一个hook函数：useImperativeHandle来告诉父可以从子这里获取哪些数据或者方法（向外暴露成员）。
   * 7. useImperativeHandle函数的语法：useImperativeHandle(ref对象,callback)，callback必须返回一个对象，对象即为向外暴露的成员。其中ref对象为函数组件函数的第二形参
   * 
   */
  
  import React, { useRef } from "react"
  import Child from "./Child"
  
  const Index = () => {
      // 通过useRef获取ref对象
      const ref1 = useRef()
      const ref2 = useRef()
  
      // 事件的处理程序
      const showRef = () => {
          console.log(ref1)
          console.log(ref2)
      }
      return (
          <div>
              {/* 默认情况下，普通的html标签是可以直接使用ref对象的 */}
              <div ref={ref1}>这是父组件</div>
              {/*如果给函数组件直接使用ref属性会报错,解决方法给该子组件使用forwardRef强化函数*/}
              <Child ref={ref2} />
  
              <div>
                  <button onClick={showRef}>打印</button>
              </div>
          </div>
      )
  }
  
  export default Index
  ~~~

  子组件Child.jsx

       ~~~
       //接下来使用 useImperativeHandle这个 hooks函数,将子组件的数据和方法暴露给父组件
       // 语法: useImperativeHandle(ref,()=>{return{要暴露给父组件的数据和方法}});第二个函数必须有返回值
       import React, { useState, forwardRef, useImperativeHandle } from "react"
       
       const Child = (props, ref) => {
           const [state, setState] = useState(66666)
       
           const showMsg = () => {
               console.log(state)
           }
           // 向外暴露成员方法、变量,注意:第一个参数ref为该函数组件的第二个形参 
           useImperativeHandle(ref, () => ({ showMsg }))
       
           return (
               <div>
                   <div>这是子组件</div>
               </div>
           )
       }
       
       // forwardRef:为强化函数,使该函数组件具备转发ref的能力,解决该函数组件添加ref属性报错的问题.
       export default forwardRef(Child)
       ~~~

面试题：

- useRef能否用createRef替换？【可以】
- useRef与之前学习的createRef有何区别？【useRef在多次渲染页面的时候始终是同一个ref对象的引用；而createRef在每次更新后都会产生新的ref引用】

##### 2.3.4useContext 

- 作用: 实现跨级组件之间的传参

- 与类组件相比，实现同等效果，函数组件和类组件复杂度差不多

作用：createContext实现数据的共享，只是消费数据的方式不一样

案例：

- context对象产生的文件createContext.js

  import { createContext } from "react"
  // createContext函数是可以传递参数的。
  // 参数的意义可以理解成是待消费的数据的默认值
  // 默认值一般在项目做单元(组件)测试（unit test）的时候可能会被用上
  export default createContext({ a: 11, b: 22, c: 33 })

售卖方组件

~~~
/**
 * 1. 售卖方提供需要借助context对象的Provider组件
 * 2. 通过售卖方Provider组件的value属性将需要共享的数据传递给子组件
 * 3. 消费方通过useContext函数进行消费，语法：const data = useContext(context对象)
 */

import React from "react"
import Child from "./Child"
import Obj from "./createContext"

// 获取售卖方的身份
const { Provider } = Obj
const ProviderCmp = () => {
    const data = { a: 1111, b: 2222, c: 3333 }
    return (
        <div>
            <Provider value={data}>
                <Child />
            </Provider>
        </div>
    )
}

export default ProviderCmp
~~~

消费方的组件

~~~
import React, { useContext } from "react"
import Obj from "./createContext"
const Child = () => {
    // 通过useContext消费数据
    const data = useContext(Obj)
    return (
        <div>
            <div>a的值是：{data.a}</div>
            <div>b的值是：{data.b}</div>
            <div>c的值是：{data.c}</div>
        </div>
    )
}

export default Child
~~~

