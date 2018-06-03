# 进阶一下

## 深入jsx

> jsx是React.createElement(component, props, ...children)方法的语法糖。由于 JSX 编译后会调用 React.createElement 方法，所以在你的 JSX 代码中必须首先声明 React 变量。当然如果你使用 < script> 加载 React，它将作用于全局。

````
<MyButton color='blue' shadowSize={2}>
  click me
</MyBUtton>

// compiled as 
React.createElement(
  MyButton,
  {
    color: 'blue',
    shadowSize: 2
  },
  'click me'  //null for empty children
)
````

### 注意点

* 函数

  * false、null、undefined 和 true 都是有效的子代，但它们不会直接被渲染。下面的表达式是等价的：

````
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
````

## PropTypes 类型检查

````
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
````

> 关键点：PropTypes.any.isRequired / ConponentClass.defaultProps

## 非受控组件 / refs & DOM

> 主要提到对file类型的input的使用。新版refs使用回调函数来创建refs

````
class FileInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  }
  handleSubmit(event) {
    event.preventDefault();
    alert(
      `Selected file - ${this.fileInput.files[0].name}`
    );
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Upload file:
          <input
            type="file"
            ref={input => {
              this.fileInput = input;
            }}
          />
        </label>
        <br />
        <button type="submit">Submit</button>
      </form>
    );
  }
}

ReactDOM.render(
  <FileInput />,
  document.getElementById('root')
);
````

### virtual DOM

### 事件机制

> https://segmentfault.com/a/1190000008782645
> 
> 因为react中的事件机制不是原生js的事件机制了，事件没有绑定在原生的DOM上面。

