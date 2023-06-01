# 一、Hooks

## 1、简介

React中组建由函数组件与类组件，在 React Hooks 出现之前，我们可以使用函数和类组件来进行项目开发，但是如果组件中需要进行状态管理，函数组件就显得无能为力。React在v16.8 的版本中推出了 React Hooks 新特性，Hook是**一套工具函数的集合**,它增强了函数组件的功能，**hook不等于函数组件，所有的hook函数都是以use开头。**（以use开头的方法，称之为hook）

使用 React Hooks 相比于从前的类组件有以下几点好处：

- 代码可读性更强，原本同一块功能的代码逻辑被拆分在了不同的生命周期函数中，容易使开发者不利于维护和迭代，通过 React Hooks 可以将功能代码聚合，方便阅读维护

- 组件树层级变浅，在原本的代码中，我们经常使用 HOC/render/Props 等方式来复用组件的状态，增强功能等，无疑增加了组件树层数及渲染，而在 React Hooks 中，这些功能都可以通过强大的自定义的 Hooks 来实现

- hook使用比使用类组件简单许多（仁者见仁智者见智）



## 2、hook的使用限制

- hook**只能**用在函数组件**中**，class组件不行
- **普通**函数不能使用hook（hook不能在组件函数外去使用）
- hook不能被有条件的调用，因此不能放在if/for中（如果真有有条件调用的需求，请把条件写在hook函数内）



## 3、常用的hook函数

官网文档：https://reactjs.org/docs/hooks-reference.html

hook的使用步骤：

- 导入hook成员
- 使用hook成员

### 3.1、useState

作用：保存组件的状态

语法：

~~~js
const [state, setstate] = useState(initialState);
// state：名字可以是其他的，数据的名字，获取数据的时候使用。其性质像类组件中state对象中的属性名
// setstate：名字也可以是其他的，这是一个方法，用于修改数据使用的
// initialState：数据的初始值（可以是任意数据类型）
~~~

案例：计数器案例，初始值默认为0，按一次按钮，数字+1

~~~react
// 作用：演示函数组件的useState的hook使用
// useState：模拟类似于类组件的状态实现
// 使用注意点：
// 1. 得先导入useState，该hook是react自带的
// 2. 在使用的时候一定要在函数组件的函数体内的return之前用
// 3. 语法，一般比较固定：
//      const [变量名,设置数据值的方法] = useState(默认值)
//      变量名：获取数据时使用
//      设置数据值的方法：用于后期修改数据使用
//      请注意，这里有一个约定俗成的规则，假设变量名为abc，那么设置变量的值的方法名就会被起成setAbc
// 4. 在设置数据的时候，设置数据的方法实现的是数据的替换，因此对于复合数据类型的数据注意保留以前的数据

import React, { useState } from "react";

const StateHook = () => {
    // 必须写在函数组件的函数体内
    const [count, setCount] = useState(0);
    const [user, setUser] = useState({ username: "zhangsan", age: 29 });
    return (
        <div>
            <div>这是一个函数组件</div>
            <div>count的值是：{count}</div>
            <button onClick={() => setCount(count + 1)}>给count+1</button>
            <hr />
            <div>用户信息如下</div>
            <div>
                用户名是：{user.username}，年龄是：{user.age}
            </div>
            <button onClick={() => setUser({ ...user, username: "张三" })}>修改用户名为“张三”，年龄是29</button>
        </div>
    );
};

export default StateHook;
~~~



### 3.2、useEffect

作用：模拟类组件中的生命周期的

函数组件对于在一些生命周期中操作还是无能为力，所以 React提供了 useEffect 来帮助开发者处理函数组件，来帮助模拟完成一部份的开发中**非常常用的生命周期方法（并不是全部的生命周期）**。常被称为：**副作用处理函数**。此函数的操作是异步的。

useEffect 相当类组件中的3个生命周期 

- componentDidMount
- componentDidUpdate 
- componetWillUnMount

语法：

~~~js
useEffect(() => {
    effect
    return () => {
        cleanup
    };
}, [input]);

// effect：
//	情况1：在没有第二个参数的情况下，该位置默认是模拟componentDidMount和componentDidUpdate生命周期的
//	情况2：在有第二个参数的情况下：
//		情况2的第1种情况：若第2个参数是空数组，则其表示模拟componentDidMount
//		情况2的第2种情况：若第2个参数是非空数组，则其表示只关注特定的数据更新和挂载时的componentDidMount和componentDidUpdate生命周期

// return：
//	return需要返回一个函数，该函数是模拟componetWillUnMount生命周期的
~~~

案例：编写一个声明式的导航，切换组件1和组件2，在组件1和组件2中做useEffect的使用

~~~react
// 作用：模拟类组件的生命周期
// 使用注意：
// 1. 该hook函数作用是模拟类组件的挂载完毕、更新完毕和解挂的生命周期；
// 2. 语法：
//          useEffect(() => {
//              .....
//              return () => {
//                  cleanup...
//              }
//          },[el1,el2.....])
// 3. 其写法有好几种，每一种的含义表示不一样
//      a. 参数1的返回值决定什么？
//           返回值是一个函数，这里函数里可以写代码，该函数用于模拟解挂生命周期
//      b. 参数2决定什么？
//           参数2是一个数组，该数组有两种情况，1是不写，2是写。不写表示模拟组件的挂载完毕和更新完毕的生命周期；如果写，则数组的元素名字必须是state里的可变数据名，表示模拟挂载完毕后及对应的数据更新后的生命周期（监听特定数据的变化）

// 案例：通过声明式导航的切换来演示useEffect的使用

import React, { useEffect, useState } from "react";
import { Link, Route } from "react-router-dom";

const EffectHook = () => {
    return (
        <div>
            <ul>
                <li>
                    <Link to="/home">首页</Link>
                </li>
                <li>
                    <Link to="/news">新闻</Link>
                </li>
                <li>
                    <Link to="/about">关于</Link>
                </li>
            </ul>
            <Route path="/home" component={Home} />
            <Route path="/news" component={News} />
            <Route path="/about" component={About} />
        </div>
    );
};

const Home = () => {
    // 模拟解除挂载操作
    useEffect(() => {
        return () => {
            // 清除对于组件有副作用的操作
            console.log("Home组件将要解除挂载");
        };
    });
    return (
        <div>
            <div>首页页面</div>
        </div>
    );
};

const News = () => {
    // 定义数据状态
    const [state, setstate] = useState(0);
    // 模拟组件挂载完毕、更新完毕的操作

    useEffect(() => {
        // 挂载完毕和更新完毕的操作代码写在函数体里即可，不需要加return
        console.log("News组件已经挂载完毕、更新完毕");
    });

    return (
        <div>
            <div>新闻页面</div>
            <div>数据值是{state}</div>
            <button onClick={() => setstate(state + 1)}>点击+1</button>
        </div>
    );
};

// 模拟挂载和监听特定数据的变化
// 有一种类似于监听器watch的实现
// 这个时候只依赖于state1的变化，state2变化不会触发
const About = () => {
    const [state1, setstate1] = useState(0);
    const [state2, setstate2] = useState(0);
    useEffect(() => {
        console.log("挂载完毕，数据1更新完毕");
    }, [state1]);
    return (
        <div>
            <div>关于页面</div>
            <div>数据1：{state1}</div>
            <button onClick={() => setstate1(state1 + 1)}>数据1+1</button>
            <div>数据2：{state2}</div>
            <button onClick={() => setstate2(state2 + 1)}>数据2+1</button>
        </div>
    );
};

export default EffectHook;
~~~



### 3.3、useRef

作用：用来生成对 DOM 对象的引用（类似于类组件中的createRef方法）

案例：实现表单项数据的获取

~~~react
// 作用：用于获取dom对象，类似于createRef
// 1. 导入
// 2. 使用

import React, { useRef } from "react";

const RefHook = () => {
    // 使用useRef获取ref对象
    const divRef = useRef();
    return (
        <div>
            <div ref={divRef}>这是一个函数组件</div>
            <button onClick={() => console.log(divRef.current)}>获取对象</button>
        </div>
    );
};

export default RefHook;
~~~



### 3.4、redux相关

注意点：官方自带的`useReducer`hook也可以实现针对reudx的操作，但是企业一般不直接用它。而是使用react-redux中提供的封装过的hook（**useSelector,useDispatch**）。如果企业项目有自己封装redux相关的hook的时候才会使用`useReducer`。其实react-redux提供的自定义的hook底层也是基于`useReducer`实现的。

作用：

- useSelector：帮助我们获取仓库中的数据，参数是callback，函数有一形参state（默认数据源）
- useDispatch：帮助我们派发用于修改的action

案例：通过**useSelector,useDispatch**实现对于redux中数据的读写（写法比类组件的写法简单）

~~~react
import React from "react";
import { useSelector, useDispatch } from "react-redux";

const App4 = () => {
    // 数据进行初始化
    // 通过useSelector获取仓库的数据
    const state = useSelector((state) => state.toJS());
    // 通过useDispatch方法产生dispatch方法
    const dispatch = useDispatch();
    return (
        <div>
            <div>当前关键词是：{state.search.keyword}</div>
            <button
                onClick={() => dispatch({ type: "set", payload: "20210201" })}
            >
                改变关键词
            </button>
        </div>
    );
};

export default App4;
~~~



### 3.5、react-route-dom相关

~~~js
import { useHistory, useParams, useLocation } from "react-router-dom";
~~~

作用：快速获取路由信息的

- useHistory：获取history路由信息对象
- useParams：获取路由中动态路由参数对象
- useLocation：获取路由中的location对象信息

案例：在路由中使用三个hook分别打印结果

~~~react
import React, { useEffect } from "react";
import { useHistory, useParams, useLocation } from "react-router-dom";

const Home = () => {
    const history = useHistory();
    console.log(history);
    const params = useParams();
    console.log(params);
    const location = useLocation();
    console.log(location);
    return <div>主页页面</div>;
};

export default Home;
~~~

注意点：用了这个三个hook，组件在导出的时候就不用再withRouter。



### 3.6、自定义hook

案例：自定义在线状态hook，要求使用了这个hook可以自动判断当前网络连接的情况

应用场景：在线聊天类型的项目，可以用这个hook动态判断当前用户的网络连接状态

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/02/f0ddcffdab69175b76024005a708968bef6da0f0.png?sign=153f9147240e75331423bdd8dc046f54&t=6017cfbd)

实现代码：

~~~react
import React, { useState, useEffect } from "react";
// 导入在线状态的图标
import online from "@/assets/icon/online.jpg";
import offline from "@/assets/icon/offline.jpg";

// 网络在线状态检测的hook

// 核心思想：
//      1. 定义hook实质上就是定义函数
//      2. hook的函数名必须以use开头

function useOnline() {
    // 用于获取用户是否在线的状态
    const [state, setstate] = useState(navigator.onLine);

    useEffect(() => {
        // 定义2个方法，用于设置网络状态
        const isOnLine = () => setstate(true);
        const isOffLine = () => setstate(false);

        // 监听网络状态的事件
        window.addEventListener("online", isOnLine, false);
        window.addEventListener("offline", isOffLine, false);

        return () => {
            // 离开组件之前取消监听网络状况的事件（优化）
            window.removeEventListener("online", isOnLine, false);
            window.removeEventListener("offline", isOffLine, false);
        };
    });
    // 返回用户的在线状态
    return state;
}

const App5 = () => {
    // 使用自定义useOnline的hook
    const status = useOnline();
    // console.log(status);
    return (
        <div>
            {status ? (
                <img src={online} width="100" />
            ) : (
                <img src={offline} width="100" />
            )}
        </div>
    );
};

export default App5;
~~~

