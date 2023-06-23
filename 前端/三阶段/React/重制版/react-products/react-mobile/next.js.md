# next.js

# 1.什么是服务端渲染SSR

React.js 是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出 React 组件，进行生成 DOM 和操作 DOM。然而，也可以将同一个组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器，最后将这些静态标记"激活"为客户端上完全可交互的应用程序。

服务器渲染的 React.js 应用程序也可以被认为是"同构"或"通用"，因为应用程序的大部分代码都可以在**服务器**和**客户端**上运行。

## 1.1 后端渲染

```sh
$ npx express express-app --view=ejs
```

```ts
// 服务端渲染 --- 后端渲染  ---- express --- ejs模板
// 后端代码
router.get('/', function(req, res, next) {
  res.render('index', { 
    title: 'Express',
    list: ['a', 'b', 'c', 'd']
  });
});
// 前端
 <ul>
    <% for(var i = 0; i < list.length; i++) {%>
      <li><%= list[i] %></li>
    <% } %>
 </ul>
```

后端的渲染模式：前端开发页面，写完发给后端，后端更改后缀名为ejs，从数据库提取数据，说明要渲染的ejs页面，后端从数据库提取数据，通过render函数渲染页面时给ejs页面提供数据，再通过ejs模板语法来完成渲染操作。

缺点：

* 前后端不断的返工（前端交给后端时，样式没有问题，后端拿到代码，看起来差不多-特别是列表，删除的只剩余一个，然后用模板语法-导致样式出错-前端返工-后端返工）
* 不利于前后端分离开发

优点：利于搜索引擎（SEO）的抓取

## 1.2 前端渲染

ajax局部渲染模式

缺点：不利于搜索引擎的抓取

优点：利于多人分离开发

## 1.3 为什么使用服务器端渲染 SSR

与传统 SPA (单页应用程序 (Single-Page Application)) 相比，服务器端渲染 (SSR) 的优势主要在于：

- 更好的 SEO，由于搜索引擎爬虫抓取工具可以直接查看完全渲染的页面。

  请注意，截至目前，Google 和 Bing 可以很好对同步 JavaScript 应用程序进行索引。在这里，同步是关键。如果你的应用程序初始展示 loading 菊花图，然后通过 Ajax 获取内容，抓取工具并不会等待异步完成后再行抓取页面内容。也就是说，如果 SEO 对你的站点至关重要，而你的页面又是异步获取内容，则你可能需要服务器端渲染(SSR)解决此问题。

- 更快的内容到达时间 (time-to-content)，特别是对于缓慢的网络情况或运行缓慢的设备。无需等待所有的 JavaScript 都完成下载并执行，才显示服务器渲染的标记，所以你的用户将会更快速地看到完整渲染的页面。通常可以产生更好的用户体验，并且对于那些「内容到达时间(time-to-content) 与转化率直接相关」的应用程序而言，服务器端渲染 (SSR) 至关重要 - 应用程序首页白屏。

使用服务器端渲染 (SSR) 时还需要有一些权衡之处：

- 开发条件所限。浏览器特定的代码，只能在某些生命周期钩子函数 (lifecycle hook) 中使用；一些外部扩展库 (external library) 可能需要特殊处理，才能在服务器渲染应用程序中运行。
- 涉及构建设置和部署的更多要求。与可以部署在任何静态文件服务器上的完全静态单页面应用程序 (SPA) 不同，服务器渲染应用程序，需要处于 Node.js server 运行环境。
- 更多的服务器端负载。在 Node.js 中渲染完整的应用程序，显然会比仅仅提供静态文件的 server 更加大量占用 CPU 资源 (CPU-intensive - CPU 密集)，因此如果你预料在高流量环境 (high traffic) 下使用，请准备相应的服务器负载，并明智地采用缓存策略。

在对你的应用程序使用服务器端渲染 (SSR) 之前，你应该问的第一个问题是，是否真的需要它。这主要取决于内容到达时间 (time-to-content) 对应用程序的重要程度。例如，如果你正在构建一个内部仪表盘，初始加载时的额外几百毫秒并不重要，这种情况下去使用服务器端渲染 (SSR) 将是一个小题大作之举。然而，内容到达时间 (time-to-content) 要求是绝对关键的指标，在这种情况下，服务器端渲染 (SSR) 可以帮助你实现最佳的初始加载性能。

react框架中对应的服务端渲染的框架：next.js: https://nextjs.zcopy.site/

vue框架中对应的服务端渲染的框架： nuxt.js:https://www.nuxtjs.cn/

## 1.4 服务器端渲染 vs 预渲染 (SSR vs Prerendering)

如果你调研服务器端渲染 (SSR) 只是用来改善少数营销页面（例如 `/`, `/about`, `/contact` 等）的 SEO，那么你可能需要**预渲染**。无需使用 web 服务器实时动态编译 HTML，而是使用预渲染方式，在构建时 (build time) 简单地生成针对特定路由的静态 HTML 文件。优点是设置预渲染更简单，并可以将你的前端作为一个完全静态的站点。

# 2.react服务端渲染

Next.js 为您提供生产环境所需的所有功能以及最佳的开发体验: 包括静态及服务器端融合渲染、 支持 TypeScript、智能化打包、 路由预取等功能 无需任何配置。

## 2.1 创建项目

```sh
$ npx create-next-app
```

默认使用yarn 作为包资源管理器

```sh
$ cnpm i yarn -g
$ yarn -v # 1.22.17
```

## 2.2 构建项目基本结构以及页面

next.js的基本路由是基于pages目录结构的，无需手动配置

```sh
$ cnpm i sass -D
```

```scss
// styles/globals.css 
* {
  padding: 0;
  margin: 0;
  list-style: none;
  text-decoration: none;
  box-sizing: border-box;
}

// 配置图标  ---- 阿里字体图标本地配置
@font-face {
  font-family: 'iconfont';
  src: url('/font/iconfont.woff2?t=1666248228988') format('woff2'),
       url('/font/iconfont.woff?t=1666248228988') format('woff'),
       url('/font/iconfont.ttf?t=1666248228988') format('truetype');
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

html, body, #__next {
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

    ul { 
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
        color: #333;
        span {
          font-size: 0.20rem;
        }

        p {
          font-size: 0.12rem;
        }
		&.active {
          color: #f66;
        }
      }
    }
  }
}


```

```js
// pages/_app.js   就是布局文件
import '../styles/globals.scss'

function MyApp({ Component, pageProps }) {
  return (
    <div className='container'>
      {/* 装载头部和内容区域 router-view  Outlet*/}
      <Component {...pageProps} />
      {/* 添加底部 */}
      <footer className='footer'>
        <ul>
          <li>
            <span className='iconfont'>&#xe6cb;</span>
            <p>首页</p>
          </li>
          <li>
            <span className='iconfont'>&#xe71b;</span>
            <p>分类</p>
          </li>
          <li>
            <span className='iconfont'>&#xe70b;</span>
            <p>购物车</p>
          </li>
          <li>
            <span className='iconfont'>&#xe615;</span>
            <p>我的</p>
          </li>
        </ul>
      </footer>
    </div>
  )
}

export default MyApp

```



```js
// pages/home.js
import React from 'react';

const Home = () => {
  return (
    <>
      <header className='header'>home header</header>
      <div className='content'>home content</div>
    </>
  );
};

export default Home;
```

```js
// pages/kind.js
import React from 'react';

const Kind = () => {
  return (
    <>
      <header className='header'>kind header</header>
      <div className='content'>kind content</div>
    </>
  );
};

export default Kind;
```

```js
// pages/cart.js
import React from 'react';

const Cart = () => {
  return (
    <>
      <header className='header'>cart header</header>
      <div className='content'>cart content</div>
    </>
  );
};

export default Cart;
```

```js
// pages/user.js
import React from 'react';

const User = () => {
  return (
    <>
      <header className='header'>user header</header>
      <div className='content'>user content</div>
    </>
  );
};

export default User;
```

```js
// pages/detail/[proid].js
import React from 'react';

const Detail = () => {
  return (
    <>
      <header className='header'>detail header</header>
      <div className='content'>detail content</div>
    </>
  );
};

export default Detail;
```

```js
// pages/login.js
import React from 'react';

const Login = () => {
  return (
    <>
      <header className='header'>login header</header>
      <div className='content'>login content</div>
    </>
  );
};

export default Login;
```

## 2.3 点击底部选项卡切换页面

```js
// pages/_app.js   就是布局文件
import '../styles/globals.scss'
import Link from 'next/link'
import { useRouter } from 'next/router'

function MyApp({ Component, pageProps }) {
  const router = useRouter()
  const { pathname } = router
  // console.log(router)
  return (
    <div className='container'>
      {/* 装载头部和内容区域 router-view  Outlet*/}
      <Component {...pageProps} />
      {/* 添加底部 */}
      <footer className='footer'>
        <ul>
          <Link href="/home">
            <li className={ pathname === '/home' ? 'active': undefined }>
              <span className='iconfont'>&#xe6cb;</span>
              <p>首页</p>
            </li>
          </Link>
          <Link href="/kind">
            <li className={ pathname === '/kind' ? 'active': undefined }>
              <span className='iconfont'>&#xe71b;</span>
              <p>分类</p>
            </li>
          </Link>
          <Link href="/cart">
            <li className={ pathname === '/cart' ? 'active': undefined }>
              <span className='iconfont'>&#xe70b;</span>
              <p>购物车</p>
            </li>
          </Link>
          <Link href="/user">
            <li className={ pathname === '/user' ? 'active': undefined }>
              <span className='iconfont'>&#xe615;</span>
              <p>我的</p>
            </li>
          </Link>


        </ul>
      </footer>
    </div>
  )
}

export default MyApp

```

## 2.4 页面底部取消

通过设置白名单判断当前路由是否应该显示

```js
// pages/_app.js   就是布局文件
import '../styles/globals.scss'
import Link from 'next/link'
import { useRouter } from 'next/router'

function MyApp({ Component, pageProps }) {
  const router = useRouter()
  const { pathname } = router
  // console.log(router)

  // 页面底部白名单
  const arrs = ['/home', '/kind', '/cart', '/user']
  return (
    <div className='container'>
      {/* 装载头部和内容区域 router-view  Outlet*/}
      <Component {...pageProps} />
      {/* 添加底部 */}
      {
        arrs.includes(pathname) && <footer className='footer'>
          <ul>
            <Link href="/home">
              <li className={ pathname === '/home' ? 'active': undefined }>
                <span className='iconfont'>&#xe6cb;</span>
                <p>首页</p>
              </li>
            </Link>
            <Link href="/kind">
              <li className={ pathname === '/kind' ? 'active': undefined }>
                <span className='iconfont'>&#xe71b;</span>
                <p>分类</p>
              </li>
            </Link>
            <Link href="/cart">
              <li className={ pathname === '/cart' ? 'active': undefined }>
                <span className='iconfont'>&#xe70b;</span>
                <p>购物车</p>
              </li>
            </Link>
            <Link href="/user">
              <li className={ pathname === '/user' ? 'active': undefined }>
                <span className='iconfont'>&#xe615;</span>
                <p>我的</p>
              </li>
            </Link>


          </ul>
        </footer>
      }
      
    </div>
  )
}

export default MyApp

```

还可以设置两套布局文件：https://www.jb51.cc/faq/2953879.html

# 3.添加组件库

```sh
$ cnpm i antd-mobile -S
```

在 Next.js 中使用 antd-mobile 需要做一些额外的配置。

首先，需要安装 `next-transpile-modules` 依赖：

```
$ cnpm i next-transpile-modules -D
```

然后在 `next.config.js` 中进行配置：

```js
/** @type {import('next').NextConfig} */
const withTM = require('next-transpile-modules')([ // ++++
  'antd-mobile',
]);

const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
}

module.exports = withTM(nextConfig) // ++++

```

# 4.构建首页

```sh
$ cnpm i axios js-cookie -S
```

本次存储不要再使用 localStorage,改用 cookie https://www.npmjs.com/package/js-cookie

```js
import Cookie from 'js-cookie'

Cookies.set('foo', 'bar')

Cookies.set('name', 'value', { expires: 7 })

Cookies.get('nothing') 

Cookies.remove('name') 
```

```js
// utils/request.js
// 1.引入axios
import axios from 'axios'
import Cookie from 'js-cookie'

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
  config.headers.token = Cookie.get('token')
  return config
}, (err) => {
  return Promise.reject(err)
})

// 响应拦截器 所有的接口返回值先执行响应拦截器，再返回自己的响应的数据
ins.interceptors.response.use((response) => {
  // 关闭loading动画  --- 使用组件不必要 ----  js模块隐藏
  // 验证token，如果验证通过，返回数，如果验证不通过，直接跳转到登录页面
  if (response.data.code === '10119') {
    // window.location.href = "/login"
    window.location.href = "/login"
    return 
  } else {
    return response
  }
}, (err) => Promise.reject(err))

// 6.暴露自定义axios
export default ins
```

```js
// // utils/nav.js
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

```js
// api/home.js
// 首页相关数据请求的接口的封装
import request from '../utils/request' // request其实就可以看作式自定义之后的axois


export function getBannerListData () {
  return request.get('/banner/list')
}

export function getSeckillListData () {
  return request.get('/pro/seckilllist')
}

export function getProListData (params) {
  return request.get('/pro/list', { params })
}
```

```js
// pages/home.js
import React, { useEffect, useState } from 'react';
import { Swiper, Image, Grid } from 'antd-mobile'
import { getBannerListData } from '../api/home'
import nav_list from '../utils/nav'
const Home = () => {
  const [bannerList, setBannerList] = useState([{ 
    alt: "",
    bannerid: "c",
    flag: true,
    img: "",
    link: ""
  }])
  useEffect(() => {
    getBannerListData().then(res => {
      setBannerList(res.data.data)
    })
  }, [])

  const [navList] = useState(nav_list)
  return (
    <>
      <header className='header'>Home header</header>
      <div className='content'>
        <Swiper autoplay loop={true} style={{
          '--border-radius': '8px',
        }}>
          {
            bannerList && bannerList.map((item) => {
              return (
                <Swiper.Item key = {item.bannerid}>
                  <Image src={item.img}/>
                </Swiper.Item>
              )
            })
          }
        </Swiper>

        <Grid columns={5} gap={0}>
          {
            navList && navList.map(item => {
              return (
                <Grid.Item key = {item.navid} style={{ display: 'flex', flexDirection: 'column', justifyContent: 'center', alignItems: 'center'}}>
                  <Image src={ item.imgurl } style={{ width: 44, height: 44 }}></Image>
                  <p>{ item.title }</p>
                </Grid.Item>
              )
            })
          }
        </Grid>
      </div>
    </>
  );
};

export default Home;
```

后续开发功能参照vue移动端项目以及react移动端项目

# 5.next如何使用状态管理器

## 5.1 方式1： redux + react-redux

```js
// store/index.js
import { createStore } from 'redux'

const reducer = (state = {
  count: 1
}, action) => {
  switch (action.type) {
    case 'ADD':
      return { ...state, count: state.count + action.payload }
    case 'REDUCE':
      return { ...state, count: state.count - action.payload }
    default:
      return state
  }
}

const store = createStore(reducer)

export default store
```

```js
// pages/_app.js   就是布局文件
import '../styles/globals.scss'
import Link from 'next/link'
import { useRouter } from 'next/router'
import { Provider } from 'react-redux'
import store from '../store'
function MyApp({ Component, pageProps }) {
  const router = useRouter()
  const { pathname } = router
  // console.log(router)

  // 页面底部白名单
  const arrs = ['/home', '/kind', '/cart', '/user']
  return (
    <Provider store = { store }>

      <div className='container'>
        {/* 装载头部和内容区域 router-view  Outlet*/}
        <Component {...pageProps} />
        {/* 添加底部 */}
        {
          arrs.includes(pathname) && <footer className='footer'>
            <ul>
              <Link href="/home">
                <li className={ pathname === '/home' ? 'active': undefined }>
                  <span className='iconfont'>&#xe6cb;</span>
                  <p>首页</p>
                </li>
              </Link>
              <Link href="/kind">
                <li className={ pathname === '/kind' ? 'active': undefined }>
                  <span className='iconfont'>&#xe71b;</span>
                  <p>分类</p>
                </li>
              </Link>
              <Link href="/cart">
                <li className={ pathname === '/cart' ? 'active': undefined }>
                  <span className='iconfont'>&#xe70b;</span>
                  <p>购物车</p>
                </li>
              </Link>
              <Link href="/user">
                <li className={ pathname === '/user' ? 'active': undefined }>
                  <span className='iconfont'>&#xe615;</span>
                  <p>我的</p>
                </li>
              </Link>


            </ul>
          </footer>
        }
        
      </div>
    </Provider>
    
  )
}

export default MyApp

```

```js
// pages/kind.js
import React from 'react';
import { connect } from 'react-redux'
const Kind = (props) => {
  console.log(props)
  return (
    <>
      <header className='header'>kind header</header>
      <div className='content'>
        { props.count }
        <button onClick = { () => { props.dispatch({ type: 'ADD', payload: 1})}}>加1</button>
        <button onClick = { () => { props.dispatch({ type: 'REDUCE', payload: 1})}}>减1</button>
      </div>
    </>
  );
};

export default connect((state) => {
  return {
    count: state.count
  }
})(Kind);
```

```js
// pages/cart.js
import React from 'react';
import { connect } from 'react-redux'
const Cart = (props) => {
  console.log(props)
  return (
    <>
      <header className='header'>Cart header</header>
      <div className='content'>
        { props.count }
        <button onClick = { () => { props.dispatch({ type: 'ADD', payload: 10})}}>加10</button>
        <button onClick = { () => { props.dispatch({ type: 'REDUCE', payload: 10})}}>减10</button>
      </div>
    </>
  );
};

export default connect((state) => {
  return {
    count: state.count
  }
})(Cart);
```

## 5.2其他方案

* redux + react-redux + redux-thunk
* redux + react-redux + redux-saga 
* redux + react-redux + redux-thunk + immutable + redux-immutable
* redux + react-redux + redux-saga + immutable + redux-immutable
* redux + rtk
* mobx + mobx-react
* useReducer + useContext

# 6.next请求数据方式

```js
// pages/home.js
import React, { useEffect, useState } from 'react';
import { Swiper, Image, Grid } from 'antd-mobile'
import { getBannerListData, getProListData } from '../api/home'
import nav_list from '../utils/nav'
export async function getStaticProps() { // +++++++
  // Call an external API endpoint to get posts.
  // You can use any data fetching library
  const proRes = await getProListData()
  const bannerRes = await getBannerListData()

  // By returning { props: posts }, the Blog component
  // will receive `posts` as a prop at build time
  return {
    props: {
      proList: proRes.data.data,
      bannerTest: bannerRes.data.data,
    },
  }
}
const Home = (props) => {
  console.log(props)
  const [bannerList, setBannerList] = useState([{ 
    alt: "",
    bannerid: "c",
    flag: true,
    img: "",
    link: ""
  }])
  useEffect(() => {
    getBannerListData().then(res => {
      setBannerList(res.data.data)
    })
  }, [])

  const [navList] = useState(nav_list)
  return (
    <>
      <header className='header'>Home header</header>
      <div className='content'>
        <Swiper autoplay loop={true} style={{
          '--border-radius': '8px',
        }}>
          {
            bannerList && bannerList.map((item) => {
              return (
                <Swiper.Item key = {item.bannerid}>
                  <Image src={item.img}/>
                </Swiper.Item>
              )
            })
          }
        </Swiper>
        <Swiper autoplay loop={true} style={{
          '--border-radius': '8px',
        }}>
          {
            props.bannerTest && props.bannerTest.map((item) => {
              return (
                <Swiper.Item key = {item.bannerid}>
                  <Image src={item.img}/>
                </Swiper.Item>
              )
            })
          }
        </Swiper>
        <Grid columns={5} gap={0}>
          {
            navList && navList.map(item => {
              return (
                <Grid.Item key = {item.navid} style={{ display: 'flex', flexDirection: 'column', justifyContent: 'center', alignItems: 'center'}}>
                  <Image src={ item.imgurl } style={{ width: 44, height: 44 }}></Image>
                  <p>{ item.title }</p>
                </Grid.Item>
              )
            })
          }
        </Grid>

        <ul>
          {
            props.proList && props.proList.map(item => {
              return (
                <li key = { item.proid }>{ item.proname }</li>
              )
            })
          }
        </ul>
      </div>
    </>
  );
};

export default Home;
```

