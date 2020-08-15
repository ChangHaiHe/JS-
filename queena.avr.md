#react从创建的销毁都会经历哪些生命周期（react15和react16+有较大变化，我更希望听到后者）

[初始化阶段]：
  getDefaultProps:获取实例的默认属性
  getInitialState:获取每个实例的初始化状态
  componentWillMount：组件即将被装载、渲染到页面上
  render:组件在这里生成虚拟的 DOM 节点
  componentDidMount:组件真正在被装载之后
[运行中状态]:
  componentWillReceiveProps: 组件将要接收到属性的时候调用
  shouldComponentUpdate:组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行）componentWillUpdate:组件即将更新不能修改属性和状态
  render:组件重新描绘
  componentDidUpdate:组件已经更新
[销毁阶段]：
  componentWillUnmount:组件即将销毁


#父子组件通信有哪些？

#react class组件事件绑定this的方式

#react如何避免重复渲染

#无状态组件和有状态组件有什么区别

#是否了解HOC和render props，简要介绍下

#是否了解context api，应用场景有哪些

#是否有了解react新特性（hooks api， lazy suspense等等）

#是否有应用redux，简述redux数据流

#redux如何处理异步操作

#React 中 keys 的作用是什么？
Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。


react 
<Query>