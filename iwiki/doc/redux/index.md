# Redux学习笔记

## Redux介绍
Redux 是 JavaScript 状态容器， 提供可预测化的状态管理。它专门为React.js这门框架设计，但并不是只能用于react,可以用于任何界面库。
## Redux的设计原则
Redux在设计与实现时，遵循三大原则或者说规范限制
### 单一数据源
整个应用程序的转态数据仅存储子一个Store中，数据

### 只读的 State

唯一的 Store 也是只读的，应用程序无法直接修改状态数据。Store 对象本身的 API 非常少，仅有四个方法：
+ `getState()`：获取当前状态数据
+ `dispatch(action)`：推送触发变化
+ `subscribe(listener)`：订阅数据变化
+ `replaceReducer(nextReducer)`：

显而易见，没有提供设置状态的方法。其中的 `dispatch(action)` 是唯一改变数据的方法，比如：
```javascript
var action = {
  type: 'ADD_USER',
  user: {name: 'chuonye.com'}
};
store.dispatch(action);
```

`dispatch` 方法会将 `action` 传递给 `Redux`，`action` 就是一个普通对象，包含触发的操作类型以及数据。

### 使用纯函数执行状态的更新

Redux 接收到 `action` 后，会使用一个纯函数来处理，这些函数被称为 Reducers：
```javascript
var someReducer = function(state, action) {
  ...
  return state;
}
```
Reducer 接收当前的 state 和 action 作为参数，它也不能直接修改原有的数据，而是通过返回一个新的 state 来修改。总的来说，Redux 是一个帮助应用统一管理状态数据的工具，它遵循严格的单向数据流（Store 只读）设计，使得应用的行为变得可预测且容易理解。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a2b3356cfe6b498ebd130357fcf78ec5~tplv-k3u1fbpfcp-watermark.image)


## 使用Redux的基本流程
在介绍使用Redux的基本使用流程前，我们基于原生的方法实现TodoList的案例，在此基础之上分别演示Redux和React-Redux的使用流程
### 原生React的实现TodoList

示例：
```javascript
import React, { Component } from 'react';
import {Row,Col,Button,Divider,Input,List,} from 'antd';

class TodoList extends Component {

    constructor(props) {
        super(props)
        this.state = {
            todos: ["篮球", "足球", "羽毛球"],
            inpVal: ""
        }
    }

    handleChange = (e) => {
        this.setState({ inpVal: e.target.value })
    }

    handleClick = () => {
        const { inpVal } = this.state
        let todos = this.state.todos
        todos.push(inpVal)
        this.setState({ todos })

    }

    handleDelete = (id) => {
        let {todos} = this.state
        todos = todos.filter((v,index) => index != id)
        this.setState({todos})
    }

    render() {
        return (
            <div>
                <h3>React原生实现TodoList</h3>
                <Divider />
                <Input
                    onChange={this.handleChange}
                    placeholder="请输入"
                ></Input>
                <Button onClick={this.handleClick}>添加</Button>
                <Divider />
                <Row>
                    <Col span={6}>
                        <List
                            bordered
                            dataSource={this.state.todos}
                            renderItem={(item, index) => (
                                <List.Item
                                    extra={<a onClick={this.handleDelete.bind(this,index)}>删除</a>}
                                >{item}</List.Item>)}
                        >
                        </List>
                    </Col>
                </Row>
            </div>
        )
    }
}

export default TodoList
```

### 基于Redux实现TodoList

**安装 Redux**
```javascript
yarn add redux --save
```
基于Redux实现TodoList的具体工作步骤：

+ **创建 Action**
  + 根目录下创建 `action` 文件夹并在文件夹下创建 `index.jsx`
  + 构建`action`函数用来返回 `action` 对象，注意 `action` 必须包含 `type` 属性
  + 将` action` 并导出

示例：
```javascript
// src/action/index.jsx
// 创建Todo的Action
const addTodoList = (iptVal) => ({ type: 'ADD', iptVal })
// 查询当前的TodoList
const seleteTodoList = () => ({ type: 'SELECT' })
// 监听Input
const InputChange = (iptVal) => ({ type: 'INPUT', iptVal })
// 删除Todo的Action
const DeleteTodoList = (index) => ({ type: 'DELETE', index })

module.exports = {
    addTodoList,seleteTodoList,InputChange,DeleteTodoList
}
```

+ **创建 Reducer**
  + 根目录下创建 `reducer` 文件夹并在文件夹下创建 `index.jsx`
  + 构建 `reducer`，注意 `reducer` 要接收两个参数
  + 第一个参数是默认状态，定义一个初始化的 `state`，然后进行赋值

示例：
```javascript
/src/reducer/index.jsx
// 1. 初始化
const initState = {
    todos: ["篮球", "足球", "羽毛球"],
    iptVal: ""
}
// 2. 创建reducer
const reducer = (state = initState, action) => {
    let newState = Object.assign({}, state, action);
    switch (action.type) {
        case "ADD":
            const { iptVal, todos } = state
            newState.todos.push(iptVal)
            return newState
        case 'SELECT':
            return Object.assign({}, state, action)
        case 'INPUT':
            return Object.assign({}, state, action)
        case 'DELETE':
            const { index } = action
            newState.todos.splice(index, 1)
            return newState
        default:
            return state;
    }
}

export default reducer
```

+ **创建 Store**
    + 跟目录下创建 `store` 的文件夹并且在文件夹下创建 `index.jsx`
    + 使用 `createStore` 函数里面第一个参数接收的是 `reducer`
    + 导入 `reducer` 设置到函数中 `createStore(reducer)`
    + 将创建的 `store` 进行导出

示例：
```javascript
import { createStore } from 'redux'

import reducer from './../reducer/'

const store = createStore(reducer)

export default store
```
+ **组件中使用Redux**
    + 绑定按钮监听事件
    + 在组件加载完毕的时候通过 `store` 进行监听器的注册，返回值可以用来监听注册
    + 通过 `store.dispatch(action)` 发送action
示例：
```javascript
import React, { Component } from 'react';
import {
    Divider,
    Input,
    Button,
    Row,
    Col,
    List
} from 'antd'
import store from '../../store/index'

import {
    addTodoList,
    seleteTodoList,
    InputChange,
    DeleteTodoList
} from './../../action/index'


class Home extends Component {

    constructor(props) {
        super(props)
        this.state = store.getState()
    }

    componentWillMount() {
        const action = seleteTodoList()
        // 发送action
        store.dispatch(action)
    }

    handleChange = (e) => {
        const action = InputChange(e.target.value)
        // 发送action
        store.dispatch(action)
    }

    handleClick = () => {
        const { iptVal } = this.state
        const action = addTodoList(iptVal)
        // 发送action
        store.dispatch(action)
    }

    handleDelete = (index) => {
        const action = DeleteTodoList(index)
        // 发送action
        store.dispatch(action)
    }

    componentDidMount() {
        store.subscribe(() => this.setState(store.getState()))
    }

    render() {
        return (
            <div
                style={{
                    width: 500, height: 500, textAlign: 'center',
                    margin: '0 auto'
                }}
            >
                <h3>Redux实现TodoList</h3>
                <Divider />
                <Input
                    onChange={this.handleChange}
                    placeholder="请输入"
                    style={{ width: 300, marginRight: 50 }}
                    value={this.state.iptVal}
                ></Input>
                <Button onClick={this.handleClick}>添加</Button>
                <Divider />
                <Row>
                    <Col>
                        <List
                            bordered
                            dataSource={this.state.todos}
                            renderItem={(item, index) => (
                                <List.Item
                                    extra={<span onClick={this.handleDelete.bind(this, index)}>删除</span>}
                                >{item}</List.Item>)}
                        >
                        </List>
                    </Col>
                </Row>
            </div>
        )
    }
}

export default Home;
```

#### Redux调用流程的总结

**基本的使用步骤：**
1. 创建`action`
2. 发送 `action` 至 `reducer` -- `store.dispatch(action)`
3. `reducer(preState,action)`进行业务逻辑的处理**返回**给 `store`
4. 监听 `store` 的变化，基于`store` 提供的api `store.getState()`获取最新的`state`
```
action --> store.dispatch(action) --> reducer(action) --> store
```


### 基于React-Redux实现TodoList

#### 创建项目
```
create-react-app demo02
cd demo02
yarn add redux --save
yarn add react-redux --save
yarn add antd@"^3.26.20" --save
yarn add react@"^16.14.0" --save
```

#### 实现步骤
1. 基于 `redux` 构建 `store`，在 `src` 下创建 `store` 目录并创建 `index.jsx` 文件
2. 创建 `reducer/index.jsx` 文件，构建 `reducer` 响应 `action`
3. 通过 `createStore` 把 `reducer` 注入返回 `store`
4. 在 `src/App.js` 引入 `store` 并导入 `Provider` 将整个结构进行包裹，并且传递 `store`
5. 引入 `connect` 对组件进行加强，`mapDispatchToProps` 返回的是一个对象 `{key:方法名,value:调用dispatch发送action}`，在组件中通过 `this.props` 可以获取到该对象 。**connect** 函数的基本介绍：
| 参数名                                       | 类型     | 说明                                                        |
| -------------------------------------------- | -------- | ------------------------------------------------------------ |
| **mapStateToProps(state,ownProps)**          | Function | 该函数将 **store** 中的数据作为 **props** 绑定到到组件<br />**state**：redux 中的 store <br />**ownProps**：组件中的 props |
| **mapDispatchToProps(dispatch,ownProps)**    | Function | 将 **action** 作为 **props** 绑定到组件中<br />**dispatch**：就是 store.dispatch() <br />**ownProps**：组件中的props |
| mergeProps(stateProps,dispachProps,ownProps) | Function | 不管是stateProps,dispachProps 都需要和ownProps 合并以后赋给组件，通常情况下可以不传递这个参数connect会使用Object.assign替代该方法 |
| options                                      | Object   | 定制 connector 的行为                                        |

---

#### 构建 reducer
```javascript
// src/reducer/index.jsx

const initState = {
  todos: ["篮球", "足球", "羽毛球"],
}

const rootReducer = (state = initState, action) => {

  if (action.type === "ADD") {
    const { iptVal } = action
    let newState = JSON.parse(JSON.stringify(state))
    newState.todos.push(iptVal)
    return newState
  } else if (action.type === 'SELECT') {
    let newState = JSON.parse(JSON.stringify(state))
    return newState
  } else if (action.type === "DELETE") {
    const { index } = action
    let newState = JSON.parse(JSON.stringify(state))
    newState.todos.splice(index, 1)
    return newState
  } else {
    return state
  }
}

export default rootReducer
```
#### 构建 store
```javascript
// src/store/index.jsx
import { createStore } from "redux";
import reducer from "../reducer";
const store = createStore(reducer)
export default store
```
#### 导入Provider传递store
```javascript
// App.jsx
import React, {
  Component
} from 'react';

import {
  Provider
} from 'react-redux'

import store from './store';
import AddTodo from './pages/AddTodo';
import TodoList from './pages/TodoList'
import './App.css';


class App extends Component {

  render() {
    return (
      <div style={{ width: 500, height: 500, textAlign: 'center', margin: '0 auto' }}>
        <Provider store={store}>
          <h3>React-Reudx实现TodoList</h3>
          <AddTodo />
          <br />
          <TodoList />
        </Provider>
      </div>
    );
  }
}

export default App;
```

#### AddTodo 组件
```javascript
import React, { Component } from 'react';
import {
  Input,
  Button,
} from 'antd'

import { connect } from 'react-redux'

class AddTodo extends Component {
  constructor(props) {
    super(props);
    this.state = {
      iptVal: ""
    }
  }

  handleClick = () => {
    const { iptVal } = this.state
    this.props.AddAction(iptVal)
  }

  render() {
    return (
      <div>
        <Input
          onChange={(e) => this.setState({ iptVal: e.target.value })}
          placeholder="请输入"
          style={{ width: 300, marginRight: 50 }}
          value={this.state.iptVal}
        ></Input>
        <Button onClick={this.handleClick}>添加</Button>
      </div>
    );
  }
}


const mapDispatchToProps = (dispatch) => {
  return {
    AddAction: (iptVal) => {dispatch({type: 'ADD',iptVal})}
  }
}

export default connect(null, mapDispatchToProps)(AddTodo);

```

#### TodoList 组件
```javascript

import React, { Component } from 'react';
import {
  Row,
  Col,
  List
} from 'antd'
import { connect } from 'react-redux'
class TodoList extends Component {

  handleDelete = (index) => {
    this.props.RemoveAction(index)
  }

  render() {
    return (
      <div>
        <Row>
          <Col>
            <List
              bordered
              dataSource={this.props.todos}
              renderItem={(item, index) => (
                <List.Item
                  extra={<span onClick={this.handleDelete.bind(this, index)}>删除</span>}
                >{item}</List.Item>)}
            >
            </List>
          </Col>
        </Row>
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return state
}

const mapDispatchToProps = (dispatch) => {
  return {
    RemoveAction: (index) => {dispatch({type: 'DELETE',index})}
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(TodoList)

```


推荐阅读：
[React-Redux的用法](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)
[Redux 图解原理](https://zhuanlan.zhihu.com/p/144222700)
[React-Redux 中文文档](https://segmentfault.com/a/1190000017064759)
[Redux中文文档](https://www.redux.org.cn/docs/introduction/ThreePrinciples.html)
[Redux工作流](https://juejin.cn/post/6844903870246699022)
[Redux工作步骤](https://www.cnblogs.com/goodjobluo/p/9077010.html)
[Redux图解原理](https://zhuanlan.zhihu.com/p/144222700)
[源码分析](https://zhuanlan.zhihu.com/p/26354860)
[Redux项目](https://zhuanlan.zhihu.com/p/24056049)
[实现思想](https://zhuanlan.zhihu.com/p/57209324)
[Redux-thunk](https://www.daimajiaoliu.com/daima/4833b3ea39003e0)
[Redux简介](https://www.cnblogs.com/wendyw/p/10070663.html)

[Redux入门](https://www.cnblogs.com/chuonye/p/14320166.html)
[Redux+Flux](https://www.cnblogs.com/yoissee/p/5928314.html?hmsr=toutiao.io&utm_campaign=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
[Redux合并](https://www.jianshu.com/p/888eb2cab46d)
