## 组件的生命周期

### 挂载

~~~
constructor()  最早执行，状态的初始化
static getDerivedStateFromProps()    更新派生状态 不常用 
render()   渲染组件的内容
componentDidMount() 
	作用:
	1.拿到节点，做dom操作 
    2.父组件调用子组件里的方法
        let bs =React.createRef();
        bs.current.子组件的方法名（）   //父组件调用子组件里的方法

这个挂载阶段常用的钩子函数有三个 constructor,render,componentDidMount
执行次序
  constructor
  render
  componentDidMount
~~~

### 更新阶段钩子函数

~~~
static getDerivedStateFromProps()
shouldComponentUpdate()   可以优化组件渲染的次数
  //这个钩子函数会返回布尔值  true 组件才渲染 false 组件不渲染
    shouldComponentUpdate(nextProps,nextState){ //优化渲染的次数
        return this.props.flag!==nextProps.flag
    }
  (PureComponent也可以优化渲染的次数)
render()
getSnapshotBeforeUpdate()  一定要和componentDidUpdate一起使用  
//dom更新之前的状态叫快照(snapshot)   在更新之前获取快照
componentDidUpdate()  // 监控组件里数据变化
~~~

### 卸载阶段

~~~
componentWillUnmount()
主要清理资源和善后的工作
~~~



## 建立脚手架项目

~~~
npx create-react-app 项目的名字

项目结构
  readme.md 项目的说明文件
  package.json npm包的管理文件
  .gitignore  上传时候忽略的文件
  src   源码目录
  public 项目的静态资源文件
  node_modules 项目的依赖文件
  
~~~

安装ES7+ React/Redux/React-Native snippets插件

rcc 产生一个类组件

rfc 产生一个函数组件

