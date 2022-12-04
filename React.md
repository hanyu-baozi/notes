# 定义

**构建用户界面 JavaScript 库**

1.  发送请求获取数据
2.  处理数据（过滤，整理格式等）
3.  操作 DOM 呈现页面

# 特性

1、声明式编程

2、组件化

# 命令

创建

```
npx create-react-app my-app
create-react-app jsx
```

运行

```
cd jsx
npm start
```

停止

```
ctrl+c
```

|                   |                                         |
| ----------------- | --------------------------------------- |
| src               | 源代码                                  |
| public            | 公共目录                                |
| node_modules      | 依赖项                                  |
| package.json      | JSON 文件，项目所需要的依赖项，项目配置 |
| package-lock.json | 记录安装到项目中的所有依赖项的确切版本  |

```
const App = () => {
	return <div>Hi there!</div>;
};

const App = () => {
  return React.createElement(
	"div", //创建的元素类型
	null,
	"Hi there!"//div显示文本
  );
};

```

```
const App = () => {
	return <div>
      <ul>
        <li>Hi</li>
        <li>Bye There</li>
        <li>Gotcha</li>
      </ul>
    </div>;
};

const App = () => {
  return React.createElement("div", null, React.createElement("ul", null, React.createElement("li", null, "Hi"), React.createElement("li", null, "Bye There"), React.createElement("li", null, "Gotcha")));
};
```

# JSX

**用花括号把 JavaScript 表达式嵌入到 JSX 中**

三元运算符 立即执行函数 闭包

基本数据类型 （number|string|boolean(不显示|undefined(不显示)|null(不显示)）

数组（不嵌套对象）

对象的具体值

对象方法返回值

样式类名（className）

```react
let a = {
  value: 'ok',
  getData() { return +new Date() }
};

ReactDOM.render(
  <section>
    {a.getData()}
    <br />
    {a.value}
  </section>,
  root,
  () => { console.log(a); }
)
```

```html
<div style="background-color:red;"></div>
```

```jsx
<div style={{ backgroundColor: "red" }}></div>
```

```react
import React from 'react';
import ReactDOM from 'react-dom';



const App = () =>{
    const buttonText = {text: 'Click me'};
    const labelText = 'Enter name:';
    return (
        <div>
            <label className="label" htmlFor="name">
                {labelText}
            </label>
            <input id='name' type="text"/>
            <button style={{backgroundColor:'blue',color:'white'}}>
                {buttonText.text}
            </button>
        </div>
    );
};

ReactDOM.render(
    <App />,
    document.querySelector('#root')
)
```

# babel.js 作用

浏览器不能直接解析 JSX 代码，需要 babe 转译为纯代码运行

加上 type="text/babel"，声明 babel 处理

# 提供默认样式

https://semantic-ui.com/introduction/getting-started.html

虚拟数据 faker js

```
npm install --save faker
```

# 地理定位服务

```react
    window.navigator.geolocation.getCurrentPosition(
        (position) => console.log(position),
        (err) => console.log(err)
    );
    return <div>Hi there!</div>;
```

# 组件

## 类

```javascript
class App extends React.Component {
  render() {
    window.navigator.geolocation.getCurrentPosition(
      (position) => console.log(position),
      (err) => console.log(err)
    );
    return <div>Latitude</div>;
  }
}

ReactDOM.render(<App />, document.querySelector("#root"));
```

## 函数

```React
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

# 生命周期

constructor -》render-》componentDidMount-》

componentDidMount-》componentDidUpdate-》componentWillUnMount

constructor 不要做数据在构造函数内部加载

componentDidUpdate 调用组件更新方法（数据加载）

componentWillUnmount 卸载方法

```react
class Clock extends React.Component {
    state = {time: new Date().toLocaleTimeString()};

    componentDidMount() {
        setInterval(() => {
            this.setState({time: new Date().toLocaleTimeString()})
        },1000)
    }

    render() {
        return (
        	<div className="time">
            	The time is: {this.state.time}
            </div>
        );
    }
}
```

React 18 不支持 ReactDOM.render

解决办法

```react
import ReactDOM from 'react-dom';
ReactDOM.render(<App />, document.getElementById('root'))
```

```react
import { createRoot } from 'react-dom/client';
const container = document.getElementById('root');
const root = createRoot(container);
root.render(<App />)
```

上下文和这些事件处理程序

```react
class Car{
	constructor(){
		super()
        //解决办法一 绑定驱动函数
        // this.drive = this.drive.bind(this);

	}

	setDriveSound(sound){
		this.sound = sound;
	}

	drive(){
		return this.sound;
	}
}

const car = new Car();
car.setDriveSound('room');

const drive = car.drive;
//SyntaxError:'super'keyword unexpected here 没有扩展任何东西
```

```react
import React from "react";

class SearchBar extends React.Component {
  state = { term: " " };

  onFormSubmit = (event) => {
    event.preventDefault();

    console.log(this.state.term);
  };
    //解决办法二 箭头函数
    //onFormSubmit = (event) => {
        //event.preventDefault();
        //console.log(this.state.term);
    //};
  render() {
    return (
      <div className="ui segment">
        <form className="ui form" onSubmit={this.onFormSubmit}>
          {/*传递对函数的引用*/}
          <div className="field">
            <label>Image Search</label>
            {/* <input type="text" onChange={this.onInputChange()}/> */}
            {/* 更改自动发生 */}
            <input
              type="text"
              value={this.state.term}
              onChange={(e) => this.setState({ term: e.target.value })}
            />
            {/* <input type="text" onChange={(e) =>console.log(e.target.value)}/> */}
            {/* 缩写语法 */}
          </div>
        </form>
      </div>
    );
  }
}

export default SearchBar;
```

# axios

调用的时候会返回称为 Promise 的对象

```React
import axios from "axios";

const KEY = "AIzaSyASW5c97fiTkN-mmArMPzuWHiEYg7qvrMQ";

export default axios.create({
  baseURL: "https://www.googleapis.com/youtube/v3",
  params: {
    part: "snippet",
    type: "video",
    maxResults: 5,
    key: KEY,
  },
});
```

# fetch 函数

数据请求

**fetch API**

获取资源接口（包括跨域请求）

fetch 没有自动处理返回值的格式。返回的是一个 respnse 对象

```React
  fetch(
    'https://www.easy-mock.com/mock/59801fd8a1d30433d84f198c/example/user/all'
  )
    .then(res => res.json())
    .then(data => {
      console.log(data)
      this.setState({users: data})
    })
    .catch(e => console.log('错误:', e))
```

# Refs

弹性文件系统

用来绑定到 render()输出的任何组件上

```react
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

# Hook （钩子系统）

## 状态管理函数

状态等于一对象

## useState

访问函数组件内部状态（跟踪）

## useEffect

用途：获取数据

事件监听或订阅

输出日志

## useRef

获取 DOM 元素

```
通过 .current 拿到当前 dom 元素
可使用原生 dom 事件
inputRef.current.focus()
```

保存变量

1. 用于获取并存放组件的 dom 节点, 以便直接对 dom 节点进行原生的事件操作，如果监听事件或进行大小测量。
2. 利用 useRef 解决由于 hooks 函数式组件产生闭包时无法获取最新 state 的问题。
3. 存放想要持久化( instant )的数据, 该数据不和 react 组件树的渲染绑定。该数据可以是任何类型，数字、数组、对象、函数，都可以。

管理焦点，文本选择或媒体播放。

注：current 更新不会主动使页面渲染

# Router

路由

window.location.pathname==="/名字"

PopStateEvent 是 popstate 事件的接口

# 报错

不允许标记我们要传入的箭头函数，将效果用作异步

```
Warning: useEffect must not return anything besides a function, which is used for clean-up.

It looks like you wrote useEffect(async () => ...) or returned a Promise. Instead, write the async function inside your effect and call it immediately:

useEffect(() => {
  async function fetchData() {
    // You can await here
    const response = await MyAPI.getData(someId);
    // ...
  }
  fetchData();
}, [someId]); // Or [] if effect doesn't need props or state

Learn more about data fetching with Hooks: https://reactjs.org/link/hooks-data-fetching
    at Search (http://localhost:3000/main.1d0908c403d661778086.hot-update.js:28:74)
    at div
    at __WEBPACK_DEFAULT_EXPORT__
```

解决方法

1、声明辅助函数（推荐）

```react
useEffect(() =>{
	const search = async () =>{
	await axios.get('aslkdfj');
	};
	search();
},[term]);
```

2、promise

```React
  useEffect(() => {
    axios.get("aslkdfj").then((response) => {
      console.log(response.data);
    });
  }, [term]);
```

报错“A component is changing an uncontrolled input of type text to be controlled”

解决办法

受控和非受控

```
给val一个初始值

const [val, setVal] = useState('');

给input的value属性一个初始值

<input value={val || ''}/>
```

渲染字符串出现标签

找出禁令的每一个实例和关闭跨度用字符串替换

创建 span 元素包装程序，创建另一个 span 包装程序

# dangerouslySetInnerHTML

dangerouslySetInerHTML 是 React 为浏览器里 DOM 提供 innerHTML 的替换方案

确保查询时不会发大量请求

# Redux

状态容器，提供可预测化的状态管理

![image-20221129092258086](C:\Users\包子\AppData\Roaming\Typora\typora-user-images\image-20221129092258086.png)

## 问题

```React
import { createStore } from "redux";
//createstore中间有到横线
```

解决

```react
import { legacy_createStore as createStore } from "redux";
```

**提供 API 包装应用程序**

```react
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import TodoApp from './TodoApp'

import { Provider } from 'react-redux'
import store from './redux/store'

// As of React 18
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <Provider store={store}>
    <TodoApp />
  </Provider>
)
```

**connect 函数**

从 Redux store 读取值

## Store

在 redux 里面，只有一个`Store`，整个应用需要管理的数据都在这个`Store`里面。这个`Store`我们不能直接去改变，我们只能通过返回一个新的`Store`去更改它。`redux`提供了一个`createStore`来创建`state`

```javascript
import { createStore } from "redux";
const store = createStore(reducer);
```

## action

这个 `action` 指的是视图层发起的一个操作，告诉`Store` 我们需要改变。比如用户点击了按钮，我们就要去请求列表，列表的数据就会变更。每个 `action` 必须有一个 `type` 属性，这表示 `action` 的名称，然后还可以有一个 `payload` 属性，这个属性可以带一些参数，用作 `Store` 变更：

```javascript
const action = {
  type: "ADD_ITEM",
  payload: "new item", // 可选属性
};
```

## Reducer

在上面我们定义了一个`Action`,但是`Action`不会自己主动发出变更操作到`Store`，所以这里我们需要一个叫`dispatch`的东西，它专门用来发出`action`，不过还好，这个`dispatch`不需要我们自己定义和实现，`redux`已经帮我们写好了，在`redux`里面，`store.dispatch()`是 `View`发出 `Action` 的唯一方法。

```javascript
store.dispatch({
  type: "ADD_ITEM",
  payload: "new item", // 可选属性
});
```

当 `dispatch` 发起了一个 `action` 之后，会到达 `reducer`，那么这个 `reducer` 用来干什么呢？顾名思义，这个`reducer`就是用来计算新的`store`的，`reducer`接收两个参数：当前的`state`和接收到的`action`，然后它经过计算，会返回一个新的`state`。(前面我们已经说过了，不能直接更改`state`，必须通过返回一个新的`state`来进行变更。)

```javascript
const reducer = function(prevState, action) {
  ...
  return newState;
};
```

## **Redux Thunk**

原始的 Redux 里面，action creator 必须返回 plain object 必须是同步的，使用 Redx-Thunk 可以发出异步

直接写异步代码

```react
store.dispatch({type: 'SHOW_NOTIFICATION',text:'You logged in.'})
setTimeout(()=>{
	store.dispatch({type: 'HIDE_NOTIFICATION'})
},5000)
```

连接 Redux 组件

```react
this.props.dispatch({type: 'SHOW_NOTIFICATION',text:'You logged in.'})
setTimeout(() => {
	this.props.dispatch({type:'HIDE_NOTIFICATION'})
},5000)
```

## **deucers**

Reducers 指定了应用状态的变化响应 actions 并发送到 store

不能在 reducer 做的操作：

​ 修改传入参数

​ 执行有副作用的操作，如 API 请求和路由跳转

​ 调用非纯函数，如 Date.now()或 Math.random()

## 规则

始终返回除 undefined 之外的任何值

使用它返回先前状态值和操作对象的某些状态

```react
import axios from "axios";

export default (state, action) => {
  return {}; //必须始终返回除undefined之外的任何值

  //bad
  return document.querySelector("input");
  return axios.get("/posts");
  //根据状态和动作对象返回某种类型的组合或变化
  //只返回输入参数的值
  return state + action;
};

```

不改变其输入状态的参数

（直接修改对象或者数组是不会被检测到并且组件不会被重新渲染）

![image-20221129092404704](C:\Users\包子\AppData\Roaming\Typora\typora-user-images\image-20221129092404704.png)

# lodash

**安装**

```
npm install --save lodash
```

## **\_.memoize(func,[resolver])**

创建会缓存 func 结果的函数

提供了 `resolver` ，用 resolver 的返回值作为 key 缓存函数的结果，解析 result 的键。 默认情况下用第一个参数作为缓存的 key。 `func` 在调用时 `this` 会绑定在缓存函数上。

(防止重复请求)

```js
//参数调用函数
export const fetchUser = (id) => async (dispatch) => {
  const response = await jsonPlaceholder.get(`/users/${id}`);
  dispatch({ type: "FETCH_USER", payload: response.data });
};
```

## \_.uniq(array)

去重数组副本

## \_.map

迭代遍历处理数组或对象元素

# router

安装

```
npm install react-router-dom@5
```

## 关键字

exact

默认为 false

如果 true 时需要路由相同时匹配

```React
<BrowserRouter>
<div>
    <Route path="/" component={PageOne} />
    <Route path="/" component={PageOne} />
    <Route path="/pagetwo" component={PageTwo} />
</div>
</BrowserRouter>
```

显示效果

1.未使用 exact 2.使用 exact

![image-20221203123827962](C:\Users\包子\AppData\Roaming\Typora\typora-user-images\image-20221203123827962.png)![image-20221203123949551](C:\Users\包子\AppData\Roaming\Typora\typora-user-images\image-20221203123949551.png)
