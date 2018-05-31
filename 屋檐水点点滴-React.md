1. http、浏览器缓存

2. react新特性 传送门（React Portal）

3. jqeury React 对比，不同点

4. gulp 和 webpack有什么不同

5. 自己实现一个观察者

6. ES6 Set Map 数据类型

7. react完整声明周期，从初始化创建到结束， setState之后触发了哪些生命周期

8. 写过哪些功能性的组件

9. 事件委托

10. es6 promise有哪些方法

11. React父组件更新，不想让子组件更新，改怎么做

12. 说说你实现过的一个比较复杂的模块 

13. router 2.0和4.0的主要区别  4--当做组件  2--集中处理

14. dva  redux-sage   antd组件

15. 项目问题点、 项目模块功能

16. dom哪些地方最消耗性能
  避免重绘和回流
    使用 DocumentFragment 进行缓存操作,引发一次回流和重绘。（兼容 IE9+ 及主流浏览器）
    使用 cloneNode (true or false) 和 replaceChild 技术，引发一次回流和重绘。

40. 如何优化页面scroll性能。 函数节流、防抖
https://juejin.im/entry/59e631c46fb9a04525773144
  函数节流场景:
    窗口调整（resize）
    页面滚动（scroll）
    抢购疯狂点击（mousedown）
  函数防抖
    实时搜索（keyup）
    拖拽（mousemove）
27. 网站性能优化
[Yahoo团队经验：网站性能优化的34条黄金法则](http://www.ha97.com/2710.html)

43. react 性能优化
https://juejin.im/post/5ac34810f265da23870f0748
  shouldComponentUpdate -> PureComponent
  imuutable

17. setState接收几个参数,是同步还是异步  2  异步
[【React】setState详解](https://juejin.im/post/5a155f906fb9a045284622b4)
    shouldComponentUpdate()
    componentWillUpdate()
    render()
    componentDidUpdate()

18. 数据可视化

19. 中间件思想， 中间件Promise

20. ES6

21. JS作用域链  JS继承 ES5(混合模式) + ES6(class) 
  箭头函数和一般函数的区别 ->  this指向.
  this指向, bind, call, apply。 

  1.函数调用  ->  (指向window)
  2.作为构造函数调用  -> 实例化)这个对象
  3.作为对象的方法调用  -> this就是这个上级的对象
  4.可以作为元素的节点
  5.call()和apply()  this=改变后的 调用这个函数的对象

22. React-router原理  hashHistory() 请求一次   browsHistory() ->  请求很多次 - 后台控制 设计上区别

23. 提供一个场景，input框...状态state 用redux怎么设计


24. 原生JS -> ajax步骤，具体到每一步.   
innerHtml  和 innerText 一些兼容性 
e.target  获取dom节点

25. jsonp和原理， script标签， 原生ajax的jsonp请求 返回的是json还是js 

26. 路由原理



28. 闭包作用场景  

29. socket内部原理

30. Echars图表

31. 用过哪些前端框架

32. 箭头函数如果不想写return，那应该怎么写

33. router单页和多页的应用 hashrouter怎么监听哈希值  router的原理


34. 移动端flex、rem原理，基于什么取值， em， 他们区别
  rem是css3 的一个长度单位 ，相对文档跟元素 html；比如设置html font-size=100px;那么1rem=100px;之后的所有元素都可以用这个基准值来设置大小；
  1em = 16px;
35. 如何判断两个对象是否相等

36. 二维数组扁平化&数组去重的优化算法 -- 最简单的复制一个数组，越简单越好

37. 金额千位分隔符问题，看解题思路；简单版考察字符串转数组、数组倒序、三位取余数做法，复杂版提示用正则表达式解题，考察正则表达式熟悉程度和递归思想。

38. JavaScript实现模块的方式（AMD/CommonJS/UMD）。

39. 原生DOM操作：DOM遍历（nodeType/firstChild/previousSibling/nextSibling），inserAfter实现。



41. 自己是怎么做技术选型的

42. ajax能做聊天室吗，如果不能的话为什么



44. es5的this指向

45. 三列布局
方案1:
content { display: flex } // 父div
.left {width: 200}
.middle { flex: 1 }
.right { width: 20011 }
方案2:
.left { position: absolute;left: 0;top:0 }
.left {  margin: 0 200px; }
.right { position: absolute;right: 0; top:0 }
  两列布局
方案1:
  flex
方案2:
  浮动/定位(左边)+margin-left(右边)

46. redux技术选型，为什么要用redux， 状态管理器如果不用redux又有什么其他比较好的解决方案

47. 类似于babel转码原理，项目中如果用箭头函数或者promise进行报错，怎么解决 -> webpack配置--(也再看看babel转码原理吧)

48. 浏览器从输入url之后开始到页面展示出来具体会经历哪些步骤，可以深入一点
https://www.cnblogs.com/engeng/articles/5943382.html

49. 最简单的方法复制一个新数组
var a = [1,2,3]
var b = a.clice();
b // [1,2,3];

50. 知道哪些前端框架

51. redux设计模式，前端存储用户数据，用户注销，怎么清楚数据，用户登录，怎么拿到这些数据，
另一个用户登录，怎么清空之前的数据，然后自己再存储这些数据

52. 为什么用redux，redux具体解决了哪些问题，除了状态管理，如果不用redux状态管理，用router，怎么去实现react的
状态管理
  1: 就知道一个全局变量
  2：思考高阶组件不知可不可以
  3: 可以思考Node

53. 用redux产生过哪些问题

53. 原生JS实现懒加载

54. 客户端离线存储，用户注销了，下次上线，再拿到数据

55. Redux到底解决了哪些问题
    组件通信(组件层级很深,props传好几层)/跨组件数据存储(如多页面购物车数据)/

56. 写过webpack插件 loader吗 webpack发布过npm包吗

57. 正则一些优化

58. ajax和fetch的区别

59. 两栏布局，左边固定，右边自适应
