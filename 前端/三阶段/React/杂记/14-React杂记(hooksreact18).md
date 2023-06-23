## 几个不常用的hook

### useSyncExternalStore  （同步外部仓部数据到组件)

~~~
  useEffect(()=>{
    setN(store.getState().n)
  },[store.getState().n])
  useSyncExternalStore(store.subscribe,store.getState)
~~~

### useInsertionEffect (作用插入style)

~~~
import React,{useInsertionEffect} from 'react'

import { useDispatch,useSelector } from 'react-redux';

export default function Home() {

  let state=useSelector(state=>state.user)

  useInsertionEffect(()=>{

   let style =  document.createElement("style");

   style.innerHTML='.current{background:#ccc;}';

   document.head.appendChild(style)

  })

 return (

    <div>Home {state.adminname && <span className='current'>你好{state.adminname}</span>}</div>

 )

}
~~~

### useReducer（有用)

~~~
export const initialState = {
    n:6
}

export default (state = initialState, { type, payload }) => {
  switch (type) {

  case 'ADD':
    return { ...state, n:state.n+payload }

  default:
    return state
  }
}
~~~

context

~~~
import {createContext} from 'react'

let context=createContext(); 
let {Provider} = context;

export {context,Provider}
~~~

App.js

~~~
function App() {
  let [state,dispatch]= useReducer(reducer,initialState)

  return (
    <Provider value={{state,dispatch}}>
    <div className="App">
        <One></One>
        <Two></Two>
    </div>
    </Provider>
  );
}
~~~

One.jsx

~~~
import React,{useContext} from 'react'
import { context } from '../context'

export default function One() {
    let {state}=useContext(context);
  return (
    <div>One {state.n}</div>
  )
}
~~~

Two.jsx

~~~
import React,{useContext} from 'react'
import { context } from '../context'

export default function Two() {
  let {dispatch} = useContext(context);
  return (
    <div>Two <button onClick={()=>dispatch({type:'ADD',payload:10})}>+</button></div>
  )
}
~~~

### useId

~~~
  let id=useId()  //用来生成一个id，使服务端和客户端能有一个稳定一个唯一的id，避免组件水化后id不匹配
  console.log(id)
~~~

### useDeferredValue

~~~
import React,{useState,useDeferredValue } from 'react'

export default function Two() {
    let [str,setStr]=useState("")
    let delayStr = useDeferredValue(str);  //delayStr 是非紧急渲染的值
  return (
    <div>
        <input type="text" value={str} onChange={(e)=>setStr(e.target.value)} /> {str}
        {
            Array(10000).fill(0).map((item,index)=>{
                return <p key={index}>{delayStr}</p>
            })
        }
    </div>
  )
}

~~~

### useTransition

~~~
useTransition 又叫过渡, 他的作用就是标记非紧急更新, 这些被标记非紧急更新会在紧急更新完之后进行更新

import React,{useState,useDeferredValue,useTransition } from 'react'

export default function Two() {
    let [str,setStr]=useState("")
    let delayStr = useDeferredValue(str);  //delayStr 是非紧急渲染的值
    let [pending,startTransition]=useTransition();
    const change=(e)=>{
        startTransition(()=>{  //不紧急的时候再执行这个回调
            setStr(e.target.value) 
        })
       
    }
  return (
    <div>
        <input type="text" value={str} onChange={change} /> {pending && 'waiting....'}{str}
        {
            Array(10000).fill(0).map((item,index)=>{
                return <p key={index}>{str}</p>
            })
        }
    </div>
  )
}

~~~

## Suspense和fallback异步加载组件

~~~
import {Suspense,lazy} from 'react'

const One=lazy(()=>import("./views/One"))
const Two=lazy(()=>import("./views/Two"))

  <Suspense fallback={<Loading />}>
        <Link to="/one">one</Link>
        <Link to="/two">two</Link>
        <Switch>
           <Route path="/one" component={One} />
           <Route path="/two" component={Two} />
        </Switch>
 </Suspense>
~~~

