# react-mobile

# 1.项目介绍

实现react移动端项目

# 2.目标：
* 能够应用CRA+React+Mobx+Antd-mobile开发C端项目
* 掌握基于React的C端项目开发流程
* 学会如何应用next优化项目

# 3.使用技术栈

* 脚手架：cra
  * dva-cli
  * umi
* 脚本：ts

* react版本：react v18 2022年更新
  * react 17 
* 路由：react-router v6   2021年10-11月
  * react-router v5
* 状态管理器：mobx v6
  * redux
  * redux + react-redux
  * redux + react-redux + 分模块
  * redux + react-redux + 分模块 + redux-thunk
  * redux + react-redux + 分模块 + redux-saga
  * redux + react-redux + 分模块 + redux-thunk + immutable + redux-immutable
  * redux + react-redux + 分模块 + redux-saga + immutable + redux-immutable
  * rtk
  * mobx v6
* 组件库：antd-mobile v5
  * http://ant-design-mobile.antgroup.com/zh
  * 更像是vant UI库了
* hooks

# 4.构建项目

```sh
$ npx create-react-app react-mobile-app --template typescript
```

## 4.1 是否抽离配置文件

一般企业级项目，很少会直接抽离配置文件

> 抽离配置文件目的：对webpack进行二次封装
>
> 推荐使用 craco 进行覆盖

## 4.2 使用craco覆盖webpack配置

https://www.npmjs.com/package/@craco/craco

```sh
$ cnpm i @craco/craco -D
```

为了支持 commonjs 规范，安装如下模块

```sh
$ cnpm i @types/node -D
```

`@types/*`这种文件称之为 ts 中的声明文件（`ts中的定义的类型的一个整合`）

项目根目录创建 `craco.config.js`，代码如下：

```ts
const path = require('path')
module.exports = {
  webpack: {
    alias: {
      '@': path.resolve(__dirname, 'src')
    }
  }
}
// 否则会报错
export {}
```

为了使 TS 文件引入时的别名路径能够正常解析，需要配置 `tsconifg.json`，在 `compilerOptions`选项里添加 path 等属性。为了防止配置被覆盖，需要单独创建一个文件 `tsconfig.path.json`，添加以下代码

```json
// tsconfig.path.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    "types": [
      "node"
    ]
  }
}
```

在 `tsconifg.json` 引入配置文件：

```json
// /tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "extends": "./tsconfig.path.json", //+++
  "include": [
    "src"
  ]
}
```

修改 `package.json` 如下：

```json
"scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test"
},
```

```sh
$ npm run start
```

## 4.3 确定项目 css 预处理器

https://create-react-app.bootcss.com/docs/adding-a-sass-stylesheet

```sh
$ cnpm i node-sass sass -D
```

cra 默认自带sass支持，只需要安装模块即可自动启动

## 4.4 改造项目目录结构

```
- mobile-react-app
  - src
    - api
    - components
    - router
    - store
    - utils
    - views
    App.tsx
    index.tsx
    logo.svg
    react-app-env.d.ts
    reportWebVitals.ts
```

```tsx
// src/index.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import ErrorBoundary from './ErrorBundary'; // 错误边界
import App from './App';
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);

// 开发环境下，严格模式会触发组件的二次装载，修改状态时尽量不要使用 函数形式
root.render(
  <React.StrictMode>
    <ErrorBoundary>
      <App />
    </ErrorBoundary>
  </React.StrictMode>
);

reportWebVitals();


```

```tsx
// src/App.tsx
import React, { FC } from 'react';

interface IAppProps {
  
};

const App:FC<IAppProps> = () => {
  return (
    <div>
      <h1>hello world</h1>
    </div>
  )
};

export default App;
```

```tsx
// src/ErrorBundary.tsx
import React from 'react'
// 如何给类组件添加类型注解
interface IState {
  hasError: boolean
}
class ErrorBoundary extends React.Component <any, IState> {
// class ErrorBoundary extends React.Component <{ children: any }, {hasError: boolean}> {
  constructor(props: any) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: any) {
    // 更新 state 使下一次渲染可以显示降级 UI
    return { hasError: true };
  }

  componentDidCatch(error: any, info: { componentStack: any; }) {
    // "组件堆栈" 例子:
    //   in ComponentThatThrows (created by App)
    //   in ErrorBoundary (created by App)
    //   in div (created by App)
    //   in App
    console.log(info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      // 你可以渲染任何自定义的降级 UI
      return <h1>代码出错了，请自信检查一下</h1>;
    }

    return this.props.children; 
  }
}

export default ErrorBoundary
```

# 5 构建项目基本结构

```scss
// src/main.scss
* {
  padding: 0;
  margin: 0;
  list-style: none;
  text-decoration: none;
}

html, body, #root, .container {
  height: 100%;
}

html {
  // font-size: 100px; // 1rem = 100px
  // iphone4  100 / 320 * 100  31.25vw
  font-size: 26.666666667vw; // iphone6为设计稿时 1rem = 100px  100 / 375 * 100
}

body {
  font-size: 12px; // 根据设计稿的最小的字体大小为基准
}
@media only screen and (orientation: landscape) { // 横屏
  html {
    font-size: 100px;
  }
}
```

```scss
// src/App.scss
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  .header {
    height: 0.44rem;
    background-color: #f66;
  }
  .content {
    flex: 1;
    overflow: auto;
  }
  .footer {
    // height: 50px;
    height: 0.5rem;
    background-color: #efefef;
  }
}
.landscape-tip {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, .8);
  color: #fff;
  display: none;
  justify-content: center;
  align-items: center;
}

@media only screen and (orientation: landscape) { // 横屏
  .landscape-tip  {
    display: flex;
  }
}
```



```tsx
// src/App.tsx
import React, { FC } from 'react';
import '@/App.scss';  // ++++++
interface IAppProps {
};

const App:FC<IAppProps> = () => { // ++++++
  return (
    <div className='container'>
      <header className="header">header</header>
      <div className="content">content</div>
      <footer className="footer">footer</footer>
      <div className='landscape-tip'>
        请将屏幕竖向浏览
      </div>
    </div>
  )
};

export default App;
```

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import ErrorBoundary from '@/ErrorBundary'; // 错误边界
import App from '@/App';
import reportWebVitals from '@/reportWebVitals';
import '@/main.scss'; // +++

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);

// 开发环境下，严格模式会触发组件的二次装载，修改状态时尽量不要使用 函数形式
root.render(
  <React.StrictMode>
    <ErrorBoundary>
      <App />
    </ErrorBoundary>
  </React.StrictMode>
);

reportWebVitals();

```

# 6.构建项目基本页面

思考每个页面的头部和内容区域是根据用户的选择而一起改变的，那么可以创建以下四个基本页面

## 6.1 构建首页面

```tsx
// src/views/home/Index.tsx
import React, { FC } from 'react';

interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  return (
    <>
      <header className="header">home header</header>
      <div className="content">home content</div>
    </>
  )
};

export default Home;
```

## 6.2 构建分类页面

```tsx
// src/views/kind/Index.tsx
import React, { FC } from 'react';

interface IKindProps {
  
};

const Kind:FC<IKindProps> = () => {
  return (
    <>
      <header className="header">kind header</header>
      <div className="content">kind content</div>
    </>
  )
};

export default Kind;
```

## 6.3 构建购物车页面

```tsx
// src/views/cart/Index.tsx
import React, { FC } from 'react';

interface ICartProps {
  
};

const Cart:FC<ICartProps> = () => {
  return (
    <>
      <header className="header">cart header</header>
      <div className="content">cart content</div>
    </>
  )
};

export default Cart;
```

## 6.4 构建个人中心页面

```tsx
// src/views/user/Index.tsx
import React, { FC } from 'react';

interface IUserProps {
  
};

const User:FC<IUserProps> = () => {
  return (
    <>
      <header className="header">user header</header>
      <div className="content">user content</div>
    </>
  )
};

export default User;
```

# 6.引入路由

https://reactrouter.com/en/main

```tsx
// src/App.tsx
import React, { FC } from 'react';
// BrowserRouter  /  /kind  /cart /user
// HashRouter  /#/ /#/kind /#/cart /#/user
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import '@/App.scss';
import Cart from '@/views/cart/Index';
import Home from '@/views/home/Index';
import Kind from '@/views/kind/Index';
import User from '@/views/user/Index';
interface IAppProps {
};

const App: FC<IAppProps> = () => {
  return (
    <BrowserRouter>
      <div className='container'>
        <Routes>
          <Route path="/home" element={<Home />} />
          <Route path="/kind" element={<Kind />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/user" element ={<User />} />
        </Routes>
        <footer className="footer">footer</footer>
        <div className='landscape-tip'>
          请将屏幕竖向浏览
        </div>
      </div>

    </BrowserRouter>

  )
};

export default App;

```

> 此时地址栏分别输入
> `http://localhost:3000/home`、`http://localhost:3000/kind`、`http://localhost:3000/cart`、`http://localhost:3000/user`
> 查看项目运行结果，
> 可以得知已经可以通过路由显示不同的页面
> 但是用户一般都是通过底部选项卡来切换页面的

# 7.构建页面底部组件

在`src`文件夹下创建`components`文件夹，在`components`文件夹下创建底部组件

因为底部选项卡需要字体图标，可以选择 [iconfont](https://www.iconfont.cn/)阿里字体图标库，搜索图标，加入购物车，添加至项目`mobile-vue-app`，选择`font-class`,点击`查看在线链接`，拷贝css链接

项目根目录下`public/index.html`中引入css链接

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
   
    <title>React App</title>
    <!--+++++++-->
    <link rel="stylesheet" href="//at.alicdn.com/t/c/font_3665887_2jcst3szxcd.css">
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>

```

底部组件展示如下：

```tsx
// src/components/Footer.tsx
import React, { FC } from 'react';

interface IFooterProps {
  
};

const Footer:FC<IFooterProps> = () => {
  return (
    <ul>
      <li>
        <span className="iconfont icon-shouye"></span>
        <p>首页</p>
      </li>
      <li>
        <span className="iconfont icon-fenlei"></span>
        <p>分类</p>
      </li>
      <li>
        <span className="iconfont icon-gouwuche"></span>
        <p>购物车</p>
      </li>
      <li>
        <span className="iconfont icon-shouye1"></span>
        <p>我的</p>
      </li>
    </ul>
  )
};

export default Footer;
```

```scss
// src/App.scss
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  .header {
    height: 0.44rem;
    background-color: #f66;
  }
  .content {
    flex: 1;
    overflow: auto;
  }
  .footer {
    // height: 50px;
    height: 0.5rem;
    background-color: #efefef;
    user-select: none;

    ul { // ++++++++++++++++++++++++++++++++
      width: 100%;
      height: 100%;
      display: flex;
      li {
        flex: 1;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        span {
          font-size: 0.20rem;
        }

        p {
          font-size: 0.12rem;
        }
      }
    }
  }
}
.landscape-tip {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, .8);
  color: #fff;
  display: none;
  justify-content: center;
  align-items: center;
}

@media only screen and (orientation: landscape) { // 横屏
  .landscape-tip  {
    display: flex;
  }
}
```



# 8.点击页面底部跳转路由

此项选择使用声明式导航跳转

react提供了两个可以使用 声明式导航跳转方式 ： Link   NavLink

如果不需要设置选中的样式，可以使用Link 组件

如果需要设置选中的样式，建议使用NavLink

```tsx
// src/components/Footer.tsx
import React, { FC } from 'react';
import { NavLink } from 'react-router-dom'
interface IFooterProps {
  
};

const Footer:FC<IFooterProps> = () => {
  return (
    <ul>
      <NavLink to="/home" style={({ isActive }) =>
              isActive ? { color: '#f66 '} : undefined
            }>
        <span className="iconfont icon-shouye"></span>
        <p>首页</p>
      </NavLink>
      <NavLink to="/kind" style={({ isActive }) =>
              isActive ? { color: '#f66 '} : undefined
            }>
        <span className="iconfont icon-fenlei"></span>
        <p>分类</p>
      </NavLink>
      <NavLink to="/cart" style={({ isActive }) =>
              isActive ? { color: '#f66 '} : undefined
            }>
        <span className="iconfont icon-gouwuche"></span>
        <p>购物车</p>
      </NavLink>
      <NavLink to="/user" style={({ isActive }) =>
              isActive ? { color: '#f66 '} : undefined
            }>
        <span className="iconfont icon-shouye1"></span>
        <p>我的</p>
      </NavLink>
    </ul>
  )
};

export default Footer;
```

```sccc
// src/App.scss
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  .header {
    height: 0.44rem;
    background-color: #f66;
  }
  .content {
    flex: 1;
    overflow: auto;
  }
  .footer {
    // height: 50px;
    height: 0.5rem;
    background-color: #efefef;
    user-select: none;

    ul { // ++++++++++++++++++++++++++++++++
      width: 100%;
      height: 100%;
      display: flex;
      a { // +++++
        flex: 1;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: #333; // +++++
        span {
          font-size: 0.20rem;
        }

        p {
          font-size: 0.12rem;
        }

      }
    }
  }
}
.landscape-tip {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, .8);
  color: #fff;
  display: none;
  justify-content: center;
  align-items: center;
}

@media only screen and (orientation: landscape) { // 横屏
  .landscape-tip  {
    display: flex;
  }
}
```



> 此时发现 当用户只输入 / 时，也就是路由的重定向需要实现
>
> * 1.编程式
>
>   https://reactrouter.com/en/6.4.2/hooks/use-navigate
>
>   先把路由对象提到入口文件
>
> ```tsx
> // src/index.tsx
> import React from 'react';
> import ReactDOM from 'react-dom/client';
> import ErrorBoundary from '@/ErrorBundary'; // 错误边界
> import App from '@/App';
> import reportWebVitals from '@/reportWebVitals';
> import { BrowserRouter } from 'react-router-dom' // ++++++
> import '@/main.scss';
> 
> const root = ReactDOM.createRoot(
>   document.getElementById('root') as HTMLElement
> );
> 
> // 开发环境下，严格模式会触发组件的二次装载，修改状态时尽量不要使用 函数形式
> root.render(
>   <React.StrictMode>
>     <ErrorBoundary>
>       <BrowserRouter> // ++++++
>         <App />
>       </BrowserRouter>
>     </ErrorBoundary>
>   </React.StrictMode>
> );
> 
> reportWebVitals();
> 
> ```
>
> ```tsx
> // src/App.tsx
> import React, { FC, useEffect } from 'react';
> // BrowserRouter  /  /kind  /cart /user
> // HashRouter  /#/ /#/kind /#/cart /#/user
> import { Routes, Route, useLocation, useNavigate } from 'react-router-dom'
> import '@/App.scss';
> import Cart from '@/views/cart/Index';
> import Home from '@/views/home/Index';
> import Kind from '@/views/kind/Index';
> import User from '@/views/user/Index';
> import Footer from '@/components/Footer'
> interface IAppProps {
> };
> const App: FC<IAppProps> = () => {
>   const { pathname } = useLocation() // 获取请求路径
>   const navigate = useNavigate() // 获取到 路由器对象
> 
>   useEffect(() => {
>     // 获取当前的地址栏的地址
>     console.log(pathname)
>     // navigate 类似于 vue中 this.$router
>     pathname === '/' && navigate('/home', { replace: true })
>   }, [])
>   return (
>     <div className='container'>
>       <Routes>
>         <Route path="/home" element={<Home />} />
>         <Route path="/kind" element={<Kind />} />
>         <Route path="/cart" element={<Cart />} />
>         <Route path="/user" element ={<User />} />
>       </Routes>
>       <footer className="footer"><Footer /></footer>
>       <div className='landscape-tip'>
>         请将屏幕竖向浏览
>       </div>
>     </div>
> 
>   )
> };
> 
> export default App;
> ```
>
> * 2.使用声明式实现重定向 -- 在某条件下使用
>
>   https://reactrouter.com/en/6.4.2/components/navigate
>
>   ```tsx
>   // src/App.tsx
>   import React, { FC } from 'react';
>   // BrowserRouter  /  /kind  /cart /user
>   // HashRouter  /#/ /#/kind /#/cart /#/user
>   import { Routes, Route, Navigate } from 'react-router-dom'
>   import '@/App.scss';
>   import Cart from '@/views/cart/Index';
>   import Home from '@/views/home/Index';
>   import Kind from '@/views/kind/Index';
>   import User from '@/views/user/Index';
>   import Footer from '@/components/Footer'
>   interface IAppProps {
>   };
>
>   const App: FC<IAppProps> = () => {
>
>     return (
>       <div className='container'>
>
>         <Routes>
>           <Route path="/" element={ <Navigate to="/home" replace={true}/>} />
>           <Route path="/home" element={<Home />} />
>           <Route path="/kind" element={<Kind />} />
>           <Route path="/cart" element={<Cart />} />
>           <Route path="/user" element ={<User />} />
>         </Routes>
>         <footer className="footer"><Footer /></footer>
>         <div className='landscape-tip'>
>           请将屏幕竖向浏览
>         </div>
>       </div>
>
>     )
>   };
>
>   export default App;
>
>   ```
>
> * 3.使用嵌套路由 配合index属性
>
>   具体实现后台管理系统项目中实现
>
>   /banner      /banner/add   /banner/list
>
>   /banner ====> /banner/list
>
> * 如果使用的式react-router-domv5版本
>
>   ```tsx
>   import { Redirect } from 'react-router-dom'
>         
>   <Redirect from ="/" to="/home" />
>   ```

# 9.引入UI组件库

http://ant-design-mobile.antgroup.com/zh

react 移动端项目建议使用 Ant Design Mobile

```
$ cnpm i antd-mobile -S
```

直接引入组件即可，antd-mobile 会自动为你加载 css 样式文件

# 10.封装数据请求

在vue/react项目中建议使用 `axios` 作为数据请求的方案

axios官网：http://www.axios-js.com/

```sh
$ cnpm i axios -S
```

```ts
// src/utils/request.ts
// 1.引入axios
import axios from 'axios'

// 2.项目环境
// 生产环境 process.env.NODE_ENV === 'production'  cnpm run build
// 测试环境 ？
// 开发环境 process.env.NODE_ENV === 'devlopment   cnpm run start
const isDev = process.env.NODE_ENV === 'development'

// 3.给axios添加默认选项
// 设置跨域是否需要携带凭证
axios.defaults.withCredentials = false
// axios.defaults.timeout = 6000 // 6秒超时时间
// axios.defaults.baseURL = isDev ? 'http://121.89.205.189:3001/api' : 'http://121.89.205.189:3001/api'

// 4.自定义axios
const ins = axios.create({
  baseURL: isDev ? 'http://121.89.205.189:3001/api' : 'http://121.89.205.189:3001/api',
  timeout: 6000
})


// 5.设置拦截器
// 请求的拦截器 所有的请求在开始之前先执行请求拦截器，再执行自己的请求
ins.interceptors.request.use((config) => {
  // 设置请求的loading显示 --- 使用组件不必要  ----  js模块显示
  // 设置token，一般token传递给后端通过 请求头传递 config.headers.token = ''
  return config
}, (err) => {
  return Promise.reject(err)
})

// 响应拦截器 所有的接口返回值先执行响应拦截器，再返回自己的响应的数据
ins.interceptors.response.use((response) => {
  // 关闭loading动画  --- 使用组件不必要 ----  js模块隐藏
  // 验证token，如果验证通过，返回数，如果验证不通过，直接跳转到登录页面
  return response
}, (err) => Promise.reject(err))

// 6.暴露自定义axios
export default ins
```

# 11.构建首页

## 11.1 封装首页相关数据请求

```ts
// src/api/home.ts
// 首页相关数据请求的接口的封装
import request from '@/utils/request' // request其实就可以看作式自定义之后的axois

interface IPageParams {
  limitNum?: number
  count?: number
}

export function getBannerListData () {
  return request.get('/banner/list')
}

export function getSeckillListData () {
  return request.get('/pro/seckilllist')
}

export function getProListData (params?: IPageParams) {
  return request.get('/pro/list', { params })
}
```

## 11.2 构建首页轮播图组件以及渲染

```tsx
// src/views/home/components/BannerComponent.tsx
import React, { FC } from 'react';

interface IBannerComponentProps {
  
};

const BannerComponent:FC<IBannerComponentProps> = () => {
  return (
    <>
      <h1>轮播图</h1>
    </>
  )
};

export default BannerComponent;
```

```tsx
// src/views/home/Index.tsx
import React, { FC } from 'react';
import { Button } from 'antd-mobile';

import BannerComponent from './components/BannerComponent'
interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent />
      </div>
    </>
  )
};

export default Home;
```

```tsx
// src/views/home/Index.tsx
import React, { FC, useState, useEffect } from 'react';

import { getBannerListData } from '@/api/home'

import BannerComponent from './components/BannerComponent'
interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState([])
  useEffect(() => {
    getBannerListData().then(res => {
      setBannerList(res.data.data)
    })
  }, [])
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent list = { bannerList }/>
      </div>
    </>
  )
};

export default Home;
```

```tsx
// src/views/home/components/BannerComponent.tsx
import React, { FC } from 'react';
import { Swiper, Image } from 'antd-mobile'
interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
  flag: boolean
}
interface IBannerComponentProps {
  list: IBanner[]
};

const BannerComponent:FC<IBannerComponentProps> = ({ list }) => {
  return (
    <div style={{ padding: '15px'}}>
      <Swiper autoplay loop={true} style={{
        '--border-radius': '8px',
      }}>
        {
          list && list.map((item) => {
            return (
              <Swiper.Item key = {item.bannerid}>
                <Image src={item.img}/>
              </Swiper.Item>
            )
          })
        }
      </Swiper>
    </div>
  )
};

export default BannerComponent;
```

## 11.3 构建nav导航组件以及渲染

```ts
// src/utils/nav.ts
const navList = [
  { navid: 1, title: '嗨购超市', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/125678/35/5947/4868/5efbf28cEbf04a25a/e2bcc411170524f0.png' },
  { navid: 2, title: '数码电器', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/178015/31/13828/6862/60ec0c04Ee2fd63ac/ccf74d805a059a44.png' },
  { navid: 3, title: '嗨购服饰', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/41867/2/15966/7116/60ec0e0dE9f50d596/758babcb4f911bf4.png' },
  { navid: 4, title: '嗨购生鲜', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/177902/16/13776/5658/60ec0e71E801087f2/a0d5a68bf1461e6d.png' },
  { navid: 5, title: '嗨购到家', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/196472/7/12807/7127/60ec0ea3Efe11835b/37c65625d94cae75.png' },
  { navid: 6, title: '充值缴费', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/185733/21/13527/6648/60ec0f31E0fea3e0a/d86d463521140bb6.png' },
  { navid: 7, title: '9.9元拼', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/36069/14/16068/6465/60ec0f67E155f9488/595ff3e606a53f02.png' },
  { navid: 8, title: '领券', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/186080/16/13681/8175/60ec0fcdE032af6cf/c5acd2f8454c40e1.png' },
  { navid: 9, title: '领金贴', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/196711/35/12751/6996/60ec1000E21b5bab4/38077313cb9eac4b.png' },
  { navid: 10, title: 'plus会员', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/37709/6/15279/6118/60ec1046E4b5592c6/a7d6b66354efb141.png' }
]

export default navList
```

```tsx
// src/views/home/Index.tsx
import React, { FC, useState, useEffect } from 'react';

import nav_List from '@/utils/nav'
import { getBannerListData } from '@/api/home'

import BannerComponent from './components/BannerComponent'
import NavComponent from './components/NavComponent'
interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState([])
  useEffect(() => {
    getBannerListData().then(res => {
      console.log(res.data)
      setBannerList(res.data.data)
    })
  }, [])


  const [navList] = useState(nav_List)
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent list = { bannerList }/>
        {/* nav导航 */}
        <NavComponent list = { navList } />
      </div>
    </>
  )
};

export default Home;
```

```tsx
// src/views/home/components/NavComponent.tsx
import React, { FC } from 'react';
import { Grid, Image } from 'antd-mobile'
interface INav {
  navid: number
  title: string
  imgurl: string
}
interface INavComponentProps {
  list: INav[]
};

const NavComponent:FC<INavComponentProps> = ({ list }) => {
  return (
    <Grid columns={5} gap={0}>
      {
        list && list.map(item => {
          return (
            <Grid.Item key = {item.navid} style={{ display: 'flex', flexDirection: 'column', justifyContent: 'center', alignItems: 'center'}}>
              <Image src={ item.imgurl } style={{ width: 44, height: 44 }}></Image>
              <p>{ item.title }</p>
            </Grid.Item>
          )
        })
      }
    </Grid>
  )
};

export default NavComponent;
```

## 11.4 构建秒杀列表实现

```tsx
// src/views/home/components/SeckillComponent.tsx
import React, { FC } from 'react';

interface ISeckillComponentProps {
  
};

const SeckillComponent:FC<ISeckillComponentProps> = () => {
  return (
    <>
      <h1>秒杀列表</h1>
    </>
  )
};

export default SeckillComponent;
```

```tsx
// src/views/home/Index.tsx
import React, { FC, useState, useEffect } from 'react';

import nav_List from '@/utils/nav'
import { getBannerListData, getSeckillListData } from '@/api/home'

import BannerComponent from './components/BannerComponent'
import NavComponent from './components/NavComponent'
import SeckillComponent from './components/SeckillComponent'
interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  // 轮播图业务
  const [bannerList, setBannerList] = useState([])
  useEffect(() => {
    getBannerListData().then(res => {
      console.log(res.data)
      setBannerList(res.data.data)
    })
  }, [])

  // nav业务
  const [navList] = useState(nav_List)

  // 秒杀业务
  const [seckillList, setSeckillList] = useState([])
  useEffect(() => {
    getSeckillListData().then(res => {
      setSeckillList(res.data.data)
    })
  }, [])
  
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent list = { bannerList }/>
        {/* nav导航 */}
        <NavComponent list = { navList } />
        {/* 秒杀列表 */}
        <SeckillComponent list = { seckillList } />
      </div>
    </>
  )
};

export default Home;
```

```tsx
// src/views/home/components/SeckillComponent.tsx
import React, { FC, useEffect, useState } from 'react';
import { Image } from 'antd-mobile'
import './Seckill.scss';
interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}

interface ISeckillComponentProps {
  list: IPro[]
};

const SeckillComponent:FC<ISeckillComponentProps> = ({list}) => {
  const [time, setTime] = useState(0)
  const [field, setField] = useState(0)
  const [hour, setHour] = useState(0)
  const [minutes, setMinutes] = useState(0)
  const [seconds, setSeconds] = useState(0)

  useEffect(() => {
    function timeCountDown () {
      let originTime = new Date(new Date().toLocaleDateString());
      let hours = new Date().getHours();
      if (hours % 2 === 0) {
        setField(hours)
        let twohour = originTime.getTime() + (hours + 2) * 60 * 60 * 1000
        setTime(twohour.valueOf() - new Date().valueOf())
      } else {
        setField(hours - 1)
        let twohour = originTime.getTime() + 60 * (hours + 1) * 60 * 1000;
        setTime(twohour - new Date().valueOf())
      }
    }

    const timer = setInterval(() => {
      timeCountDown()
    }, 1000)

    return () => {
      clearInterval(timer)
    }
  }, [])

  // 检测到time发生改变，则计算 hour minutes seconds
  useEffect(() => {
    console.log('1', time)
    const hour = Math.floor(time / 3600000)
    const minutes = Math.floor((time - hour * 3600000) / 60000)
    const seconds = Math.floor((time - hour * 3600000 - minutes * 60000) / 1000)
    setHour(hour)
    setMinutes(minutes)
    setSeconds(seconds)
  }, [time])

  
  return (
    <div className="seckillBox">
    <div className="title_wrap">
      <ul>
        <li>
          <span>嗨购秒杀</span>
          <span>{ field }点场</span>
          {/* { hour } - { minutes } - { seconds } */}
          <div className="myTimer">
            <span className="block">{ hour < 10 ? '0' + hour : hour }</span>
            <span className="colon">:</span>
            <span className="block">{ minutes < 10 ? '0' + minutes : minutes }</span>
            <span className="colon">:</span>
            <span className="block">{ seconds < 10 ? '0' + seconds : seconds }</span>
          </div>
        </li>
        <li>
          爆款轮番秒
        </li>
      </ul>
    </div>
    <ul className="seckillList">
      {
        list && list.map(item => (
          <li key = { item.proid }>
            <Image src={item.img1} />
            <p>￥{ item.originprice }</p>
          </li>
        ))
      }
    </ul>
  </div>
  )
};

export default SeckillComponent;
```

```scss
// src/views/home/components/Seckill.scss
.seckillBox {
  width: 96%;
  height: 1.3rem;
  background-color: #fff;
  border-radius: 15px;
  margin: 10px 2%;
  .title_wrap {
    width: 100%;
    height: 0.36rem;
    background: url('./title_wrap.png') no-repeat center center;
    background-size: cover;
    overflow: hidden;
    ul {
      display: flex;
      padding: 0 10px;
      box-sizing: border-box;
      li {
        
        &:nth-child(1) {
          flex: 3;
          display: flex;
          &>span {
            line-height: 0.36rem;
            &:nth-child(2) {
              color: #f66;
            }
            margin-right: 10px;
          }
          .myTimer {
            padding-top: 0.08rem;
            span {
              line-height: 0.2rem;
            }
          }
        }
        &:nth-child(2) {
          flex: 2;
          display: flex;
          justify-content: flex-end;
          font-size: 0.12rem;
          color: #f66;
          line-height: 0.36rem;

          .van-icon {
            font-size: 0.12rem;
            transform-origin: left top;
            transform: rotate(225deg);
            line-height: 0.36rem;
            margin-top: 0.34rem;
            box-sizing: border-box;
          }
        }
      }
    }
  }
  .seckillList {
    padding: 0 10px;
    box-sizing: border-box;
    margin-top: 0.1rem;
    li {
      float: left;
      width: calc(100% / 6);
      height: 0.94rem;
      // background-color: #f66;
      .adm-image-img {
        width: 96%;
        margin: 0 2%;
        height: 0.54rem;
        overflow: hidden;
      }
      p {
        text-align: center;
        color: #f66;
        line-height: 0.3rem;
        font-weight: bold;
      }
    }
  }
}

.colon {
  display: inline-block;
  margin: 0 2px;
  font-weight: bold;
  font-size: 0.16rem;
  color: #ee0a24;
}
.block {
  display: inline-block;
  color: #fff;
  width: 16px;
  font-size: 12px;
  font-weight: bold;
  text-align: center;
  background-color: #ee0a24;
  border-radius: 4px;
}
```

## 11.5 构建产品列表

```tsx
// src/views/home/ProComponent.tsx
import React, { FC } from 'react';

interface IProComponentProps {
  
};

const ProComponent:FC<IProComponentProps> = ({}) => {
  return (
    <>
      <h1>产品列表</h1>
    </>
  )
};

export default ProComponent;
```

```tsx
// src/views/home/Index.tsx
import React, { FC, useState, useEffect } from 'react';

import nav_List from '@/utils/nav'
import { getBannerListData, getProListData, getSeckillListData } from '@/api/home'

import BannerComponent from './components/BannerComponent'
import NavComponent from './components/NavComponent'
import SeckillComponent from './components/SeckillComponent'
import ProComponent from './components/ProComponent'
interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  // 轮播图业务
  const [bannerList, setBannerList] = useState([])
  useEffect(() => {
    getBannerListData().then(res => {
      console.log(res.data)
      setBannerList(res.data.data)
    })
  }, [])

  // nav业务
  const [navList] = useState(nav_List)

  // 秒杀业务
  const [seckillList, setSeckillList] = useState([])
  useEffect(() => {
    getSeckillListData().then(res => {
      setSeckillList(res.data.data)
    })
  }, [])
  
  // 产品列表
  const [proList, setProList] = useState([])
  useEffect(() => {
    getProListData().then(res => setProList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent list = { bannerList }/>
        {/* nav导航 */}
        <NavComponent list = { navList } />
        {/* 秒杀列表 */}
        <SeckillComponent list = { seckillList } />
        {/* 产品列表 */}
        <ProComponent list = { proList } />
      </div>
    </>
  )
};

export default Home;
```

```scss
// src/views/home/Pro.scss
.proList {
  display: flex;
  width: 96%;
  margin: 10px 2% 15px;
  .left {
    width: 49%;
    min-height: 300px;
    margin-right: 1%;
    // background-color: #f66;
  }
  .right {
    width: 49%;
    min-height: 300px;
    margin-left: 1%;
    // background-color: #f66;
  }

  ul {
    li {
      margin-top: 5px;
      background-color: #fff;
      border-radius: 10px;
      overflow: hidden;
      .itemImage {
        width: 100%;
        .van-image {
          min-height: 1.3rem;
        }
      }
      .itemInfo {
        padding: 10px;
        .itemTitle {
          font-size: 0.14rem;
        }
        .itemPrice {
          color: #f66;
          font-size: 0.2rem;
          span {
            font-size: 0.14rem;
          }
        }
      }

    }
  }
}
```

```tsx
// src/views/home/ProComponent.tsx
import React, { FC } from 'react';
import { Image, Ellipsis } from 'antd-mobile';
import './Pro.scss';

interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
interface IProComponentProps {
  list: IPro[]
};

const ProComponent:FC<IProComponentProps> = ({ list }) => {
  return (
    <div className="proList">
      <ul className="left">
        {
          list && list.map((item, index) => {
            if (index % 2 === 0) {
              return (
                <li key = { item.proid }>
                  <div className="itemImage">
                    <Image src={item.img1} />
                  </div>
                  <div className="itemInfo">
                    <div className="itemTitle">
                      <Ellipsis direction='end' rows={2} content={item.proname } />
                    </div>
                    <div className="itemPrice">￥{ item.originprice / 1 }</div>
                  </div>
              </li>
              )
            } else {
              return null
            }
           
          })
        }
        
      </ul>
      <ul className="right">
        {
          list && list.map((item, index) => {
            if (index % 2 === 1) {
              return (
                <li  key = { item.proid }>
                  <div className="itemImage">
                    <Image src={item.img1} />
                  </div>
                  <div className="itemInfo">
                    <div className="itemTitle ">
                      <Ellipsis direction='end' rows={2} content={ item.proname } />
                    </div>
                    <div className="itemPrice">￥{ item.originprice / 1 }</div>
                  </div>
              </li>
              )
            } else {
              return null
            }
           
          })
        }
      </ul>
    </div>
  )
};

export default ProComponent;
```

## 11.6 实现上拉加载操作

http://ant-design-mobile.antgroup.com/zh/components/infinite-scroll

```tsx
// src/views/home/Index.tsx
import React, { FC, useState, useEffect } from 'react';

import { InfiniteScroll } from 'antd-mobile'; 

import nav_List from '@/utils/nav'
import { getBannerListData, getProListData, getSeckillListData } from '@/api/home'

import BannerComponent from './components/BannerComponent'
import NavComponent from './components/NavComponent'
import SeckillComponent from './components/SeckillComponent'
import ProComponent from './components/ProComponent'

interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  // 轮播图业务
  const [bannerList, setBannerList] = useState([])
  useEffect(() => {
    getBannerListData().then(res => {
      console.log(res.data)
      setBannerList(res.data.data)
    })
  }, [])

  // nav业务
  const [navList] = useState(nav_List)

  // 秒杀业务
  const [seckillList, setSeckillList] = useState<IPro[]>([])
  useEffect(() => {
    getSeckillListData().then(res => {
      setSeckillList(res.data.data)
    })
  }, [])
  
  // 产品列表
  const [proList, setProList] = useState<IPro[]>([])
  useEffect(() => {
    getProListData().then(res => setProList(res.data.data))
  }, [])

  // 加载更多
  const [hasMore, setHasMOre] = useState<boolean>(true)
  const [count, setCount] = useState<number>(2)
  const loadMore = async () => {
    const res = await getProListData({ count })
    if (res.data.data.length === 0) {
      setHasMOre(false)
    } else {
      setCount(count + 1)
      setProList([ ...proList, ...res.data.data ])
    }
  }
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent list = { bannerList }/>
        {/* nav导航 */}
        <NavComponent list = { navList } />
        {/* 秒杀列表 */}
        <SeckillComponent list = { seckillList } />
        {/* 产品列表 */}
        <ProComponent list = { proList } />

        {/* 上拉加载组件 */}
        <InfiniteScroll loadMore={ loadMore } hasMore = { hasMore } />
      </div>
    </>
  )
};

export default Home;
```

## 11.7 实现下拉刷新

http://ant-design-mobile.antgroup.com/zh/components/pull-to-refresh

```tsx
// src/views/home/Index.tsx
import React, { FC, useState, useEffect } from 'react';

import { InfiniteScroll, PullToRefresh } from 'antd-mobile'; 

import nav_List from '@/utils/nav'
import { getBannerListData, getProListData, getSeckillListData } from '@/api/home'

import BannerComponent from './components/BannerComponent'
import NavComponent from './components/NavComponent'
import SeckillComponent from './components/SeckillComponent'
import ProComponent from './components/ProComponent'

interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  // 轮播图业务
  const [bannerList, setBannerList] = useState([])
  useEffect(() => {
    getBannerListData().then(res => {
      console.log(res.data)
      setBannerList(res.data.data)
    })
  }, [])

  // nav业务
  const [navList] = useState(nav_List)

  // 秒杀业务
  const [seckillList, setSeckillList] = useState<IPro[]>([])
  useEffect(() => {
    getSeckillListData().then(res => {
      setSeckillList(res.data.data)
    })
  }, [])
  
  // 产品列表
  const [proList, setProList] = useState<IPro[]>([])
  useEffect(() => {
    getProListData().then(res => setProList(res.data.data))
  }, [])

  // 加载更多
  const [hasMore, setHasMOre] = useState<boolean>(true)
  const [count, setCount] = useState<number>(2)
  const loadMore = async () => {
    const res = await getProListData({ count })
    if (res.data.data.length === 0) {
      setHasMOre(false)
    } else {
      setHasMOre(true)
      setCount(count + 1)
      setProList([ ...proList, ...res.data.data ])
    }
  }

  // 下拉刷新
  const onRefresh = async () => {
    const res = await getProListData()
    setProList(res.data.data)
    setHasMOre(true)
    setCount(2)
  }
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        <PullToRefresh onRefresh = { onRefresh }>
          {/* 轮播图 */}
          <BannerComponent list = { bannerList }/>
          {/* nav导航 */}
          <NavComponent list = { navList } />
          {/* 秒杀列表 */}
          <SeckillComponent list = { seckillList } />
          {/* 产品列表 */}
          <ProComponent list = { proList } />

        </PullToRefresh>
        
        {/* 上拉加载组件 */}
        <InfiniteScroll loadMore={ loadMore } hasMore = { hasMore } />
      </div>
    </>
  )
};

export default Home;
```

## 11.8返回顶部

分析清除到底是哪一个容器产生了滚动条

分析得知 content 容器产生了滚动条，可以给它绑定一个 scroll 事件用于判断 回到顶部按钮显示还是不显示

通过 content 的dom的scrollTop 属性可以设置滚动条距离

图标是在一个单独的 npm 包中，如果你想使用图标，需要先安装它：

```sh
$ cnpm install --save antd-mobile-icons
```

```scss
// src/views/home/style.scss
.backTop {
  position: fixed;
  bottom: 0.6rem;
  right: 10px;
  width: 36px;
  height: 36px;
  background-color: #fff;
  border: 1px solid #efefef;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
}
```

```tsx
// src/views/home/Index.tsx
import React, { FC, useState, useEffect, useRef } from 'react';

import { InfiniteScroll, PullToRefresh } from 'antd-mobile'; 
import { UpOutline } from 'antd-mobile-icons';

import './style.scss'

import nav_List from '@/utils/nav'
import { getBannerListData, getProListData, getSeckillListData } from '@/api/home'

import BannerComponent from './components/BannerComponent'
import NavComponent from './components/NavComponent'
import SeckillComponent from './components/SeckillComponent'
import ProComponent from './components/ProComponent'

interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  // 轮播图业务
  const [bannerList, setBannerList] = useState([{
    alt: "",
    bannerid: "c",
    flag: true,
    img: "",
    link: ""
  }])
  useEffect(() => {
    getBannerListData().then(res => {
      console.log(res.data)
      setBannerList(res.data.data)
    })
  }, [])

  // nav业务
  const [navList] = useState(nav_List)

  // 秒杀业务
  const [seckillList, setSeckillList] = useState<IPro[]>([])
  useEffect(() => {
    getSeckillListData().then(res => {
      setSeckillList(res.data.data)
    })
  }, [])
  
  // 产品列表
  const [proList, setProList] = useState<IPro[]>([])
  useEffect(() => {
    getProListData().then(res => setProList(res.data.data))
  }, [])

  // 加载更多
  const [hasMore, setHasMOre] = useState<boolean>(true)
  const [count, setCount] = useState<number>(2)
  const loadMore = async () => {
    const res = await getProListData({ count })
    if (res.data.data.length === 0) {
      setHasMOre(false)
    } else {
      setHasMOre(true)
      setCount(count + 1)
      setProList([ ...proList, ...res.data.data ])
    }
  }

  // 下拉刷新
  const onRefresh = async () => {
    const res = await getProListData()
    setProList(res.data.data)
    setHasMOre(true)
    setCount(2)
  }

  // 返回顶部
  const contentRef = useRef<any>()
  const [top, setTop] = useState<number>(0)
  const scroll = () => {
    console.log(contentRef.current.scrollTop)
    setTop(contentRef.current.scrollTop)
  }
  const backTop = () => {
    contentRef.current.scrollTop = 0
  }
  return (
    <>
      <header className="header">home header</header>
      <div className="content" ref={ contentRef } onScroll = { scroll }>
        <PullToRefresh onRefresh = { onRefresh }>
          {/* 轮播图 */}
          <BannerComponent list = { bannerList }/>
          {/* nav导航 */}
          <NavComponent list = { navList } />
          {/* 秒杀列表 */}
          <SeckillComponent list = { seckillList } />
          {/* 产品列表 */}
          <ProComponent list = { proList } />

        </PullToRefresh>
        
        {/* 上拉加载组件 */}
        <InfiniteScroll loadMore={ loadMore } hasMore = { hasMore } />
        { top > 300 ? <div className="backTop" onClick={ backTop }><UpOutline /></div> : null }
      </div>
    </>
  )
};

export default Home;
```

## 11.9 优化项目

提取首页面组件的业务逻辑，封装自定义hooks，统一导出

```tsx
// src/views/home/components/index.ts
export { default as BannerComponent } from './BannerComponent'
export { default as NavComponent } from './NavComponent'
export { default as SeckillComponent } from './SeckillComponent'
export { default as ProComponent } from './ProComponent'

// import BannerComponent from './BannerComponent'
// import NavComponent from './NavComponent'
// import SeckillComponent from './SeckillComponent'
// import ProComponent from './ProComponent'

// export {
//   BannerComponent,
//   NavComponent,
//   SeckillComponent,
//   ProComponent
// }

```

```tsx
// src/views/home/hooks/useBanner.ts
import { useState, useEffect } from 'react'
import { getBannerListData } from '@/api/home'
export default function useBanner () {
  const [bannerList, setBannerList] = useState([{
    alt: "",
    bannerid: "c",
    flag: true,
    img: "",
    link: ""
  }])
  useEffect(() => {
    getBannerListData().then(res => {
      console.log(res.data)
      setBannerList(res.data.data)
    })
  }, [])

  return {
    bannerList
  }
}
```

```ts
// src/views/home/hooks/useSeckill.ts
import { useState, useEffect } from 'react'
import { getSeckillListData } from '@/api/home'
export default function useSeckill () {
  const [seckillList, setSeckillList] = useState([])
  useEffect(() => {
    getSeckillListData().then(res => {
      setSeckillList(res.data.data)
    })
  }, [])

  return {
    seckillList
  }
}
```

```ts
// src/views/home/hooks/usePro.ts
import { useState, useEffect } from 'react'
import { getProListData } from '@/api/home'
interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
export default function usePro () {
  // 产品列表
  const [proList, setProList] = useState<IPro[]>([])
  useEffect(() => {
    getProListData().then(res => setProList(res.data.data))
  }, [])

  // 加载更多
  const [hasMore, setHasMOre] = useState<boolean>(true)
  const [count, setCount] = useState<number>(2)
  const loadMore = async () => {
    const res = await getProListData({ count })
    if (res.data.data.length === 0) {
      setHasMOre(false)
    } else {
      setHasMOre(true)
      setCount(count + 1)
      setProList([ ...proList, ...res.data.data ])
    }
  }

  // 下拉刷新
  const onRefresh = async () => {
    const res = await getProListData()
    setProList(res.data.data)
    setHasMOre(true)
    setCount(2)
  }

  return {
    proList,
    onRefresh,
    hasMore,
    loadMore
  }
}
```

```ts
// src/views/home/hooks/useBacktop.ts
import { useState, useRef } from 'react'
export default function useBacktop () {
  const contentRef = useRef(null)
  const [top, setTop] = useState<number>(0)
  const scroll = () => {
    const test = contentRef.current as unknown as HTMLDivElement
    setTop(test.scrollTop)
  }
  const backTop = () => {
    const test = contentRef.current as unknown as HTMLDivElement
    test.scrollTop = 0
  }
  return {
    contentRef, top, scroll, backTop
  }
}
```

```
// src/views/home/hooks/index.ts
export { default as useBanner } from './useBanner'
export { default as useSeckill } from './useSeckill'
export { default as usePro } from './usePro'
export { default as useBacktop } from './useBacktop'
```

```tsx
// src/views/home/Index.tsx
import React, { FC, useState } from 'react';

import { InfiniteScroll, PullToRefresh } from 'antd-mobile'; 
import { UpOutline } from 'antd-mobile-icons';

import './style.scss'

import nav_List from '@/utils/nav'

import { BannerComponent, NavComponent, SeckillComponent, ProComponent } from './components'
import { useBacktop, useBanner, usePro, useSeckill } from './hooks'


interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  // 轮播图业务
  const { bannerList } = useBanner()

  // nav业务
  const [navList] = useState(nav_List)

  // 秒杀业务
  const { seckillList } = useSeckill()
  
  // 产品列表 下拉刷线  上拉加载
  const { onRefresh, proList, loadMore, hasMore  } = usePro()

  // 返回顶部
  // const contentRef = useRef(null)
  // const [top, setTop] = useState<number>(0)
  // const scroll = () => {
  //   const test = contentRef.current as unknown as HTMLDivElement
  //   setTop(test.scrollTop)
  // }
  // const backTop = () => {
  //   const test = contentRef.current as unknown as HTMLDivElement
  //   test.scrollTop = 0
  // }
  const { contentRef, top, scroll, backTop } = useBacktop()
  return (
    <>
      <header className="header">home header</header>
      <div className="content" ref={ contentRef } onScroll = { scroll }>
        <PullToRefresh onRefresh = { onRefresh }>
          {/* 轮播图 */}
          <BannerComponent list = { bannerList }/>
          {/* nav导航 */}
          <NavComponent list = { navList } />
          {/* 秒杀列表 */}
          <SeckillComponent list = { seckillList } />
          {/* 产品列表 */}
          <ProComponent list = { proList } />

        </PullToRefresh>
        
        {/* 上拉加载组件 */}
        <InfiniteScroll loadMore={ loadMore } hasMore = { hasMore } />
        { top > 300 ? <div className="backTop" onClick={ backTop }><UpOutline /></div> : null }
      </div>
    </>
  )
};

export default Home;
```

现在主流手机都有安全区域，那么写代码时一定要注意

http://ant-design-mobile.antgroup.com/zh/components/safe-area

## 11.10 自定义头部

````scss
// src/views/home/components/Header.scss
.header {
  ul {
    width: 100%;
    height: 100%;
    display: flex;
    li {
      height: 100%;

      display: flex;
      justify-content: center;
      align-items: center;

      color: #fff;
      

      &:nth-child(1), &:nth-child(3) {
        width: 50px;
      }
      &:nth-child(2) {
        flex: 1;
        .searchBox {
          width: 100%;
          height: 70%;
          background-color: #fff;
          border-radius: 16px;
          color: #666;
          display: flex;
          .adm-image-img {
            width: 40px;
            margin-top: 4px;
            margin-left: 10px;
          }
          .divider {
            width: 12px;
            font-size: 24px;
            margin-left: 10px;
            color: #999;
          }
          .antd-mobile-icon {
            width: 18px;
            height: 18px;
            margin-top: 6px;
            display: flex;
            justify-content: center;
            align-items: center;
          }
          .searchText {
            flex: 1;
            line-height: .31rem;
            display: flex;
            align-items: center;
          }
        }
      }
    }
  }
}
````

```tsx
// src/views/home/components/HeaderComponent.tsx
import React, { FC } from 'react';
import { Image } from 'antd-mobile'
import { SearchOutline } from 'antd-mobile-icons';

import logo from './logo.png'
import './Header.scss';
interface IHeaderComponentProps {
  
};

const HeaderComponent:FC<IHeaderComponentProps> = ({}) => {
  return (
    <header className="header">
    <ul>
      <li>西安</li>
      <li>
        <div className="searchBox">
          <Image src={ logo } />
          <span className="divider ">|</span>
          <SearchOutline fontSize={18} />
          <span className="searchText">立柜式空调</span>
        </div>
      </li>
      <li>登录</li>
    </ul>
  </header>
  )
};

export default HeaderComponent;
```

```ts
// src/views/home/components/index.ts
export { default as BannerComponent } from './BannerComponent'
export { default as NavComponent } from './NavComponent'
export { default as SeckillComponent } from './SeckillComponent'
export { default as ProComponent } from './ProComponent'
export { default as HeaderComponent } from './HeaderComponent'

// import BannerComponent from './BannerComponent'
// import NavComponent from './NavComponent'
// import SeckillComponent from './SeckillComponent'
// import ProComponent from './ProComponent'

// export {
//   BannerComponent,
//   NavComponent,
//   SeckillComponent,
//   ProComponent
// }

```

```tsx
// src/views/home/Index.tsx
import React, { FC, useState } from 'react';

import { InfiniteScroll, PullToRefresh } from 'antd-mobile'; 
import { UpOutline } from 'antd-mobile-icons';

import './style.scss'

import nav_List from '@/utils/nav'

import { BannerComponent, NavComponent, SeckillComponent, ProComponent, HeaderComponent } from './components'
import { useBacktop, useBanner, usePro, useSeckill } from './hooks'


interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  // 轮播图业务
  const { bannerList } = useBanner()

  // nav业务
  const [navList] = useState(nav_List)

  // 秒杀业务
  const { seckillList } = useSeckill()
  
  // 产品列表 下拉刷线  上拉加载
  const { onRefresh, proList, loadMore, hasMore  } = usePro()

  // 返回顶部
  // const contentRef = useRef(null)
  // const [top, setTop] = useState<number>(0)
  // const scroll = () => {
  //   const test = contentRef.current as unknown as HTMLDivElement
  //   setTop(test.scrollTop)
  // }
  // const backTop = () => {
  //   const test = contentRef.current as unknown as HTMLDivElement
  //   test.scrollTop = 0
  // }
  const { contentRef, top, scroll, backTop } = useBacktop()
  return (
    <>
      <HeaderComponent />
      <div className="content" ref={ contentRef } onScroll = { scroll }>
        <PullToRefresh onRefresh = { onRefresh }>
          {/* 轮播图 */}
          <BannerComponent list = { bannerList }/>
          {/* nav导航 */}
          <NavComponent list = { navList } />
          {/* 秒杀列表 */}
          <SeckillComponent list = { seckillList } />
          {/* 产品列表 */}
          <ProComponent list = { proList } />

        </PullToRefresh>
        
        {/* 上拉加载组件 */}
        <InfiniteScroll loadMore={ loadMore } hasMore = { hasMore } />
        { top > 300 ? <div className="backTop" onClick={ backTop }><UpOutline /></div> : null }
      </div>
    </>
  )
};

export default Home;
```

# 12.实现详情

## 12.1 构建详情页面以及路由

* 构建详情页面组件

```tsx
// src/views/detail/Index.tsx
import React, { FC } from 'react';

interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  return (
    <>
      <header className="header">Detail header</header>
      <div className="content">Detail content</div>
    </>
  )
};

export default Detail;
```

* 构建路由

```tsx
// src/App.tsx
import React, { FC } from 'react';
// BrowserRouter  /  /kind  /cart /user
// HashRouter  /#/ /#/kind /#/cart /#/user
import { Routes, Route, Navigate } from 'react-router-dom'
import '@/App.scss';
import Cart from '@/views/cart/Index';
import Home from '@/views/home/Index';
import Kind from '@/views/kind/Index';
import User from '@/views/user/Index';
import Detail from '@/views/detail/Index'; // +++++
import Footer from '@/components/Footer';
interface IAppProps {
};

const App: FC<IAppProps> = () => {
 
  return (
    <div className='container'>
      <Routes>
        <Route path="/" element={ <Navigate to="/home" replace={true}/>} />
        <Route path="/home" element={<Home />} />
        <Route path="/kind" element={<Kind />} />
        <Route path="/cart" element={<Cart />} />
        <Route path="/user" element ={<User />} />
        <Route path="/detail/:proid" element ={<Detail />} />
      </Routes>
      <footer className="footer"><Footer /></footer>
      <div className='landscape-tip'>
        请将屏幕竖向浏览
      </div>
    </div>
  )
};

export default App;

```

> 通过访问地址发现可以跳转到详情，但是详情页面不应有 底部选项卡，需要处理

```tsx
// src/components/Footer.tsx
import React, { FC } from 'react';
import { NavLink } from 'react-router-dom'
interface IFooterProps {
  
};

const Footer:FC<IFooterProps> = () => {
  return (
    <footer className='footer'>
      <ul>
        <NavLink to="/home" style={({ isActive }) =>
                isActive ? { color: '#f66 '} : undefined
              }>
          <span className="iconfont icon-shouye"></span>
          <p>首页</p>
        </NavLink>
        <NavLink to="/kind" style={({ isActive }) =>
                isActive ? { color: '#f66 '} : undefined
              }>
          <span className="iconfont icon-fenlei"></span>
          <p>分类</p>
        </NavLink>
        <NavLink to="/cart" style={({ isActive }) =>
                isActive ? { color: '#f66 '} : undefined
              }>
          <span className="iconfont icon-gouwuche"></span>
          <p>购物车</p>
        </NavLink>
        <NavLink to="/user" style={({ isActive }) =>
                isActive ? { color: '#f66 '} : undefined
              }>
          <span className="iconfont icon-shouye1"></span>
          <p>我的</p>
        </NavLink>
      </ul>
    </footer>
    
  )
};

export default Footer;
```

```tsx
// src/App.tsx
import React, { FC } from 'react';
// BrowserRouter  /  /kind  /cart /user
// HashRouter  /#/ /#/kind /#/cart /#/user
import { Routes, Route, Navigate } from 'react-router-dom'
import '@/App.scss';
import Cart from '@/views/cart/Index';
import Home from '@/views/home/Index';
import Kind from '@/views/kind/Index';
import User from '@/views/user/Index';
import Detail from '@/views/detail/Index';
import Footer from '@/components/Footer';
interface IAppProps {
};

const App: FC<IAppProps> = () => {
 
  return (
    <div className='container'>
      <Routes>
        <Route path="/" element={ <Navigate to="/home" replace={true}/>} />
        {/* <Route path="/home" element={<Home />} />
        <Route path="/kind" element={<Kind />} />
        <Route path="/cart" element={<Cart />} />
        <Route path="/user" element ={<User />} /> */}
        <Route path="/home" element={<><Home /><Footer /></>} />
        <Route path="/kind" element={<><Kind /><Footer /></>} />
        <Route path="/cart" element={<><Cart /><Footer /></>} />
        <Route path="/user" element ={<><User /><Footer /></>} />
        <Route path="/detail/:proid" element ={<Detail />} />
      </Routes>
      {/* <footer className="footer"><Footer /></footer> */}
      {/* <Routes>
        <Route path="/home" element={<Footer />} />
        <Route path="/kind" element={<Footer />} />
        <Route path="/cart" element={<Footer />} />
        <Route path="/user" element ={<Footer />} />
      </Routes> */}
      <div className='landscape-tip'>
        请将屏幕竖向浏览
      </div>
    </div>
  )
};

export default App;
```

## 12.2 点击列表进入产品详情

* 秒杀列表声明式进入详情

```tsx
// src/views/home/components/SeckillComponent.tsx
import React, { FC, useEffect, useState } from 'react';
import { Image } from 'antd-mobile'
import './Seckill.scss';
import { Link } from 'react-router-dom'; // ++++++++
interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}

interface ISeckillComponentProps {
  list: IPro[]
};

const SeckillComponent:FC<ISeckillComponentProps> = ({list}) => {
  const [time, setTime] = useState(0)
  const [field, setField] = useState(0)
  const [hour, setHour] = useState(0)
  const [minutes, setMinutes] = useState(0)
  const [seconds, setSeconds] = useState(0)

  useEffect(() => {
    function timeCountDown () {
      let originTime = new Date(new Date().toLocaleDateString());
      let hours = new Date().getHours();
      if (hours % 2 === 0) {
        setField(hours)
        let twohour = originTime.getTime() + (hours + 2) * 60 * 60 * 1000
        setTime(twohour.valueOf() - new Date().valueOf())
      } else {
        setField(hours - 1)
        let twohour = originTime.getTime() + 60 * (hours + 1) * 60 * 1000;
        setTime(twohour - new Date().valueOf())
      }
    }

    const timer = setInterval(() => {
      timeCountDown()
    }, 1000)

    return () => {
      clearInterval(timer)
    }
  }, [])

  // 检测到time发生改变，则计算 hour minutes seconds
  useEffect(() => {
    // console.log('1', time)
    const hour = Math.floor(time / 3600000)
    const minutes = Math.floor((time - hour * 3600000) / 60000)
    const seconds = Math.floor((time - hour * 3600000 - minutes * 60000) / 1000)
    setHour(hour)
    setMinutes(minutes)
    setSeconds(seconds)
  }, [time])

  
  return (
    <div className="seckillBox">
    <div className="title_wrap">
      <ul>
        <li>
          <span>嗨购秒杀</span>
          <span>{ field }点场</span>
          {/* { hour } - { minutes } - { seconds } */}
          <div className="myTimer">
            <span className="block">{ hour < 10 ? '0' + hour : hour }</span>
            <span className="colon">:</span>
            <span className="block">{ minutes < 10 ? '0' + minutes : minutes }</span>
            <span className="colon">:</span>
            <span className="block">{ seconds < 10 ? '0' + seconds : seconds }</span>
          </div>
        </li>
        <li>
          爆款轮番秒
        </li>
      </ul>
    </div>
    <ul className="seckillList">
      {
        list && list.map(item => (
          <li key = { item.proid }>
            {/* 使用Link组件跳转即可 +++++++++++++++++++++ */}
            <Link to={ '/detail/' + item.proid }>
              <Image src={item.img1} />
              <p>￥{ item.originprice }</p>
            </Link>
          </li>
        ))
      }
    </ul>
  </div>
  )
};

export default SeckillComponent;
```

* 产品列表编程式进入详情

```tsx
// src/views/home/ProComponent.tsx
import React, { FC } from 'react';
import { Image, Ellipsis } from 'antd-mobile';
import './Pro.scss';
import { useNavigate } from 'react-router-dom';

interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
interface IProComponentProps {
  list: IPro[]
};

const ProComponent:FC<IProComponentProps> = ({ list }) => {
  const navigate = useNavigate()
  const toDetail = (proid: string) => {
    navigate('/detail/' + proid)
  }
  return (
    <div className="proList">
      <ul className="left">
        {
          list && list.map((item, index) => {
            if (index % 2 === 0) {
              return (
                <li key = { item.proid }  onClick = { () => toDetail(item.proid) }>
                  <div className="itemImage">
                    <Image src={item.img1} />
                  </div>
                  <div className="itemInfo">
                    <div className="itemTitle">
                      <Ellipsis direction='end' rows={2} content={item.proname } />
                    </div>
                    <div className="itemPrice">￥{ item.originprice / 1 }</div>
                  </div>
              </li>
              )
            } else {
              return null
            }
           
          })
        }
        
      </ul>
      <ul className="right">
        {
          list && list.map((item, index) => {
            if (index % 2 === 1) {
              return (
                <li  key = { item.proid } onClick = { () => toDetail(item.proid) }>
                  <div className="itemImage">
                    <Image src={item.img1} />
                  </div>
                  <div className="itemInfo">
                    <div className="itemTitle ">
                      <Ellipsis direction='end' rows={2} content={ item.proname } />
                    </div>
                    <div className="itemPrice">￥{ item.originprice / 1 }</div>
                  </div>
              </li>
              )
            } else {
              return null
            }
           
          })
        }
      </ul>
    </div>
  )
};

export default ProComponent;
```

## 12.3 详情页获取路由参数

```tsx
// src/views/detail/Index.tsx
import React, { FC } from 'react';
import { useParams } from 'react-router-dom';

interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  const { proid } = useParams()
  console.log(proid)
  return (
    <>
      <header className="header">Detail header</header>
      <div className="content">Detail content</div>
    </>
  )
};

export default Detail;
```

## 12.4 封装详情页数据请求

```
// src/api/detail.ts
import request from '../utils/request'

export function getDetailData (proid: string) {
  return request.get('/pro/detail/' + proid)
}

// 推荐商品接口
export function getDetailRecommendData () {
  return request.get('/pro/recommendlist')
}
```

```tsx
// src/views/detail/Index.tsx
import React, { FC, useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { getDetailData } from '@/api/detail'
interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  const { proid } = useParams()
  console.log(proid)


  const [obj, setObj] = useState({
    banners: [],
    proname: '',
    originprice: 0,
    discount: 0,
    brand: '',
    category: '',
    sales: 0,
    issale: 1
  })
  useEffect(() => {
    getDetailData(proid!).then(res => {
      console.log(res.data.data)
      setObj({
        banners: res.data.data.banners[0].split(','),
        proname: res.data.data.proname,
        originprice:  res.data.data.originprice,
        discount: res.data.data.discount,
        brand:  res.data.data.brand,
        category: res.data.data.category,
        sales: res.data.data.sales,
        issale: res.data.data.issale
      })
    })
  }, [proid])

  return (
    <>
      <header className="header">Detail header</header>
      <div className="content">Detail content</div>
    </>
  )
};

export default Detail;
```

## 12.5 渲染详情页面

### 12.5.1 轮播图以及大图预览

```scss
// src/views/detail/components/Banner.module.scss
.bannerBox {
  position: relative;
  height: 3.5rem;
  overflow: hidden;
}

.tip {
  position: absolute;
  bottom: 0.3rem;
  right: 0;
  z-index: 1;
  width: 60px;
  height: 24px;
  background-color: rgba(0, 0, 0, 0.2);
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  border-bottom-left-radius: 12px;
  border-top-left-radius: 12px;

  span {
    &:nth-child(1) {
      font-size: 0.16rem;
    }
  }
}

.imagePreviewBox {
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background-color: #000;
}
```



```tsx
// src/views/detail/components/BannerComponent.tsx
import { Swiper, Image, ImageViewer } from 'antd-mobile';
import React, { FC, useState, useRef } from 'react';
import styled from './Banner.module.scss'
interface IBannnerComponentProps {
  banners: string []
  current: number
  onChange: Function
};

const BannnerComponent:FC<IBannnerComponentProps> = ({ banners, current, onChange }) => {
  const [visible, setVisible] = useState(false)
  const mySwiperRef = useRef(null)
  return (
    <div className={ styled.bannerBox }>
      <Swiper 
        ref = { mySwiperRef }
        defaultIndex={ current }
        onIndexChange = { (index: number) => {
          onChange(index)
        }}
      >
        {/* <Swiper.Item>
          <Image src="" />
        </Swiper.Item> */}
        {
          banners && banners.map((item, index) => {
            return (
              <Swiper.Item key = { index } onClick = { () => {
                setVisible(true)
              }}>
                <Image src = { item } />
              </Swiper.Item>
            )
          })
        }
      </Swiper>
      <div className={ styled.tip }>
        <span>{ current + 1 }</span> / <span>{ banners.length }</span>
      </div>
      {
        visible ? <ImageViewer.Multi
          images={banners}
          visible={visible}
          defaultIndex={current}
          onIndexChange = {(index: number) => {
            (mySwiperRef.current! as any).swipeTo(index) // 预览图片时 底片也跟着改变
            onChange(index)
          }}
          onClose={() => {
            setVisible(false)
          }}
        /> : null
      }
    </div>
  )
};

export default BannnerComponent;
```

```ts
// src/views/detail/components/index.ts
export { default as BannerComponent } from './BannerComponent'
```

```tsx
// src/views/detail/Index.tsx
import React, { FC, useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { getDetailData } from '@/api/detail'

import { BannerComponent } from './components'
interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  const { proid } = useParams()
  console.log(proid)


  const [obj, setObj] = useState({
    banners: [],
    proname: '',
    originprice: 0,
    discount: 0,
    brand: '',
    category: '',
    sales: 0,
    issale: 1
  })
  useEffect(() => {
    getDetailData(proid!).then(res => {
      console.log(res.data.data)
      setObj({
        banners: res.data.data.banners[0].split(','),
        proname: res.data.data.proname,
        originprice:  res.data.data.originprice,
        discount: res.data.data.discount,
        brand:  res.data.data.brand,
        category: res.data.data.category,
        sales: res.data.data.sales,
        issale: res.data.data.issale
      })
    })
  }, [proid])

  const [current, setCurrent] = useState(0)
  const onChange = (index: number) => {
    setCurrent(index)
  }
  return (
    <>
      <header className="header">Detail header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent banners = { obj.banners } current = { current } onChange = { onChange }/>
      </div>
    </>
  )
};

export default Detail;
```



> 大图预览遇到了 点击穿透问题
>
> * 使用tap事件代替 click 事件（tap事件原生不支持，需要额外引入插件）
>
> * 使用mouse事件代替click 事件
>
> * 使用fastclick事件代替click事件（一般页面引入插件即可）
>
>   https://antd-mobile-v2.surge.sh/docs/react/introduce-cn
>
>   > 引入 [FastClick](https://github.com/ftlabs/fastclick) 并且设置 html `meta` (更多参考 [#576](https://github.com/ant-design/ant-design-mobile/issues/576))
>   >
>   > 引入 Promise 的 fallback 支持 (部分安卓手机不支持 Promise)
>
> ```html
> <!DOCTYPE html>
> <html>
> <head>
>   <!-- set `maximum-scale` for some compatibility issues -->
>   <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no" />
>   <script src="https://as.alipayobjects.com/g/component/fastclick/1.0.6/fastclick.js"></script>
>   <script>
>     if ('addEventListener' in document) {
>       document.addEventListener('DOMContentLoaded', function() {
>         FastClick.attach(document.body);
>       }, false);
>     }
>     if(!window.Promise) {
>       document.writeln('<script src="https://as.alipayobjects.com/g/component/es6-promise/3.2.2/es6-promise.min.js"'+'>'+'<'+'/'+'script>');
>     }
>   </script>
> </head>
> <body></body>
> </html>
> ```
>
> 

`public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <!--
      manifest.json provides metadata used when your web app is installed on a
      user's mobile device or desktop. See https://developers.google.com/web/fundamentals/web-app-manifest/
    -->
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <!--
      Notice the use of %PUBLIC_URL% in the tags above.
      It will be replaced with the URL of the `public` folder during the build.
      Only files inside the `public` folder can be referenced from the HTML.

      Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
      work correctly both with client-side routing and a non-root public URL.
      Learn how to configure a non-root public URL by running `npm run build`.
    -->
    <title>React App</title>
    <link rel="stylesheet" href="//at.alicdn.com/t/c/font_3665887_2jcst3szxcd.css">
    <script src="https://as.alipayobjects.com/g/component/fastclick/1.0.6/fastclick.js"></script>
    <script>
      if ('addEventListener' in document) {
        document.addEventListener('DOMContentLoaded', function() {
          FastClick.attach(document.body);
        }, false);
      }
      if(!window.Promise) {
        document.writeln('<script src="https://as.alipayobjects.com/g/component/es6-promise/3.2.2/es6-promise.min.js"'+'>'+'<'+'/'+'script>');
      }
    </script>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.

      You can add webfonts, meta tags, or analytics to this file.
      The build step will place the bundled scripts into the <body> tag.

      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
  </body>
</html>

```



### 12.5.2 构建产品详细信息

```scss
// src/views/detail/components/ProInfo.scss
.proInfo {
  background-color: #fff;
  padding: 15px;
  border-bottom-right-radius: 16px;
  border-bottom-left-radius: 16px;
  .priceBox {
    span {
      line-height: 32px;
      &:nth-child(1) {
        font-size: 24px;
        color: #f66;
      }
      &:nth-child(2) {
        float: right;
      }
    }
  }
  .proName {
    font-weight: bold;
    font-size: 0.14rem;
    .adm-tag {
      margin-right: 5px;
    }
  }
}
```

```tsx
// src/views/detail/components/ProInfoComponent.tsx
import React, { FC } from 'react';
import { Tag } from 'antd-mobile'
import './ProInfo.scss';
interface IProInfoCompomnentProps {
  originprice: number
  sales:number
  brand: string
  category: string
  proname: string
};

const ProInfoCompomnent:FC<IProInfoCompomnentProps> = ({ originprice, sales, brand, category, proname }) => {
  return (
    <div className="proInfo">
      <div className="priceBox">
        <span>￥{ originprice }</span>
        <span>销量：{ sales }</span>
      </div>
      <div className="proName">
        <Tag color='danger'>{ brand }</Tag>
        <Tag color='primary'>{ category }</Tag>
        <span>{ proname }</span>
      </div>
    </div>
  )
};

export default ProInfoCompomnent;
```

```ts
// src/views/detail/components/index.ts
export { default as BannerComponent } from './BannerComponent'
export { default as ProInfoComponent } from './ProInfoComponent'
```

```tsx
// src/views/detail/Index.tsx
import React, { FC, useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { getDetailData } from '@/api/detail'

import { BannerComponent, ProInfoComponent } from './components'
interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  const { proid } = useParams()
  console.log(proid)


  const [obj, setObj] = useState({
    banners: [],
    proname: '',
    originprice: 0,
    discount: 0,
    brand: '',
    category: '',
    sales: 0,
    issale: 1
  })
  useEffect(() => {
    getDetailData(proid!).then(res => {
      console.log(res.data.data)
      setObj({
        banners: res.data.data.banners[0].split(','),
        proname: res.data.data.proname,
        originprice:  res.data.data.originprice,
        discount: res.data.data.discount,
        brand:  res.data.data.brand,
        category: res.data.data.category,
        sales: res.data.data.sales,
        issale: res.data.data.issale
      })
    })
  }, [proid])

  const [current, setCurrent] = useState(0)
  const onChange = (index: number) => {
    setCurrent(index)
  }
  return (
    <>
      <header className="header">Detail header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent banners = { obj.banners } current = { current } onChange = { onChange }/>
        {/* 产品详情 */}
        <ProInfoComponent 
          originprice = { obj.originprice } 
          sales = { obj.sales } 
          brand = { obj.brand } 
          category = { obj.category }
          proname = { obj.proname }
        />
      </div>
    </>
  )
};

export default Detail;
```

### 12.5.3 猜你喜欢

```tsx
// src/views/detail/Index.tsx
import React, { FC, useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { getDetailData, getDetailRecommendData } from '@/api/detail'// ++++
import { Divider} from 'antd-mobile' // ++++
import { BannerComponent, ProInfoComponent } from './components'
import { ProComponent } from '@/views/home/components' // ++++
interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  const { proid } = useParams()
  console.log(proid)


  const [obj, setObj] = useState({
    banners: [],
    proname: '',
    originprice: 0,
    discount: 0,
    brand: '',
    category: '',
    sales: 0,
    issale: 1
  })
  useEffect(() => {
    getDetailData(proid!).then(res => {
      console.log(res.data.data)
      setObj({
        banners: res.data.data.banners[0].split(','),
        proname: res.data.data.proname,
        originprice:  res.data.data.originprice,
        discount: res.data.data.discount,
        brand:  res.data.data.brand,
        category: res.data.data.category,
        sales: res.data.data.sales,
        issale: res.data.data.issale
      })
    })
  }, [proid])

  const [current, setCurrent] = useState(0)
  const onChange = (index: number) => {
    setCurrent(index)
  }


  const [proList, setProList] = useState([]) // ++++
  useEffect(() => { // ++++
    getDetailRecommendData().then(res => setProList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">Detail header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent banners = { obj.banners } current = { current } onChange = { onChange }/>
        {/* 产品详情 */}
        <ProInfoComponent 
          originprice = { obj.originprice } 
          sales = { obj.sales } 
          brand = { obj.brand } 
          category = { obj.category }
          proname = { obj.proname }
        />
        {/* 猜你喜欢 // ++++ */}
        <Divider
          style={{
            color: '#1677ff',
            borderColor: '#1677ff',
            borderStyle: 'dashed',
          }}
        >
          猜你喜欢
        </Divider>
        <ProComponent list = { proList } />
      </div>
    </>
  )
};

export default Detail;
```



### 12.5.4 详情底部

```scss
// src/views/detail/components/action/index.scss
.action_box{
  border-top: 0.01rem solid #bfbfbf55;
  position: fixed;
  bottom: 0;
  display: flex;
  align-items: center;
  width: 100%;
  height: 0.5rem;
  .action_item{
    padding-left: 0.1rem;
    width: 0.4rem;
    height: 100%;
    justify-content: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    .antd-mobile-icon {
      width: 0.24rem;
      height: 0.24rem;
    }
  }
  .button{
    margin: 0 0.1rem;
    height: 0.36rem;
    border-radius: 0.2rem;
    overflow: hidden;
    flex: 1;
    display: flex;
    justify-content: space-between;
    align-items: center;
    Button{
      span {
        font-size: 0.14rem;
      }
      height: 100%;
      border-radius: 0;
      border: none;
      flex: 1;
    }
  }
}
```

```tsx
// src/views/detail/components/action/index.tsx
// 作者：曹继承
import React, { FC } from "react";
import {
  StarOutline,
  ShopbagOutline,
  MessageOutline,
  StarFill,
} from "antd-mobile-icons";
import "./index.scss";
import { Button } from "antd-mobile";
interface IActionBarProps {
  BgColor?:string;
  CartText?: string;
  CartTextColor?: string;
  buyTextColor?: string;
  BuyText?: string;
  CartColor?: string;
  buyColor?: string;
  cart?: boolean;
  buy?: boolean;
  Customer?: boolean;
  onCustomerClick?: any;
  shop?: boolean;
  onShopClick?: any;
  star?: boolean;
  onStar?: boolean;
  onStarColor?: string;
  onStarClick?: any;
  onCartClick?: any;
  onBuyClick?: any;
}

const ActionBar: FC<IActionBarProps> = ({
  BgColor = "#fff",
  CartText = "加入购物车", //购物车文字默认值
  BuyText = "立即购买", //默认值
  CartColor = "#7233d9", //购物车按钮颜色默认值
  CartTextColor = "#fff", //购物车字体默认值
  buyColor = "#ff64fe", //默认值
  buyTextColor = "#fff", //默认值
  onStarColor = "#76c6b8",//收藏选中颜色默认值
  cart = true, //购物车按钮是否存在
  buy = true, //购买按钮是否存在
  Customer = true, //客服按钮是否存在
  onStar = false, //星星选中状态
  shop = true, //购物袋按钮是否存在
  star = true,//收藏按钮是否存在
  onCustomerClick = () => {},
  onShopClick = () => {},
  onStarClick = () => {},
  onCartClick = () => {},
  onBuyClick = () => {},
}) => {
  return (
    <>
      <div className="action_box" style={{backgroundColor:BgColor}}>
        <div
          className="action_Customer action_item"
          style={{ display: Customer ? "" : "none" }}
          onClick={(event) => onCustomerClick()}
        >
          <MessageOutline />
          <p>客服</p>
        </div>
        <div
          className="action_shop action_item"
          style={{ display: shop ? "" : "none" }}
          onClick={(event) => onShopClick()}
        >
          <ShopbagOutline />
          <p>购物车</p>
        </div>
        <div
          className="action_star action_item"
          style={{ display: star ? "" : "none" }}
          onClick={(event) => {
            onStarClick();
          }}
        >
          {(function fn() {
            return onStar ? <StarFill color={onStarColor} /> : <StarOutline />;
          })()}
          <p>收藏</p>
        </div>
        <div className="button">
          <Button
            className="action_goBuy"
            style={{
              color: buyTextColor,
              backgroundColor: buyColor,
              display: buy ? "" : "none",
            }}
            onClick={() => onBuyClick()}
          >
            {BuyText}
          </Button>
          <Button
            className="action_addCart"
            style={{
              color: CartTextColor,
              backgroundColor: CartColor,
              display: cart ? "" : "none",
            }}
            onClick={() => onCartClick()}
          >
            {CartText}
          </Button>
        </div>
      </div>
    </>
  );
};

export default ActionBar;

```

```tsx
// src/views/detail/components/index.ts
export { default as BannerComponent } from './BannerComponent'
export { default as ProInfoComponent } from './ProInfoComponent'
export { default as HeaderComponent } from './HeaderComponent'
export { default as ActionBar } from './action/Index'

```

```tsx
// src/views/detail/Index.tsx
import React, { FC, useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { getDetailData, getDetailRecommendData } from '@/api/detail'
import { Divider} from 'antd-mobile'
import { BannerComponent, ProInfoComponent, ActionBar } from './components'
import { ProComponent } from '@/views/home/components'
interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  const { proid } = useParams()
  console.log(proid)


  const [obj, setObj] = useState({
    banners: [],
    proname: '',
    originprice: 0,
    discount: 0,
    brand: '',
    category: '',
    sales: 0,
    issale: 1
  })
  useEffect(() => {
    getDetailData(proid!).then(res => {
      console.log(res.data.data)
      setObj({
        banners: res.data.data.banners[0].split(','),
        proname: res.data.data.proname,
        originprice:  res.data.data.originprice,
        discount: res.data.data.discount,
        brand:  res.data.data.brand,
        category: res.data.data.category,
        sales: res.data.data.sales,
        issale: res.data.data.issale
      })
      setCurrent(0)
    })
  }, [proid])

  const [current, setCurrent] = useState(0)
  const onChange = (index: number) => {
    setCurrent(index)
  }


  const [proList, setProList] = useState([])
  useEffect(() => {
    getDetailRecommendData().then(res => setProList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">Detail header</header>
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent banners = { obj.banners } current = { current } onChange = { onChange }/>
        {/* 产品详情 */}
        <ProInfoComponent 
          originprice = { obj.originprice } 
          sales = { obj.sales } 
          brand = { obj.brand } 
          category = { obj.category }
          proname = { obj.proname }
        />
        {/* 猜你喜欢 */}
        <Divider
          style={{
            color: '#1677ff',
            borderColor: '#1677ff',
            borderStyle: 'dashed',
          }}
        >
          猜你喜欢
        </Divider>
        <ProComponent list = { proList } />

        {/* 底部选项 */}
        <ActionBar />
      </div>
    </>
  )
};

export default Detail;
```

### 12.5.5 详情头部

```scss
// src/views/detail/components/Header.scss
.myHeader {
  user-select: none;
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 999;
  .header1 {
    height: 0.44rem;
    padding: 6px 15px;
    box-sizing: border-box;
    ul {
      width: 100%;
      height: 100%;
      display: flex;
      li {
        &:nth-child(1), &:nth-child(3) {
          font-size: 32px;
          width: 44px;
        }
        &:nth-child(2) {
          flex: 1;
        }
      }
    }
    
  }
  .header2 {
    height: 0.44rem;
    padding: 6px 15px;
    box-sizing: border-box;
    background-color: #fff;
    ul {
      width: 100%;
      height: 100%;
      display: flex;
      li {
        &:nth-child(1), &:nth-child(3) {
          font-size: 24px;
          width: 44px;
        }
        &:nth-child(2) {
          flex: 1;
          display: flex;
          span {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
          }
        }
      }
    }
    
  }
}
```

```tsx
// src/views/detail/components/HeaderComponent.tsx
import React, { FC } from 'react';
import {LeftOutline, MoreOutline} from 'antd-mobile-icons' 
import './Header.scss'
import { useNavigate } from 'react-router-dom';
interface IHeaderComponentProps {
  
};

const HeaderComponent:FC<IHeaderComponentProps> = ({}) => {
  const navigate = useNavigate()
  return (
    <div className='myHeader'>
      <header className="header2" v-show="scrollTop > 300">
        <ul>
          <li className="left" onClick={ () => navigate(-1) } >
            <LeftOutline />
          </li>
          <li className="middle">
            <span>详情</span>
            <span>推荐</span>
          </li>
          <li className="right">
            <MoreOutline />
          </li>
        </ul>
      </header>
    </div>
  )
};

export default HeaderComponent;
```

```ts
// src/views/detail/components/index.ts
export { default as BannerComponent } from './BannerComponent'
export { default as ProInfoComponent } from './ProInfoComponent'
export { default as HeaderComponent } from './HeaderComponent'
```

```tsx
// src/views/detail/Index.tsx
import React, { FC, useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { getDetailData, getDetailRecommendData } from '@/api/detail'
import { Divider} from 'antd-mobile'
import { BannerComponent, ProInfoComponent, HeaderComponent } from './components'
import { ProComponent } from '@/views/home/components'
interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  const { proid } = useParams()
  console.log(proid)


  const [obj, setObj] = useState({
    banners: [],
    proname: '',
    originprice: 0,
    discount: 0,
    brand: '',
    category: '',
    sales: 0,
    issale: 1
  })
  useEffect(() => {
    getDetailData(proid!).then(res => {
      console.log(res.data.data)
      setObj({
        banners: res.data.data.banners[0].split(','),
        proname: res.data.data.proname,
        originprice:  res.data.data.originprice,
        discount: res.data.data.discount,
        brand:  res.data.data.brand,
        category: res.data.data.category,
        sales: res.data.data.sales,
        issale: res.data.data.issale
      })
    })
  }, [proid])

  const [current, setCurrent] = useState(0)
  const onChange = (index: number) => {
    setCurrent(index)
  }


  const [proList, setProList] = useState([])
  useEffect(() => {
    getDetailRecommendData().then(res => setProList(res.data.data))
  }, [])
  return (
    <>
      {/* <header className="header">Detail header</header> */}
      <HeaderComponent />
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent banners = { obj.banners } current = { current } onChange = { onChange }/>
        {/* 产品详情 */}
        <ProInfoComponent 
          originprice = { obj.originprice } 
          sales = { obj.sales } 
          brand = { obj.brand } 
          category = { obj.category }
          proname = { obj.proname }
        />
        {/* 猜你喜欢 */}
        <Divider
          style={{
            color: '#1677ff',
            borderColor: '#1677ff',
            borderStyle: 'dashed',
          }}
        >
          猜你喜欢
        </Divider>
        <ProComponent list = { proList } />

        {/* 底部选项 */}
      </div>
    </>
  )
};

export default Detail;
```

# 13.登录功能

其实要登录需要先注册，借用vue项目的注册账户，此处直接实现登录

## 13.1 构建注册组件

```tsx
// src/views/register/components/Step1.tsx
import React, { FC } from 'react';
import { Button } from 'antd-mobile'
import { useNavigate } from 'react-router-dom';
interface IRegisterStep1Props {
  
};

const RegisterStep1:FC<IRegisterStep1Props> = () => {
  const navigate = useNavigate()
  return (
    <>
      <h1>注册第一步</h1>
      <Button color='danger' onClick={ () => navigate('/register/step2')}>下一步</Button>
    </>
  )
};

export default RegisterStep1;
```

```tsx
// src/views/register/components/Step2.tsx
import React, { FC } from 'react';
import { Button } from 'antd-mobile'
import { useNavigate } from 'react-router-dom';
interface IRegisterStep2Props {
  
};

const RegisterStep2:FC<IRegisterStep2Props> = () => {
  const navigate = useNavigate()
  return (
    <>
      <h1>注册第二步</h1>
      <Button color='danger' onClick={ () => navigate('/register/step3') }>下一步</Button>
    </>
  )
};

export default RegisterStep2;
```

```tsx
// src/views/register/components/Step3.tsx
import React, { FC } from 'react';
import { Button } from 'antd-mobile'
import { useNavigate } from 'react-router-dom';
interface IRegisterStep3Props {
  
};

const RegisterStep3:FC<IRegisterStep3Props> = () => {
  const navigate = useNavigate()
  return (
    <>
      <h1>注册第三步</h1>
      <Button color='danger' onClick={ () => navigate(-3) }>完成</Button>
    </>
  )
};

export default RegisterStep3;
```

```tsx
// src/views/register/components/index.ts
export { default as Step1 } from './Step1'
export { default as Step2 } from './Step2'
export { default as Step3 } from './Step3'
```

```tsx
// src/views/Register/Index.tsx
import React, { FC } from 'react';
import { Outlet } from 'react-router-dom';

interface IRegisterProps {
  
};

const Register:FC<IRegisterProps> = () => {
  return (
    <>
      <header className="header">Register header</header>
      <div className="content">
        <Outlet />
      </div>
    </>
  )
};

export default Register;
```

## 13.2 构建登录组件

```tsx
// src/views/Login/Index.tsx
import React, { FC } from 'react';

interface ILoginProps {
  
};

const Login:FC<ILoginProps> = () => {
  return (
    <>
      <header className="header">Login header</header>
      <div className="content">Login content</div>
    </>
  )
};

export default Login;
```

## 13.3 设置登录以及注册路由

注册路由使用嵌套路由

```tsx
// src/App.tsx
import React, { FC } from 'react';
// BrowserRouter  /  /kind  /cart /user
// HashRouter  /#/ /#/kind /#/cart /#/user
import { Routes, Route, Navigate } from 'react-router-dom'
import '@/App.scss';
import Cart from '@/views/cart/Index';
import Home from '@/views/home/Index';
import Kind from '@/views/kind/Index';
import User from '@/views/user/Index';
import Detail from '@/views/detail/Index';
import Footer from '@/components/Footer';
import Login from './views/login/Index';
import Register from './views/register/Index';
import { Step1, Step2, Step3 } from './views/register/components';
interface IAppProps {
};

const App: FC<IAppProps> = () => {
 
  return (
    <div className='container'>
      <Routes>
        <Route path="/" element={ <Navigate to="/home" replace={true}/>} />
        {/* <Route path="/home" element={<Home />} />
        <Route path="/kind" element={<Kind />} />
        <Route path="/cart" element={<Cart />} />
        <Route path="/user" element ={<User />} /> */}
        <Route path="/home" element={<><Home /><Footer /></>} />
        <Route path="/kind" element={<><Kind /><Footer /></>} />
        <Route path="/cart" element={<><Cart /><Footer /></>} />
        <Route path="/user" element ={<><User /><Footer /></>} />
        <Route path="/detail/:proid" element ={<Detail />} />
        <Route path="/login" element ={<Login />} />
        <Route path="/register" element ={<Register />}>
          <Route index element = {<Navigate to="/register/step1" replace={true} /> } />
          <Route path='/register/step1' element = { <Step1 /> } />
          <Route path='/register/step2' element = { <Step2 /> } />
          <Route path='/register/step3' element = { <Step3 /> } />
        </Route>
      </Routes>
      {/* <footer className="footer"><Footer /></footer> */}
      {/* <Routes>
        <Route path="/home" element={<Footer />} />
        <Route path="/kind" element={<Footer />} />
        <Route path="/cart" element={<Footer />} />
        <Route path="/user" element ={<Footer />} />
      </Routes> */}
      <div className='landscape-tip'>
        请将屏幕竖向浏览
      </div>
    </div>
  )
};

export default App;
```

## 13.4 修改登录组件

```tsx
// src/views/Login/Index.tsx
import React, { FC,useMemo,useState } from 'react';
import { Form, Input, Button } from 'antd-mobile'
interface ILoginProps {
  
};

const Login:FC<ILoginProps> = () => {
  const [loginname, setLoginname] = useState('')
  const [password, setPassword] = useState('')
  const onFinish = (values: any) => {
    console.log(values)
  }
  // 用来后期修改按钮可点状态
  const onValuesChange = (changedValues: any, allValues: any) => {
    console.log(changedValues, allValues)
    // 方法1：获取当前输入框的key值，修改状态
    // console.log(Object.keys(changedValues)) 
    // switch (Object.keys(changedValues)[0]) {
    //   case 'loginname':
    //     console.log(1)
    //     setLoginname(allValues.loginname)
    //     break;
    //   case 'password':
    //     console.log(2)
    //     setPassword(allValues.password)
    //     break;
    // }

    // 方法2：判定 allValues 不为undefined
    if (allValues.loginname === undefined) {
      setPassword(allValues.password)
    } else if (allValues.password === undefined) {
      setLoginname(allValues.loginname)
    } else {
      setLoginname(allValues.loginname)
      setPassword(allValues.password)
    }
    
    
  }

  const flag = useMemo(() => {
    return loginname === '' || password === ''
  }, [loginname, password])
  return (
    <>
      <header className="header">Login header</header>
      <div className="content">
        <Form 
          layout='horizontal'
          footer={
            <Button block type='submit' color='danger' disabled = { flag } size='large'>
              登录
            </Button>
          }
          onFinish = { onFinish }
          onValuesChange = { onValuesChange }
        >
          <Form.Item  name='loginname'>
            <Input placeholder='手机号/用户名/邮箱' clearable />
          </Form.Item>
          <Form.Item  name='password'>
            <Input placeholder='密码' clearable type='password' />
          </Form.Item>
        </Form>
      </div>
    </>
  )
};

export default Login;
```

## 13.5 封装用户数据请求

```ts
// src/api/user.ts
import request from './../utils/request'

// 检测手机号是否被注册过
export function doCheckPhone (params: { tel: string }) {
  return request.post('/user/docheckphone', params)
}

// 发送短信验证码
export function doSendMsgCode (params: { tel: string }) {
  return request.post('/user/dosendmsgcode', params)
}

// 验证验证码
export function doCheckCode (params: { tel: string, telcode: string }) {
  return request.post('/user/docheckcode', params)
}

// 设置密码完成注册
export function doFinishRegister (params: { tel: string, password: string }) {
  return request.post('/user/dofinishregister', params)
}

// 登录
export function doLogin (params: { loginname: string, password: string }) {
  return request.post('/user/login', params)
}
```

## 13.6 实现登录功能

```tsx
// src/views/Login/Index.tsx
import React, { FC,useMemo,useState } from 'react';
import { Form, Input, Button, Toast, Modal, NavBar } from 'antd-mobile'
import { doLogin } from '@/api/user';
import { useNavigate, Link } from 'react-router-dom';
interface ILoginProps {
  
};

const Login:FC<ILoginProps> = () => {
  const navigate = useNavigate()

  const [loginname, setLoginname] = useState('')
  const [password, setPassword] = useState('')
  const onFinish = (values: any) => {
    console.log(values)
    doLogin(values).then(res => {
      console.log(res.data)
      if (res.data.code === '10011') {
        Toast.show({
          content: '密码错误',
          duration: 1000
        })
      } else if (res.data.code === '10010') {
        Modal.confirm({
          content: <div>该用户还未注册</div>,
          confirmText: '立即注册',
          onConfirm: () => {
            navigate('/register')
          }
        })
      } else {
        Toast.show({
          content: '登录成功',
          duration: 1000
        })
        // 保存数据到本地
        // 保存数据到状态管理器
        // 返回上一页
        localStorage.setItem('loginState', String(true))
        localStorage.setItem('userid', res.data.data.userid)
        localStorage.setItem('token', res.data.data.token)

        navigate(-1)

      }
    })
  }
  // 用来后期修改按钮可点状态
  const onValuesChange = (changedValues: any, allValues: any) => {
    console.log(changedValues, allValues)
    // 方法1：获取当前输入框的key值，修改状态
    // console.log(Object.keys(changedValues)) 
    // switch (Object.keys(changedValues)[0]) {
    //   case 'loginname':
    //     console.log(1)
    //     setLoginname(allValues.loginname)
    //     break;
    //   case 'password':
    //     console.log(2)
    //     setPassword(allValues.password)
    //     break;
    // }

    // 方法2：判定 allValues 不为undefined
    if (allValues.loginname === undefined) {
      setPassword(allValues.password)
    } else if (allValues.password === undefined) {
      setLoginname(allValues.loginname)
    } else {
      setLoginname(allValues.loginname)
      setPassword(allValues.password)
    }
    
    
  }

  const flag = useMemo(() => {
    return loginname === '' || password === ''
  }, [loginname, password])
  return (
    <>
      <header className="header">
        <NavBar
          style={{
            '--height': '0.44rem',
            '--border-bottom': '1px #eee solid',
            color: '#fff'
          }}
          onBack={ () => navigate(-1) }
        >
          登录
        </NavBar>
      </header>
      <div className="content">
        <Form 
          layout='horizontal'
          footer={
            <Button block type='submit' color='danger' disabled = { flag } size='large'>
              登录
            </Button>
          }
          onFinish = { onFinish }
          onValuesChange = { onValuesChange }
        >
          <Form.Item  name='loginname'>
            <Input placeholder='手机号/用户名/邮箱' clearable />
          </Form.Item>
          <Form.Item  name='password'>
            <Input placeholder='密码' clearable type='password' />
          </Form.Item>
        </Form>
        <Link to="/register">手机号快速注册</Link>
      </div>
    </>
  )
};

export default Login;
```

# 14.mobx状态管理器

https://cn.mobx.js.org/ --- v5

https://www.mobxjs.com/   -v6

## 14.1 安装

```sh
$ cnpm i mobx mobx-react -S
```

## 14.2 创建状态管理器

```ts
// src/store/modules/user.ts
import { makeAutoObservable } from "mobx";

class UserStore {
  // 定义初始化状态
  loginState: boolean = Boolean(localStorage.getItem('loginState'))
  userid: string = localStorage.getItem('userid') || ''
  token: string = localStorage.getItem('token') || ''

  constructor () {
    // 讲此类设置为可被观察的
    makeAutoObservable(this)
  }

  // 修改状态
  changeLoginState (action: { payload: any; }) {
    this.loginState = action.payload
    // console.log(11, this.loginState)
  }
  changeUserId ( action: { payload: any; }) {
    this.userid = action.payload
    // console.log(2, this.userid )
  }
  changeToken (action: { payload: any; }) {
    this.token = action.payload
    // console.log(3, this.token)
  }
}

export default UserStore
```

```ts
// src/store/index.ts
import { makeAutoObservable } from 'mobx'
import UserStore from './modules/user'
export class Store {
  user
  constructor () {
    makeAutoObservable(this)
    this.user = new UserStore()
  }
}

const store = new Store()

export default store
```

```ts
import React from 'react';
import ReactDOM from 'react-dom/client';
import ErrorBoundary from '@/ErrorBundary'; // 错误边界
import App from '@/App';
import reportWebVitals from '@/reportWebVitals';
import { BrowserRouter } from 'react-router-dom'
import { Provider } from 'mobx-react' // ++++++
import store from '@/store' // ++++++
import '@/main.scss';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);

// 开发环境下，严格模式会触发组件的二次装载，修改状态时尽量不要使用 函数形式
root.render(
  <React.StrictMode>
    <ErrorBoundary>
      <BrowserRouter>
        <Provider store = { store }>
          <App />
        </Provider>
      </BrowserRouter>
    </ErrorBoundary>
  </React.StrictMode>
);

reportWebVitals();

```

```tsx
// src/views/Login/Index.tsx
import React, { FC,useMemo,useState } from 'react';
import { Form, Input, Button, Toast, Modal, NavBar } from 'antd-mobile'
import { doLogin } from '@/api/user';
import { useNavigate, Link } from 'react-router-dom';
import { inject, observer } from 'mobx-react'; // +++++++++
interface ILoginProps {
  
};

const Login:FC<ILoginProps> = (props: any) => {
  console.log('login', props)
  const navigate = useNavigate()

  const [loginname, setLoginname] = useState('')
  const [password, setPassword] = useState('')
  const onFinish = (values: any) => {
    console.log(values)
    doLogin(values).then(res => {
      console.log(res.data)
      if (res.data.code === '10011') {
        Toast.show({
          content: '密码错误',
          duration: 1000
        })
      } else if (res.data.code === '10010') {
        Modal.confirm({
          content: <div>该用户还未注册</div>,
          confirmText: '立即注册',
          onConfirm: () => {
            navigate('/register')
          }
        })
      } else {
        Toast.show({
          content: '登录成功',
          duration: 1000
        })
        // 保存数据到本地
        // 保存数据到状态管理器
        // 返回上一页
        localStorage.setItem('loginState', String(true))
        localStorage.setItem('userid', res.data.data.userid)
        localStorage.setItem('token', res.data.data.token)
		
        // +++++++++
        props.store.user.changeLoginState({ payload: true })
        props.store.user.changeUserId({ payload: res.data.data.userid })
        props.store.user.changeToken({ payload: res.data.data.token })

        navigate(-1)

      }
    })
  }
  // 用来后期修改按钮可点状态
  const onValuesChange = (changedValues: any, allValues: any) => {
    console.log(changedValues, allValues)
    // 方法1：获取当前输入框的key值，修改状态
    // console.log(Object.keys(changedValues)) 
    // switch (Object.keys(changedValues)[0]) {
    //   case 'loginname':
    //     console.log(1)
    //     setLoginname(allValues.loginname)
    //     break;
    //   case 'password':
    //     console.log(2)
    //     setPassword(allValues.password)
    //     break;
    // }

    // 方法2：判定 allValues 不为undefined
    if (allValues.loginname === undefined) {
      setPassword(allValues.password)
    } else if (allValues.password === undefined) {
      setLoginname(allValues.loginname)
    } else {
      setLoginname(allValues.loginname)
      setPassword(allValues.password)
    }
    
    
  }

  const flag = useMemo(() => {
    return loginname === '' || password === ''
  }, [loginname, password])
  return (
    <>
      <header className="header">
        <NavBar
          style={{
            '--height': '0.44rem',
            '--border-bottom': '1px #eee solid',
            color: '#fff'
          }}
          onBack={ () => navigate(-1) }
        >
          登录
        </NavBar>
      </header>
      <div className="content">
        <Form 
          layout='horizontal'
          footer={
            <Button block type='submit' color='danger' disabled = { flag } size='large'>
              登录
            </Button>
          }
          onFinish = { onFinish }
          onValuesChange = { onValuesChange }
        >
          <Form.Item  name='loginname'>
            <Input placeholder='手机号/用户名/邮箱' clearable />
          </Form.Item>
          <Form.Item  name='password'>
            <Input placeholder='密码' clearable type='password' />
          </Form.Item>
        </Form>
        <Link to="/register">手机号快速注册</Link>
      </div>
    </>
  )
};

export default inject('store')(observer(Login)); // +++++++++
```

```tsx
// src/views/cart/Index.tsx
import React, { FC } from 'react';
import { observer, inject } from 'mobx-react'
// import { Store } from '@/store';
interface ICartProps {
  // store: any
};

const Cart:FC<ICartProps> = (props: any) => {
  console.log(props.store.user)
  return (
    <>
      <header className="header">cart header</header>
      <div className="content">cart content</div>
    </>
  )
};
// Cart 单独组件
// observer(Cart) // 将组件设置为观察者
// inject('store')(observer(Cart)) // 给组件提供状态，后代组件获取到祖先组件的状态
export default inject('store')(observer(Cart));
```

# 15.加入购物车

## 15.1 封装数据请求

给请求添加token

```ts
// src/utils/request.ts
// 1.引入axios
import axios from 'axios'

// 2.项目环境
// 生产环境 process.env.NODE_ENV === 'production'  cnpm run build
// 测试环境 ？
// 开发环境 process.env.NODE_ENV === 'devlopment   cnpm run start
const isDev = process.env.NODE_ENV === 'development'

// 3.给axios添加默认选项
// 设置跨域是否需要携带凭证
axios.defaults.withCredentials = false
// axios.defaults.timeout = 6000 // 6秒超时时间
// axios.defaults.baseURL = isDev ? 'http://121.89.205.189:3001/api' : 'http://121.89.205.189:3001/api'

// 4.自定义axios
const ins = axios.create({
  baseURL: isDev ? 'http://121.89.205.189:3001/api' : 'http://121.89.205.189:3001/api',
  timeout: 6000
})


// 5.设置拦截器
// 请求的拦截器 所有的请求在开始之前先执行请求拦截器，再执行自己的请求
ins.interceptors.request.use((config) => {
  // 设置请求的loading显示 --- 使用组件不必要  ----  js模块显示
  // 设置token，一般token传递给后端通过 请求头传递 config.headers.token = ''
  (config.headers!.token as string) = localStorage.getItem('token')! // +++++
  return config
}, (err) => {
  return Promise.reject(err)
})

// 响应拦截器 所有的接口返回值先执行响应拦截器，再返回自己的响应的数据
ins.interceptors.response.use((response: any) => {
  // 关闭loading动画  --- 使用组件不必要 ----  js模块隐藏
  // 验证token，如果验证通过，返回数，如果验证不通过，直接跳转到登录页面
  if (response.data.code === '10119') {
    window.location.href = "/login"
    return 
  } else {
    return response
  }
}, (err) => Promise.reject(err))

// 6.暴露自定义axios
export default ins
```

```ts
// src/api/cart.ts
import request from '../utils/request'

// 加入购物车
export function addCart (params: { userid: string, proid: string, num: number }) {
  return request.post('/cart/add', params)
}

// 获取购物车列表数据
export function getCartListData (params: { userid: string }) {
  return request.post('/cart/list', params)
}

// 删除某个用户的购物车的所有数据
export function removeAllData (params: { userid: string }) {
  return request.post('/cart/removeall', params)
}

// 删除某个用户的一条购物车的数据
export function removeOneData (params: { cartid: string }) {
  return request.post('/cart/remove', params)
}

// 更新某个用户的一条购物车的数据的选中状态
export function selectOneData (params: { cartid: string, flag: boolean }) {
  return request.post('/cart/selectone', params)
}

// 更新某个用户的购物车的所有数据的选中状态
export function selectAllData (params: { userid: string, type: boolean }) {
  return request.post('/cart/selectall', params)
}

// 更新某个用户的购物车的某个产品的数量
export function updateOneDataNum (params: { cartid: string, num: number }) {
  return request.post('/cart/updatenum', params)
}

// 推荐商品接口
export function getCartRecommendData () {
  return request.get('/pro/recommendlist')
}
```

## 15.2 加入购物车

底部组件提供了 各个选项的事件，需要按照组件提供写法去写

```tsx
// src/views/detail/Index.tsx
import React, { FC, useEffect, useState } from 'react';
import { useNavigate, useParams } from 'react-router-dom';
import { getDetailData, getDetailRecommendData } from '@/api/detail'
import { Divider, Toast } from 'antd-mobile'
import { BannerComponent, ProInfoComponent, HeaderComponent, ActionBar } from './components'
import { ProComponent } from '@/views/home/components'

import { observer, inject } from 'mobx-react'
import { addCart } from '@/api/cart';
interface IDetailProps {
};

const Detail:FC<IDetailProps> = (props: any) => {
  const { proid } = useParams()
  console.log(proid)


  const [obj, setObj] = useState({
    banners: [],
    proname: '',
    originprice: 0,
    discount: 0,
    brand: '',
    category: '',
    sales: 0,
    issale: 1
  })
  useEffect(() => {
    getDetailData(proid!).then(res => {
      console.log(res.data.data)
      setObj({
        banners: res.data.data.banners[0].split(','),
        proname: res.data.data.proname,
        originprice:  res.data.data.originprice,
        discount: res.data.data.discount,
        brand:  res.data.data.brand,
        category: res.data.data.category,
        sales: res.data.data.sales,
        issale: res.data.data.issale
      })
      setCurrent(0)
    })
  }, [proid])

  const [current, setCurrent] = useState(0)
  const onChange = (index: number) => {
    setCurrent(index)
  }


  const [proList, setProList] = useState([])
  useEffect(() => {
    getDetailRecommendData().then(res => setProList(res.data.data))
  }, [])

  const navigate = useNavigate()
  const addCartData = () => {
    const loginState = props.store.user.loginState
    const userid = props.store.user.userid
    if (loginState) {
      addCart({
        proid: proid!, userid, num: 1
      }).then(res => {
        if (res) {
          Toast.show({
            content: '加入购物车成功'
          })
        } else {
          Toast.show({
            content: '请先登录'
          })
        }
      })
    } else {
      navigate('/login')
    }
  }

  return (
    <>
      {/* <header className="header">Detail header</header> */}
      <HeaderComponent />
      <div className="content">
        {/* 轮播图 */}
        <BannerComponent banners = { obj.banners } current = { current } onChange = { onChange }/>
        {/* 产品详情 */}
        <ProInfoComponent 
          originprice = { obj.originprice } 
          sales = { obj.sales } 
          brand = { obj.brand } 
          category = { obj.category }
          proname = { obj.proname }
        />
        {/* 猜你喜欢 */}
        <Divider
          style={{
            color: '#1677ff',
            borderColor: '#1677ff',
            borderStyle: 'dashed',
          }}
        >
          猜你喜欢
        </Divider>
        <ProComponent list = { proList } />

        {/* 底部选项 */}
        <ActionBar
          onCartClick = { addCartData }
          onShopClick = { () => navigate('/cart')}
        ></ActionBar>
      </div>
    </>
  )
};

export default inject('store')(observer(Detail));
```

# 16.购物车相关

## 16.1 判断登录状态

实现类似于vue的导航守卫，定义路由时处理

```tsx
// src/App.tsx
import React, { FC } from 'react';
// BrowserRouter  /  /kind  /cart /user
// HashRouter  /#/ /#/kind /#/cart /#/user
import { Routes, Route, Navigate } from 'react-router-dom'
import '@/App.scss';
import Cart from '@/views/cart/Index';
import Home from '@/views/home/Index';
import Kind from '@/views/kind/Index';
import User from '@/views/user/Index';
import Detail from '@/views/detail/Index';
import Footer from '@/components/Footer';
import Login from './views/login/Index';
import Register from './views/register/Index';
import { Step1, Step2, Step3 } from './views/register/components';
import { inject, observer } from 'mobx-react'; // ++++++++
interface IAppProps {
};

const App: FC<IAppProps> = (props:any) => {
 
  return (
    <div className='container'>
      <Routes>
        <Route path="/" element={ <Navigate to="/home" replace={true}/>} />
        {/* <Route path="/home" element={<Home />} />
        <Route path="/kind" element={<Kind />} />
        <Route path="/cart" element={<Cart />} />
        <Route path="/user" element ={<User />} /> */}
        <Route path="/home" element={<><Home /><Footer /></>} />
        <Route path="/kind" element={<><Kind /><Footer /></>} />
        {/* <Route path="/cart" element={<><Cart /><Footer /></>} /> */}
        <Route path="/cart" element={ // ++++++++
          // 类似于导航守卫
          props.store.user.loginState ? <><Cart /><Footer /></> : <Navigate to='/login' replace={true}/>
        } />

        <Route path="/user" element ={<><User /><Footer /></>} />
        <Route path="/detail/:proid" element ={<Detail />} />
        <Route path="/login" element ={<Login />} />
        <Route path="/register" element ={<Register />}>
          <Route index element = {<Navigate to="/register/step1" replace={true} /> } />
          <Route path='/register/step1' element = { <Step1 /> } />
          <Route path='/register/step2' element = { <Step2 /> } />
          <Route path='/register/step3' element = { <Step3 /> } />
        </Route>
      </Routes>
      {/* <footer className="footer"><Footer /></footer> */}
      {/* <Routes>
        <Route path="/home" element={<Footer />} />
        <Route path="/kind" element={<Footer />} />
        <Route path="/cart" element={<Footer />} />
        <Route path="/user" element ={<Footer />} />
      </Routes> */}
      <div className='landscape-tip'>
        请将屏幕竖向浏览
      </div>
    </div>
  )
};

export default inject('store')(observer(App)) ; // ++++++++
```

## 16.2 判断是否有数据

```tsx
// src/views/cart/Index.tsx
import React, { FC, useCallback, useEffect, useMemo, useState } from 'react';
import { observer, inject } from 'mobx-react'
import { getCartListData } from '@/api/cart';
import { Button, Empty, List, Image, Ellipsis, Stepper } from 'antd-mobile';
import { GiftOutline } from 'antd-mobile-icons'
import { useNavigate } from 'react-router-dom';
interface IState {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
interface ICartProps {
};

const Cart:FC<ICartProps> = (props: any) => {
  const navigate = useNavigate()
 

  const [cartList, setCartList] = useState<IState[]>([])

  const getCartList = useCallback(() => {
    const userid = props.store.user.userid
    console.log('userid', userid)
    getCartListData({ userid: userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setCartList([])
      } else {
        setCartList(res.data.data)
      }
    })
  }, [props.store.user.userid])

  useEffect(() => {
    getCartList()
  }, [getCartList])

  const empty = useMemo(() => { return cartList.length === 0 }, [cartList])

  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        { 
          empty ? <Empty
                    style={{ padding: '64px 0' }}
                    image={
                      <GiftOutline
                        style={{
                          color: 'var(--adm-color-light)',
                          fontSize: 100,
                        }}
                      />
                    }
                    description= { 
                      <>
                        <p>购物车空空如也</p>
                        <Button
                          color="danger" 
                          style={{ marginTop: 30 }}
                          onClick = { () => navigate('/kind')}
                        >立即购物</Button>
                      </>
                    }
                  /> 
                : <List >
                  {
                    cartList && cartList.map(item => {
                      return (
                        <List.Item
                          key={item.cartid}
                          prefix={
                            <Image
                              src={item.img1}
                              style={{ borderRadius: 20 }}
                              fit='cover'
                              width={80}
                              height={80}
                            />
                          }
                          description = {
                            <div style={{ display: 'flex', justifyContent: 'space-between' }}>
                              <span>{item.originprice}</span>
                              <Stepper digits={0} value={item.num } onChange = { (value) => {
                                console.log(value)
                                
                              }}/>
                            </div>
                          }
                        >
                          <Ellipsis direction='end' rows={2} content={item.proname} />
                        </List.Item>
                      )
                    })
                  }
                </List>
                
        }
      </div>
    </>
  )
};
// Cart 单独组件
// observer(Cart) // 将组件设置为观察者
// inject('store')(observer(Cart)) // 给组件提供状态，后代组件获取到祖先组件的状态
export default inject('store')(observer(Cart));
```

## 16.3 数量更新

```tsx
// src/views/cart/Index.tsx
import React, { FC, useCallback, useEffect, useMemo, useState } from 'react';
import { observer, inject } from 'mobx-react'
import { getCartListData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Ellipsis, Stepper } from 'antd-mobile';
import { GiftOutline } from 'antd-mobile-icons'
import { useNavigate } from 'react-router-dom';
interface IState {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
interface ICartProps {
};

const Cart:FC<ICartProps> = (props: any) => {
  const navigate = useNavigate()
 

  const [cartList, setCartList] = useState<IState[]>([])

  const getCartList = useCallback(() => {
    const userid = props.store.user.userid
    console.log('userid', userid)
    getCartListData({ userid: userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setCartList([])
      } else {
        setCartList(res.data.data)
      }
    })
  }, [props.store.user.userid])

  useEffect(() => {
    getCartList()
  }, [getCartList])

  const empty = useMemo(() => { return cartList.length === 0 }, [cartList])

  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        { 
          empty ? <Empty
                    style={{ padding: '64px 0' }}
                    image={
                      <GiftOutline
                        style={{
                          color: 'var(--adm-color-light)',
                          fontSize: 100,
                        }}
                      />
                    }
                    description= { 
                      <>
                        <p>购物车空空如也</p>
                        <Button
                          color="danger" 
                          style={{ marginTop: 30 }}
                          onClick = { () => navigate('/kind')}
                        >立即购物</Button>
                      </>
                    }
                  /> 
                : <List >
                  {
                    cartList && cartList.map(item => {
                      return (
                        <List.Item
                          key={item.cartid}
                          prefix={
                            <Image
                              src={item.img1}
                              style={{ borderRadius: 20 }}
                              fit='cover'
                              width={80}
                              height={80}
                            />
                          }
                          description = {
                            <div style={{ display: 'flex', justifyContent: 'space-between' }}>
                              <span>{item.originprice}</span>
                              <Stepper digits={0} value={item.num } onChange = { (value) => { //++++++++++++++++++++++
                                console.log(value)
                                updateOneDataNum({ cartid: item.cartid, num: value}).then(() => {
                                  getCartList()
                                })
                              }}/>
                            </div>
                          }
                        >
                          <Ellipsis direction='end' rows={2} content={item.proname} />
                        </List.Item>
                      )
                    })
                  }
                </List>
                
        }
      </div>
    </>
  )
};
// Cart 单独组件
// observer(Cart) // 将组件设置为观察者
// inject('store')(observer(Cart)) // 给组件提供状态，后代组件获取到祖先组件的状态
export default inject('store')(observer(Cart));
```

## 16.4 删除以及选择

```tsx
// src/views/cart/Index.tsx
import React, { FC, useCallback, useEffect, useMemo, useState } from 'react';
import { observer, inject } from 'mobx-react'
import { getCartListData, removeOneData, selectAllData, selectOneData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Ellipsis, Stepper, SwipeAction, Modal, Checkbox } from 'antd-mobile';
import { GiftOutline } from 'antd-mobile-icons'
import { useNavigate } from 'react-router-dom';
interface IState {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
interface ICartProps {
};

const Cart:FC<ICartProps> = (props: any) => {
  const navigate = useNavigate()
 

  const [cartList, setCartList] = useState<IState[]>([])
  const [checked, setChecked] = useState(false)

  const getCartList = useCallback(() => {
    const userid = props.store.user.userid
    console.log('userid', userid)
    getCartListData({ userid: userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setCartList([])
      } else {
        setCartList(res.data.data)
        const val = res.data.data.every((item: IState) => item.flag)
        setChecked(val)
      }
    })
  }, [props.store.user.userid])

  useEffect(() => {
    getCartList()
  }, [getCartList])

  const empty = useMemo(() => { return cartList.length === 0 }, [cartList])


  

  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        { 
          empty ? <Empty
                    style={{ padding: '64px 0' }}
                    image={
                      <GiftOutline
                        style={{
                          color: 'var(--adm-color-light)',
                          fontSize: 100,
                        }}
                      />
                    }
                    description= { 
                      <>
                        <p>购物车空空如也</p>
                        <Button
                          color="danger" 
                          style={{ marginTop: 30 }}
                          onClick = { () => navigate('/kind')}
                        >立即购物</Button>
                      </>
                    }
                  /> 
                : <List >
                  {
                    cartList && cartList.map(item => {
                      return (
                        <SwipeAction
                          key={item.cartid}
                          rightActions={[
                            {
                              key: 'delete',
                              text: '删除',
                              color: 'danger',
                              onClick: () => {
                                Modal.confirm({
                                  content: <div>确定删除吗</div>,
                                  confirmText: '删除',
                                  onConfirm: () => {
                                    removeOneData({ cartid: item.cartid}).then(() => {
                                      getCartList()
                                    })
                                  }
                                })
                                
                              }
                            },
                          ]}
                        >
                          <List.Item
                          
                            prefix={
                              <div style={{ display: 'flex', alignItems: 'center'}}>
                                <div onClick={e => { 
                                  e.stopPropagation()
                                  selectOneData({ cartid: item.cartid, flag: !item.flag }).then(() => {
                                    getCartList()
                                  })
                                }}>
                                  <Checkbox checked={item.flag} />
                                </div>
                                <Image
                                  src={item.img1}
                                  style={{ borderRadius: 20 }}
                                  fit='cover'
                                  width={80}
                                  height={80}
                                />
                              </div>
                            }
                            description = {
                              <div style={{ display: 'flex', justifyContent: 'space-between' }}>
                                <span>{item.originprice}</span>
                                <Stepper digits={0} value={item.num } onChange = { (value) => {
                                  console.log(value)
                                  updateOneDataNum({ cartid: item.cartid, num: value}).then(() => {
                                    getCartList()
                                  })
                                }}/>
                              </div>
                            }
                          >
                            <Ellipsis direction='end' rows={2} content={item.proname} />
                          </List.Item>
                        </SwipeAction>
                      )
                    })
                  }
                  <div style={{
                    position: 'absolute',
                    width: '100%',
                    height: '0.5rem',
                    backgroundColor: '#efefef',
                    bottom: 0,
                    display: 'flex'
                  }}>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      justifyContent: 'start',
                      alignItems: 'center',
                      marginLeft: 10
                    }}
                      onClick = { () => {
                        console.log(1)
                        selectAllData({ userid: props.store.user.userid, type: !checked }).then(() => {
                          getCartList()
                          setChecked(!checked)
                        })
                      }}
                    >
                      <Checkbox checked = { checked }>全选</Checkbox>
                    </div>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      justifyContent: 'center',
                      alignItems: 'center'
                    }}>
                      总计：10000
                    </div>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      justifyContent: 'center',
                      alignItems: 'center'
                    }}>
                      <Button color='danger' size='small'>提交订单</Button>
                    </div>
                  </div>
                </List>
                
        }
      </div>
    </>
  )
};
// Cart 单独组件
// observer(Cart) // 将组件设置为观察者
// inject('store')(observer(Cart)) // 给组件提供状态，后代组件获取到祖先组件的状态
export default inject('store')(observer(Cart));
```

## 16.5 计算总价以及总数量

使用useMemo计算属性

```tsx
// src/views/cart/Index.tsx
import React, { FC, useCallback, useEffect, useMemo, useState } from 'react';
import { observer, inject } from 'mobx-react'
import { getCartListData, removeOneData, selectAllData, selectOneData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Ellipsis, Stepper, SwipeAction, Modal, Checkbox } from 'antd-mobile';
import { GiftOutline } from 'antd-mobile-icons'
import { useNavigate } from 'react-router-dom';
interface IState {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
interface ICartProps {
};

const Cart:FC<ICartProps> = (props: any) => {
  const navigate = useNavigate()
 

  const [cartList, setCartList] = useState<IState[]>([])
  const [checked, setChecked] = useState(false)

  const getCartList = useCallback(() => {
    const userid = props.store.user.userid
    console.log('userid', userid)
    getCartListData({ userid: userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setCartList([])
      } else {
        setCartList(res.data.data)
        const val = res.data.data.every((item: IState) => item.flag)
        setChecked(val)
      }
    })
  }, [props.store.user.userid])

  useEffect(() => {
    getCartList()
  }, [getCartList])

  const empty = useMemo(() => { return cartList.length === 0 }, [cartList])

  const totalNum = useMemo(() => {
    return cartList.reduce((sum, item) => {
      return item.flag ? sum += item.num : sum += 0
    }, 0)
  }, [cartList])
  const totalPrice = useMemo(() => {
    return cartList.reduce((sum, item) => {
      return item.flag ? sum += item.num * item.originprice : sum += 0
    }, 0)
  }, [cartList])

  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        { 
          empty ? <Empty
                    style={{ padding: '64px 0' }}
                    image={
                      <GiftOutline
                        style={{
                          color: 'var(--adm-color-light)',
                          fontSize: 100,
                        }}
                      />
                    }
                    description= { 
                      <>
                        <p>购物车空空如也</p>
                        <Button
                          color="danger" 
                          style={{ marginTop: 30 }}
                          onClick = { () => navigate('/kind')}
                        >立即购物</Button>
                      </>
                    }
                  /> 
                : <List >
                  {
                    cartList && cartList.map(item => {
                      return (
                        <SwipeAction
                          key={item.cartid}
                          rightActions={[
                            {
                              key: 'delete',
                              text: '删除',
                              color: 'danger',
                              onClick: () => {
                                Modal.confirm({
                                  content: <div>确定删除吗</div>,
                                  confirmText: '删除',
                                  onConfirm: () => {
                                    removeOneData({ cartid: item.cartid}).then(() => {
                                      getCartList()
                                    })
                                  }
                                })
                                
                              }
                            },
                          ]}
                        >
                          <List.Item
                          
                            prefix={
                              <div style={{ display: 'flex', alignItems: 'center'}}>
                                <div onClick={e => { 
                                  e.stopPropagation()
                                  selectOneData({ cartid: item.cartid, flag: !item.flag }).then(() => {
                                    getCartList()
                                  })
                                }}>
                                  <Checkbox checked={item.flag} />
                                </div>
                                <Image
                                  src={item.img1}
                                  style={{ borderRadius: 20 }}
                                  fit='cover'
                                  width={80}
                                  height={80}
                                />
                              </div>
                            }
                            description = {
                              <div style={{ display: 'flex', justifyContent: 'space-between' }}>
                                <span>{item.originprice}</span>
                                <Stepper digits={0} value={item.num } onChange = { (value) => {
                                  console.log(value)
                                  updateOneDataNum({ cartid: item.cartid, num: value}).then(() => {
                                    getCartList()
                                  })
                                }}/>
                              </div>
                            }
                          >
                            <Ellipsis direction='end' rows={2} content={item.proname} />
                          </List.Item>
                        </SwipeAction>
                      )
                    })
                  }
                  <div style={{
                    position: 'absolute',
                    width: '100%',
                    height: '0.5rem',
                    backgroundColor: '#efefef',
                    bottom: 0,
                    display: 'flex'
                  }}>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      justifyContent: 'start',
                      alignItems: 'center',
                      marginLeft: 10
                    }}
                      onClick = { () => {
                        console.log(1)
                        selectAllData({ userid: props.store.user.userid, type: !checked }).then(() => {
                          getCartList()
                          setChecked(!checked)
                        })
                      }}
                    >
                      <Checkbox checked = { checked }>全选</Checkbox>
                    </div>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      flexDirection: 'column',
                      justifyContent: 'center',
                      alignItems: 'start'
                    }}>
                      <p>总价: <span style={{ color: '#f66'}}>{ totalPrice }</span></p>
                      <p>共计<span style={{ color: '#f66'}}>{ totalNum }</span>件商品</p>
                    </div>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      justifyContent: 'center',
                      alignItems: 'center'
                    }}>
                      <Button color='danger' size='small'>提交订单</Button>
                    </div>
                  </div>
                </List>
                
        }
      </div>
    </>
  )
};
// Cart 单独组件
// observer(Cart) // 将组件设置为观察者
// inject('store')(observer(Cart)) // 给组件提供状态，后代组件获取到祖先组件的状态
export default inject('store')(observer(Cart));
```

## 16.6 购物车头部

```tsx
// src/views/cart/Index.tsx
import React, { FC, useCallback, useEffect, useMemo, useState } from 'react';
import { observer, inject } from 'mobx-react'
import { getCartListData, removeOneData, selectAllData, selectOneData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Ellipsis, Stepper, SwipeAction, Modal, Checkbox, NavBar } from 'antd-mobile';
import { GiftOutline } from 'antd-mobile-icons'
import { useNavigate } from 'react-router-dom';
interface IState {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
interface ICartProps {
};

const Cart:FC<ICartProps> = (props: any) => {
  const navigate = useNavigate()
 

  const [cartList, setCartList] = useState<IState[]>([])
  const [checked, setChecked] = useState(false)

  const getCartList = useCallback(() => {
    const userid = props.store.user.userid
    console.log('userid', userid)
    getCartListData({ userid: userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setCartList([])
      } else {
        setCartList(res.data.data)
        const val = res.data.data.every((item: IState) => item.flag)
        setChecked(val)
      }
    })
  }, [props.store.user.userid])

  useEffect(() => {
    getCartList()
  }, [getCartList])

  const empty = useMemo(() => { return cartList.length === 0 }, [cartList])

  const totalNum = useMemo(() => {
    return cartList.reduce((sum, item) => {
      return item.flag ? sum += item.num : sum += 0
    }, 0)
  }, [cartList])
  const totalPrice = useMemo(() => {
    return cartList.reduce((sum, item) => {
      return item.flag ? sum += item.num * item.originprice : sum += 0
    }, 0)
  }, [cartList])

  return (
    <>
      <header className="header">
      <NavBar
          style={{
            '--height': '0.44rem',
            '--border-bottom': '1px #eee solid',
            color: '#fff'
          }}
          onBack={ () => navigate(-1) }
        >
          购物车
        </NavBar>
      </header>
      <div className="content">
        { 
          empty ? <Empty
                    style={{ padding: '64px 0' }}
                    image={
                      <GiftOutline
                        style={{
                          color: 'var(--adm-color-light)',
                          fontSize: 100,
                        }}
                      />
                    }
                    description= { 
                      <>
                        <p>购物车空空如也</p>
                        <Button
                          color="danger" 
                          style={{ marginTop: 30 }}
                          onClick = { () => navigate('/kind')}
                        >立即购物</Button>
                      </>
                    }
                  /> 
                : <List >
                  {
                    cartList && cartList.map(item => {
                      return (
                        <SwipeAction
                          key={item.cartid}
                          rightActions={[
                            {
                              key: 'delete',
                              text: '删除',
                              color: 'danger',
                              onClick: () => {
                                Modal.confirm({
                                  content: <div>确定删除吗</div>,
                                  confirmText: '删除',
                                  onConfirm: () => {
                                    removeOneData({ cartid: item.cartid}).then(() => {
                                      getCartList()
                                    })
                                  }
                                })
                                
                              }
                            },
                          ]}
                        >
                          <List.Item
                          
                            prefix={
                              <div style={{ display: 'flex', alignItems: 'center'}}>
                                <div onClick={e => { 
                                  e.stopPropagation()
                                  selectOneData({ cartid: item.cartid, flag: !item.flag }).then(() => {
                                    getCartList()
                                  })
                                }}>
                                  <Checkbox checked={item.flag} />
                                </div>
                                <Image
                                  src={item.img1}
                                  style={{ borderRadius: 20 }}
                                  fit='cover'
                                  width={80}
                                  height={80}
                                />
                              </div>
                            }
                            description = {
                              <div style={{ display: 'flex', justifyContent: 'space-between' }}>
                                <span>{item.originprice}</span>
                                <Stepper digits={0} value={item.num } onChange = { (value) => {
                                  console.log(value)
                                  updateOneDataNum({ cartid: item.cartid, num: value}).then(() => {
                                    getCartList()
                                  })
                                }}/>
                              </div>
                            }
                          >
                            <Ellipsis direction='end' rows={2} content={item.proname} />
                          </List.Item>
                        </SwipeAction>
                      )
                    })
                  }
                  <div style={{
                    position: 'absolute',
                    width: '100%',
                    height: '0.5rem',
                    backgroundColor: '#efefef',
                    bottom: 0,
                    display: 'flex'
                  }}>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      justifyContent: 'start',
                      alignItems: 'center',
                      marginLeft: 10
                    }}
                      onClick = { () => {
                        console.log(1)
                        selectAllData({ userid: props.store.user.userid, type: !checked }).then(() => {
                          getCartList()
                          setChecked(!checked)
                        })
                      }}
                    >
                      <Checkbox checked = { checked }>全选</Checkbox>
                    </div>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      flexDirection: 'column',
                      justifyContent: 'center',
                      alignItems: 'start'
                    }}>
                      <p>总价: <span style={{ color: '#f66'}}>{ totalPrice }</span></p>
                      <p>共计<span style={{ color: '#f66'}}>{ totalNum }</span>件商品</p>
                    </div>
                    <div style={{ 
                      flex: 1,
                      display: 'flex',
                      justifyContent: 'center',
                      alignItems: 'center'
                    }}>
                      <Button color='danger' size='small'>提交订单</Button>
                    </div>
                  </div>
                </List>
                
        }
      </div>
    </>
  )
};
// Cart 单独组件
// observer(Cart) // 将组件设置为观察者
// inject('store')(observer(Cart)) // 给组件提供状态，后代组件获取到祖先组件的状态
export default inject('store')(observer(Cart));
```

# 17.项目上线

默认执行`cnpm run build` 打包出来的项目资源以绝对路径方式引入

通过给 `package.json`文件中添加`"homepage": "./"`更改为相对路径

然后修改 入口文件处，使用 HashRouter 代替 BrowserRouter

然后修改 utils/request，未登陆时  跳转地址改为 `/#/login`

执行`cnpm run build` 打包,上传服务器，服务器测试

http://121.89.205.189:3001/gpreact/

测试账号： 188113007812  Xagp01
注册账号：http://121.89.205.189:3001/gpvue/#/register/index
