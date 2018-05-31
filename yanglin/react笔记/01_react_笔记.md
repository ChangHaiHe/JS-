##1. 几个重要概念理解
  * 模块与组件
    * 模块:
      * 理解: 向外提供特定(局部)功能的js程序, 一般就是一个js文件
      * 为什么: js代码更多更复杂
      * 作用: 复用js, 简化js的编写, 提高js运行效率
    * 组件: 
      * 理解: 用来实现特定功能效果的代码集合(html/css/js)
      * 为什么: 一个界面的功能更复杂
      * 作用: 复用编码, 简化项目编码, 提高运行效率
  * 模块化与组件化
    * 模块化:
      * 当应用的js都以模块来编写的, 这个应用就是一个模块化的应用
    * 组件化:
      * 当应用是以多组件的方式实现功能, 这上应用就是一个组件化的应用
##2. React的基本认识
  * Facebook开源的一个js库
  * 一个用来动态构建用户界面的js库
  * React的特点
    * Declarative(声明式编码)
    * Component-Based(组件化编码)
		* 基本思路：
			1、拆分页面功能
			2、确定大体组件模块
			3、编写组件
			4、渲染组件
    * Learn Once, Write Anywhere(支持客户端与服务器渲染)
	  双端渲染：
	  即可以在浏览器端渲染也可以在服务器端渲染
		1、浏览器端：
			* 默认请求的是React-dom中的index。
			* 暴露的对象是：ReactDOM
			* ReactDOM的渲染方法为render()
		2、服务器端：
			* 默认请求的是React-dom中的server。
			* 暴露的对象是：ReactDOMServer
			* ReactDOM的渲染方法为renderToString()
    * 高效
	  1、虚拟DOM
	    * 在真是DOM对象上层映射了一层虚拟DOM对象。
		* 虚拟DOM对象和真是的DOM对象是一一对应的。
		* 即使修改虚拟DOM对象页面不会重新渲染。
		* 虚拟DOM对象操作完，转换为真实的DOM对象，此时页面才会渲染，但渲染的次数较常规的DOM对象次数要少。
	  2、DOM Diff算法
	    react会将DOM对象映射成虚拟DOM对象树，
		  * 只比较变化的部分，达到最小化页面重绘
		  * 通过key表示来比较
		  * 注意input等表单项的时候的问题，此时不能用index作为key的属性值，可以用其他自己设计的。
		  * react会计算出页面中变化之后和变化之前变化的部分，渲染的时候只会渲染变化的部分。局部渲染。
    * 单向数据流
  * React高效的原因
    * 虚拟(virtual)DOM, 不总是直接操作DOM(批量更新, 减少更新的次数) 
    * 高效的DOM Diff算法, 最小化页面重绘(减小页面更新的区域)
##3. 使用React
	* 导入相关js库文件(react.js, react-dom.js, babel.min.js)
	* 编码:
		```
      <div id="container"></div>
      <script type="text/babel">
        var aa = 123
        ReactDOM.render(<h1>{aa}</h1>, containerDOM);
      </script>
		```
##4. JSX
* 全称: JavaScript XML
  * react定义的一种类似于XML的JS扩展语法: XML+JS
  * 作用: 用来创建react虚拟DOM(元素)对象
	* js中直接可以套标签, 但标签要套js需要放在{}中
	* 在解析显示js数组时, 会自动遍历显示
	* 把数据的数组转换为标签的数组: 
		```
      var liArr = dataArr.map(function(item, index){
        return <li key={index}>{item}</li>
      })
		```
	* 注意:
    * 标签必须有结束
    * 标签的class属性必须改为className属性
    * 标签的style属性值必须为: {{color:'red', width:12}}
##5. Component : React是面向组件编程的(组件化编码开发)
	* 基本理解和使用
		* 自定义的标签: 组件类(函数)/标签
		* 创建组件类
	      ```
	      //方式1: 无状态函数(最简洁, 推荐使用)
	      function MyComponent1() {
	        return <h1>自定义组件标题11111</h1>;
	      }
	      //方式2: ES6类语法(复杂组件, 推荐使用)
	      class MyComponent3 extends React.Component {
	        render () {
	          return <h1>自定义组件标题33333</h1>;
	        }
	      }
	      //方式3: ES5老语法(不推荐使用了)
	      var MyComponent2 = React.createClass({
	        render () {
	          return <h1>自定义组件标题22222</h1>;
	        }
	      });
	      ```
		* 渲染组件标签
			```
			ReactDOM.render(<MyComp />,  cotainerEle);
			```
    * ReactDOM.render()渲染组件标签的基本流程
      * React内部会创建组件实例对象/调用组件函数, 得到虚拟DOM对象
      * 将虚拟DOM并解析为真实DOM
      * 插入到指定的页面元素内部
	* props
		* 所有组件标签的属性的集合对象
		* 给标签指定属性, 保存外部数据(可能是一个function)
		* 在组件内部读取属性: this.props.propertyName
		* 作用: 从目标组件外部向组件内部传递数据
		* 对props中的属性值进行类型限制和必要性限制
      ```
      Person.propTypes = {
        name: PropTypes.string.isRequired,
        age: PropTypes.number.isRequired
      }
      ```
    * 扩展属性: 将对象的所有属性通过props传递
        ```
        <Person {...person}/>
        ```
	* 组件的组合
		* 组件标签中包含子组件标签
		* 拆分组件: 拆分界面, 抽取组件
		* 通过props传递数据
	* refs
		* 组件内包含ref属性的标签元素的集合对象
		* 给操作目标标签指定ref属性, 打一个标识
		* 在组件内部获得标签对象: this.refs.refName(只是得到了标签元素对象)
		* 作用: 操作组件内部的真实标签dom元素对象
	* 事件处理
		* 给标签添加属性: onXxx={this.eventHandler}
		* 在组件中添加事件处理方法
		  ```
		    eventHandler(event) {
		                
		    }
		  ```
		* 使自定义方法中的this为组件对象
		  * 在constructor()中bind(this)
		  * 使用箭头函数定义方法(ES6模块化编码时才能使用)
	* state
		* 组件被称为"状态机", 页面的显示是根据组件的state属性的数据来显示
		* 初始化指定:
	        ```
	        constructor() {
	          super();
	          this.state = {
	            stateName1 : stateValue1,
	            stateName2 : stateValue2
	          };
	        }
	        ```
		* 读取显示: 
		    this.state.stateName1
		* 更新状态-->更新界面 : setState是异步操作
		    this.setState({stateName1 : newValue})
				setState 方法接收一个 function 作为回调函数。这个回掉函数会在 setState 完成以后直接调用，这样就可以获取最新的 state 。
				this.setState((prevState, props) => ({
					counter: prevState.counter + props.increment
				}));
	* 实现一个双向绑定的组件
		* React是单向数据流
		* 需要通过onChange监听手动实现
	* 组件生命周期
		* 组件的三个生命周期状态:
		* Mount：插入真实 DOM
		* Update：被重新渲染
		* Unmount：被移出真实 DOM
    * 生命周期流程:
      * 第一次初始化显示
        ```
        constructor()
        componentWillMount() : 将要插入回调
        render() : 用于插入虚拟DOM回调
        componentDidMount() : 已经插入回调
        ```
      * 每次更新state
        ```
        componentWillReceiveProps(newprops): 接收父组件新的属性
				shouldComponentUpdate(nextProps, nextState)：是否进行更新组件
        componentWillUpdate(nextProps, nextState) : 将要更新回调
        render() : 更新(重新渲染)
        componentDidUpdate() : 已经更新回调
        ```
      * 删除组件
        ```
        ReactDOM.unmountComponentAtNode(document.getElementById('example')) : 移除组件
        componentWillUnmount() : 组件将要被移除回调
        ```
    * 常用的方法
      ```
      render(): 必须重写, 返回一个自定义的虚拟DOM
      constructor(): 初始化状态, 绑定this(可以箭头函数代替)
      componentDidMount() : 只执行一次, 已经在dom树中, 适合启动/设置一些监听
      ```   
##6. ajax
	* React没有ajax模块
	* 集成其它的js库(如axios/fetch/jQuery/), 发送ajax请求
	  * axios
	    * 封装XmlHttpRequest对象的ajax
	    * promise
	    * 可以用在浏览器端和服务器
	  * fetch
	    * 不再使用XmlHttpRequest对象提交ajax请求
	    * fetch就是用来提交ajax请求的函数, 只是新的浏览才内置了fetch
	    * 为了兼容低版本的浏览器, 可以引入fetch.js
	* 在哪个方法去发送ajax请求
	  * 只显示一次(请求一次): componentDidMount()
	  * 显示多次(请求多次): componentWillReceiveProps()
	  
##7. 虚拟DOM
  * 虚拟DOM是什么?
    * 一个虚拟DOM(元素)是一个一般的js对象, 准确的说是一个对象树(倒立的)
    * 虚拟DOM保存了真实DOM的层次关系和一些基本属性，与真实DOM一一对应
    * 如果只是更新虚拟DOM, 页面是不会重绘的
  * Virtual DOM 算法的基本步骤
    * 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
    * 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异
    * 把2所记录的差异应用到步骤1所构建的真正的DOM树上，视图就更新了
  * 进一步理解
    * Virtual DOM 本质上就是在 JS 和 DOM 之间做了一个缓存。
    * 可以类比 CPU 和硬盘，既然硬盘这么慢，我们就在它们之间加个缓存：既然 DOM 这么慢，我们就在它们 JS 和 DOM 之间加个缓存。CPU（JS）只操作内存（Virtual DOM），最后的时候再把变更写入硬盘（DOM）。
		
		let render = function (eleObj, container) {
    //解构赋值
    let {type, props} = eleObj;
    //创建虚拟DOM元素
    let ele = document.createElement(type);
    //遍历props所有属性
    for (let attr in props) {
        if (attr === 'children') {
            props[attr].forEach(function (item) {
                if (typeof item === 'string') {
                    let textNode = document.createTextNode(item);
                    ele.appendChild(textNode);
                } else {
                    render(item, ele);
                }
            });
        } else if (attr === 'classname') {
            // ele.classname=props[attr];
            ele.setAttribute('class', props[attr]);
        } else {
            ele.setAttribute(attr, props[attr]);
        }
    }
    container.appendChild(ele);
}

##Diff算法
		传统 diff 算法的复杂度为 O(n^3)，显然这是无法满足性能要求的。React 通过制定大胆的策略，将 O(n^3) 复杂度的问题转换成 O(n) 复杂度的问题。
		React 通过分层求异的策略，对 tree diff 进行算法优化；
		React 通过相同类生成相似树形结构，不同类生成不同树形结构的策略，对 component diff 进行算法优化；
		React 通过设置唯一 key的策略，对 element diff 进行算法优化；
	react的Diff算法：
		根据 React 的设计，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这种算法叫做 DOM diff ，它可以极大提高网页的性能表现。
	虚拟DOM为什么使用Diff算法：
		1.DOM操作太耗费性能，尽量减少DOM操作
		2.找到本次DOM必须更新的节点更新，其他不更新
		3.找出真实DOM和虚拟DOM的区别中‘找出’的过程就是Diff算法
	Diff实现过程
		patch(container,vhode)
		patch(vnode,newVnode)

为什么列表的key尽量不要用index
		简单来说: 当数组中的数据发生变化时: React 比较更新前后的元素 key 值，
    如果相同则更新，如果不同则销毁之前的，重新创建一个元素

##组件化的理解
	1、封装：视图，数据，变化逻辑(数据驱动试图变化)
	2、复用：传递props

##React中高阶组件
	在业务组件中经常会有大量类似的附属功能，可以把这些功能抽离成公用的装饰器方法（也就是react中的高阶组件）。至于高阶组件需要返回一个容器组件，这只是为了实现装饰功能的写法而已

##JSX的本质
	JSX实际就是语法糖，JSX就是在js中写xml，最后会解析为js(React.createElement())；降低了我们的学习和代码成本

##JSX和虚拟DOM的关系
	JSX会创建出虚拟DOM

##setState的过程
	setState是异步的，vue的修改属性也是异步的
		可能会一次执行多次setState
		没必要每次setState都重新渲染，考虑性能，只需最后看到效果即可
	setState的过程
		每个组件的实例都有renderComponent方法
		执行renderComponent会重新执行实例的render
		render函数返回newVnode，然后拿到preVnode
		经过diff算法对比重新渲染组件

##React和Vue的认识
	vue:本质是MVVM框架，由MVC发展而来(支持组件化由MVVM上扩展)
			vue模板由anglar发展而来(模板分离强)
	React:本质是前端组件化框架，由后端组件化框架发展而来
				JSX(模板语法强)
	相同点：都支持组件化，都是数据驱动视图

##React性能优化
	性能检测工具：React.addons.Perf 插件
	打开console面板，先输入Perf.start()执行一些组件操作，引起数据变动，组件更新，然后输入Perf.stop()。（建议一次只执行一个操作，好进行分析）
	解决：immutable.js