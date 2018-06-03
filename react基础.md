# React学习笔记

## JSX简介

> 一个简单的JSX示例

````
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
    document.getElementById('root')
);
````

* 因为 JSX 的特性更接近 JavaScript 而不是 HTML , 所以 React DOM 使用 camelCase 小驼峰命名 来定义属性的名称，而不是使用 HTML 的属性名称。

> 例如，class 变成了 className，而 tabindex 则对应着 tabIndex。

````
function Avatar(props) {
    return (
        <div>
            <img className='avatar'
                src={props.img}
                alt={props.name}
            />
        </div>
    )
}
````

* Babel 转译器会把 JSX 转换成一个名为 React.createElement() 的方法调用。

> 注意新建一个react元素是用小括号来初始化的。以下两种方法等效：

````
const element = (
    <h1 className="greeting">
        Hello World
    </h1>
)

// equals to

const element = {
    type = 'h1',
    props: {
        className：'greeting',
        children: 'Hello World'
    }
}

// This is a React Object
````

> React DOM 在渲染之前默认会过滤所有传入的值。它可以确保你的应用不会被注入攻击。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 XSS(跨站脚本) 攻击。

## 元素渲染

> 元素是构成 React 应用的最小单位。

* 要将React元素渲染到根DOM节点中，我们通过把它们都传递给 ReactDOM.render() 的方法来将其渲染到页面上：

````
// html
<div id='root'></div>

// jsx
const element = <h1>Hello World</h1>;
ReactDOM.render(
    element,
    document.getElementById('#root');
)
````

> React 元素都是immutable 不可变的。当元素被创建之后，你是无法改变其内容或属性的。一个元素就好像是动画里的一帧，它代表应用界面在某一时间点的样子。
>
> 因此更新页面的第一个方法是创建一个新的React元素，传入ReactDOM.render()方法。但在实际生产开发中，大多数React应用只会调用一次 ReactDOM.render()。
> 
> 不过即便是这种方法，React DOM 首先会比较元素内容先后的不同，而在渲染过程中只会更新改变了的部分。

````
// ticking
function tick() {
    const element = (
        <div>
            <h1>hello world</h1>
            <h2>It is {new Date().toLocaleTimeString()}</h2>
        </div>
    )

    ReactDOM.render(
        element,
        document.getElementById('root');
    )
}

setTimeInterval(tick, 1000);
````

## 组件 & props

> 组件从概念上看就像是函数，它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。

* 组件定义

> 注意：组件名称必须以大写字母开头。

````
// use function to create:
function Welcome(props) {
    return <h1>Hello {props.name}</h1>;
}

// use class to create. in ES6
class Welcome extends React.Component {
    render() {
        return <h1>Hello {this.props.name}</h1>;
    }
}
````

* 组件渲染

> 组件标签后面要用/>结尾

````
// declare a Welcome Component
const element = <Welcome name='Dave'/>;

ReactDOM.render(
    element,
    document.getElementById('root');
)
````

* 组件组合

> 这样就可以把一个大的组件模块化了。

````
function App() {
    return (
        <div>
            <Welcome name='Jiahui'/>
            <Welcome name='Peng'/>
        </div>
    )
}

ReactDOM.render(
    <App />,
    document.getElementById('root');
)
````

* props 的只读性

> 有一个很严格的要求：所有的React组件必须像纯函数那样使用它们的props。在这一背景下，将引入state来实现渲染改变。

## state & 生命周期

> 定义组件的时候，使用类定义，可以让组件拥有局部状态和生命周期钩子等等。

````
class Ticking extends React.Component {

    // in constructor declare this.state
    constructor(props) {
        super(props);
        this.state = {
            date: new Date()
        };
    }

    // declare your own function
    tick() {
        // setState() is used to update this.state
        this.setState({
            date: new Date()
        })
    }

    // this function will run when the component is about to be rendered
    componentDidMount() {
        this.timerID = setInterval(()=> {
            this.tick();
        }, 1000)
    }

    // this function will run before the component is about to be destroyed
    componentWillUnmount() {
        clearInterval(this.timerID);
    }

    render() {
        return (
            <div>
                <h1>Hello World</h1>
                <h2>It is {this.state.date.toLocaleTImeString()}</h2>
            </div>
        )
    }
}
````

注意点：

* 更新state的时候必须使用this.setState()而不能直接this.state.prop=''。
* 构造函数是位移能够初始化this.state的地方。
* setState()可以只更新部分state的值（废话）。
* state是局部封装的。除了自身组件以外的组件、父组件或子组件都不能直接访问他的state。但组件可以将state作为props传递给子组件。例如对ticking的DOM做这样的修改：

````
//...
    render() {
        return (
            <div>
                <h1>Hello World</h1>
                <FormattedDate date={this.state.date}/>
            </div>
        )
    }
//...

function FormattedDate(props) {
    return (
        <h2>It is {props.date.toLocaleTimeString()}</h2>
    )
}
````

* this.props和this.state是异步更新的，所以不能保证每次调用的时候已经获得了最新值。要使用这两个值来更新的话，需要这样使用setState()：

````
this.setState((prevState, props)=> ({
    counter: prevState.counter + props.increment
}));

// equals to
this.setState(function(prevState, props) {
    return {
        counter: prevState.counter + props.increment
    }
})
````

## 事件

* 事件声明也要采用驼峰式写法。需要传入函数作为事件处理函数而不是传统html的传入字符串。

````
<button onClick={btnclick}>
    CLick Me
</button>
````

* 阻止默认行为：不能使用返回 false 的方式阻止默认行为。你必须明确的使用 preventDefault。而且不需要担心跨浏览器（主要是IE）的兼容性问题。

* 注意！！！类的方法默认是不会绑定 this 的。两种方法：

````
//...
  constructor {
    this.yourFunction = this.yourFunction.bind(this);
  }

// ...
  yourFunction = ()=> {
    // ...
  }
````

````
//...
    render() {
        <button onClick={(e)=>this.handleClick(e)}>
            Click Me
        </button>
    }
//...
````

* 向事件处理程序传递参数：

````
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<!- equals to -->
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
````

上面两个例子中，参数 e 作为 React 事件对象将会被作为第二个参数进行传递。通过箭头函数的方式，事件对象必须显式的进行传递，但是通过 bind 的方式，事件对象以及更多的参数将会被隐式的进行传递。

通过 bind 方式向监听函数传参，在类组件中定义的监听函数，事件对象 e 要排在所传递参数的后面，例如:

````
//...
    preventPop(name, e){    //事件对象e要放在最后
        e.preventDefault();
        alert(name);
    }
//...
````

> 事件绑定部分详见官方文档 https://doc.react-china.org/docs/handling-events.html

## 条件渲染

> 没太多好说的。但是从官方文档学到一招：因为JS中,，true && expression 总返回expression，false && expression 总返回false

````
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}
````

* 阻止组件渲染：render()返回null即可。这样并不会影响该组件生命周期方法的回调。例如 componentWillUpdate 和 componentDidUpdate 依然可以被调用。

##　列表

> 也没太多可说的看代码吧。

````
function NumberList(props) {
    let array = props.numbers;
    let list = array.map((item)=> (
        <li key={item.toString()}>
            {item}
        </li>
    ))
    return (
        <ul>
            {list}
        </ul>
    )
}

const numbers = [1, 2, 3, 4];

ReactDOM.render(
    <NumberList number='{numbers}'/>,
    document.getElementById('root');
)
````

> 注意，元素的key只有在它和它的兄弟节点对比时才有意义。比如说，一个list组件下要有多个item组件，在item组件里声明key是没有意义的，因为它还不一定要被放到list下面。应该在list下添加item时再生命key，这个时候是在列表下面了。

## 表单

* input textarea select 等输入，传值是用event.target.value来实现的。
* input 输入是用event.target.name传值的。

## 状态提升

* 关键点：为了共享父元素的state和方法，子元素可以通过props来读取父元素的这些值。

````
//codes are here: https://codepen.io/anon/pen/jxodeb?editors=0010
````

> 注意一下，在这份代码里，Calculator组件中的TemperatureInput中声明了一个onTemperatureChange属性，这个名字并不是固定的，但在TemperatureInput组件的定义里面，onChange要调用的函数名要与这个名字对应，只有这个要求。onChange则是input标签固定的。

## 组合和继承

* 包含关系

> 一些组件不能提前知道它们的子组件是什么。这对于 Sidebar 或 Dialog 这类通用容器尤其常见。我们建议这些组件使用 children 属性将子元素直接传递到输出。这是默认的。或者可以另外自己声明。

* 继承

> "在 Facebook 网站上，我们的 React 使用了数以千计的组件，然而却还未发现任何需要推荐你使用继承的情况。"感谢不杀之恩。


# 问题记录

* render必须要有一个父元素，只有自定component不行。