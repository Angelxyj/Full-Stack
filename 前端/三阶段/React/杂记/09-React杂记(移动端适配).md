## 移动端适配

1. npm i amfe-flexible postcss-pxtorem -S

- amfe-flexible 用于设置根字体大小的
- postcss-pxtorem 用来自动转换单位的

2. npm i  @craco/craco -S

    package.json

   ~~~
   "scripts": {
       "start": "craco start",
       "build": "craco build",
       "test": "craco test",
       "eject": "craco eject"
     },
   ~~~

   

3. 项目的根目录下建立 craco.config.js

   ~~~
   module.exports = {
       style: {
             postcss: {
                 mode: 'extends',
                 loaderOptions: (postcssLoaderOptions, { env, paths }) => {
                     postcssLoaderOptions.postcssOptions.plugins = [
                         ...postcssLoaderOptions.postcssOptions.plugins,
                         [
                             'autoprefixer',
                             {
                                 overrideBrowserslist: [
                                     'last 2 version',
                                     '>1%',
                                     'Android >= 4.0',
                                     'iOS >= 7'
                                 ]
                             }
                         ],
                         [
                             'postcss-pxtorem',
                             {
                                 rootValue ({ file }) {
                                     // return file.indexOf('antd-mobile') > -1 ? 37.5 : 75;
                                     return 37.5;
                                 },
                                 unitPrecision: 2, //只转换到两位小数
                                 propList: ['*']
                             }
                         ]
                     ];
                     return postcssLoaderOptions;
                 }
             }
         },  
     }
   ~~~

   4.src/index.js 引入amfe-flexible

   ~~~
   import "amfe-flexible/index"
   ~~~

5. npm i normalize.css -S   css Reset  重置css样式

​      .src/index.js 引入    import "normalize.css"

## 复习

1.jsx

2.组件

  类组件

​		constructor   componentDidMount render componentDidUpdate componentWillUnmount

 函数组件

​      useRef  标识组件或节点

​      useState 存储或修改状态

​      useEffect  在组件挂载，更新，卸载时候执行回调

​       useLayoutEffect  useEffect的同步版本

​      useContext  使用Provider的value属性提供的数据 
​      useMemo   缓存函数的返回值

​      useCallBack  缓存函数

​       useImperativeHandle  暴露子组件的方法给父组件用

​       useDebugValue  在开发者工具显示自定义hook的信息

​      自定义hook

​      useLocation (pathname ,search ,state )

​	  useHistory  （go goBack goForward push replace)

​	  useParams  (拿到动态路由的参数)  params.xxx

​     useRouteMatch 

   路由相关的对象

## 项目打包

​    npm  run build

   路由要改为hash模式

~~~
import {HashRouter as Router} from 'react-router-dom'
~~~

package.json

 "homepage":"."



​    
