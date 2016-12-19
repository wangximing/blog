* Action 只是描述了有事情发生了这一事实
* reducer 指明应用如何更新 state
*  Redux 应用只有一个单一的 store
*  任何一个从 connect() 包装好的组件都可以得到一个 dispatch 方法作为组件的 props
*  connect() 的唯一参数是 selector。此方法可以从 Redux store 接收到全局的 state，然后返回给组件中的 props。
*   redux-actions 这类的辅助库来生成 action creator 和 reducer 
*   Redux middleware 被用于解决不同的问题，但其中的概念是类似的。它提供的是位于 action 被发起之后，到达 reducer 之前的扩展点。

## React

* React 通过将数组中的每个元素渲染为一个节点的方式对数组进行自动求值。
* JSX中不能使用if else,可以通过下面的方法解决
	
	*  	使用三目运算符
	*   设置一个变量，并在属性中引用它
	*   将逻辑转换到函数中
	*   使用&&
* htmlFor, className

#### 声明周期

##### 实例化
getDefaultProps-->getInitailState-->componentWillMount-->render-->componentDidMount

##### 存在期
componentWillReceiveProps-->shouldComponentUpdate-->componentWillUpdate-->render-->componentDidUpdate