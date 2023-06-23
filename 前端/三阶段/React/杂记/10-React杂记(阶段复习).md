## 复习

 jsx

组件

​      类组件

​     函数组件

传值有几种方式，

父--》子     父组件调用子组件里的方法  属性 

子--》父    子组件调用父组件的方法

兄弟传值   EventEmitter 实例    emit   发送数据 on 接收数据

跨组件传值   context  Provdier value属性       类组件 static  contextType=conext this.context 就可以拿到这个值 t  Consumer

函数组件  useContext  Consumer

常用hook

​       useState

​       useEffect

​       useLayoutEffect

​		useRef

​      useContext

​     useMemo

​	 useCallback

​	 useImperativeHandle

​    useDebugValue

​    自定义hook

​    react-router-dom 

​     useLocation

​     useHistory

​     useParams

​     useRouteMatch

路由

​     BrowerRouter(HashRouter)

​	 Link(NavLink)

​     Route  (path component)

​     Redirect

​     Switch

​    withRouter

 V6于V5区别

   Switch 换 Routes

   component 换 element

   Redirect 换 Navigate 

