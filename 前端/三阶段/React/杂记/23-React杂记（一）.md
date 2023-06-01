# React

是一个开发原生和web应用的js库

## 搭建react的开发环境

~~~
npm i react react-dom -S
react 核心包
react-dom 是web开发相关的包
babel 
1. es6 转es5
2. 解析jsx
<script src="js/react.development.js"></script>
<script src="js/react-dom.development.js"></script>
<script src="js/babel.js"></script>
~~~

## 第一个程序

~~~
 <script type="text/babel">  //必须把类型设置 babel解析
  let root = ReactDOM.createRoot(document.querySelector("#root"));
    root.render(<h2>hello</h2>);
 </script>
~~~

## jsx语法糖

javascript扩展的意思（js+xml)

原理就是React.createElement

~~~
let content=<div>hello world</div>;
let content = React.createElement("div",null,"hello world123");
~~~

jsx不是必须的但是用jsx提高开发的效率

class--->className 

style={{样式对象的键值对}}

{差值表达式}

{/*注释的内容*/}

布尔值不能直接显示到页面

数组可以直接渲染出来

对象不能直接渲染

~~~
  let root=ReactDOM.createRoot(document.getElementById("box"));
        let a=5;            //数字字符串可以直接渲染出来
        let b=!true;            //渲染到页面上不显示
        let arr=[1,2,3,4,5];  //数据可以直接渲染
        let obj={a:1,b:2};  //对象不能直接渲染
        let rootDiv=<div>hello world {a} b:{b?"true":"false"}  {arr} {obj.a} {obj.b}</div>;
        root.render(rootDiv);
~~~

## 合成事件

> React 合成事件（SyntheticEvent）是 React 模拟原生 DOM 事件所有能力的一个事件对象，即浏览器原生事件的跨浏览器包装器

~~~
在React17之前，React是把事件委托在document上的，React17及以后版本不再把事件委托在document上，而是委托在挂载的容器上了
事件名的首字母大写的
~~~

## 经典案例渲染列表

## 渲染html

~~~
 <div dangerouslySetInnerHTML={{__html:'<div>hello<i>world</i></div>'}}></div>
~~~

## 类组件渲染列表

~~~
   class List extends React.Component {
      state = {    //放置组件内部的数据
        productList: [
          {
            "id": 3,
            "img": "https://img12.360buyimg.com/jdcms/s300x300_jfs/t1/156232/40/12942/112817/6041ced3Ef7f383b3/7e29e51c25f7b77e.jpg.webp",
            "text": "<i class=\"more2_info_self\">自营</i>都市丽人文胸无钢圈聚拢中薄款蜂巢杯透气美背小花心波斯菊蕾丝胸罩内衣女2B9506 浅肤32/70A杯",
            "price": 98
          },
          {
            "id": 4,
            "img": "https://img12.360buyimg.com/jdcms/s300x300_jfs/t1/176546/26/4766/230854/607aa012E97def171/7417547d2d2e5da3.jpg.webp",
            "text": "兰叶源  创意中式陶瓷仕女香插香座插香器线香室内檀香家用线香薰炉摆件 小宫娥 坐  哑光",
            "price": 166
          },
          {
            "id": 5,
            "img": "https://img12.360buyimg.com/jdcms/s300x300_jfs/t1/119997/6/15535/123334/5f8d237bE36364127/a4d5ee3e0209d2a0.jpg.webp",
            "text": "<i class=\"more2_info_chosen\"></i>得力（deli）4905 电脑椅 家用办公椅 转椅人体工学网布椅子 时尚升降座椅",
            "price": 459
          },
          {
            "id": 6,
            "img": "https://img11.360buyimg.com/jdcms/s300x300_jfs/t1/158542/28/20137/213892/607aa011Ef99cf77f/ee308525809e7d32.jpg.webp",
            "text": "兰叶源  创意简约现代手工陶瓷人物家居小摆件客厅办公室书房工艺品装饰品 小胖",
            "price": 96
          },
          {
            "id": 7,
            "img": "https://img14.360buyimg.com/jdcms/s300x300_jfs/t1/184019/38/1640/74927/608bce5aEcbe6dfa2/7649c02195797afa.jpg.webp",
            "text": "<i class=\"more2_info_self\">自营</i>浪琴(Longines)瑞士手表 嘉岚系列 石英钢带女表 L42091917",
            "price": 9800
          },
        ]
      }
      render() { //必须的  渲染组件的内容
        console.log(this)
        //获取组件内部的数据的方法是this.state.xxx
        //获取组件外部数据的方法是this.props.xxx
        return <div>
          <ul>
            {
              this.props.productList.map((item) => {
              return  <li key={item.id}>
                  <img src={item.img} width="200" />
                  {item.price}
                  <p dangerouslySetInnerHTML={{ __html: item.text }}></p>

                </li>
              })
            }
          </ul>
        </div>
      }
    }
~~~

