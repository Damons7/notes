# 搭建JSX

**在终端：**

- cnpm i babel-core babel-loader babel-plugin-transform-runtime -D
- cnpm i babel-preset-env babel-preset-stage-0 babel-preset-react -D

**往webpack.config.js添加：**

```js
module.exports = {
     module:{//所有第三方 模块的配置规则
        rules:[ //第三方匹配规则
             {test:/\.js|jsx$/,use:'babel-loader',exclude:/node_modules/},
            //exclude排除项
        ]
    }
}
```

**在根目录创建.babelrc**

```js
{
    "presets":["env","stage-0","react"],
    "plugins": ["transform-runtime"]
}
```

npm run dev之后就能实时更新拉

**在index.js**

```js
import React from 'react';
import ReactDOM from 'react-dom';
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}
const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};
const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);
ReactDOM.render(
  element,
  document.getElementById('#app')
);
```

**JSX特定属性**

可以使用引号

```js
const element = <div tabIndex="0"></div>;
```

也可以使用大括号

```js
const element = <img src={user.avatarUrl}></img>;
```

**注意：**

因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 `camelCase`（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。

例如，JSX 里的 `class` 变成了 [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)，而 `tabindex` 则变为 [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)。



#### JSX 防止注入攻击

```js
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的：
const element = <h1>{title}</h1>;

```

# **渲染组件**

自定义（组件名称必须以大写字母开头。小写会默认为标签，大写会在作用域内查找）

```js
function Welcome(props) {  
    return <h1>Hello, {props.name}</h1>;
}
const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

这里具体渲染为

- 调用 `ReactDOM.render()` 函数，并传入 `<Welcome name="Sara" />` 作为参数。
- React 调用 `Welcome` 组件，并将 `{name: 'Sara'}` 作为 props 传入。
- `Welcome` 组件将 `<h1>Hello, Sara</h1>` 元素作为返回值。
- React DOM 将 DOM 高效地更新为 `<h1>Hello, Sara</h1>`。

**下面是一个组合组件的案例**

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```



**在看一个提取组件的例子**

该组件用于描述一个社交媒体网站上的评论功能，它接收 `author`（对象），`text` （字符串）以及 `date`（日期）作为 props。

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

- 首先，我们将提取 `Avatar` 组件：（我们给它的 props 起了一个更通用的名字：`user`，而不是 `author`）

  ```js
  function Avatar(props) {
    return (
      <img className="Avatar"
        src={props.user.avatarUrl}    //props可以理解为组件的所有集合属性。props.user则是传进来user={props.author}右边的值
        alt={props.user.name}
      />
    );
  }
  ```

  

- 然后，我们先看看效果

```js
function Comment(props) {
   //此时用了上面封装的组件Avatar，并将user={props.author}传入
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />  
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

- 接下来，提取 `UserInfo` 组件，该组件在用户名旁渲染 `Avatar` 组件

```js
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  ); 
}
```

- 然后整理

  ```js
  function Comment(props) {
    return (
      <div className="Comment">
        <UserInfo user={props.author} />      <div className="Comment-text">
          {props.text}
        </div>
        <div className="Comment-date">
          {formatDate(props.date)}
        </div>
      </div>
    );
  }
  ```

# Props 的只读性

#### 不能修改自身的props

# State

state 与 props 类似，但是 state 是私有的，并且完全受控于当前组件。

先看一个例子

```js
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}
function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}
setInterval(tick, 1000);
```

然后，我们把它分为5步修改：

- 创建一个同名的 [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)，并且继承于 `React.Component`。
- 添加一个空的 `render()` 方法。
- 将函数体移动到 `render()` 方法之中。
- 在 `render()` 方法中使用 `this.props` 替换 `props`。
- 删除剩余的空函数声明。

```js
class Clock extends React.Component {
    constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}//先把他变成class，将this.props.date变成this.state.date,并添加一个 class 构造函数，然后在该函数中为 this.state 赋初值，将 props 传递到父类的构造函数中
```

最终，移除`<Clock />` 元素中的 `date` 属性：

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

# 生命周期方法

继续上例，当 `Clock` 组件第一次被渲染到 DOM 中的时候，就为其设置定时器。这在 React 中被称为“挂载（mount）”。

同时，当 DOM 中 `Clock` 组件被删除的时候，应该清楚定时器。这在 React 中被称为“卸载（unmount）”。

```JS
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
   //componentDidMount() 方法会在组件已经被渲染到 DOM 中后运行，所以，最好在这里设置计时器
  componentDidMount() {
      this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
  componentWillUnmount() {
      clearInterval(this.timerID);//清除计时器
  }
  //Clock 组件每秒都会调用它。
  tick() {
    this.setState({
      date: new Date()
    });
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

#### **注意,如何使用好state**

- 不要直接修改state

```js
// Wrong
this.state.comment = 'Hello';

// true
this.setState({comment: 'Hello'});
```

- state的更新可能是异步的（this.props和this.state都有可能），所以你不要依赖他们的值来更新下一个状态

- state的更新会被合并

  

# 事件处理

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }
  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

# 条件渲染

```js
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}



class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }
  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }
  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }
  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}
ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

# 表单受控组件

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }
  handleChange(event) {
    this.setState({value: event.target.value});
  }
  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value);
    event.preventDefault();
  }
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

# 状态提升

