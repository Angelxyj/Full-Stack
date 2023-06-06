## jsx

1.  样式   style={styleObject}   className="类名" （样式文件通过import 引入进来）

BEMcss命名规范  BEM是Block（块）、Element（元素）、Modifier（修饰符）的简写

2. 根元素只能有一个 （可以用Fragment(或<></>)包裹起来,)  

   Fragment 是一个空标记，不会增加页面的结构和复杂性

3.  图片引入的时候要用require

   ~~~
   <img src={require(图片的路径).default}  />
   ~~~

4. 合成事件（事件名的首字母要大写)

   ~~~
   <button onClick={()=>this.fun()}>+</button>
   <button onClick={this.fun.bind(null)}>+</button>
    <button onClick={this.fun.bind(this)}>+</button>
    事件对象是原生的事件对象
     e.stopPropagation();  //阻止冒泡
     e.preventDefault(); //阻止默认的行为
   ~~~

5. 字符串，数值 ，布尔值,undefined ,null 可以直接渲染，但 对象不能直接渲染 

6. React.createElement是jsx底层原理，可以用来动态渲染组件

   ~~~
   import {One} from '../components/one/One'
   import {Two} from '../components/two/Two'
   import {Shop} from '../components/shop/Shop'
   export {One,Two,Shop}
   
   import * as coms from '../../allComponents/index'
   React.createElement(coms[“要渲染组件的名字”]
   ~~~

   

## 组件

### 1.函数组件

### 2.类组件

####  2.1常用属性

+  state  存储组件的本身的数据 要修改state 一定要用setState,而不要直接修改

+ props 是只读的

+ refs 标识节点或元素    

  <input ref={(node)=>this.inputNode=node} />

  let inputRef=createRef(); <input ref={inputRef} />  inputRef.current就可以获取到节点或组件了

####   2.2生命周期的钩子函数

#####   挂载

​          constructor 

​		  getDerivedStateFromProps

​		  render

​		  componentDidMount

#####     更新

​           getDerivedStateFromProps

​			shouldComponentUpdate

​			render

​			getSnapshotBeforeUpdate

​			componentDidUpdate

#####     卸载

​		componentWillUnmount

#### 2.3组件的传值

##### 2.3.1  父组件传值给子组件

​	a. 通过属性传值给子组件  <组件  属性={值}></组件>

​	b. 通过方法传值给子组件

        ~~~~
 import React, {createRef } from 'react'
 1.state={
    ...
    counterRef:createRef() //创建一个标识给Counter组件
  }
 2. <Counter n={n}  ref={counterRef}></Counter>
 3.在子组件里写一个方法
 	 getN=(n)=>{  //n就是父组件传递过来的值
    console.log(n);
  }
 4. sendValueToChild=()=>{  //把state里的值传递给子组件
     this.state.counterRef.current.getN(this.state.n)  //父组件调用子组件里的方法把值传给子组件
  }
        ~~~~

##### 2.3.2 子组件向父组件传值

父组件向子组件传递一个 方法，子组件调用父组件的方法把数据传递给父组件

  1) 父组件先把方法传递给子组件

  ~~~
  editOk=(newValue,index)=>{  //子组件要调用这个方法，来更新当前编辑项目的内容
      let arrCopy=[...this.state.arr]; //得到一个副本
      arrCopy[index]=newVal; //改数组的数据
      this.setState({
        arr:arrCopy
      })
  }
  
  <Dialog  editOk={this.editOk}></Dialog>
  this.props.editOk(this.state.curItem,this.state.curIndex)  //子组件向父组件传值 调用了父组件的方法进行向父组件传值
  ~~~

