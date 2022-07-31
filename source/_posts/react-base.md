---
title: 'React基础'
date: '2022-03-05'
updated: '2021-03-26'
tags:
  - react
categories:
  - react
  - 前端框架
description: ' '
---

## React 介绍

### React 是什么

> 一个专注于构建用户界面的 JavaScript 库，和 vue 和 angular 并称前端三大框架，不夸张的说，react 引领了很多新思想，世界范围内是最流行的 js 前端框架，最近发布了 18 版本，加入了很多新特性

### React 有什么特点

#### 声明式 UI（JSX）

> 写 UI 就和写普通的 HTML 一样，抛弃命令式的繁琐实现

#### 组件化

> 组件是 react 中最重要的内容，组件可以通过搭积木的方式拼成一个完整的页面，通过组件的抽象可以增加复用能力和提高可维护性

#### 跨平台

> react 既可以开发 web 应用也可以使用同样的语法开发原生应用（react-native），比如安卓和 ios 应用，甚至可以使用 react 开发 VR 应用，想象力空间十足，react 更像是一个 元框架 为各种领域赋能

## 环境初始化

### 安装

```bash
npx create-react-app react-basic
```

### 启动项目

```bash
npm start
```

## JSX 基础

### JSX 介绍

概念：JSX 是 JavaScript XML（HTML）的缩写，表示在 JS 代码中书写 HTML 结构
作用：在 React 中创建 HTML 结构（页面 UI 结构）
优势：

1. 采用类似于 HTML 的语法，降低学习成本，会 HTML 就会 JSX
2. 充分利用 JS 自身的可编程能力创建 HTML 结构

注意：JSX 并不是标准的 JS 语法，是 JS 的语法扩展，浏览器默认是不识别的，脚手架中内置的 @babel/plugin-transform-react-jsx 包，用来解析该语法
![896dcceeeb4dbc9c4c49f4007c5d9139.png](https://img.gejiba.com/images/896dcceeeb4dbc9c4c49f4007c5d9139.png)

### JSX 中使用 js 表达式

#### 语法以错误检查调试等

`{ JS表达式 }`

```JSX
const name = 'Hello World'
<h1>{name}</h1>
```

#### 可以使用的表达式

1. 字符串、数值、布尔值、null、undefined、object（ [] / {} ）
2. 1 + 2、'123abc'.split('')、['a', 'b', 'c'].join('-')
3. fn()

#### 特别注意

if 语句/ switch-case 语句/ 变量声明语句，这些叫做语句，不是表达式，不能出现在 {} 中！！

### JSX 列表渲染

```JSX
// 来个列表
const songs = [
  { id: 1, name: '刘备' },
  { id: 2, name: '关羽' },
  { id: 3, name: '张飞' },
  { id: 4, name: '赵云' }
]

function App() {
  return (
    <div className="App">
      <ul>
        {
          songs.map(item => <li>{item.name}</li>)
        }
      </ul>
    </div>
  )
}

export default App
```

**注意**：需要为遍历项添加 key 属性

1. key 在 HTML 结构中是看不到的，是 React 内部用来进行性能优化时使用
2. key 在当前列表中要唯一的字符串或者数值（String/Number）
3. 如果列表中有像 id 这种的唯一值，就用 id 来作为 key 值
4. 如果列表中没有像 id 这种的唯一值，就可以使用 index（下标）来作为 key 值

### JSX 条件渲染

**作用**：根据是否满足条件生成 HTML 结构，比如 Loading 效果
**实现**：可以使用 三元运算符 或 逻辑与(&&)运算符

```JSX
// 来个布尔值
const flag = true
function App() {
  return (
    <div className="App">
      {/* 条件渲染字符串 */}
      {flag ? 'react真有趣' : 'vue真有趣'}
      {/* 条件渲染标签/组件 */}
      {flag ? <span>this is span</span> : null}
    </div>
  )
}
export default App
```

### JSX 样式处理

- 行内样式 - style

```JSX
function App() {
  return (
    <div className="App">
      <div style={{ color: 'red' }}>this is a div</div>
    </div>
  )
}

export default App
```

- 行内样式 - style - 更优写法

```JSX
const styleObj = {
    color:red
}

function App() {
  return (
    <div className="App">
      <div style={ styleObj }>this is a div</div>
    </div>
  )
}

export default App
```

- 类名 - className（推荐）

```css style.css
.title {
  font-size: 30px;
  color: blue;
}
```

- 类名 - className - 动态类名控制

```JSX
import './style.css'
const showTitle = true
function App() {
  return (
    <div className="App">
      <div className={ showTitle ? 'title' : ''}>this is a div</div>
    </div>
  )
}
export default App
```

### JSX 注意事项

1. JSX 必须有一个根节点，如果没有根节点，可以使用<></>（幽灵节点）替代
2. 所有标签必须形成闭合，成对闭合或者自闭合都可以
3. JSX 中的语法更加贴近 JS 语法，属性名采用驼峰命名法 class -> className for -> htmlFor
4. JSX 支持多行（换行），如果需要换行，需使用() 包裹，防止 bug 出现

## React 组件基础

### 组件概念

组件允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。
![a5d4800b0937dda52aad91d7cab719e0.png](https://img.gejiba.com/images/a5d4800b0937dda52aad91d7cab719e0.png)

### 函数组件

使用 JS 的函数（或箭头函数）创建的组件，就叫做函数组件
组件定义与渲染

```JSX
// 定义函数组件
function HelloFn () {
  return <div>这是我的第一个函数组件!</div>
}

// 定义类组件
function App () {
  return (
    <div className="App">
      {/* 渲染函数组件 */}
      <HelloFn />
      <HelloFn></HelloFn>
    </div>
  )
}
export default App
```

**约定说明**

1. 组件的名称必须首字母大写，react 内部会根据这个来判断是组件还是普通的 HTML 标签
2. 函数组件必须有返回值，表示该组件的 UI 结构；如果不需要渲染任何内容，则返回 null
3. 组件就像 HTML 标签一样可以被渲染到页面中。组件表示的是一段结构内容，对于函数组件来说，渲染的内容是函数的返回值就是对应的内容
4. 使用函数名称作为组件标签名称，可以成对出现也可以自闭合

### 类组件

使用 ES6 的 class 创建的组件，叫做类（class）组件

```JSX
// 引入React
import React from 'react'

// 定义类组件
class HelloC extends React.Component {
  render () {
    return <div>这是我的第一个类组件!</div>
  }
}

function App () {
  return (
    <div className="App">
      {/* 渲染类组件 */}
      <HelloC />
      <HelloC></HelloC>
    </div>
  )
}
export default App
```

**约定说明**

1. 类名称也必须以大写字母开头
2. 类组件应该继承 React.Component 父类，从而使用父类中提供的方法或属性
3. 类组件必须提供 render 方法 render 方法必须有返回值，表示该组件的 UI 结构

### 事件绑定

#### 1. 如何绑定事件

- 语法
  on + 事件名称 = { 事件处理程序 } ，比如：`<div onClick={()=>{}}></div>`
- 注意点
  react 事件采用驼峰命名法，比如：`onMouseEnter`、`onFocus`
- 样例

```JSX
// 函数组件
function HelloFn () {
  // 定义事件回调函数
  const clickHandler = () => {
    console.log('事件被触发了')
  }
  return (
    // 绑定事件
    <button onClick={clickHandler}>click me!</button>
  )
}

// 类组件
class HelloC extends React.Component {
  // 定义事件回调函数
  clickHandler = () => {
    console.log('事件被触发了')
  }
  render () {
    return (
      // 绑定事件
      <button onClick={this.clickHandler}>click me!</button>
    )
  }
}
```

#### 获取事件对象

- 通过事件处理程序的参数获取事件对象 e

```JSX
// 函数组件
function HelloFn () {
  // 定义事件回调函数
  const clickHandler = (e) => {
    e.preventDefault()
    console.log('事件被触发了', e)
  }
  return (
    // 绑定事件
    <a href="http://www.baidu.com/" onClick={clickHandler}>百度</a>
  )
}
```

### 组件状态

> **前提**：在 react hook 出来之前，函数式组件是没有自己的状态的

![2b236ba8ae6567bbf8ff35cd434287b0.png](https://img.gejiba.com/images/2b236ba8ae6567bbf8ff35cd434287b0.png)

#### 初始化状态

- 通过 class 的实例属性 state 来初始化
- state 的值是一个对象结构，表示一个组件可以有多个数据状态

```JSX
class Counter extends React.Component {
  // 初始化状态
  constructor(props) {
    super(props);
    this.state = {count: 0};
  }
  render() {
    return <button>计数器</button>
  }
}
```

#### 读取状态

- 通过 this.state 来获取状态

```JSX
class Counter extends React.Component {
  // 初始化状态
  constructor(props) {
    super(props);
    this.state = {count: 0};
  }
  render() {
    // 读取状态
    return <button>计数器{this.state.count}</button>
  }
}
```

### 修改状态

- 语法
  this.setState({ 要修改的部分数据 })
- setState 方法作用
  a. 修改 state 中的数据状态
  b. 更新 UI
- 思想
  数据驱动视图，也就是只要修改数据状态，那么页面就会自动刷新，无需手动操作 dom
- 注意事项
  **不要直接修改 state 中的值，必须通过 setState 方法进行修改**

```JSX
class Counter extends React.Component {
  // 定义数据
  constructor(props) {
    super(props);
    this.state = {count: 0};
  }
  // 定义修改数据的方法
  setCount = () => {
    this.setState({
      count: this.state.count + 1
    })
  }
  // 使用数据 并绑定事件
  render () {
    return <button onClick={this.setCount}>{this.state.count}</button>
  }
}
```

### this 问题说明

![c0effcdb7343c55aa42751b43255d24c.png](https://img.gejiba.com/images/c0effcdb7343c55aa42751b43255d24c.png)

随着 js 标准的发展，主流的写法已经变成了 class fields，无需考虑太多 this 问题

### React 的状态不可变

**概念**：不要直接修改状态的值，而是基于当前状态创建新的状态值

1. 错误的直接修改

```JavaScript
state = {
  count : 0,
  list: [1,2,3],
  person: {
     name:'jack',
     age:18
  }
}
// 直接修改简单类型Number
this.state.count++
++this.state.count
this.state.count += 1
this.state.count = 1

// 直接修改数组
this.state.list.push(123)
this.state.list.spice(1,1)

// 直接修改对象
this.state.person.name = 'rose'
```

2. 基于当前状态创建新值

```JavaScript
this.setState({
    count: this.state.count + 1
    list: [...this.state.list, 4],
    person: {
       ...this.state.person,
       // 覆盖原来的属性 就可以达到修改对象中属性的目的
       name: 'rose'
    }
})
```

### 表单处理

使用 React 处理表单元素，一般有俩种方式：

1. 受控组件 （推荐使用）
2. 非受控组件

#### 受控表单组件

> 什么是受控组件？ input 框自己的状态被 React 组件状态控制
> React 组件的状态的地方是在 state 中，input 表单元素也有自己的状态是在 value 中，React 将 state 与表单元素的值（value）绑定到一起，由 state 的值来控制表单元素的值，从而保证单一数据源特性

**实现步骤**

以获取文本框的值为例，受控组件的使用步骤如下：

1. 在组件的 state 中声明一个组件的状态数据
2. 将状态数据设置为 input 标签元素的 value 属性的值
3. 为 input 添加 change 事件，在事件处理程序中，通过事件对象 e 获取到当前文本框的值（即用户当前输入的值）
4. 调用 setState 方法，将文本框的值作为 state 状态的最新值

```JSX
import React from 'react'

class InputComponent extends React.Component {
  // 声明组件状态
  constructor(props) {
    super(props);
    this.state = {message: 'this is message'};
  }
  // 声明事件回调函数
  changeHandler = (e) => {
    this.setState({ message: e.target.value })
  }
  render () {
    return (
      <div>
        {/* 绑定value 绑定事件*/}
        <input value={this.state.message} onChange={this.changeHandler} />
      </div>
    )
  }
}


function App () {
  return (
    <div className="App">
      <InputComponent />
    </div>
  )
}
export default App
```

#### 非受控表单组件

> 什么是非受控组件？
> 非受控组件就是通过手动操作 dom 的方式获取文本框的值，文本框的状态不受 react 组件的 state 中的状态控制，直接通过原生 dom 获取输入框的值

**实现步骤**

1. 导入 createRef 函数
2. 调用 createRef 函数，创建一个 ref 对象，存储到名为 msgRef 的实例属性中
3. 为 input 添加 ref 属性，值为 msgRef
4. 在按钮的事件处理程序中，通过 msgRef.current 即可拿到 input 对应的 dom 元素，而其中 msgRef.current.value 拿到的就是文本框的值

```JSX
import React, { createRef } from 'react'

class InputComponent extends React.Component {
  // 使用createRef产生一个存放dom的对象容器
  msgRef = createRef()

  changeHandler = () => {
    console.log(this.msgRef.current.value)
  }

  render() {
    return (
      <div>
        {/* ref绑定 获取真实dom */}
        <input ref={this.msgRef} />
        <button onClick={this.changeHandler}>click</button>
      </div>
    )
  }
}

function App () {
  return (
    <div className="App">
      <InputComponent />
    </div>
  )
}
export default App
```

## React 组件通信

### 组件通信的意义

组件是独立且封闭的单元，默认情况下组件**只能使用自己的数据（state）**
组件化开发的过程中，完整的功能会拆分多个组件，在这个过程中不可避免的需要互相传递一些数据
为了能让各组件之间可以进行互相沟通，数据传递，这个过程就是组件通信

1. 父子关系 - 最重要的
2. 兄弟关系 - 自定义事件模式产生技术方法 eventBus / 通过共同的父组件通信
3. 其它关系 - mobx / redux / 基于 hook 的方案

### 父传子实现

**实现步骤**

1.  父组件提供要传递的数据 - state
2.  给子组件标签添加属性值为 state 中的数据
3.  子组件中通过 props 接收父组件中传过来的数据
    a. 类组件使用 this.props 获取 props 对象
    b. 函数式组件直接通过参数获取 props 对象

![efb04fab8891b6507a321a9706b0b1c1.png](https://img.gejiba.com/images/efb04fab8891b6507a321a9706b0b1c1.png)

**代码实现**

```JSX
import React from 'react'

// 函数式子组件
function FSon(props) {
  console.log(props)
  return (
    <div>
      子组件1
      {props.msg}
    </div>
  )
}

// 类子组件
class CSon extends React.Component {
  render() {
    return (
      <div>
        子组件2
        {this.props.msg}
      </div>
    )
  }
}
// 父组件
class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      message: 'this is a message'
    }
  }

  render() {
    return (
      <>
        <div>父组件</div>
        <FSon msg={this.state.message} />
        <CSon msg={this.state.message} />
      </>
    )
  }
}

export default App
```

### props 说明

#### props 是只读对象（readonly）

根据单项数据流的要求，子组件只能读取 props 中的数据，不能进行修改

#### props 可以传递任意数据

数字、字符串、布尔值、数组、对象、函数、JSX

```JSX
class App extends React.Component {

  constructor(props) {
    super(props)
    this.state = {
      message: 'this is a message'
    }
  }

  render() {
    return (
      <div>
        <div>父组件</div>
        <FSon
          msg={this.state.message}
          age={20}
          isMan={true}
          cb={() => { console.log(1) }}
          child={<span>this is child</span>}
        />
        <CSon msg={this.state.message} />
      </div>
    )
  }
}
```

![b60880bb7a5ce85b074e0ff9784acc8a.png](https://img.gejiba.com/images/b60880bb7a5ce85b074e0ff9784acc8a.png)

### 子传父实现

**口诀**： 父组件给子组件传递回调函数，子组件调用
**实现步骤**

1. 父组件提供一个回调函数 - 用于接收数据
2. 将函数作为属性的值，传给子组件
3. 子组件通过 props 调用 回调函数
4. 将子组件中的数据作为参数传递给回调函数

![7e6f7f4d3ea9fc7b56ca52d0240603f3.png](https://img.gejiba.com/images/7e6f7f4d3ea9fc7b56ca52d0240603f3.png)

```JSX
import React from 'react'

// 子组件
function Son(props) {
  function handleClick() {
    // 调用父组件传递过来的回调函数 并注入参数
    props.changeMsg('this is newMessage')
  }
  return (
    <div>
      {props.msg}
      <button onClick={handleClick}>change</button>
    </div>
  )
}


class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      message: 'this is a message'
    }
  }
  // 提供回调函数
  changeMessage = (newMsg) => {
    console.log('子组件传过来的数据:',newMsg)
    this.setState({
      message: newMsg
    })
  }
  render() {
    return (
      <div>
        <div>父组件</div>
        <Son
          msg={this.state.message}
          // 传递给子组件
          changeMsg={this.changeMessage}
        />
      </div>
    )
  }
}

export default App
```

### 兄弟组件通信

**核心思路**： 通过状态提升机制，利用共同的父组件实现兄弟通信
![bc10d3653e637134a16cc5cef364cabe.png](https://img.gejiba.com/images/bc10d3653e637134a16cc5cef364cabe.png)

**实现步骤**

1. 将共享状态提升到最近的公共父组件中，由公共父组件管理这个状态

   - 提供共享状态
   - 提供操作共享状态的方法

2. 要接收数据状态的子组件通过 props 接收数据
3. 要传递数据状态的子组件通过 props 接收方法，调用方法传递数据

```JSX
import React from 'react'

// 子组件A
function SonA(props) {
  return (
    <div>
      SonA
      {props.msg}
    </div>
  )
}
// 子组件B
function SonB(props) {
  return (
    <div>
      SonB
      <button onClick={() => props.changeMsg('new message')}>changeMsg</button>
    </div>
  )
}

// 父组件
class App extends React.Component {
  // 父组件提供状态数据
  constructor(props) {
    super(props)
    this.state = {
      message: 'this is a message'
    }
  }
  // 父组件提供修改数据的方法
  changeMsg = (newMsg) => {
    this.setState({
      message: newMsg
    })
  }

  render() {
    return (
      <>
        {/* 接收数据的组件 */}
        <SonA msg={this.state.message} />
        {/* 修改数据的组件 */}
        <SonB changeMsg={this.changeMsg} />
      </>
    )
  }
}

export default App

```

### 跨组件通信 Context

![9decc7bd1f6bbd7b065439fcb8317a50.png](https://img.gejiba.com/images/9decc7bd1f6bbd7b065439fcb8317a50.png)

> 上图是一个 react 形成的嵌套组件树，如果我们想从 App 组件向任意一个下层组件传递数据，该怎么办呢？目前我们能采取的方式就是一层一层的 props 往下传，显然很繁琐
> Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法

**实现步骤**

1. 创建 Context 对象 导出 Provider 和 Consumer 对象

```JavaScript
const { Provider, Consumer } = createContext()
```

2. 使用 Provider 包裹根组件提供数据

```JSX
<Provider value={this.state.message}>
    {/* 根组件 */}
</Provider>
```

3.  需要用到数据的组件使用 Consumer 包裹获取数据

```JSX
<Consumer >
    {value => /* 基于 context 值进行渲染*/}
</Consumer>
```

**代码实现**

```JSX
import React, { createContext }  from 'react'

// 1. 创建Context对象
const { Provider, Consumer } = createContext()


// 3. 消费数据
function ComC() {
  return (
    <Consumer >
      {value => <div>{value}</div>}
    </Consumer>
  )
}

function ComA() {
  return (
    <ComC/>
  )
}

// 2. 提供数据
class App extends React.Component {
  state = {
    message: 'this is message'
  }
  render() {
    return (
      <Provider value={this.state.message}>
        <div className="app">
          <ComA />
        </div>
      </Provider>
    )
  }
}

export default App
```

## React 组件进阶

### children 属性

#### children 属性是什么

> 表示该组件的子节点，只要组件内部有子节点，props 中就有该属性

#### children 可以是什么

1. 普通文本
2. 普通标签元素
3. 函数
4. JSX

### props 校验-场景和使用

> 对于组件来说，props 是由外部传入的，我们其实无法保证组件使用者传入了什么格式的数据，如果传入的数据格式不对，就有可能会导致组件内部错误，有一个点很关键 - 组件的使用者可能报错了也不知道为什么，看下面的例子

```JSX
const List = props => {
  const arr = props.colors
  const lis = arr.map((item, index) => <li key={index}>{item.name}</li>)
  return <ul>{lis}</ul>
}

<List colors={20} />
```

面对这样的问题，如何解决？ **props 校验**

**实现步骤**

1. 安装属性校验包： `npm i prop-types`
2. 导入 prop-types 包
3. 使用 组件名.propTypes = {} 给组件添加校验规则

**核心代码**

```JSX
import PropTypes from 'prop-types'

const List = props => {
  const arr = props.colors
  const lis = arr.map((item, index) => <li key={index}>{item.name}</li>)
  return <ul>{lis}</ul>
}

List.propTypes = {
  colors: PropTypes.array
}
```

### props 校验-规则说明

#### 四种常见结构

1. 常见类型：array、bool、func、number、object、string
2. React 元素类型：element
3. 必填项：isRequired
4. 特定的结构对象：shape({})

```JavaScript
// 常见类型
optionalFunc: PropTypes.func,
// 必填 只需要在类型后面串联一个isRequired
requiredFunc: PropTypes.func.isRequired,
// 特定结构的对象
optionalObjectWithShape: PropTypes.shape({
	color: PropTypes.string,
	fontSize: PropTypes.number
})
```

[官网文档更多阅读](https://reactjs.org/docs/typechecking-with-proptypes.html)

### props 校验-规则说明

> 通过 `defaultProps` 可以给组件的 props 设置默认值，在未传入 props 的时候生效

#### 函数组件

直接使用函数参数默认值

```JSX
function List({pageSize = 10}) {
  return (
    <div>
      此处展示props的默认值：{ pageSize }
    </div>
  )
}

// 不传入pageSize属性
<List />
```

#### 类组件

使用类静态属性声明默认值，`static defaultProps = {}`

```JSX
class List extends Component {
  static defaultProps = {
    pageSize: 10
  }

  render() {
    return (
      <div>
        此处展示props的默认值：{this.props.pageSize}
      </div>
    )
  }
}
<List />
```

### 生命周期 - 概述

> 组件的生命周期是指组件从被创建到挂载到页面中运行起来，再到组件不用时卸载的过程，注意，只有类组件才有生命周期（类组件 实例化 函数组件 不需要实例化）
> ![6bcfce01b8fec23ca114b6c4fbac6a16.png](https://img.gejiba.com/images/6bcfce01b8fec23ca114b6c4fbac6a16.png)

[http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

### 生命周期 - 挂载阶段

![1fd62b6f68705bf5d1d562b5d764153c.png](https://img.gejiba.com/images/1fd62b6f68705bf5d1d562b5d764153c.png)

| 钩子函数          | 触发时机                                                 | 作用                                                          |
| ----------------- | -------------------------------------------------------- | ------------------------------------------------------------- |
| constructor       | constructor 创建组件时，最先执行，初始化的时候只执行一次 | 1. 初始化 state 2. 创建 Ref 3. 使用 bind 解决 this 指向问题等 |
| render            | 每次组件渲染都会触发                                     | 渲染 UI（注意： 不能在里面调用 setState() ）                  |
| componentDidMount | 组件挂载（完成 DOM 渲染）后执行，初始化的时候执行一次    | 1. 发送网络请求 2.DOM 操作                                    |

### 生命周期 - 更新阶段

![480575c7388d6aab5d3405a55eb3a837.png](https://img.gejiba.com/images/480575c7388d6aab5d3405a55eb3a837.png)

| 钩子函数           | 触发时机                                      | 作用                                                         |
| ------------------ | --------------------------------------------- | ------------------------------------------------------------ |
| render             | 每次组件渲染都会触发                          | 每次组件渲染都会触发 渲染 UI（与 挂载阶段 是同一个 render）  |
| componentDidUpdate | componentDidUpdate 组件更新后（DOM 渲染完毕） | DOM 操作，可以获取到更新后的 DOM 内容，不要直接调用 setState |

### 生命周期 - 卸载阶段

| 钩子函数             | 触发时机                 | 作用                                                        |
| -------------------- | ------------------------ | ----------------------------------------------------------- |
| componentWillUnmount | 组件卸载（从页面中消失） | 组件卸载（从页面中消失） 执行清理工作（比如：清理定时器等） |

## Hooks 基础

### Hooks 概念理解

#### 什么是 hooks

> Hooks 的本质：一套能够使函数组件更强大，更灵活的“钩子”

React 体系里组件分为 类组件 和 函数组件
经过多年的实战，函数组件是一个更加匹配 React 的设计理念 UI = f(data)，也更有利于逻辑拆分与重用的组件表达形式，而先前的函数组件是不可以有自己的状态的，为了能让函数组件可以拥有自己的状态，所以从 react v16.8 开始，Hooks 应运而生

**注意点**：

1. 有了 hooks 之后，为了兼容老版本，class 类组件并没有被移除，俩者都可以使用
2. 有了 hooks 之后，不能在把函数成为无状态组件了，因为 hooks 为函数组件提供了状态
3. hooks 只能在函数组件中使用

#### Hooks 解决了什么问题

Hooks 的出现解决了俩个问题 1. 组件的状态逻辑复用 2.class 组件自身的问题

1.  组件的逻辑复用
    在 hooks 出现之前，react 先后尝试了 mixins 混入，HOC 高阶组件，render-props 等模式
    但是都有各自的问题，比如 mixin 的数据来源不清晰，高阶组件的嵌套问题等等
2.  class 组件自身的问题
    class 组件就像一个厚重的‘战舰’ 一样，大而全，提供了很多东西，有不可忽视的学习成本，比如各种生命周期，this 指向问题等等，而我们更多时候需要的是一个轻快灵活的'快艇'

#### useState

##### 基础使用

**作用**

> useState 为函数组件提供状态（state）

**使用步骤**

1. 导入 useState 函数
2. 调用 useState 函数，并传入状态的初始值
3. 从 useState 函数的返回值中，拿到状态和修改状态的方法
4. 在 JSX 中展示状态
5. 调用修改状态的方法更新状态

**代码实现**

```JSX
import { useState } from 'react'

function App() {
  // 参数：状态初始值比如,传入 0 表示该状态的初始值为 0
  // 返回值：数组,包含两个值：1 状态值（state） 2 修改该状态的函数（setState）
  const [count, setCount] = useState(0)
  return (
    <button onClick={() => { setCount(count + 1) }}>{count}</button>
  )
}
export default App
```

##### 状态的读取和修改

**读取状态**

> 该方式提供的状态，是函数内部的局部变量，可以在函数内的任意位置使用

**修改状态**

1. setCount 是一个函数，参数表示最新的状态值
2. 调用该函数后，将使用新值替换旧值
3. 修改状态后，由于状态发生变化，会引起视图变化

**注意事项**

1. 修改状态的时候，一定要使用新的状态替换旧的状态，不能直接修改旧的状态，尤其是引用类型

##### 组件的更新过程

函数组件使用 **useState hook** 后的执行过程，以及状态值的变化

- 组件第一次渲染
  a. 从头开始执行该组件中的代码逻辑
  b. 调用 useState(0) 将传入的参数作为状态初始值，即：0
  c. 渲染组件，此时，获取到的状态 count 值为： 0
- 组件第二次渲染
  a. 点击按钮，调用 setCount(count + 1) 修改状态，因为状态发生改变，所以，该组件会重新渲染
  b. 组件重新渲染时，会再次执行该组件中的代码逻辑
  c. 再次调用 useState(0)，此时 React 内部会拿到最新的状态值而非初始值，比如，该案例中最新的状态值为 1
  d. 再次渲染组件，此时，获取到的状态 count 值为：1

注意：**useState 的初始值(参数)只会在组件第一次渲染时生效**。也就是说，以后的每次渲染，useState 获取到都是最新的状态值，React 组件会记住每次最新的状态值

```JSX
import { useState } from 'react'

function App() {
  const [count, setCount] = useState(0)
  // 在这里可以进行打印测试
  console.log(count)
  return (
    <button onClick={() => { setCount(count + 1) }}>{count}</button>
  )
}
export default App
```

##### 使用规则

1. useState 函数可以执行多次，每次执行互相独立，每调用一次为函数组件提供一个状态
2. useState 注意事项
   a. 只能出现在函数组件中
   b. 不能嵌套在 if/for/其它函数中（react 按照 hooks 的调用顺序识别每一个 hook）
   ```JSX
   let num = 1
   function List(){
     num++
     if(num / 2 === 0){
       const [name, setName] = useState('cp')
     }
     const [list,setList] = useState([])
   }
   // 俩个hook的顺序不是固定的，这是不可以的！！！
   ```
   c. 可以通过开发者工具查看 hooks 状态

#### useEffect

##### 理解函数副作用

**什么是副作用**

> 副作用是相对于主作用来说的，一个函数除了主作用，其他的作用就是副作用。对于 React 组件来说，**主作用就是根据数据（state/props）渲染 UI**，除此之外都是副作用（比如，手动修改 DOM）

**常见的副作用**

1. 数据请求 ajax 发送
2. 手动修改 dom
3. localstorage 操作

useEffect 函数的作用就是为 react 函数组件提供副作用处理的！

##### 基础使用

**作用**

> 为 react 函数组件提供副作用处理

**使用步骤**

1. 导入 useEffect 函数
2. 调用 useEffect 函数，并传入回调函数
3. 在回调函数中编写副作用处理（dom 操作）
4. 修改数据状态
5. 检测副作用是否生效

**代码实现**

```JSX
import { useEffect, useState } from 'react'

function App() {
  const [count, setCount] = useState(0)

  useEffect(()=>{
    // dom操作
    document.title = `当前已点击了${count}次`
  })
  return (
    <button onClick={() => { setCount(count + 1) }}>{count}</button>
  )
}

export default App
```

##### 依赖项控制执行时机

1. 不添加依赖项

> 组件首次渲染执行一次，以及不管是哪个状态更改引起组件更新时都会重新执行
>
> 1.  组件初始渲染
> 2.  组件更新 （不管是哪个状态引起的更新）

2. 添加空数组

> 组件只在首次渲染时执行一次

3. 添加特定依赖项

> 副作用函数在首次渲染时执行，在依赖项发生变化时重新执行

```JSX
function App() {
    const [count, setCount] = useState(0)
    const [name, setName] = useState('zs')

    useEffect(() => {
        console.log('副作用执行了')
    }, [count])

    return (
        <>
         <button onClick={() => { setCount(count + 1) }}>{count}</button>
         <button onClick={() => { setName('cp') }}>{name}</button>
        </>
    )
}
```

**注意事项**
useEffect 回调函数中用到的数据（比如，count）就是依赖数据，就应该出现在依赖项数组中，如果不添加依赖项就会有 bug 出现

## Hooks 进阶

### Hooks 进阶

**使用场景**
参数只会在组件的初始渲染中起作用，后续渲染时会被忽略。如果初始 state 需要通过计算才能获得，则可以传入一个函数，在函数中计算并返回初始的 state，此函数只在初始渲染时被调用

**语法**

```JavaScript
const [name, setName] = useState(()=>{
  // 编写计算逻辑    return '计算之后的初始值'
})
```

**语法选择**

1. 如果就是初始化一个普通的数据 直接使用 useState(普通数据) 即可
2. 如果要初始化的数据无法直接得到需要通过计算才能获取到，使用 useState(()=>{})

```JSX
import { useState } from 'react'

function Counter(props) {
  const [count, setCount] = useState(() => {
    return props.count
  })
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>{count}</button>
    </div>
  )
}

function App() {
  return (
    <>
      <Counter count={10} />
      <Counter count={20} />
    </>
  )
}

export default App
```

### useEffect - 清理副作用

**使用场景**
在组件被销毁时，如果有些副作用操作需要被清理，就可以使用此语法，比如常见的定时器
**语法及规则**

```JavaScript
useEffect(() => {
    console.log('副作用函数执行了')
    // 副作用函数的执行时机为: 在下一次副作用函数执行之前执行
    return () => {
        console.log('清理副作用的函数执行了')
        // 在这里写清理副作用的代码
    }
})
```

**定时器小案例**

> 添加副作用函数前：组件虽然已经不显示了，但是定时器依旧在运行

```JSX
import { useEffect, useState } from 'react'
function Foo() {
    useEffect(() => {
        setInterval(() => {
            console.log('副作用函数执行了')
        }, 1000)
    })
    return <div>Foo</div>
}


function App() {
    const [flag, setFlag] = useState(true)
    return (
        <>
          <button onClick={() => setFlag(false)}>click</button>
         {flag ? <Foo/> : null}
        </>
    )
}

export default App
```

> 添加清理副作用函数后：一旦组件被销毁，定时器也被清理

```JSX
import { useEffect, useState } from 'react'

function Foo() {
    useEffect(() => {
        const timerId = setInterval(() => {
            console.log('副作用函数执行了')
        }, 1000)
        // 添加清理副租用函数
        return () => {
            clearInterval(timerId)
        }
    })
    return <div>Foo</div>
}
function App() {
    const [flag, setFlag] = useState(true)
    return (
        <>
          <button onClick={() => setFlag(false)}>click</button>
         {flag ? <Foo/> : null}
        </>
    )
}

export default App
```

### useEffect - 发送网络请求

**使用场景**
如何在 useEffect 中发送网络请求，并且封装同步 async await 操作

**语法要求**
不可以直接在 useEffect 的回调函数外层直接包裹 await ，因为异步会导致清理函数无法立即返回

```JavaScript
useEffect(async ()=>{
    const res = await axios.get('http://geek.itheima.net/v1_0/channels')
    console.log(res)
},[])
```

**正确写法**
在内部单独定义一个函数，然后把这个函数包装成同步

```JSX
useEffect(()=>{
    async function fetchData(){
       const res = await axios.get('https://api.inews.qq.com/newsqa/v1/query/inner/publish/modules/list?modules=statisGradeCityDetail,diseaseh5Shelf')                            console.log(res)
    }
},[])
```

### useRef

**使用场景**

> 在函数组件中获取真实的 dom 元素对象或者是组件对象

**使用步骤**

1. 导入 useRef 函数
2. 执行 useRef 函数并传入 null，返回值为一个对象 内部有一个 current 属性存放拿到的 dom 对象（组件实例）
3. 通过 ref 绑定 要获取的元素或者组件

**获取 dom**

```JSX
import { useEffect, useRef } from 'react'
function App() {
    const h1Ref = useRef(null)
    useEffect(() => {
        console.log(h1Ref)
    },[])
    return (
        <div>
            <h1 ref={ h1Ref }>this is h1</h1>
        </div>
    )
}
export default App
```

**获取组件实例**

> 函数组件没有实例，不能使用 ref 获取，如果想获取组件实例，必须是类组件

```JSX Foo.jsx
class Foo extends React.Component {
    sayHi = () => {
        console.log('say hi')
    }
    render(){
        return <div>Foo</div>
    }
}

export default Foo
```

```JSX App.jsx
import { useEffect, useRef } from 'react'
import Foo from './Foo'
function App() {
    const h1Foo = useRef(null)
    useEffect(() => {
        console.log(h1Foo)
    }, [])
    return (
        <div> <Foo ref={ h1Foo } /></div>
    )
}
export default App
```

### useContext

**实现步骤**

1. 使用 createContext 创建 Context 对象
2. 在顶层组件通过 Provider 提供数据
3. 在底层组件通过 useContext 函数获取数据

```JSX
import { createContext, useContext } from 'react'
// 创建Context对象
const Context = createContext()

function Foo() {
    return <div>Foo <Bar/></div>
}

function Bar() {
    // 底层组件通过useContext函数获取数据
    const name = useContext(Context)
    return <div>Bar {name}</div>
}

function App() {
    return (
        // 顶层组件通过Provider 提供数据
        <Context.Provider value={'this is name'}>
            <div><Foo/></div>
        </Context.Provider>
    )
}

export default App
```
