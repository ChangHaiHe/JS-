# 0. redux要点
	1. redux理解
	2. redux相关API
	3. redux核心概念(3个)
	4. redux工作流程
	5. 使用redux及相关库编码

#1. redux理解
	什么?: redux是专门做状态管理的独立第3方库, 不是react插件
	作用?: 对应用中状态进行集中式的管理(写/读)
	开发: 与react-redux, redux-thunk等插件配合使用
	三大原则：单一数据流，state是只读的，使用纯函数执行修改

# 2. redux相关API
	redux中包含: createStore(), applyMiddleware(), combineReducers()
	store对象: getState(), dispatch(), subscribe()
	react-redux: <Provider>, connect()()

# 3. redux核心概念(3个)
	action: 是一个描述
		默认是对象(同步action), {type: 'xxx', data: value}, 
		需要通过对应的ActionCreator产生const increment = (number) => ({type: 'INCREMENT', data: number}), 
		它的值也可以是函数(异步action), 需要引入redux-thunk才可以

	bindActionCreator()

	reducer
		根据老的state和指定的action, 返回一个新的state
		不能修改老的state

	combineRucer()  合并多个reducer函数
		reducer只有一个，但是我们可以用手段来将他们拆分，就是combineReducer()

	store
		redux最核心的管理对象
		内部管理着: state和reducer
		* Redux提供createStore(reducer函数)方法，创建Store对象
		提供方法: 
		* getState() 得到state,
		* dispatch(action) 分发action, 触发reducer调用, 产生新的state 是View发出Action的唯一方法, 
		* subscribe(listener) 注册监听, 当产生了新的state时, 自动调用  listener:渲染新的组件


	redux功能：
		store保存所有的状态state
		需要改变时通过dispatch发送action
		reducer接收到state和action，生成新的state

	redux的使用方法：  发布订阅/观察者模式
		首先通过reducer函数新建store，store以组件属性的方式传进组件内，
		外部以store.subscribe(render)订阅render state 有 任何变化会重新渲染
		组件内部随时通过store.getState()获取状态和获取store还有相应的函数
		需要更改状态使用store.dispatch(action)来修改状态会触发reducer函数
		reducer函数接收state和cation，返回新的state，可以通过store.subscribe监听每次的变化

	redux处理异步需要redux-thunk插件（中间件）
		使用applyMiddleware开启redux-thunk中间件
		createStore(reducer,cpplyMiddleware(thunk))
		Action可以返回函数，使用disoatch提交action
	redux调试工具：redux-devtools-extension
	const store = createStore(reducer,compose(
		applyMiddleware(thunk),
		window.devToolsExtension?window.devToolsExtension():f=>f
	))

# 4. redux工作流程
![](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)
![](https://i.imgur.com/2R5G8bG.png)
		1.写Reducer函数  想好所有状态的可能性
			function counter(state = {"v" : 100} , action){
				if(action.type == "ZENGJIA"){
						return {"v" : state.v + 1};
				}else if(action.type == "JIANSHAO"){
						return {"v" : state.v - 1};
				}
				return state;
		}
		2.创建Store，并且注册视图
			var store = createStore(counter);
			subscribe(listener);
		3.发送action
			document.getElementById("jiaBtn").onclick = function(){
				store.dispatch({"type" : "ZENGJIA"});
			}


		
# 5. 使用redux及相关库编码
	需要引入的库: 
		redux
		react-redux
		redux-thunk
		redux-devtools-extension(这个只在开发时需要)
	redux文件夹: 
		action-types.js
		actions.js
		reducers.js
		store.js
	组件分2类: 
		ui组件(components): 不使用redux相关PAI
		容器组件(containers): 使用redux相关API

	1、<Provider>组件
		语法：<Provider store={store}>
    			<App></App>
				 </Provider>

		将下辖的所有用connect()函数链接的组件，让他们可以使用Redux的store
		通常情况下，如果没有一个父元素或者祖先元素被<Provider>包裹，那么你不能用connect()函数获得数据
		如果你非不听话，也可以通过props将store下传，一层传一层

	2、connect()函数:负责从外部获取组件需要的参数  是一个高阶组件
		语法：export default connect(
					mapStateToProps,
					mapStateToProps
				)(组件名字);
		将React组件和Redux的全局store进行连接
		它不更改你传入的组件类，而是染回一个新的、被装饰的、已经连接好store的组件给你用

			mapStateToProps  将外部的数据（即state对象）转换为UI组件的标签属性
				如果你传了这个参数，被装饰之后的组件将会订阅store的更新。
				论任何时候store被更改了（有dispatch更改了它），这个mapStateToProps参数函数就会被调用。（言外之意就是，全局store中的state被改的时候，组件会重新渲染
				你传入的第一个参数的返回值，必须是一个纯度JSON对象，这个JSON对象的key讲和组件的props进行融合。
			const mapStateToProps =   (state){
				return {}
			}

			mapDispatchToProps   将分发action的函数转换为UI组件的标签属性
				系统送给我们一个dispatch可以调用，我们return的JSON对象的key也将和props进行融合
			const mapDispatchToProps = {actions}

		使用装饰器优化connect函数？
			安装babel-pulgin-transform-decorators-legacy 配置一下webpack
			@connect(
				//你要state的什么属性放在props里
				state => ({num:state}),
				//你要的什么方法，放在props里，自动dispatch
				{addGun,removeDun}
			)

	Redux中间件
		1.applyMiddlewares() 它是 Redux 的原生方法，作用是将所有中间件组成一个数组，依次执行
			```javascript
			const store = createStore(
				reducer,
				applyMiddleware(thunk, promise, logger)
			);
			```
		异步操作的套路	
			组件发请求
			action写异步
			reducer写if

		2.redux-thunk 中间件
		







模拟redux库
//创建仓库
const createStore = (reducer)=> {
	let state; //创建一个状态
	ket listeners = [];  //监听函数的数组
	let getState = () => state; //用来获取最新的状态
	//向仓库发送action
	let dispatch = () => {

	}
	//订阅仓库内的状态变化事件，当状态发生的变化之后会调用对应的监听函数
	let subscribe = (listener) => {
		listeners.push(listener);
		return () => {
			
		}
	}
	return {
		//返回的是store对象
		getState

	}
}
export {createStore};
