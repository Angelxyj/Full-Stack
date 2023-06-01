# React综合案例

# 一、概要

## 1、开发背景

因公司某项目的业务数据管理需要，公司决定安排开发人员组成项目小组，为该项目开发一个后台管理系统，实现该项目日常业务数据的展示和维护。 



## 2、技术栈

使用react框架来完成本次项目的实现，采用前后端分离式开发，使用前端技术有如下一些：

- react

- react-router-dom

- redux

- react-redux

- react-thunk
- immutable

- styled-components 

- antd

- react-transition-group 

- axios
- ……

后端技术有：

- PHP
- MySQL
- Redis
- Laravel
- ......



## 3、开发环境

开发环境为：windows

开发工具：vscode + jsx插件 + eslint

开发调试工具：chrome浏览器

开发运行环境：node环境

上线环境为：linux + nginx + git



## 4、效果预览

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/05/2ef9749584bb0b1a69903b66ebbcaf8d6b01ab88.png?sign=13ea6d02183c0b55c400f69a8139bcb2&t=60af6972)

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/05/40957da2bae30245d941cd5f4ac20ec8d0d93518.png?sign=45f1be96622e68186c66a3f73b845501&t=60af69c1)

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/05/2295c123cdb63ece9b72f15b1b93e5fc12a7417f.png?sign=6d3e6fdcb0c34d77a4a12f57afb61fb2&t=60af80a6)

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/05/e5d74f92064046d56c61a6a06558d054baaf0172.png?sign=22b49892c30e0bf96413dba1a812bda6&t=60b3d9d1)



## 5、项目初始化

a. 首先找个位置创建一下react项目，命令如下：

~~~shell
npx create-react-app backend
~~~

b. 创建好项目后，**进入项目目录**先安装常规要使用的三方包，命令如下：

~~~shell
npm i -S axios redux react-redux redux-thunk styled-components react-router-dom react-transition-group immutable redux-immutable
# axios：网络请求库
# redux：状态管理
# react-redux：redux功能增强的包
# redux-thunk：redux中间件（异步库）
# styled-components：css-in-js热门库
# react-router-dom：路由包
# react-transition-group：动画组件
# immutable：不可变数据实现
# redux-immutable：集成immutable到redux中的包
~~~

~~~shell
npm i -D customize-cra react-app-rewired http-proxy-middleware
# react-app-rewired：默认情况下，我们是没有对于react项目的配置权的。使用了react-app-rewired之后，我们就可以获取到react项目中对于webpack的配置权。
# http-proxy-middleware：代理中间件，在vue中默认写好代理配置即可，在react中需要先安装三方的包，才能写代理配置。
~~~

c. 清理创建好的项目中不需要的文件及文件夹

> - 删除`public`目录下的全部内容
> - 删除`src`目录下的全部内容

d. 在`public`目录下放置一个项目图标文件并创建一个html入口文件，html文件内容大致如下

~~~html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
        <title>某知名网站后台管理系统</title>
        <meta
            name="viewport"
            content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no"
        />
        <link rel="icon" href="/favicon.ico" type="image/x-icon" />
        <meta name="description" content="1000phone网站后台管理系统" />
        <meta name="keywords" content="后台,管理,系统" />
    </head>
    <body>
        <div id="root"></div>
    </body>
</html>
~~~

e. 在`src`目录下创建根组件`App.jsx`与项目入口文件`index.js`

~~~jsx
// 根组件：src/App.jsx
import React, { Component } from "react";

class App extends Component {
    render() {
        return <></>;
    }
}

export default App;
~~~

~~~javascript
// 项目入口文件（编译入口）
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router } from "react-router-dom";
import App from "./App";

ReactDOM.render(
    <Router>
        <App />
    </Router>,
    document.getElementById("root")
);
~~~

f. 在**当前项目根目录下**面创建一个名称为`config-overrides.js`文件，对webpack进行配置（该配置方式不是对react内置配置进行直接修改，而是通过三方的包实现配置覆盖）

~~~js
// 配置信息可以参考：https://www.npmjs.com/package/customize-cra
const {
    override,
    addDecoratorsLegacy,
    disableEsLint,
    addBundleVisualizer,
    addWebpackAlias,
    adjustWorkbox,
} = require("customize-cra");
const path = require("path");

module.exports = override(
    // 在webpack中禁用eslint
    disableEsLint(),

    // 添加webpack别名
    addWebpackAlias({
        // 添加路径对@符号的支持
        ["@"]: path.resolve("./src"),
    })
);
~~~

g. 修改`package.json`中的脚本命令为如下：

> 参考来源：https://www.npmjs.com/package/react-app-rewired

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/d83ef02592b20151676a3161480e71ed60386109.png?sign=367d15c590345effbd1fcaa0aff6af91&t=5fa2e5f7)

h. **在`src`目录下创建一个名称为`setupProxy.js`文件**，提前为后续接口设置反向代理（如果需要的话）

> 与vue一样，代理操作仅限于本地开发环境，上线就失效了。

~~~javascript
const { createProxyMiddleware: proxy } = require("http-proxy-middleware");

module.exports = (app) => {
    app.use(
        "/api",
        proxy({
            // 此处的端口号要与后期数据请求的数据端一致
            target: "http://localhost:9000",
            changeOrigin: true,
        })
    );
};
~~~

i. 建立src/下相关的目录，划分好目录结构（模块化）

- assets：存放静态资源的目录，后续可以房图片、css等文件
- components：封装组件存储的位置
- config：存放配置文件的目录
- hoc：存放高阶组件的文件
- models：存放模型文件
- router：存放路由文件
- services：存放封装一些文件（比如，axios封装）
- store：redux相关的目录
- views：视图组件存放目录



## 6、antd组件库

官网：https://ant.design/

> 面向企业级应用研发的 UI 设计语言与前端技术实现。

**推荐使用 npm 或 yarn 的方式进行开发**，不仅可在开发环境轻松调试，也可放心地在生产环境打包部署使用，享受整个生态圈和工具链带来的诸多好处。

如果你的电脑是使用npm进行项目包管理，请在项目中执行以下命令以在项目中安装ant design：

~~~shell
npm install antd --save
~~~

若使用的是yarn管理，则请运行以下命令：

~~~shell
yarn add antd
~~~

在**项目入口文件中**引入antd的样式文件：

~~~js
import 'antd/dist/antd.css'; 
// or 'antd/dist/antd.less'
~~~

后续需要使用antd组件时，根据对应页面的引导使用即可。



# 二、登录开发

## 1、创建空组件

首先创建登录功能需要用的组件，该功能需要用到2个表单，为了便于维护，建议将俩个表单单独形成组件，加上一个大页面包裹，一共需要三个组件。

- 大组件：views/login/Index.jsx
  - 普通登录组件：views/login/NormalLogin.jsx
  - 短信登录组件：views/login/MobileLogin.jsx

为了有初步的预览效果，可以在组件中适当写一些内容填充使用。

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/05/f7b470db6f5f1e76054cc19b8365525a6f4b1760.png?sign=dbff97ce44a594106174bfba009b9182&t=60b0624e)



## 2、组件封装

目前已知需要封装俩个组件：

- loading组件
  - 封装的目的：antd自带的Spin组件虽然可以实现加载中，但是其位置不居中，在显示上很不友好，为了让路由懒加载能够有很好的用户体验，建议封装Loading组件
- 验证码组件
  - 验证码在项目中一般会重复的使用，为了降低代码重复率，建议封装

封装的组件都放在components目录下：

- src/components/Loading.jsx

~~~jsx
// 封装的目的：antd自带的Spin组件虽然可以实现加载中，但是其位置不居中，在显示上很不友好，为了让路由懒加载能够有很好的用户体验，建议封装Loading组件
import React, { Component } from "react";
import styled from "styled-components";
import { Spin } from "antd";

class Loading extends Component {
    render() {
        return (
            <Main>
                <Spin tip="不要急，请稍等片刻..." size="large" />
            </Main>
        );
    }
}

// 给Spin组件套一个Main组件的样式，让Spin组件能够适当的居中显示
const Main = styled.div`
    margin: 0 auto;
    margin-bottom: 20px;
    padding: 25% 50px;
    text-align: center;
    border-radius: 4px;
`;

export default Loading;
~~~

- src/components/Captcha.jsx

~~~jsx
// 验证码在项目中一般会重复的使用，为了降低代码重复率，建议封装
// 请注意：axios后续需要结合拦截器进行封装
// 问题：此处的axios是用封装前的还是封装后的？
// 镜像问题：此处的请求地址是用封装前的还是封装后的？
// 俩个问题的答案都是一样的：封装前的，目的是让封装的组件具备更好的可移植性（后续可能还有其它的项目也需要用到这个封装的组件，到时候只需要直接复制过去，即插即用）
// 服务端返回三个数据：
// sensitive：对用户输入的验证码内容大小写是否敏感
// key：与验证码对应的验证标识，在验证用户输入的时候需要回传给服务器
// img：验证码对应的base64格式字符串，与正常的路径一样使用，将其给img标签的src属性即可

import React, { Component } from "react";
import axios from "axios";

class Captcha extends Component {
    // 状态初始化
    state = {
        img: "",
    };

    render() {
        return (
            <div>
                {/* 接收来自父组件的高度指定 */}
                <img src={this.state.img} alt="captcha" height={this.props.height} onClick={this.loadCaptcha} />
            </div>
        );
    }

    // 发起网络请求
    componentDidMount() {
        this.loadCaptcha();
    }

    // 获取验证码
    loadCaptcha = () => {
        axios.get("https://reactapi.iynn.cn/captcha/api/math").then((ret) => {
            // 将验证码赋值给img属性
            this.setState(() => {
                return { img: ret.img };
            });
            // 把key给父组件（调用验证码的那个组件）
            // 父组件在调用该验证码组件的时候应当传递一个属性，约定属性名为setKey，该属性对应的是一个方法，接受一个形参，形参就是key
            this.props.setKey(ret.key);
        });
    };
}

export default Captcha;
~~~



## 3、路由规划

路由文件：router/index.js

> 在React中的路由懒加载：
>
> - 需要导入react包中的俩个成员
>   - lazy，其是一个方法，负责去import对应的组件的
>   - Suspense：其是一个组件，负责去应用组件的以及可以制定懒加载时需要的提示组件

~~~js
// 项目路由文件
import { lazy, Suspense } from "react";
import { Route, Redirect, Switch } from "react-router-dom";
// 导入loading组件
import Loading from "@/components/Loading";

// 使用lazy导入需要的组件
const Login = lazy(() => import("@/views/login/Index"));
// .....

// 编写路由规则
const Routes = () => {
    return (
        <Suspense fallback={<Loading />}>
            <Switch>
                <Route path="/login" component={Login}></Route>
                <Redirect from="/" to="/login" />
            </Switch>
        </Suspense>
    );
};

// 导出路由规则
export default Routes;
~~~

在写好路由规则文件后，需要在App.jsx组件中应用所有的路由规则：

~~~jsx
// 根组件
import React, { Component } from "react";
// 导入路由
import Routes from "@/router/index";

class App extends Component {
    render() {
        return (
            <>
                <Routes />
            </>
        );
    }
}

export default App;
~~~



## 4、页面布局

编辑组件：src/views/login/Index.jsx

ant组件：https://ant.design/components/tabs-cn/

设计思路：使用tab选项卡的方式实现`常规登录`和`短信登录`的切换

~~~jsx
// 思路：通过tabs选项卡的方式进行两种登录方式切换，需要导入antd中的tabs组件
import React, { Component } from "react";
import { Tabs } from "antd";
import { LockOutlined, ShakeOutlined } from "@ant-design/icons";
// 导入表单组件
import NormalLogin from "./NormalLogin";
import MobileLogin from "./MobileLogin";
// 导入styled
import styled from "styled-components";
const { TabPane } = Tabs;
// 样式
const Main = styled.div`
    margin: 0 auto;
    width: 400px;
    padding-top: 10%;
`;

class Index extends Component {
    render() {
        return (
            <Main>
                <Tabs defaultActiveKey="1" centered="true" size="large">
                    <TabPane
                        tab={
                            <span>
                                <LockOutlined />
                                常规登录
                            </span>
                        }
                        key="1"
                    >
                        {/* 调用常规登录的表单组件 */}
                        <NormalLogin />
                    </TabPane>
                    <TabPane
                        tab={
                            <span>
                                <ShakeOutlined />
                                短信登录
                            </span>
                        }
                        key="2"
                    >
                        {/* 调用手机登录的表单组件 */}
                        <MobileLogin />
                    </TabPane>
                </Tabs>
            </Main>
        );
    }
}

export default Index;
~~~



## 5、功能实现

### 5.1、常规登录

任务的拆解：

- 产生表单的效果

~~~jsx
import React, { Component, createRef } from "react";
import { Form, Input, Button, Row, Col } from "antd";
// 导入封装好的验证码组件
import Captcha from "@/components/Captcha";
const layout = {
    labelCol: {
        span: 8,
    },
    wrapperCol: {
        span: 16,
    },
};
const tailLayout = {
    wrapperCol: {
        offset: 4,
        span: 20,
    },
};

class NormalLogin extends Component {
    // 构造函数
    constructor(props) {
        super(props);
        this.ref_captcha = createRef();
    }
    // 初始化状态
    state = {
        key: "",
    };
    render() {
        return (
            <div>
                <Form
                    {...layout}
                    name="basic"
                    initialValues={{
                        remember: true,
                    }}
                    onFinish={this.onFinish}
                    onFinishFailed={this.onFinishFailed}
                >
                    <Form.Item
                        label="用户名"
                        name="username"
                        rules={[
                            {
                                required: true,
                                message: "请输入用户名！",
                            },
                        ]}
                    >
                        <Input />
                    </Form.Item>

                    <Form.Item
                        label="密码"
                        name="password"
                        rules={[
                            {
                                required: true,
                                message: "请输入密码！",
                            },
                        ]}
                    >
                        <Input.Password />
                    </Form.Item>

                    <Form.Item label="验证码">
                        <Row gutter={8}>
                            <Col span={14}>
                                <Form.Item
                                    name="captcha"
                                    noStyle
                                    rules={[
                                        {
                                            required: true,
                                            message: "请输入验证码！",
                                        },
                                    ]}
                                >
                                    <Input />
                                </Form.Item>
                            </Col>
                            <Col span={10}>
                                {/* 放上验证码组件 */}
                                <Captcha height="31.6" setKey={this.setKey} ref={this.ref_captcha} />
                            </Col>
                        </Row>
                    </Form.Item>

                    <Form.Item {...tailLayout}>
                        {/* block：让按钮与其父元素一样宽 */}
                        <Button type="primary" htmlType="submit" block>
                            登录
                        </Button>
                    </Form.Item>
                </Form>
            </div>
        );
    }

    // 登录按钮的回调
    onFinish = (values) => {
        // 需要在这里发送请求
        console.log("Success:", values);
    };

    onFinishFailed = (errorInfo) => {
        console.log("Failed:", errorInfo);
    };

    // 用于传递给子组件，获取子返回的key
    setKey = (key) => {
        this.setState(() => {
            return {
                key,
            };
        });
    };
}

export default NormalLogin;
~~~

- 逻辑部分的编写

在首次编写网络请求前，需要对地址、axios等进行封装，封装好再去进行网络请求以及其业务逻辑部分的实现。

①封装地址配置文件：src/config/url.js

~~~js
// 该文件用于配置项目中的网络请求地址
let baseUrl = "https://reactapi.iynn.cn";

// 地址定义
// 常规登录
export const NORMAL_LOGIN = baseUrl + "/api/common/auth/login";
// 短信登录
export const MOBILE_LOGIN = baseUrl + "/api/common/auth/mobile";
// ....
~~~

②封装axios配置文件：src/services/http.js

~~~js
// 对axios的封装
import axios from "axios";

// 请求拦截器
axios.interceptors.request.use((cfg) => {
    // 判断本地是否有jwt，有就带着
    let jwt = localStorage.getItem("jwt");
    if (jwt) {
        // 将jwt放到请求头中
        cfg.headers.Authorization = jwt;
    }
    return cfg;
});

// 响应拦截器
axios.interceptors.response.use((ret) => {
    // 判断
    if (ret.data.context && ret.data.context.jwt) {
        // 说明服务器返回了新的jwt值，替换掉本地已经存储的
        localStorage.setItem("jwt", ret.data.context.jwt);
    }

    // 简化返回值，省去了一个data
    return ret.data || ret;
});
// 导出
export default axios;
~~~

③封装模型文件：src/models/common.js

> 模型是干啥的？？
>
> 目的/作用：将对于请求的数据的处理以及请求放到一起，这样显得组件代码更加干净！

~~~js
// common模型：将公共的请求数据处理及请求操作写在这里
import req from "@/services/http";
import { NORMAL_LOGIN, MOBILE_LOGIN } from "@/config/url";

// 做请求方法
const model = {
    // 常规登录方法
    normalLogin(obj) {
        return req.post(NORMAL_LOGIN, obj);
    },
    // .....
};

// 导出模型
export default model;
~~~

④实现需要的登录业务需求

~~~jsx
import React, { Component, createRef } from "react";
import { Form, Input, Button, Row, Col, message } from "antd";
import { withRouter } from "react-router-dom";
// 导入封装好的验证码组件
import Captcha from "@/components/Captcha";
// 导入需要的模型
import Model from "@/models/common";
const layout = {
    labelCol: {
        span: 8,
    },
    wrapperCol: {
        span: 16,
    },
};
const tailLayout = {
    wrapperCol: {
        offset: 4,
        span: 20,
    },
};

class NormalLogin extends Component {
    // 构造函数
    constructor(props) {
        super(props);
        this.ref_captcha = createRef();
    }
    // 初始化状态
    state = {
        key: "",
    };
    render() {
        return (
            <div>
                <Form
                    {...layout}
                    name="basic"
                    initialValues={{
                        remember: true,
                    }}
                    onFinish={this.onFinish}
                    onFinishFailed={this.onFinishFailed}
                >
                    <Form.Item
                        label="用户名"
                        name="username"
                        rules={[
                            {
                                required: true,
                                message: "请输入用户名！",
                            },
                        ]}
                    >
                        <Input />
                    </Form.Item>

                    <Form.Item
                        label="密码"
                        name="password"
                        rules={[
                            {
                                required: true,
                                message: "请输入密码！",
                            },
                        ]}
                    >
                        <Input.Password />
                    </Form.Item>

                    <Form.Item label="验证码">
                        <Row gutter={8}>
                            <Col span={14}>
                                <Form.Item
                                    name="captcha"
                                    noStyle
                                    rules={[
                                        {
                                            required: true,
                                            message: "请输入验证码！",
                                        },
                                    ]}
                                >
                                    <Input />
                                </Form.Item>
                            </Col>
                            <Col span={10}>
                                {/* 放上验证码组件 */}
                                <Captcha height="31.6" setKey={this.setKey} ref={this.ref_captcha} />
                            </Col>
                        </Row>
                    </Form.Item>

                    <Form.Item {...tailLayout}>
                        {/* block：让按钮与其父元素一样宽 */}
                        <Button type="primary" htmlType="submit" block>
                            登录
                        </Button>
                    </Form.Item>
                </Form>
            </div>
        );
    }

    // 登录按钮的回调
    onFinish = (values) => {
        // 组合一下key
        values["key"] = this.state.key;
        // 需要在这里发送请求
        Model.normalLogin(values).then((ret) => {
            // 判断结果
            if (ret.errNo === 0) {
                // 没错
                message.success(ret.message, 2, () => {
                    this.props.history.push("/dashboard");
                });
                // dashboard：面板/仪表盘
                // this.props.history.push("/dashboard");
            } else {
                // 有错
                message.error(ret.errText);
                // 调用子组件（验证码组件）刷新验证码
                this.ref_captcha.current.loadCaptcha();
            }
        });
    };

    onFinishFailed = (errorInfo) => {
        console.log("Failed:", errorInfo);
    };

    // 用于传递给子组件，获取子返回的key
    setKey = (key) => {
        this.setState(() => {
            return {
                key,
            };
        });
    };
}

export default withRouter(NormalLogin);
~~~



### 5.2、短信登录

步骤分解：

- 处理大的表单，展示手机号、验证码表单项
- 在点击“获取”按钮的时候弹出模态窗口来显示图形验证码
- 在用户输入正确的验证码后（获取到一个用于请求短信验证码的token）才能允许用户点击“获取”按钮来获取短信验证码
- 输入短信验证码后再按“登录”按钮进行登录验证（传递requestId给服务器）

依次需要使用的接口：

- 获取图形验证码
- 验证图形验证码
- 获取短信验证码
- 手机号登录接口

a. 在url封装的文件中声明接口需要使用的地址

~~~js
// 验证图形验证码
export const VERIFY_CAPTCHA = baseUrl + "/api/common/captcha/verify";
// 短信验证码的获取
export const GET_SMS_CODE = baseUrl + "/api/common/sms/send";
~~~

b. 在模型文件models/common.js中增加两个模型方法

~~~js
// 记得导入相关的地址
// 图形验证码的验证
verifyCpt(obj) {
    return req.post(VERIFY_CAPTCHA, obj);
},
// 获取短信验证码
getCode(obj) {
    return req.post(GET_SMS_CODE, obj);
},
// 短信登录
mobileLogin(obj) {
    return req.post(MOBILE_LOGIN, obj);
},
~~~



完整的组件代码：

~~~jsx
import React, { Component, createRef } from "react";
import { Form, Input, Button, Col, Row, Modal, message } from "antd";
import { withRouter } from "react-router-dom";
// 导入验证码组件
import Captcha from "@/components/Captcha";
// 导入模型
import Model from "@/models/common";
const layout = {
    labelCol: {
        span: 8,
    },
    wrapperCol: {
        span: 16,
    },
};
const tailLayout = {
    wrapperCol: {
        offset: 0,
        span: 24,
    },
};
class MobileLogin extends Component {
    // 构造函数
    constructor(props) {
        super(props);
        // 创建表单需要的ref对象
        this.ref_cpt = createRef();
        // 获取验证码组件的ref
        this.ref_captcha = createRef();
        // 获取手机号
        this.ref_mobile = createRef();
    }
    // 状态的初始化
    state = {
        // 用于验证图形验证码的
        key: "",
        // 存储图形验证码的token，在验证图形验证码成功后才会有token
        token: "",
        // token的过期时间
        expire: 0,
        // 模态窗口是否可见
        isModalVisible: false,
        // 短信发送成功后的reqid
        requestId: "",
        // 倒计时计数器
        count: 60,
        // 做一个按钮的标志，用于标识当时按的状态是为获取短信还是倒计时
        can: true,
    };
    render() {
        return (
            <div>
                <Form
                    {...layout}
                    name="basic"
                    initialValues={{
                        remember: true,
                    }}
                    onFinish={this.onFinish}
                    onFinishFailed={this.onFinishFailed}
                >
                    <Form.Item
                        label="手机号"
                        name="mobile"
                        rules={[
                            {
                                required: true,
                                message: "请输入手机号！",
                            },
                        ]}
                    >
                        <Input ref={this.ref_mobile} />
                    </Form.Item>

                    <Form.Item label="短信验证码">
                        <Row gutter={8}>
                            <Col span={14}>
                                <Form.Item
                                    name="code"
                                    noStyle
                                    rules={[
                                        {
                                            required: true,
                                            message: "请输入短信验证码！",
                                        },
                                    ]}
                                >
                                    <Input />
                                </Form.Item>
                            </Col>
                            <Col span={10}>
                                {/* block：让按钮与父元素宽度一样 */}
                                <Button block onClick={this.getCode}>
                                    {this.state.can ? "获取短信" : this.state.count + "秒后获取"}
                                </Button>
                            </Col>
                        </Row>
                    </Form.Item>

                    <Form.Item {...tailLayout}>
                        <Button type="primary" htmlType="submit" block>
                            登录
                        </Button>
                    </Form.Item>
                </Form>

                {/* 模态窗口用一阶段的话来讲，其就是一个弹出的div，不用非得放在Form组件里面 */}
                <Modal title="验证码" visible={this.state.isModalVisible} onOk={this.handleOk} onCancel={this.handleCancel} okText="确定" cancelText="取消">
                    <Form>
                        <Form.Item label="验证码">
                            <Row gutter={8}>
                                <Col span={14}>
                                    <Form.Item
                                        name="captcha"
                                        noStyle
                                        rules={[
                                            {
                                                required: true,
                                                message: "请输入验证码！",
                                            },
                                        ]}
                                    >
                                        <Input ref={this.ref_cpt} />
                                    </Form.Item>
                                </Col>
                                <Col span={10}>
                                    {/* 放上验证码组件 */}
                                    <Captcha height="31.6" setKey={this.setKey} ref={this.ref_captcha} />
                                </Col>
                            </Row>
                        </Form.Item>
                    </Form>
                </Modal>
            </div>
        );
    }

    // 整体表单的提交事件
    onFinish = (values) => {
        // console.log("Success:", values);
        // 装载请求短信的id
        values["requestId"] = this.state.requestId;
        // 进行网络请求
        Model.mobileLogin(values).then((ret) => {
            if (ret.errNo === 0) {
                message.success(ret.message, 2, () => {
                    // 跳转到后台首页
                    this.props.history.push("/dashboard");
                });
            } else {
                message.error(ret.errText);
            }
        });
    };

    // 整体表单的错误事件
    onFinishFailed = (errorInfo) => {
        console.log("Failed:", errorInfo);
    };

    // 短信验证码的获取方法
    getCode = () => {
        let mobile = this.ref_mobile.current.props.value;
        if (/^1[3-9]\d{9}$/.test(mobile)) {
            if (this.state.can) {
                // 显示弹窗
                this.showModal(true);
            }
        } else {
            message.error("请输入正确的手机号");
        }
    };

    // 倒计时
    countDown = () => {
        if (this.state.count === 1) {
            this.setState(() => {
                return {
                    count: 60,
                    can: true,
                };
            });
        } else {
            this.setState((state) => {
                return {
                    count: state.count - 1,
                };
            });
            setTimeout(() => {
                this.countDown();
            }, 1000);
        }
    };

    // 控制模态窗口的显示与否
    showModal = (flag) => {
        this.setState(() => {
            return {
                isModalVisible: flag,
            };
        });
    };

    // 模态窗口确定按钮的回调事件
    handleOk = () => {
        // this.showModal(false);
        // 开始验证用户的图形验证码
        let values = {};
        // 装载请求的数据
        let mobile = this.ref_mobile.current.props.value;
        values["captcha"] = this.ref_cpt.current.props.value;
        values["key"] = this.state.key;
        // 发送请求验证图形验证码
        Model.verifyCpt(values).then((ret) => {
            // 判断结果
            if (ret.errNo === 0) {
                message.success(ret.message, 2, () => {
                    // 保存token及expire
                    this.setState(
                        () => {
                            return {
                                token: ret.context.token,
                                expire: ret.context.expire,
                            };
                        },
                        () => {
                            // 关闭窗口
                            this.showModal(false);
                            // 发送验证码
                            // 发送请求
                            let data = {};
                            data["token"] = this.state.token;
                            data["mobile"] = mobile;
                            data["type"] = 0;
                            Model.getCode(data).then((ret) => {
                                if (ret.errNo === 0) {
                                    // 测试倒计时
                                    this.setState(() => {
                                        return { can: false };
                                    });
                                    this.countDown();
                                    // 赋值
                                    message.success(ret.message, 2, () => {
                                        this.setState(() => {
                                            return {
                                                requestId: ret.requestId,
                                            };
                                        });
                                    });
                                } else {
                                    message.error(ret.errText);
                                }
                            });
                        }
                    );
                });
            } else {
                message.error(ret.errText);
                this.ref_captcha.current.loadCaptcha();
            }
        });
    };

    // 模态窗口的取消按钮的回调事件
    handleCancel = () => {
        this.showModal(false);
    };

    setKey = (key) => {
        this.setState(() => {
            return { key };
        });
    };
}

export default withRouter(MobileLogin);
~~~



# 三、后台开发

## 1、后台首页

### 1.1、防翻墙

①创建后台首页组件和路由

组件：src/views/dashboard/Index.jsx

修改完路由规则后，最新的路由规则如下：

~~~js
// 项目路由文件
import { lazy, Suspense } from "react";
import { Route, Redirect, Switch } from "react-router-dom";
// 导入loading组件
import Loading from "@/components/Loading";

// 使用lazy导入需要的组件
const Login = lazy(() => import("@/views/login/Index"));
const Dashboard = lazy(() => import("@/views/dashboard/Index"));
// .....

// 编写路由规则
const Routes = () => {
    return (
        <Suspense fallback={<Loading />}>
            <Switch>
                <Route path="/login" component={Login}></Route>
                <Route path="/dashboard" component={Dashboard}></Route>
                <Redirect from="/" to="/login" />
            </Switch>
        </Suspense>
    );
};

// 导出路由规则
export default Routes;
~~~



②怎么样去防止翻墙（难点）

原因：在react中没有类似于vue的路由守卫，如果需要实现对应的效果，则需要手动配置。

利用的知识点：HOC

a. 编写高阶组件

先在地址配置文件中添加jwt预检地址配置：

~~~js
// 配置jwt预检
export const JWT_PRE_CHECK = baseUrl + "/api/common/auth/jwtPreCheck";
~~~

在模型中添加请求方法：

~~~js
// jwt预检
checkJWT() {
    return req.get(JWT_PRE_CHECK);
},
~~~

~~~jsx
// 这是一个高阶组件，用于判断用户是否登录，如果登录了则继续访问，否则去登录页面

import React, { Component } from "react";
import { Redirect } from "react-router-dom";
// 导入模型
import Model from "@/models/common";

function CheckLogin(Cmp) {
    // 返回一个新的组件
    return class Hoc extends Component {
        // 状态
        state = {
            isLogin: false,
            // 标志状态修改是否结束
            isFinish: false,
        };

        render() {
            // isFinish必须要为true，才让其走后面的三元，否则等下再走
            // {...this.props}：表示在HOC返回新组件的时候，携带传入组件的props，目的很简单，避免数据丢失
            return <>{this.state.isFinish ? this.state.isLogin ? <Cmp {...this.props} /> : <Redirect to="/login" /> : <div />}</>;
        }

        // 在生命周期中判断用户是否登录
        componentDidMount() {
            // 判断是否有token
            let jwt = localStorage.getItem("jwt");
            if (jwt) {
                // 有token，还需要向后台发送请求二次检验jwt是否正确
                Model.checkJWT().then((ret) => {
                    if (ret.errNo === 0) {
                        // jwt正常，可以继续访问
                        this.setState(() => {
                            return {
                                isLogin: true,
                                isFinish: true,
                            };
                        });
                    } else {
                        // jwt验证失败，回登录页面
                        this.setState(() => {
                            return {
                                isLogin: false,
                                isFinish: true,
                            };
                        });
                    }
                });
            } else {
                // 无token
                console.log("没有登录");
                this.setState(() => {
                    return {
                        isFinish: true,
                    };
                });
            }
        }
    };
}

export default CheckLogin;
~~~

b. 在需要登录才能访问的组件中去使用hoc

~~~jsx
// 以后台首页组件为例：
import React, { Component } from "react";
import CheckLogin from "@/hoc/CheckLogin";
class Index extends Component {
    render() {
        return <div>后台首页</div>;
    }
}

export default CheckLogin(Index);
~~~



### 1.2、后台布局

a. 先在地址配置文件src/config/url.js中配置获取管理员信息的地址

~~~js
// 获取管理员信息
export const GET_ADMIN_INFO = baseUrl + "/api/common/auth/adminInfo";
~~~

b. 在模型文件src/models/common.js中新增获取管理员信息的方法

~~~js
getAdminInfo() {
    return req.get(GET_ADMIN_INFO);
},
~~~

c. 编写后台首页的布局及数据的获取

组件：src/views/dashboard/Index.jsx

~~~jsx
import React, { Component } from "react";
import CheckLogin from "@/hoc/CheckLogin";
import { Layout, Menu } from "antd";
import { MenuUnfoldOutlined, MenuFoldOutlined, UserOutlined, VideoCameraOutlined, UploadOutlined } from "@ant-design/icons";
// 导入从组件页面上复制的css文件
import "@/assets/css/layout.css";
// 导入需要使用的logo
import logo from "@/assets/images/logo.png";
import miniLogo from "@/assets/images/favicon.ico";
// 导入模型
import Model from "@/models/common";

const { Header, Sider, Content } = Layout;
class Index extends Component {
    state = {
        collapsed: false,
        // 保存用户信息
        admininfo: { last_login_addr: {} },
    };

    toggle = () => {
        this.setState({
            collapsed: !this.state.collapsed,
        });
    };
    render() {
        return (
            <Layout style={{ height: "100%" }}>
                <Sider trigger={null} collapsible collapsed={this.state.collapsed}>
                    {/* 通过动态的方式来控制logo的显示 */}
                    <div className="logo">{this.state.collapsed ? <img src={miniLogo} /> : <img src={logo} />}</div>
                    <Menu theme="dark" mode="inline" defaultSelectedKeys={["1"]}>
                        <Menu.Item key="1" icon={<UserOutlined />}>
                            用户管理
                        </Menu.Item>
                        <Menu.Item key="2" icon={<VideoCameraOutlined />}>
                            视频管理
                        </Menu.Item>
                        <Menu.Item key="3" icon={<UploadOutlined />}>
                            上传管理
                        </Menu.Item>
                    </Menu>
                </Sider>
                <Layout className="site-layout">
                    <Header className="site-layout-background" style={{ padding: "5 5" }}>
                        {React.createElement(this.state.collapsed ? MenuUnfoldOutlined : MenuFoldOutlined, {
                            className: "trigger",
                            onClick: this.toggle,
                        })}
                        <span>
                            {" "}
                            欢迎您：{this.state.admininfo.username}！您上次登录于
                            {" " + this.state.admininfo.last_login_addr.state + " " + this.state.admininfo.last_login_addr.isp}（
                            {this.state.admininfo.last_ip}）
                        </span>
                    </Header>
                    <Content
                        className="site-layout-background"
                        style={{
                            margin: "24px 16px",
                            padding: 24,
                            minHeight: 280,
                        }}
                    >
                        <div>欢迎使用xxx项目后台管理系统！</div>
                    </Content>
                </Layout>
            </Layout>
        );
    }

    // 生命周期
    componentDidMount() {
        // 获取管理员信息
        Model.getAdminInfo().then((ret) => {
            // console.log(ret);
            this.setState(() => {
                return {
                    admininfo: ret.accountInfo,
                };
            });
        });
    }
}

export default CheckLogin(Index);
~~~



## 2、用户模块

### 2.1、用户列表

步骤：

a. 创建出需要使用的组件及其路由，确保组件在路由规则匹配的情况下展示没有问题；

新建的路由规则文件src/router/nest.js

~~~js
// 这是一个嵌套路由规则文件

// 项目路由文件
import { lazy, Suspense } from "react";
import { Route, Redirect, Switch } from "react-router-dom";
// 导入loading组件
import Loading from "@/components/Loading";

// lazy导入需要渲染的组件
const UserList = lazy(() => import("@/views/dashboard/users/Index"));
// ....

const Routes = () => {
    return (
        <Suspense fallback={<Loading />}>
            <Switch>
                <Route path="/dashboard/users" component={UserList} />
            </Switch>
        </Suspense>
    );
};

// 导出
export default Routes;
~~~

将该路由规则文件放到指定的地方去应用（src/dashboard/Index.jsx）：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/05/da56b8cbe35350d2bcf16785624da8e5cdb7184a.png?sign=3b704038397f1525cbcd688e0f735527&t=60b48e8e)

再去编写改变菜单的处理程序：

~~~js
// 菜单点击时候触发的事件处理程序
changeMenu = (obj) => {
    // 判断key得到要跳转的地址
    if (obj.key === "1") {
        // 去用户管理页面
        this.props.history.push("/dashboard/users");
    }
};
~~~

b. 获取组件需要的展示表格的数据；

> 在排组件的时候需要注意，table组件自带分页器的，但是这个分页器是前端分页的实现，需要使用后端分页，自行去找ant中的单独的分页器。

先在地址配置文件中定义地址：

~~~js
// 用户管理模块 - 用户列表
export const GET_USER_LIST = baseUrl + "/api/users";
~~~

创建新的模型文件src/models/users.js，定义模型方法：

~~~js
// 用户模块

import req from "@/services/http";
import { GET_USER_LIST } from "@/config/url";

// 定义模块的模型
const model = {
    // 获取用户列表
    getUserList(obj) {
        // 该接口为get请求类型
        return req.get(GET_USER_LIST, {
            params: obj,
        });
    },
};

// 导出模型
export default model;
~~~

最终请求数据，展示数据。

c. 利用获取到的数据展示在表格中（结合分页实现）

> ant支持国际化，默认使用的英文，如果需要切换语言，例如切换成中文显示，则可以参考：https://ant.design/components/config-provider-cn/index.js，文件中的写法：
>
> ~~~js
> // 项目入口文件（编译入口）
> import React from "react";
> import ReactDOM from "react-dom";
> import { BrowserRouter as Router } from "react-router-dom";
> import App from "./App";
> import "antd/dist/antd.css";
> // 针对ant的全局化配置
> import { ConfigProvider } from "antd";
> // 导入中文包
> import zhCN from "antd/lib/locale/zh_CN";
> 
> ReactDOM.render(
>     <ConfigProvider locale={zhCN}>
>         <Router>
>             <App />
>         </Router>
>     </ConfigProvider>,
>     document.getElementById("root")
> );
> ~~~

完整的组件（src/views/dashboard/users/Index.js）代码：

~~~jsx
// 作用：实现用户列表功能

import React, { Component } from "react";
import { PageHeader, Button, Table, Tag, Space, Pagination } from "antd";
import Model from "@/models/users";

class Index extends Component {
    // 状态
    state = {
        paginate: {},
        // 页码
        page: 1,
        // 关键词的数据
        keyword: "",
    };

    //  表格的标题数据
    columns = [
        {
            title: "ID",
            dataIndex: "id",
            key: "id",
        },
        {
            title: "用户名",
            dataIndex: "username",
            key: "username",
        },
        {
            title: "邮箱",
            dataIndex: "email",
            key: "email",
        },
        {
            title: "性别",
            dataIndex: "gender",
            key: "gender",
            render: (gender) => {
                switch (gender) {
                    case "1":
                        return "男";
                    case "2":
                        return "女";
                    default:
                        return "保密";
                }
            },
        },
        {
            title: "状态",
            key: "status",
            dataIndex: "status",
            render: (status) => <>{status === "1" ? <Tag color="green">正常</Tag> : <Tag color="red">禁用</Tag>}</>,
        },
        {
            title: "操作",
            key: "action",
            render: (text, record) => (
                <Space size="middle">
                    <a>编辑</a>
                    <a>删除</a>
                </Space>
            ),
        },
    ];
    render() {
        return (
            <div>
                {/* 页面头部 */}
                <PageHeader
                    ghost={false}
                    title="用户管理"
                    subTitle="这是用户管理模块，当前实现的功能是用户列表！"
                    extra={[
                        <Button key="2">统计数据</Button>,
                        <Button key="1" type="primary">
                            新增
                        </Button>,
                    ]}
                ></PageHeader>

                {/* 表格 */}
                <Table columns={this.columns} dataSource={this.state.paginate.data} pagination={false} rowKey={(row) => row.id} />

                {/* 后台分页实现 */}
                <div style={{ marginTop: "20px", textAlign: "center" }}>
                    <Pagination defaultCurrent={this.state.page} total={this.state.paginate.total} onChange={this.changePage} showSizeChanger={false} />
                </div>
            </div>
        );
    }

    // 生命周期
    componentDidMount() {
        // 请求数据
        this.loadData();
    }

    // 封装请求数据的方法
    loadData = () => {
        // 获取请求参数
        let obj = {
            keyword: this.state.keyword,
            page: this.state.page,
        };
        // 模型发送请求
        Model.getUserList(obj).then((ret) => {
            // console.log(ret);
            this.setState(() => {
                return {
                    paginate: ret.paginate,
                };
            });
        });
    };

    //页码改变事件处理程序
    changePage = (page, pageSize) => {
        this.setState(
            () => {
                return { page };
            },
            () => this.loadData()
        );
    };
}

export default Index;
~~~



### 2.2、用户统计

含义：将数据通过前端插件，以丰富的图表的形式展现在界面上。【数据可视化】

数据可视化：

- 将数据展现成图表的形式：折线图、饼状图、柱状图等
- 地图上路径规划、POI标注等等
- 公司组织架构图
- 思维导图

案例：根据接口返回的男女数量的数据，展示合适图表

本次使用的插件库：echarts

官网：https://echarts.apache.org/

> 阿里的可视化组件库：https://antv.vision/



a. 安装echarts

~~~shell
npm i -S echarts
~~~

b. 创建代码获取数据，展示数据

在url.js文件中定义请求数据的地址：

~~~js
// 用户管理模块 - 统计数据
export const GET_STATISTICS_DATA = baseUrl + "/api/users/statistics";
~~~

在模型文件src/models/users.js中定义模型方法：

~~~js
getStatistics() {
    return req.get(GET_STATISTICS_DATA);
},
~~~

完整的组件（src/views/dashboard/users/Index.jsx）代码：

~~~jsx
// 作用：实现用户列表功能

import React, { Component } from "react";
import { PageHeader, Button, Table, Tag, Space, Pagination, Modal } from "antd";
import Model from "@/models/users";
// 导入echarts
import * as echarts from "echarts/core";
import { TitleComponent, TooltipComponent, LegendComponent } from "echarts/components";
import { PieChart } from "echarts/charts";
import { CanvasRenderer } from "echarts/renderers";
echarts.use([TitleComponent, TooltipComponent, LegendComponent, PieChart, CanvasRenderer]);

class Index extends Component {
    // 状态
    state = {
        paginate: {},
        // 页码
        page: 1,
        // 关键词的数据
        keyword: "",
        // 模态窗口的控制开关
        visible: false,
        // 统计数据的值
        statistics: [],
    };

    //  表格的标题数据
    columns = [
        {
            title: "ID",
            dataIndex: "id",
            key: "id",
        },
        {
            title: "用户名",
            dataIndex: "username",
            key: "username",
        },
        {
            title: "邮箱",
            dataIndex: "email",
            key: "email",
        },
        {
            title: "性别",
            dataIndex: "gender",
            key: "gender",
            render: (gender) => {
                switch (gender) {
                    case "1":
                        return "男";
                    case "2":
                        return "女";
                    default:
                        return "保密";
                }
            },
        },
        {
            title: "状态",
            key: "status",
            dataIndex: "status",
            render: (status) => <>{status === "1" ? <Tag color="green">正常</Tag> : <Tag color="red">禁用</Tag>}</>,
        },
        {
            title: "操作",
            key: "action",
            render: (text, record) => (
                <Space size="middle">
                    <a>编辑</a>
                    <a>删除</a>
                </Space>
            ),
        },
    ];
    render() {
        return (
            <div>
                {/* 页面头部 */}
                <PageHeader
                    ghost={false}
                    title="用户管理"
                    subTitle="这是用户管理模块，当前实现的功能是用户列表！"
                    extra={[
                        <Button key="2" onClick={this.showModal}>
                            统计数据
                        </Button>,
                        <Button key="1" type="primary">
                            新增
                        </Button>,
                    ]}
                ></PageHeader>

                {/* 表格 */}
                <Table columns={this.columns} dataSource={this.state.paginate.data} pagination={false} rowKey={(row) => row.id} />

                {/* 后台分页实现 */}
                <div style={{ marginTop: "20px", textAlign: "center" }}>
                    <Pagination defaultCurrent={this.state.page} total={this.state.paginate.total} onChange={this.changePage} showSizeChanger={false} />
                </div>

                {/* 统计的模态窗口 */}
                <Modal
                    title="男女会员比例统计"
                    centered
                    visible={this.state.visible}
                    onOk={() => this.setVisible(false)}
                    onCancel={() => this.setVisible(false)}
                    width={1000}
                >
                    {/* 这里放置图表 */}
                    <div id="main" style={{ height: "450px", width: "900px" }}></div>
                </Modal>
            </div>
        );
    }

    // 更改模态窗口的状态
    setVisible = (flag) => {
        this.setState(() => {
            return {
                visible: flag,
            };
        });
    };

    // 展示统计数据的模态窗口
    showModal = () => {
        // this.setVisible(true);
        // 获取数据
        Model.getStatistics().then((ret) => {
            if (ret.errNo === 0) {
                // 成功展示数据
                this.setState(
                    () => {
                        // 赋值数据
                        return {
                            visible: true,
                            statistics: ret.data,
                        };
                    },
                    () => {
                        // 在这里展示图表
                        var chartDom = document.getElementById("main");
                        var myChart = echarts.init(chartDom);
                        var option;

                        option = {
                            title: {
                                text: "本站会员性别比例饼状图",
                                subtext: "数据来自于数据库",
                                left: "center",
                            },
                            tooltip: {
                                trigger: "item",
                                formatter: '{a} <br/>{b} : {c} ({d}%)'
                            },
                            legend: {
                                orient: "vertical",
                                left: "left",
                            },
                            series: [
                                {
                                    name: "性别",
                                    type: "pie",
                                    // 半径
                                    radius: "60%",
                                    data: this.state.statistics,
                                    emphasis: {
                                        itemStyle: {
                                            shadowBlur: 10,
                                            shadowOffsetX: 0,
                                            shadowColor: "rgba(0, 0, 0, 0.5)",
                                        },
                                    },
                                },
                            ],
                        };
                        option && myChart.setOption(option);
                    }
                );
            }
        });
    };

    // 生命周期
    componentDidMount() {
        // 请求数据
        this.loadData();
    }

    // 封装请求数据的方法
    loadData = () => {
        // 获取请求参数
        let obj = {
            keyword: this.state.keyword,
            page: this.state.page,
        };
        // 模型发送请求
        Model.getUserList(obj).then((ret) => {
            // console.log(ret);
            this.setState(() => {
                return {
                    paginate: ret.paginate,
                };
            });
        });
    };

    //页码改变事件处理程序
    changePage = (page, pageSize) => {
        this.setState(
            () => {
                return { page };
            },
            () => this.loadData()
        );
    };
}

export default Index;
~~~

