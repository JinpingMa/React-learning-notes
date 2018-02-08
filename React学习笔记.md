## 方法中使 this 对象就指向了类的实例的方法：
1.使用bind函数
```js
class ExplainBindingsComponent extends Component {
  constructor() {
    super();
    this.onClickMe = this.onClickMe.bind(this);//this对象指向类的实例方法
  }
  onClickMe() {
    console.log(this);
  }
  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
      Click Me
      </button> );
  } 
}
```
2.使用ES6的箭头函数，自动绑定
```js
class ExplainBindingsComponent extends Component { 
  onClickMe = () => {
	console.log(this); //箭头函数自动绑定
  }
  render() { 
    return (
	  <button 
	    onClick={this.onClickMe}
	    type="button"
      >
	  Click Me
	  </button> 
    );
  } 
}
```

## 有参数的方法的调用
```js
<button
  onClick={() => this.onDismiss(item.objectID)} type="button"
>
  Dismiss
</button>
```
这已经是一个复杂的例子了，因为你必须传递一个参数到类的方法，因此你需要将它封装 到另一个(箭头)函数中，基本上，由于要传递给事件处理器使用，因此它必须是一个函 数。下面的代码不会工作，因为类方法会在浏览器中打开程序时立即执行。
```js
<button onClick={this.onDismiss(item.objectID)} type="button"
>
  Dismiss
</button>
```
当使用 `onClick={doSomething()}` 时，`doSomething()` 函数会在浏览器打开程序时立即执行， 由于监听表达式是函数执行的返回值而不再是函数，所以点击按钮时不会有任何事发生。 但当使用 `onClick={doSomething}` 时，因为 `doSomething` 是一个函数，所以它会在点击按钮 时执行。同样的规则也适用于在程序中使用的 `onDismiss()` 类方法。
然而，使用 `onClick={this.onDismiss}` 并不够，因为这个类方法需要接收 `item.objectID` 属 性来识别那个将要被忽略的项，这就是为什么它需要被封装到另一个函数中来传递这个属 性。这个概念在 JavaScript 中被称为高阶函数。


## 表单元素比如 `<input>`, `<textarea>` 和 `<select>` 会以原生 HTML 的形式保存他 们自己的状态。一旦有人从外部做了一些修改，它们就会修改内部的值，在 `React` 中这被 称为不受控组件，因为它们自己处理状态。在 React 中，你应该确保这些元素变为受控组 件。
你应该怎么做呢?你只需要设置输入框的值属性，这个值已经在 searchTerm 状态属性中保存了，那么为什么不从这里访问呢?
```js
<form>
  <input
    type="text" value={searchTerm} onChange={this.onSearchChange}
  /> 
</form>
```
现在输入框的单项数据流循环是自包含的，组件内部状态是输入框的唯一数据 来源。整个内部状态管理和单向数据流可能对你来说比较新，但你一旦习惯了它，你就会自然而 然的在 React 中实现它。一般来说，React 带来一种新的模式，将单向数据流引入到单页面 应用的生态中，到目前为止，它已经被几个框架和库所采用。


## props中的:children属性的使用
在 props 对象中还有一个小小的属性可供使用: children属性。通过它你可以将元素从上 层传递到你的组件中，这些元素对你的组件来说是未知的，但是却为组件相互组合提供了 可能性。让我们来看一看，当你只将一个文本(字符串)作为子元素传递到 Search 组件中 会怎样。
```js
class App extends Component {
  ...
  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        > Search
        </Search>
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        /> 
      </div>
    ); 
  }
}
```
现在 Search 组件可以从 props 对象中解构出 children 属性。然后它就可以指定这个 children 应该显示在哪里。
```js
class Search extends Component {
  render() {
    const { value, onChange, children } = this.props;
    return (
      <form>
        {children} <input
          type="text"
          value={value}
          onChange={onChange}
      /> 
      </form>
    ); 
  }
}
```
你应该可以在输入框旁边看到这个 “Search” 文本了。当你在别的地方使用 Search 组 件时，如果你喜欢，你可以选择一个不同的文本。总之，它不仅可以把文本作为子元素传 递，还可以将一个元素或者元素树(它还可以再次封装成组件)作为子元素传递。children 属性让组件相互组合到一起成为可能。

## ES6类组件的生命周期方法
`constructor`(构造函数)只有在组件实例化并插入到 DOM 中的时候才会被调用。组件实例 化的过程称作组件的挂载(mount)。
`render()` 方法也会在组件挂载的过程中被调用，同时当组件更新的时候也会被调用。每当 组件的状态(state)或者属性(props)改变时，组件的 render() 方法都会被调用。

在组件挂载的过程中还有另外两个生命周期方法:`componentWillMount()` 和 `componentDid- Mount()`。构造函数(constructor)最先执行，`componentWillMount()` 会在 `render()` 方法之前 执行，而 `componentDidMount()` 在 `render()` 方法之后执行。
总而言之，在挂载过程中有四个生命周期方法，它们的调用顺序是这样的:
* `constructor()`
* `componentWillMount()` 
* `render()`
* `componentDidMount()`