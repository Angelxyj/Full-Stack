React综合案例

# 一、概要

## 1、开发背景

因公司某项目的业务数据管理需要，公司决定安排开发人员组成项目小组，为该项目开发一个后台管理系统，实现该项目日常业务数据的展示和维护。 【**切勿直接写增删改查**】

- 通用功能
  - 登录
- 数据的展示
  - 列表
  - 图
  - ......
- 数据添加
  - 正常表格添加数据
  - .....




开发背景需要体现：

- 为什么开发项目
- 开发项目能解决什么问题
- 比较核心的技术栈及功能实现



## 2、技术栈

使用react框架来完成本次项目的实现，采用前后端分离式开发，使用前端技术有如下一些：

- react：主框架
- react-router-dom：路由包
- redux：大规模状态管理库
- react-redux：给redux做强化
- styled-components ：css-in-js技术的实现
- antd：前端组件库（ant design、antd mobile）
- axios：网络请求库
- ……

后端技术有：

- PHP/JAVA/Go/Python/NodeJS：后端语言
- MySQL：数据库软件 (关系型数据库)
- Redis：数据库软件 (非关系型数据库)
- Laravel(php)：后端框架  Spring(JAVA)
- ......



## 3、开发环境

开发环境：Windows

开发工具：vscode + jsx插件

开发调试工具：chrome浏览器

开发运行环境：node环境>= v14

上线环境为：nginx + git



## 4、效果预览

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/11/9b9973f4eb782bdab2fb6eab57540b41eeda91bb.jpeg?sign=4777e7843f40b2d3e4f11a24d2b1a948&t=618a45a3)



![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/05/2295c123cdb63ece9b72f15b1b93e5fc12a7417f.png?sign=6d3e6fdcb0c34d77a4a12f57afb61fb2&t=60af80a6)



![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2022/01/fceb3885059891edd94906524a4a88cc2d6d0d4d.png?sign=921bf01b9054a0d98812b93bc6d15060&t=61e0d812)



## 5、项目初始化

a. 首先找个位置创建一下react项目，命令如下：

~~~shell
npx create-react-app backend
~~~

b. 创建好项目后，**进入项目目录**先安装常规要使用的三方包，命令如下：

- 注意: 在项目中引入第三方包时,有些包是开发环境的依赖, 安装方式 npm i 包名 --save-dev  或者 npm i 包名-D
- 注意: 在项目中引入第三方包时,有些包是生产环境的依赖, 安装方式 npm i 包名 或者 npm i 包名 --save

~~~shell
npm i -S axios redux react-redux  styled-components react-router-dom@5.3.3 
// 默认生产环境依赖包安装,上线时,会打包这些文件到生产环境包
# axios：网络请求库
# redux：状态管理
# react-redux：redux功能增强的包
# styled-components：css-in-js热门库
# react-router-dom：路由包

~~~

~~~shell
npm i -D customize-cra react-app-rewired http-proxy-middleware
// 开发环境安装的依赖,上线打包的时候,不会打包这些文件的
# customize-cra react-app-rewired：默认情况下，我们是没有对于react项目的配置权的。使用了react-app-rewired之后，我们就可以获取到react项目中对于webpack的配置权。
# http-proxy-middleware：代理中间件，在vue中默认写好代理配置即可，在react中需要先安装三方的包，才能写代理配置。
~~~

c. 清理创建好的项目中不需要的文件及文件夹

> - 删除`public`目录下的不需要的内容
> - 删除`src`目录下的不需要的内容

d. 给项目引入路由模式 `BrowserRouter`  历史模式路由

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

e.  在**当前项目根目录下**面创建一个名称为`config-overrides.js`文件，对webpack进行配置（该配置方式不是对react内置配置进行直接修改，而是通过三方的包实现配置覆盖）

~~~js
// 配置信息可以参考：https://www.npmjs.com/package/customize-cra
// 修改react脚手架中的默认的webpack的配置, 在该文件中 添加额外的配置,覆盖掉node_nodules中webpack的默认配置,达到修改项目webpack的目的
const {
    override,
    addDecoratorsLegacy,
    disableEsLint,
    addBundleVisualizer,
    addWebpackAlias,
    adjustWorkbox,
} = require("customize-cra");
const path = require("path");\

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

f. 修改`package.json`中的脚本命令为如下：

如上修改完webpack配置,需要修改package.json中的一些脚本命令配置,这样如上修改的配置才能生效. 将react-scripts 替换成 react-app-rewired

> 参考来源：https://www.npmjs.com/package/react-app-rewired

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2020/11/d83ef02592b20151676a3161480e71ed60386109.png?sign=367d15c590345effbd1fcaa0aff6af91&t=5fa2e5f7)

g. **在`src`目录下创建一个名称为`setupProxy.js`文件**，提前为后续接口设置反向代理（**如果需要的话**）

> 与vue一样，代理操作仅限于本地开发环境，上线就失效了。

~~~javascript
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app) {
    app.use(
        createProxyMiddleware('/api', { 
            target: 'https://api.i-lynn.cn/college',
            changeOrigin: true,
            pathRewrite: { '^/api': '' }
        })
    )
}
~~~

i. 建立src/下相关的目录，划分好目录结构（模块化，可以暂时不去建立）

- assets：存放静态资源的目录，后续可以放图片、css、js等文件
- components：封装组件存储的位置
- config：存放配置文件的目录
- hoc：存放高阶组件的文件
- router：存放路由文件
- services：存放封装一些文件（比如，axios封装）
- store：redux相关的目录
- views：视图组件存放目录



完毕后，初始化仓库，同步本地与线上仓库。



## 6、远程仓库的创建与同步

远程仓库的选择：

- github
- gitlab
- gitee

①在登录gitee后，在右上角选择新建仓库

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/11/ba787b8570a96ba7dbec1234a4f9c729f6b319fa.png?sign=640372c42e0242cd4fe5ffdf2bca6b77&t=618b84f6)





②在新建仓库界面根据需要填写或选择一些信息，以下为例

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/11/4d7d2ade6f515f916a66f8b38ce29f16484aed46.png?sign=f10027c9a9a1c00f9763fa73435dd0f2&t=618b85c1)

③将操作选择到SSH上，后续需要基于SSH进行对Gitee操作

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/11/a4b74634a3a6901ec42dac2ed5e0f1d7a4fc5ad2.png?sign=998ac99b5425484dd5a55dabb8d6ee75&t=618b86cf)

④默认情况下，无论Windows还是Mac，都不支持使用SSH免密登录，需要产生免密登录的**公私钥**，然后再在gitee上进行配置后才可以使用免密鉴权（其它远程仓库平台操作一致）

> 公私玥，也就是公钥+私钥，是非对称加密算法的密钥体现。
>
> 密码学中主要有两种加密算法类型：对称加密、非对称加密。

a. 生成公私钥对

~~~shell
ssh-keygen
~~~

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/11/f239d74a2277e58d84a1328dbe136e41668fbf5f.png?sign=ddbbf950cb0c2f9188504fef4f9c5010&t=618b8a2c)

> Mac系统的公私玥在以下路径下：~/.ssh/    【~，当前用户的家目录】

b. 进入gitee的个人设置页面，将生成的公钥输入到远程仓库平台

地址如下：https://gitee.com/profile/account_information

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2021/11/a932c423e0202ae7340953a5e5b3477268b0f203.png?sign=07d7c5c3c97420219d402dd6384513e6&t=618b8bd9)

至此，免密操作已经配置完成！

c. 企业开发正常不会在master分支下开发（该分支是稳定代码的分支），开发的时候一般会有开发分支，例如开发可以在dev分支下开发（后续多半为模块名作为分支或姓名作为分支名），开发好后确认没有问题再往master分支合并

~~~shell
git branch dev
git checkout dev
git push -u origin dev
~~~

建议：

- git操作的相关命令请提高熟练度
- 熟悉git的可视化工具，下载地址：https://tortoisegit.org/download/



## 7、antd组件库

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

在**项目入口文件index.js 中**引入antd的样式文件：

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



## 2、组件封装（重点）

目前已知需要封装俩个组件：

- loading组件
  - 封装的目的：antd自带的Spin组件虽然可以实现加载中，但是其位置不居中，在显示上很不友好，为了让路由懒加载能够有很好的用户体验，建议封装Loading组件
- 验证码组件
  - 验证码在项目中一般会重复的使用，为了降低代码重复率，建议封装

封装的组件都放在components目录下：

- src/components/Loading.jsx

~~~jsx
/**
 * 组件：Loading.jsx
 * 作用：封装用于复用的加载中提示组件
 * 注意点：
 *      对于封装的组件要注意其良好的通用性及可以执行。
 */
import React, { Component } from 'react';
import { Spin } from "antd"
import styled from 'styled-components';
class Loading extends Component {
    static defaultProps = {
        tip: "加载中...",
        spinning: true
    }
    render() {
        return (
            <Main>
                <Spin size="large" spinning={this.props.spinning} tip={this.props.tip} />
            </Main>
        );
    }
}

// 编写样式，让其居中，在设置高度100%的时候需要注意也同时设置其父级元素高度也为100%
const Main = styled.div`
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
`
export default Loading;
~~~

- src/components/Captcha.jsx

~~~jsx
/**
 * 组件：Captcha
 * 作用：产生图形验证码
 * 验证码的工作流程：
 *      a. 后端生成验证码（有诸多格式：图片、url、base64等），随后将验证码验证结果存入到缓存中
 *      b. 客户端发起请求，能够取到验证码图片及一个与验证码验证结果相关联的key值
 *      c. 图片需要页面上展示，key值需要存储在前端中（最合理的是存储在组件state中）
 *      d. 用户输入验证码
 *      e. 将用户输入的验证码值与key值一起发送给服务器
 *      f. 服务器将验证码的值与key值与缓存中的数据进行比较，一致则返回成功，否则失败
 *      g. 如果成功，则前端允许用户继续下一步操作；否则前端提示用户并且刷新验证码
 * 
 * 细节：
 *      细节1：点击刷新问题
 *      细节2：key存在哪
 *              可能存在部分场景，在同一个组件显示中两次及以上次数需要使用Captcha，容易导致后加载的验证码的key值覆盖之前的验证码的key值,可以考虑将key存到各自使用Captcha的父组件中,因为父组件肯定是不相同的.
 *      细节3：验证码如何做到场景通用（尺寸）
 *      
 * 
 * 切记 父需要传递一个方法
 */

import React,{useState,useEffect,useImperativeHandle,forwardRef} from 'react'
import axios from '../service/index'
export default forwardRef(function Captcha(props,ref) {
  
    let [captcha,setCaptcha]=useState("")//用户输入的用于核对的验证码
    let [img,setImg]=useState("") //图片验证码
    const getCode=()=>{
        axios.get("/captcha/api/math").then((res)=>{
            props.setKey(res.data.key);  //子组件调用父组件的方法，目的是让密码登录组件拿到key 
            setImg(res.data.img);
        })
    }
    useEffect(()=>{
        //请求图片验证码
       getCode()
    },[])
    useImperativeHandle(ref,()=>{
        return {
            getCode  //暴露这个方法给父组件调用，目的是，父组件登录失败的时候，调用这个方法可以重新生成验证码
        }
    })
  return (

        <img src={img} width="130" height="30" onClick={getCode} />

  )
})


~~~



## 3、登录页布局

编辑组件：src/views/login/Index.jsx

ant组件：https://ant.design/components/tabs-cn/

设计思路：使用tab选项卡的方式实现`常规登录`和`短信登录`的切换

组件：

- Card组件：https://ant.design/components/card-cn/
- Tabs组件：https://ant.design/components/tabs-cn/

~~~jsx
import React from 'react'

import { Tabs } from 'antd';
import NormalLogin from '../../components/PasswordLogin';
import "./login.css"
import MobileLogin from '../../components/Mobile';
export default function Login() {
  const onChange = (key) => {
    console.log(key);
  };
  const items = [
    {
      key: '1',
      label: `密码登录`,
      children: <NormalLogin></NormalLogin>
    },
    {
      key: '2',
      label: `短信登录`,
      children: <MobileLogin></MobileLogin>,
    },
    
  ];
  return (
    <div className='login'>
      <Tabs defaultActiveKey="1" items={items} onChange={onChange} />
    </div>
  )
}

~~~



## 4、路由规划

路由文件：router/index.js

> 在React中的路由懒加载：
>
> - 需要导入react包中的俩个成员
>   - lazy，它是一个方法，负责去import对应的组件的
>   - Suspense：它是一个组件，负责去应用组件以及可以制定懒加载时需要的提示组件
>
> - 注意: 使用路由规则前,先去项目入口文件index.js 文件中定义路由模式,(BrowserRouter or HashRouter )
>
>   需要包裹根组件App.jsx

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



## 5、功能实现

### 5.1、常规登录

步骤：

- 排版出用户登录的表单
- 用户输入并且提交后端验证
  - url地址封装（首次）
  
    ```js
    const prefix = 'https://reactapi.iynn.cn/'; // 公共域名地址
    const url = {
        // 用户名密码登录接口
        pwdlogin: prefix + 'api/common/auth/login',
    	...
    }
    export default url
    ```
  
  - axios的封装（首次）
  
    ```js
    import axios from 'axios'
    // axios 设置请求拦截器和响应拦截器
    axios.interceptors.request.use(config => {
        // 在请求拦截器中,我们一般可以处理参数,请求头信息,一般用来设置请求头token
        const Authorization = localStorage.getItem('jwt');
        if (Authorization) {
            config.headers.Authorization = Authorization
            // config.headers['Authorization'] = Authorization
            // config.headers.common['Authorization'] = Authorization
        }
        return config
    })
    
    axios.interceptors.response.use(res => {
    // 一般用于处理响应数据,使用场景可以用来存储token,有时后台的接口设计登录成功后返回的token是有时
    //假如 2小时token有效, 后续接口中,只要调用任意一个接口,都会重新返回token,这样事件就顺延2小时,,所有每次token 都需要存
         if (res.data.context && res.data.context.jwt) {
            localStorage.setItem('jwt', res.data.context.jwt)
        }
    
        // 用来存储用户菜单(根据用户角色返回的首页左侧菜单)
        if (res.data.context && res.data.context.acl) {
            localStorage.setItem('acl', JSON.stringify(res.data.context.acl))
        }
        return res
    })
    
    export default axios
    ```
  
  - 发送请求

组件：

- Form组件：https://ant.design/components/form-cn/

- Icon组件：https://ant.design/components/icon-cn/
- Space组件：https://ant.design/components/space-cn/



~~~jsx
import { LockOutlined, UserOutlined,KeyOutlined } from '@ant-design/icons';
import { Button,  Form, Input,Space,message } from 'antd';
import Captcha from '../../components/Captcha';
import {useState,useRef} from 'react'
import { useHistory } from 'react-router-dom/cjs/react-router-dom.min';
import axios from '@/services/index'  //封装后的axios
const App = () => {
  let [key,setKey]=useState("")   //登录时候需要的key值   setKey 要被子组件调用，把key值传过来
  const history=useHistory();
  const captchaRef=useRef() //标识Captcha组件
  const onFinish = (values) => {
    values.key=key;
    console.log('Received values of form: ', values);
    axios.post("/common/auth/login",values).then((res)=>{
        if(res.data.errNo===0){
           
          message.success(res.data.message,1,()=>{
            history.push("/dashboard")//登录成功后跳转到首页
          })
        }
        else{
          message.error(res.data.errText,1)
            //登录失败重新获取验证码   获取验证的方法在子组件里 方法getImg
            captchaRef.current.getImg();  //父组件调用子组件的方法
        }
       
    })
  };
  return (
    <Form
      name="normal_login"
      className="login-form"
    
      onFinish={onFinish}
    >
      <Form.Item
        name="username"
        rules={[
          {
            required: true,
            message: 'Please input your Username!',
          },
        ]}
      >
        <Input prefix={<UserOutlined className="site-form-item-icon" />} placeholder="Username" />
      </Form.Item>
      <Form.Item
        name="password"
        rules={[
          {
            required: true,
            message: 'Please input your Password!',
          },
        ]}
      >
        <Input
          prefix={<LockOutlined className="site-form-item-icon" />}
          type="password"
          placeholder="Password"
        />
      </Form.Item>
      <Form.Item
        name="captcha"
        rules={[
          {
            required: true,
            message: 'Please input your captcha!',
          },
        ]}
      >
        {/* 间距组件，可以把组件放置一行，并拉开适当的间距 */}
        <Space>
        <Input prefix={<KeyOutlined />} placeholder="验证码" />
        <Captcha setKey={setKey} ref={captchaRef}></Captcha>
        </Space>
      </Form.Item>

      <Form.Item>
        <Button type="primary" htmlType="submit" className="login-form-button">
          登录
        </Button>
    
      </Form.Item>
    </Form>
  );
};
export default App;
~~~



### 5.2、短信登录

步骤：

- 页面的布局
- 倒计时
- 短信发送
- 获取表单信息提交去验证登录

~~~jsx
import { LockOutlined, UserOutlined, KeyOutlined } from '@ant-design/icons';
import { Button, Form, Input, Space, message } from 'antd';
import Captcha from '../../components/Captcha';
import { useState, useRef } from 'react'
import { useHistory } from 'react-router-dom/cjs/react-router-dom.min';
import axios from '@/services/index'  //封装后的axios
const App = (props) => {
  let [key, setKey] = useState("")   //登录时候需要的key值   setKey 要被子组件调用，把key值传过来
  let [time,setTime]=useState(5); //倒计时的时间
  let  [requestId,setRequestId]=useState("");  //存储requestId 用于手机短信登录
  let [disabled,setDisabled]=useState(false); //发送验证码按钮是否可用 false是可用 true是不可用状态
  const history = useHistory();
  const formRef = useRef(); //标识表单
  const captchaRef = useRef() //标识Captcha组件
  const onFinish = (values) => {
    values.key = key;
    console.log('Received values of form: ', values);
      //axios post  /api/common/auth/mobile  requestid mobile code 
      if(values.code==="2303" && values.mobile==="15677888888" && requestId===1234 ){
        message.success("登录成功",1,()=>{
          history.push("/dashboard")//登录成功后跳转到首页
        })
      }
  };


  const send = () => {
    //1表单验证 
    //  formRef.current.validateFields(['mobile','captcha']).then((res)=>{
    //   //只要这个then回调执行，就说明发送验证码前已经输入了手机号验证码了
    //     //2图形验证码验证
    //     axios.post("/common/captcha/verify",{key,captcha:res.captcha}).then((res1)=>{
    //       if(res1.data.errNo===0){
    //          //3短信验证
    //         axios.post("/common/sms/send",{token:res1.data.context.token,mobile:res.mobile,type:0}).then((res2)=>{
    //           message.info("短信下发成功")
    //         })
    //       }
    //       else{
    //         captchaRef.current.getImg();  //验证码失败刷新验证码
    //         message.error(res1.data.errText,1)
    //       }
    //     })


    //  }).catch((error)=>{
    //      console.log("验证失败了,有可能手机号或密码忘输入了",error)
    //  })
    let result = {}  //存储表单验证的结果
    formRef.current.validateFields(['mobile', 'captcha'])
      .then((res) => {  //表单验证
        result = res;
        return axios.post("/common/captcha/verify", { key, captcha: res.captcha }) //验证码验证
      }).then((res1) => {
        if (res1.data.errNo === 0) {
          return axios.post("/common/sms/send", { token: res1.data.context.token, mobile: result.mobile, type: 0 })
        }
        else {
          captchaRef.current.getImg();
          message.error(res1.data.errText, 1)
          return Promise.reject("error")
        }
      }).then(()=>{
          message.info("短信下发成功")
          setRequestId(1234) //因为手机短信接口不能用，模拟的一个requestid
          console.log("send");
         setDisabled(true)  //按钮变为不可用
        // 倒计时
         cutDown();
      }).catch((error)=>{
        console.log(error)
      })
  }
  let timer =null;  //倒计时的计数器
  const cutDown=()=>{ //倒计时 
      if(time===0){
         setTime(5);  //时间设置为5秒
         setDisabled(false); //按钮可以用
         clearTimeout(timer);
      }
      else{
        time--;
        setTime(time);
        timer=setTimeout(()=>{
            cutDown();
        },1000)
      }
  }
  return (
    <Form
      ref={formRef}
      name="mobile_login"
      className="login-form"
      onFinish={onFinish}
    >
      <Form.Item
        name="mobile"
        rules={[
          {
            required: true,
            pattern: /^1[3-9]\d{9}$/,
            message: '请输入你的手机号!',
          },
        ]}
      >
        <Input prefix={<UserOutlined className="site-form-item-icon" />} placeholder="手机号" />
      </Form.Item>

      <Form.Item
        name="captcha"
        rules={[
          {
            required: true,
            message: 'Please input your captcha!',
          },
        ]}
      >
        {/* 间距组件，可以把组件放置一行，并拉开适当的间距 */}
        <Space>
          <Input prefix={<KeyOutlined />} placeholder="验证码" />
          <Captcha setKey={setKey} ref={captchaRef}></Captcha>
        </Space>
      </Form.Item>
      <Form.Item
        name="code"
        rules={[
          {
            required: true,
            message: '请输入手机验证码!',
          },
        ]}
      >
        {/* 间距组件，可以把组件放置一行，并拉开适当的间距 */}
        <Space>
          <Input prefix={<KeyOutlined />} placeholder="验证码" />
          <Button disabled={disabled} onClick={send}>{time===5?'发送验证码':time+'秒之后再发送'}</Button>
        </Space>
      </Form.Item>
      <Form.Item>
        <Button type="primary" htmlType="submit" className="login-form-button">
          登录
        </Button>

      </Form.Item>
    </Form>
  );
};
export default App;
~~~

### 5.3存储token和导航菜单到store

~~~
//这个切片存储用户的token和用户拥有的角色对应的菜单

import  {createSlice} from '@reduxjs/toolkit'

const userSlice=createSlice({
    name:'user',
    initialState:{  // 初始状态
        token:'',  //令牌
        menus:[]   //权限菜单
    },
    reducers:{
        saveToken(state,{payload}){  //保存令牌
            state.token=payload
        },
        saveMenu(state,{payload}){  //保存导航菜单
            state.menus=payload
        }
    }

})

export const {saveToken,saveMenu} = userSlice.actions;  //导出方法给组件使用
export default  userSlice.reducer;
~~~

建立仓库

~~~
import {configureStore} from '@reduxjs/toolkit'
import userReducer from './userSlice'  //拿到userSlice的reducer
const store = configureStore({  //创建一个仓库
    reducer:{
        user:userReducer
    }
})

export default store;
~~~

提供给所有组件

~~~
import {Provider} from 'react-redux'
import store from './store/index'

  <Provider store={store}>
  <Router>
    <App />
  </Router>
  </Provider>
 
~~~

NormalLogin.jsx

~~~
 axios.post("/common/auth/login",values).then((res)=>{
        if(res.data.errNo===0){
           //存储token和menu到store里
            dispatch(saveToken(res.data.context.jwt))
            dispatch(saveMenu(res.data.context.acl))
           ....
        }
 }
~~~

仓库数据持久化

~~~
npm  i redux-persist -S
import { persistStore, persistReducer } from "redux-persist";
import storage from "redux-persist/lib/storage"; 

const persistConfig = {  //持久化的配置对象
    //数据存储
    key: "persist",               //自定义
    storage
}

const reducers = combineReducers({  //所有的reducer合并为一个
    user:userReducer
  });
  //持久化reducer
  const persistedReducer = persistReducer(persistConfig, reducers);

  
const store = configureStore({  //创建一个仓库
    reducer:persistReducer,
     middleware:getDefaultMiddleware => getDefaultMiddleware({  //解决动作无法序列化的问题
        //关闭redux序列化检测
        serializableCheck:false
    })
})
//持久化store
export const persistor = persistStore(store);

export default store;


~~~

index.js

~~~
import store, { persistor } from './store/index'
import { PersistGate } from 'redux-persist/integration/react'
<Provider store={store}>
    <PersistGate loading={null} persistor={persistor}>
    <Router>
      <App />
    </Router>
  </PersistGate>
~~~



# 三、后台开发

## 1、后台首页

### 1.1、组件及路由规则创建

- Dashboard组件：负责展示后台的基本结构（布局组件）由其决定后台的基本架子
- 内容组件：
  - 欢迎页
  - 用户列表
  - 用户添加
  - 用户编辑
  - .....

> 在后台项目中，路由100%会用到相同开始的情况，例如：
>
> - /dashboard/welcome
> - /dashboard/users/index
> - /dashboard/users/add
> - ...
>
> 因此这里会用到嵌套路由。

HomeRoutes.jsx

~~~
import React,{Suspense,lazy} from 'react'
import {Route,Switch,Redirect} from 'react-router-dom'
import Loding from '../../components/Loding';

const Welcome=lazy(()=>import('../welcome/Welcome'));
const UserList=lazy(()=>import("../user/UserList"));
const FilmList=lazy(()=>import('../film/FilmList'));
const Cinema=lazy(()=>import('../cinema/Cinema'))
export default function HomeRoutes() {
  return (
    <div>
        <Suspense fallback={<Loding />}>
        <Switch>
            <Route path="/dashboard/welcome" component={Welcome}></Route>
            <Route path='/dashboard/user/index' component={UserList} />
            <Route path='/dashboard/film/index' component={FilmList} />
            <Route path='/dashboard/cinema/index' component={Cinema} />
        </Switch>
      </Suspense>
    </div>
  )
}
~~~

### 1.2、后台首页布局

~~~jsx
import React,{useEffect} from 'react'
import {
  MenuUnfoldOutlined,
  MenuFoldOutlined,
  UploadOutlined,
  UserOutlined,
  VideoCameraOutlined,
} from '@ant-design/icons';
import {useLocation} from 'react-router-dom'
import "./layout.css"
import { Button, Layout, Menu, theme } from 'antd';
import { useState } from 'react';
import {Link} from 'react-router-dom'
import Routes from '../ruotes/Routes';
const { Header, Sider, Content } = Layout;

const Home = () => {
  const [collapsed, setCollapsed] = useState(false);
  const {
    token: { colorBgContainer },
  } = theme.useToken();
  let loc=useLocation();

  return (
    <Layout>
      <Sider trigger={null} collapsible collapsed={collapsed}>
        <div className="demo-logo-vertical" />
        <Menu
          theme="dark"
          mode="inline"
          defaultSelectedKeys={[loc.pathname]}
          items={[
            {
              key: '1',
              icon: <UserOutlined />,
              label: <Link to="/dashboard/welcome">首页</Link>,
            },
            {
              key: '2',
              icon: <VideoCameraOutlined />,
              label: '用户管理',
              children:[
                {
                  key:'21',
                  label:<Link to="/dashboard/user/index">用户列表</Link>
                }
              ]
            },
            {
              key: '3',
              icon: <VideoCameraOutlined />,
              label: '电影管理',
              children:[
                {
                  key:'31',
                  label:<Link to="/dashboard/film/index">电影列表</Link>
                }
              ]
            },
            {
              key: '4',
              icon: <VideoCameraOutlined />,
              label: '影院管理',
              children:[
                {sho
                  key:'41',
                  label:<Link to="/dashboard/cinema/index">影院列表</Link>
                }
              ]
            },
            {
              key: '5',
              icon: <VideoCameraOutlined />,
              label: '院校管理',
              children:[
                {
                  key:'51',
                  label:<Link to="/dashboard/college/index">院校列表</Link>
                }
              ]
            }
          ]}
        />
      </Sider>
      <Layout>
        <Header
          style={{
            padding: 0,
            background: colorBgContainer,
          }}
        >
          <Button
            type="text"
            icon={collapsed ? <MenuUnfoldOutlined /> : <MenuFoldOutlined />}
            onClick={() => setCollapsed(!collapsed)}
            style={{
              fontSize: '16px',
              width: 64,
              height: 64,
            }}
          />
        </Header>
        <Content
          style={{
            margin: '24px 16px',
            padding: 24,
            minHeight: 280,
            background: colorBgContainer,
          }}
        >
          <Routes></Routes>
        </Content>
      </Layout>
    </Layout>
  );
};
export default Home;
~~~

此时应该得到如下效果：

![](https://storage.lynnn.cn/assets/markdown/91147/pictures/2022/01/0d66622a9be1861bad156d3666d7cccd799d2f85.png?sign=6eb477cb2d202bc886b47892979fcbfb&t=61e520dc)



### 1.3、顶部信息展示

步骤：

- 如何统一提供jwt
- 发送ajax，获取数据并且展示

a. 获取数据

~~~js
 const [accountInfo,setAccountInfo ]=useState({}); //管理员的信息  
useEffect(()=>{  //获取管理员的信息
      axios.get("/common/auth/adminInfo").then((res)=>{
          if(res.data.errNo===0){
              setAccountInfo(res.data.accountInfo)
          }
      })
  },[])
~~~

b. 展示数据

~~~jsx
<span>{accountInfo.username+"/"+accountInfo.role}</span>
~~~



### 1.4、注销登录功能

步骤：

- 清除jwt
- 跳转到登录页面

~~~js
const handleOk=()=>{ //退出系统的时候要执行的函数
    localStorage.removeItem("token")
    localStorage.removeItem("acl")
    dispatch(saveToken(""))
    dispatch(saveMenu([]));
    //清除token acl 复位store里的数据 跳到登录页面  关闭对话框 跳转到登录页面
    setIsModalOpen(false) //关闭对话框
    props.history.push("/login")
  }
~~~



### 1.5、动态菜单生成

1. 不同的用户,对应不同的角色(role_id) ; 而不同的角色对应有不同的操作菜单. 将对应的菜单展示在左侧菜单栏

```jsx
  const mapMenus=navMenus.map((item)=>{
    //判断是否有子菜单
    if(item.children.length>0){  // 有子菜单
          return getItem(item.auth_path,item.auth_name,null,
              item.children.map((subItem)=>{
                if(subItem.is_nav===1){
                  return getItem(subItem.auth_path,<Link to={subItem.auth_path}>{subItem.auth_name}</Link>)
                }
                 
              })
            )
    }
    else{  //没有子菜单项目
        return getItem(item.auth_path,<Link to={item.auth_path}>{item.auth_name}</Link>)
    }
 })
  setItems(mapMenus)

```



### 1.6、防止翻墙

 01: 当用户未登录访问登录后的页面的时候,让其跳转到登录页

 02:  因为dashboard为首页的基础组件,所有直接在dashboard组件中进行判断就可以啦.

```jsx
  axios.get("/common/auth/jwtPreCheck").then((res)=>{
       if(res.data.errNo===0){  //预检通过，说明用户是通过登录进入的这个页面
            setItems(mapMenus)
          axios.get("/common/auth/adminInfo").then((res)=>{
              if(res.data.errNo===0){
                  setAccountInfo(res.data.accountInfo)
              }
          })
       }
       else{  //用户没有通过登录进入的这个页面
          props.history.push("/login")
       }
    })
```

03: 在dashboard组件中,进行渲染判断,根据登录状态 islogined 判断是否为登录状态, 登录则渲染该组件,否则不渲染该组件 (渲染空组件)

```jsx
    const [isLogin,setIsLogin]=useState(false); //设置登录状态，false表示没有登录 true登录成功了
axios.get("/common/auth/jwtPreCheck").then((res)=>{
       if(res.data.errNo===0){  //预检通过，说明用户是通过登录进入的这个页面
            setItems(mapMenus)  //设置导航菜单
            setIsLogin(true)  //设置登录状态为登录完成
          axios.get("/common/auth/adminInfo").then((res)=>{  //登录成功后获取管理员的信息
              if(res.data.errNo===0){
                  setAccountInfo(res.data.accountInfo)
              }
          })
       }
       else{  //用户没有通过登录进入的这个页面
          props.history.push("/login")
       }
    })
   {isLogin && <Layout></Layout>}
```

## 2、首页图表展示页面

### 2.1获取统计数据

~~~
import React,{useEffect,useState} from 'react'
import axios from '@/services/index'
import Pie from '../../../components/Pie'
export default function ShowEcharts() {
  const [gender,setGender]=useState([])   //统计数据，按性别分类的数组
  const [area,setArea]=useState([])   //统计数据，按地区分类的数组
  const [accessFrom,setAccessFrom]=useState([])  //统计数据，按访问来源分类的数组
  useEffect(()=>{
    axios.get("/users/statistics/getData").then((res)=>{  //获取数据对三个进行赋值
       setGender(res.data.data.gender)
       setArea(res.data.data.area)
       setAccessFrom(res.data.data.accessFrom)
    })
  },[])
  return (
    <div style={{display:"flex"}}>
      {
        area.length>0 && <Pie id="area" data={area} title="千锋学生分布"></Pie>
      }
      {
        gender.length>0 && <Pie id="gender" data={gender} title="千锋男女比例"></Pie>
      }
      {
        accessFrom.length>0 && <Pie id="accessfrom" data={accessFrom} title="千锋学生来源"></Pie>
      }
    </div>
  )
}

~~~

### 2.2绘制饼形图

~~~
import React,{useEffect} from 'react'
import * as echarts from 'echarts'
export default function Pie(props) {
    console.log(props.data)
    useEffect(()=>{
        var myChart = echarts.init(document.getElementById(props.id));
        let options ={
            title: {
                text: props.title,
                subtext: "",
                x: "center"
            },
            tooltip: {
                trigger: "item",
                formatter: "{a} <br/>{b} : {c} ({d}%)"
            },
            legend: {
                orient: "vertical",
                x: "left",
                data:[]
            },
           
            series: [
                {
                    name: "访问来源",
                    type: "pie",
                    radius: "55%",
                    center: ["50%", "60%"],
                    data: props.data.map(item=>({name:item.name,value:item.value}))
                }
            ]
        }
     
          // 使用刚指定的配置项和数据显示图表。
          myChart.setOption(options);
  
    },[])
  return (
    <div id={props.id} style={{width:'100%',height:'300px'}}>Pie</div>
  )
}

~~~



## 3、用户模块

###  3.1用户列表

~~~
import React, { useEffect, useState,useRef } from 'react'
import axios from '@/services/index'
import { Table, Tag,Space,Button,Pagination } from 'antd'
import "./user.css"
import AddEdit from '../../components/AddEdit'
export default function UserList() {
  let [userList, setUserList] = useState([]) // 存储的是用户的列表
  let [page, setPage] = useState(1) //当前页面
  let [total, setTotal] = useState(0) //总的记录个数
  const [visible,setVisible]=useState(false) //设置对话框不可见
  const [title,setTitle]=useState("") //设置对话框的标题
  const editRef=useRef() ;//标识添加和编辑的对话框
  useEffect(() => {
    getList();
  }, [])
  const getList = (page = 1) => {  //获取用户的列表   后端分页
    axios.get("/users?page=" + page).then((res) => {
      setUserList(res.data.paginate.data)
      setPage(res.data.paginate.current_page)
      setTotal(res.data.paginate.total)
    })
  }
  const columns = [{ //表格组件的列
    title: "序号",
    dataIndex: 'id'

  },
  {
    title: "用户名",
    dataIndex: "username",
    key: "username"
  },
  {
    title: "手机号",
    dataIndex: "mobile",
    key: "mobile"
  },
  {
    title: "邮箱",
    dataIndex: "email",

  },
  {
    title: "性别",
    //  dataIndex:'gender',
    render(text, record) {  //当前项目的数据  record 当前行的数据
      //  console.log(text,record)  //dataIndex存在时候，text就是当前列的数据 如果dataIndex不存在 ，同record 也是当前行的数据
      if (record.gender === "1") {
        return <Tag color='green'>男</Tag>
      }
      else if (record.gender === "2") {
        return <Tag color='red'>女</Tag>
      }
      else {
        return <Tag color='red'>保密</Tag>
      }
    }
  },
  {
    title: "状态",
    //  dataIndex:'gender',
    render(text, record) {  //当前项目的数据  record 当前行的数据
      //  console.log(text,record)  //dataIndex存在时候，text就是当前列的数据 如果dataIndex不存在 ，同record 也是当前行的数据
      if (record.status === "1") {
        return <Tag color='green'>启用</Tag>
      }
      else {
        return <Tag color='red'>禁用</Tag>
      }

    }
  },
  {
    title: "操作",
    //  dataIndex:'gender',
    render(text, record) {  //当前项目的数据  record 当前行的数据
      //  console.log(text,record)  //dataIndex存在时候，text就是当前列的数据 如果dataIndex不存在 ，同record 也是当前行的数据
        return <Space><Button type='primary' onClick={()=>edit(record)}>编辑</Button>
        <Button type='primary'>删除</Button></Space>
    }
  },

  ]
  const edit=(record)=>{  //弹出编辑框，进入编辑状态
       setVisibleTitle('编辑')
       editRef.current.setUser({...record})  //调用子组件的方法放置编辑的内容
  }
  const setVisibleTitle=(title)=>{ //设置对话框显示隐藏并设置标题
       setTitle(title);
       setVisible(!visible);
  }
  const titleContent=()=>{ //表头的内容
     return <div className="userlist">
              <h3>用户列表</h3>
              <Button type="primary" onClick={()=>setVisibleTitle('添加')}>添加</Button>
     </div>
  }
  return (
    <div>
      <Table title={()=>titleContent()} bordered dataSource={userList} columns={columns} rowKey={(item) => item.id}
       pagination={{defaultCurrent:page,total:total,onChange:getList}} />
      {/* <Pagination defaultCurrent={page} total={total} onChange={getList}  /> */}
      <AddEdit visible={visible} title={title} ref={editRef} setVisibleTitle={setVisibleTitle} 
      getList={getList} page={page} />
    </div>
  )
}

~~~



###  3.2用户添加编辑

~~~~
import React,{useState,forwardRef,useImperativeHandle} from 'react'
import {Input,Radio,Modal} from 'antd'
import axios from '@/services/index'
export default forwardRef(function AddEdit(props,ref) {
   let [user,setUser]=useState({
     username:"",
     password:"",
     gender:'',
     email:"",
     mobile:""
   })
   //父组件调用子组件的setUser方法就可以把当前行的编辑数据传递过来了
   useImperativeHandle(ref,()=>{  //暴露setUser方法给父组件使用
    return {
        setUser
    }
   })
   const input=(e)=>{
      user[e.target.name]=e.target.value;
      setUser({...user})
   }
   const handleOk=()=>{
      //处理添加和修改完成的操作
      if(props.title==="添加"){
        user.mobile=Number(user.mobile)  //转为和提交的类型一致
        user.gender=Number(user.gender)
        //注意添加的时候，手机号，邮箱都不能重复
         axios.post("/users/add",user).then((res)=>{
            props.getList(1)
         })
      }
      else {
         axios.put("/users/"+user.id,user).then((res)=>{
              //修改完数据要刷新列表
              props.getList(props.page)

         })
      }
      close();
   }
   const close=()=>{

    //关闭对话框 //复位数据
    setUser({
        username:"",
        gender:'',
        email:"",
        mobile:""
      })
    //子组件调用父组件的方法关闭对话框
     props.setVisibleTitle("");
   }
  return (
    <Modal title={props.title} open={props.visible} onOk={handleOk} onCancel={close}>
        <div>
          <Input name="username" value={user.username} onChange={input} />
        </div>
        <div>
          <Input name="password" value={user.password} onChange={input} />
        </div>
        <div>
          <Input name="email" value={user.email} onChange={input} />
        </div>
        <div>
           <Radio.Group name="gender" value={user.gender} onChange={input}>
                <Radio value="1">男</Radio>
                <Radio value="2">女</Radio>
                <Radio value="3">保密</Radio>
           </Radio.Group>
        </div>
        <div>
          <Input name="mobile" value={user.mobile} onChange={input} />
        </div>
    </Modal>
  )
})


~~~~



## 4、影院管理

### 4.1影院列表

~~~
    import React, { useEffect, useState } from 'react'
import axios from '@/services/index'
import "./cinema.css"
export default function Cinema() {
  let [points, setPoints] = useState([]) //存储30家影院的地址和坐标信息
  useEffect(() => {
    if (points.length === 0) {  //数组是空的，发请求获取电影院的信息
      axios.get("/cinemas/infos/loca").then((res) => {
        if (res.data.errNo === 0) {
          setPoints(res.data.paginate.slice(0, 30))
        }
      })
    }
    else { //已经获取了电影院的数据
      //百度地图的实例
      var map = new window.BMapGL.Map("container");
      // 创建点坐标 
      points.forEach((item) => {
        let jd = Number(item.gpsaddress.split(",")[0])
        let wd = Number(item.gpsaddress.split(",")[1])
        console.log(jd, wd)
        var point = new window.BMapGL.Point(jd, wd);  // 创建点坐标 
        map.centerAndZoom(point, 12);    // 初始化地图，设置中心点坐标和地图级别
        var marker = new window.BMapGL.Marker(point);        // 创建标注   
        //建议一个信息窗口
        var opts = {
          width: 250,     // 信息窗口宽度
          height: 50,    // 信息窗口高度
          title: "当前位置:"  // 信息窗口标题
      }
      var infoWindow = new window.BMapGL.InfoWindow(item.address, opts);  // 创建信息窗口对象
        marker.addEventListener("click",()=>{  //点击marker进行事件响应
          //打开信息窗口
          map.openInfoWindow(infoWindow, point); 
        })
        map.addOverlay(marker);                     // 将标注添加到地图中
      })
      map.enableScrollWheelZoom(true) //开启鼠标滚轮缩放
      // map.setHeading(64.5);   //设置地图旋转角度
      // map.setTilt(73);       //设置地图的倾斜角度

    }

  }, [points])
  return (
    <div id="container" className='container'>Cinema {points.length}</div>
  )
}


~~~

