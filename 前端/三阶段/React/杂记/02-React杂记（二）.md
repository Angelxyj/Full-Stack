## jsx

class-->className

style-->样式对象

事件 onXxxx

string number 

babel 

## 组件

类组件（old)

函数组件

## 类组件(class component)

~~~
 class Counter extends React.Component{ //类组件
              state={
                 c:1 //计数器的初始值
              }
              render(){
                //let {c:a,d=0}=this.state;   //解构赋值改名，默认值
                let {c}=this.state;
                return <div>Counter组件 {c}</div>
              }
           }
~~~

## 修改state的值

~~~
 change(){  //这个方法作用就是要改变计数器的值
                console.log(this)
                this.setState({    //不但可以修改state的值还可以进行渲染
                    c:this.state.c+1
                })
              }
 tips:
    //找回this两种方案，1.箭头函数 2.改变this指向用bind
~~~

## setState

是异步的 拿到最新的值，要在回调里拿到

多次连续的setState，对象会进行合并，后面同名的键值会覆盖前面的
多次连续的setState，回调函数的形式，回调函数会依次执行

## 受控组件与非受控组件

表单元素的值受到数据控制(state或props)，受控组件

否则非受控组件

~~~
 <input value={str} onChange={this.input} onKeyUp={this.add} />
  input=(e)=>{  // 拿到文本框你输入的值
                this.setState({
                    str:e.target.value
                })
     }
~~~

## createPortal

~~~
 class MyDialog extends React.Component {
                render(){
                    return <div>我的对话框</div>
                }
            }
            let DialogObj=ReactDOM.createPortal(<MyDialog />,document.querySelector("body"))
            function App(){
                return <div>app  {DialogObj} <User /></div>
            }
~~~

## 类组件中常用的属性

~~~
state   setState
props 是只读的

ref标识节点 
1.<input id="username" value={username} onChange={this.input} ref={(node)=>this.userNameNode=node} />
this.userNameNode 就可以拿到节点了
2 React.createRef() 创建标识
 constructor(props){
          super(props);
                   
                    this.state={
                      ....
                    txtRef:React.createRef()
                   }
                }
  <input id="username" value={username} onChange={this.input} ref={txtRef} />
  this.state.txtRef.current  可以拿到节点
  
  constructor
  render
  componentDidMount
~~~

