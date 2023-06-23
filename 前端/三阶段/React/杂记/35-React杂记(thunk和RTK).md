

## Redux-thunk

https://cnodejs.org/api/v1/topics

~~~
const reducer = (
  state = {
    bannerList: [],
    proList: []
  },
  { type, payload }
) => {
  switch (type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: payload }
    case 'CHANGE_PRO_LIST':
      return { ...state, proList: payload }
    default:
      return state
  }
}

export default reducer

// actionCreator 本质是一个带有 dispatch 参数的函数
const action = {
  getBannerListAction (dispatch) { // 因为此接口不需要参数，所有拥有了默认参数 dispatch
    getBannerList().then(res => {
      dispatch({
        type: 'CHANGE_BANNER_LIST',
        payload: res.data
      })
    })
  },
  getProListAction (params) { // 组件调用需要传递参数
    return (dispatch) => {
      getProList(params).then(res => {
        dispatch({
          type: 'CHANGE_PRO_LIST',
          payload: res.data
        })
      })
    }
  }
}

export default action

 getBannerList () {
      dispatch(action.getBannerListAction) // 因为没有参数，所以不加()
    },
getProList () {
  dispatch(action.getProListAction({ count: 2, limitNum: 3 }))  // 因为有参数，所以加()
}
~~~



## Redux Toolkit (Rtk)

RTK:操作redux 的一个工具包

第一步: 需要下载安装

```shell
npm install @reduxjs/toolkit
```

第二步: 在src目录下新建 store目录, 在该目录下创建index.js 文件, 内容可以使用官网的示例代码:

redux官网地址: https://cn.redux.js.org/introduction/getting-started

```js
//01: 引入rtk 中提供的两个方法
import { createSlice, configureStore } from '@reduxjs/toolkit';
//02: 引入切片, 相当于一个相当于vuex 中的一个模块,维护自己的数据
const counterSlice = createSlice({
  name: 'counter',  // 切片的命名,相当于vuex 的命名空间
  initialState: {  // 定义该模块的初始数据
    count: 0 
  },
  reducers: { // 定义修改该数据对应的方法, 相当于vuex 中的mutations
    addCount: (state) => { //不接收参数的情形
      state.count +=1
    },
    jianCount: (state,action) => { // 接收参数的情形
      state.count -= action.payload
    }
  }
})

//03:导出同步操作数据的方法
export const { addCount, jianCount } = counterSlice.actions;

//04: 创建store 仓库
const store = configureStore({
  reducer: counterSlice.reducer
})

//05:导出仓库, 并挂载到根实例上
export default store


```



在项目入口文件中,引入 store, 并挂载到根组件

```js
// 引入store
import store from './store';
import { Provider } from 'react-redux';
const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <Provider store={store}>
    <Router>
      <App />
    </Router>
  </Provider>
);
```



在类组件页面中使用 store 仓库中的数据

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { addCount, jianCount } from '../../store';

class Myclass extends Component {
    render() {
        console.log(this);
        return (
            <div>
                类组件中使用
                <p>
                    <button onClick={this.props.add}>同步+1</button>
                    count:{this.props.count}
                    <button onClick={this.props.jian}>同步-1</button>
                </p>
            </div>
        );
    }
}

function mapstatetoprops(state) {
    return state
}
function mapdispatchtoprops(dispatch) {
    return {
        add() {
            dispatch(addCount(10))
        },
        jian() {
            dispatch(jianCount(5))
        },
    }
}

export default connect(mapstatetoprops, mapdispatchtoprops)(Myclass);
```

在函数组件页面中使用store 仓库中的数据

```jsx
import React from 'react';
// 引入react-redux 中的两个钩子函数
import { useSelector, useDispatch } from 'react-redux';
// 引入store 中抛出的两个方法
import { addCount, jianCount } from '../../store/index'
const Myfun = () => {
    const { count } = useSelector((state) => state);
    const dispatch = useDispatch();
    return (
        <div>
            函数组件中使用
            <p>
                {/*调用addCount方法*/}
                <button onClick={() => {
                    dispatch(addCount(2))
                }}>+2</button>
                count:{count}
                {/*调用jianCount方法*/}
                <button onClick={() => {
                    dispatch(jianCount(2))
                }}>-2</button>
            </p>
        </div>
    );
}

export default Myfun;

```



以上是最简单的操作方式, 但是当操作异步方法时, 并需要将store仓库模块化

将如上仓库进行模块化, 不同的模块管理自己仓库的数据, 在store 目录下, 新建reducers, 在该目录中新建userReducer.js 和 goodsReducer.js

goodsReducer.js 文件

```jsx
import axios from 'axios';
// 引入 createSlice 创建切片
import { createSlice } from '@reduxjs/toolkit';
const goodsSlice = createSlice({
    name: 'goods', // 相当于vuex中的命名空间
    initialState: { // 初始数据
        count: 0,
        goodArr: []
    },
    reducers: {
        addCount: (state, action) => {
            state.count += action.payload

        },
        jianCount: (state, action) => {
            state.count -= action.payload
        },
        addgoods: (state, action) => {
            state.goodArr = action.payload
        }
    }
})

// 导出同步操作数据的方法
export const { addCount, jianCount, addgoods } = goodsSlice.actions;
// 导出异步操作数据的方法
export const asyncAddGoods = (payload) => (dispatch) => {
    // setTimeout(() => {
    //     dispatch(addCount(payload))
    // }, 1000)
    axios.get('http://kumanxuan1.f3322.net:8001/index/index').then(res => {
        console.log(11, res);
        dispatch(addgoods(res.data.data.banner))
    })
}

export default goodsSlice


```

userReducer.js 文件

```jsx
import { createSlice } from '@reduxjs/toolkit';
const userSlice = createSlice({
    name: 'user', // 相当于vuex中的命名空间
    initialState: { // 初始数据
        userinfo: {
            name: '张三',
            age: 20
        }
    },
    reducers: {
        edituser: (state, action) => {
            state.userinfo = action.payload
        },
        addAge: (state, action) => {
            state.userinfo.age += action.payload
        }
    }
})

// 导出同步操作数据的方法
export const { edituser, addAge } = userSlice.actions;
// 导出异步操作数据的方法
export const asyncedituser = (payload) => (dispatch) => {
    setTimeout(() => {
        dispatch(edituser(payload))
    }, 1000)
}

export default userSlice

```

store文件中的index.js 文件

```js
import { configureStore } from '@reduxjs/toolkit';
// 分别引入抽离出去的两个数据模块 goodsSlice 和 userSlice
import goodsSlice from './reducers/goodsReducer';
import userSlice from './reducers/userReducer';

const store = configureStore({
    // 方式2: 使用模块化拆分的写法
    reducer: {
        goods: goodsSlice.reducer,
        user: userSlice.reducer
    }
})

export default store

```

将如上的store 导入 项目的入口文件index.js

```js
// 引入store
import store from './store';
import { Provider } from 'react-redux';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <Provider store={store}>
    <Router>
      <App />
    </Router>
  </Provider>

);

```

在类组件的页面中使用store 仓库的数据

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
// 导入 对应的操作方法
import { addCount, jianCount, asyncAddGoods } from '../../store/reducers/goodsReducer';

class Myclass extends Component {
    render() {
        // console.log(this);
        return (
            <div>
                类组件中使用
                <div>
                    <button onClick={this.props.add}>同步+10</button>
                    count:{this.props.goods.count}
                    <button onClick={this.props.jian}>同步-10</button>
                    <button onClick={this.props.addAsync}>异步+100</button>
                    <ul>
                        {this.props.goods.goodArr.map((item) => {
                            return <li key={item.id}><img src={item.image_url} /></li>
                        })}
                    </ul>
                </div>
            </div>
        );
    }
}

function mapstatetoprops(state) {
    return state
}
function mapdispatchtoprops(dispatch) {
    return {
        add() {
            dispatch(addCount(10))
        },
        jian() {
            dispatch(jianCount(10))
        },
        addAsync() {
            dispatch(asyncAddGoods())

        },
    }
}

export default connect(mapstatetoprops, mapdispatchtoprops)(Myclass);

```

在函数组件中使用 store 仓库数据

```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { addAge, asyncedituser } from '../../store/reducers/userReducer';
const Myfun = () => {
    const { userinfo } = useSelector((state) => state.user);
    const dispatch = useDispatch();
    return (
        <div>
            函数组件中使用
            <p>
                <button onClick={() => {
                    dispatch(addAge(1))
                }}>用户年龄+1</button>
                <button onClick={() => {
                    dispatch(asyncedituser({ name: "测试", age: 30 }))
                }}>修改用户信息</button>
                userinfo:{userinfo.name} -- {userinfo.age}
            </p>
        </div>
    );
}

export default Myfun;

```

### `createSelector`

~~~
  import { createSelector } from '@reduxjs/toolkit';

   //相当于vue里的computed 
    let fn = createSelector(state=>state.list.list,(list)=>list.filter(item=>item.title.length<10))
    
    let filterList= useSelector(fn);  //过滤的标题长度小于10
~~~

