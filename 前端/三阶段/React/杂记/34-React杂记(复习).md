## 复习

### jsx

### 组件(类组件和函数组件)

常用hook

  useState

  useRef

 useImperativeHandle

 useEffect

useLayoutEffect

useCallback

useMemo

useContext

 useDebugValue

### 路由 

(react-router-dom)

useLocation

useHistory

useRouteMatch

useParams

### redux 

(react-redux)

useSelector

useDispatch

存数据

取数据    useSelector(state=>state.模块名)

改数据    useDispatch 获取一个dispatch  dispatch(动作) reducer 

分模块：一个reducer 拆分为多个reducer 或者事先就分好了多个reducer 

   combineReducers 把多个reducer合并为一个   key 模块名  value 引入的reducer

异步处理 ：redux-thunk 是一个中间件    createStore(reducer,applayMiddleware( thunk))

actionCreator里 同步方法，返回对象  异步的方法要返回回调函数    

