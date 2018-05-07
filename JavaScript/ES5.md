
##原生JS-Others

面向对象

  特征: 
    对象唯一性
    抽象性
    继承性
    多态


	属性类型
	[[Configurable]]
	[[Enumerable]]
	[[Writable]]
	[[value]]
	修改属性默认值特性: Object.defineProperty()
	eg: Object.defineProperty(person, "name", {
		configurable: false,
		value: 'Nicholas'
	})

创建对象历经的故事
	1. 工厂模式(创建新对象->返回新对象)
	2. 构造函数(this) 
		问题: 无法共享方法
	3. 原型模式(prototype)
		理解1:
			原型对象:
				__proto__是存在实例与原型之间
				isPrototypeOf() 验证实例与原型之间的关系 eg: Person.prototype.isPrototypeOf(person1); // true
				Object.getPrototypeOf  eg: Object.getPrototypeOf(person1) == Person.prototype; // true
				hasOwnProperty() 检测属性存在实例还是存在原型,存在实例返回true eg: person1.hasOwnProperty('name'); // true
				for-in 访问可枚举的属性,既包括实例中的属性,也包括原型中的属性(原型中不可枚举的属性无法访问)
				Object.keys() 访问对象上所有可枚举的实例属性
			优点: 属性方法能被所有实例共享
			问题: 属性值是引用类型也能被所有实例共享.
	4. 组合使用构造函数模式和原型模式(混合模式)
	5. 寄生构造函数模式
	6. 稳妥构造函数模式(不使用this,不使用new)

继承的故事
	1. 原型链
			理解1:
				默认原型 Object.prototype
				原型和实例 instanceof eg: instance instanceof Object // true
				isPrototypeOf()  eg: Object.prototype.isPrototypeOf(instance); // true
				不能用对象字面量创建原型方法,会重写原型链
			问题: 引用类型值的原型属性在实现原型链继承时会被所有实例共享
						创建子类型实例时,不能向超类型的构造函数中传参
	2. 借用构造函数(伪造对象or经典继承)
			理解1:
				call()、apply() 子类型构造函数内部调用超类型构造函数(函数是在特定环境中执行代码的对象)
				问题: 方法都在构造函数中定义,无法实现函数的复用
	3. 组合继承(原型链+构造函数)
			理解:
				原型链实现属性方法继承,构造函数实现实例属性的继承
			问题: 无论什么情况, 都会两次调用超类构造函数(创建子类原型+子类构造函数内部)
	4. 原型式继承(基于已有的对象创建新对象)
	5. 寄生式继承
		 	理解:
			 	创建一个仅用于封装继承过程的函数,改函数在内部以某种方式增强对象
	6. 寄生式组合继承
			
函数的故事
	1. 递归: 一个函数通过名字调用自身
		  理解:
			 arguments.callee 指向正在执行的函数的指针
	2. 闭包： 有权访问另一个函数作用域中变量的函数
			理解:
				闭包VS变量: 闭包只能取得包含函数中任何变量的最后一个值(闭包保存的是整个变量对象,而不是某个特殊的变量)
        ps:
          闭包就是能够读取其他函数内部变量的函数。
          由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。
          本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
	3. this对象:
			理解:
				this对象是在运行时基于函数的执行环境绑定的. 全局函数中,函数运行时this=window, 而当函数被作为某个对象的方法调用时,this指向那个对象
				

引用类型

	Array
		栈方法:
			LIFO: push()、pop()
		队列方法:
			LIFO: shift()、unshift()
		重排序:
			reserve()、sort()
		操作方法:
			concat() 接收参数、一个或多个数组，返回副本.
			slice() 接收1-2个参数, 返回起始和结束为止之间的项,不包含结束位置,(不影响原数组). 可以接收负数(第二个参数为负数时会将负的参数加上字符串的长度。)，没有返回空数组
			splice() 接收2-3个参数, 返回数组中包含原始数组删除的项,没有删除返回[]  接收2个参数表示从起始位置删除+删除的个数.(会改变原数组)
		位置方法:
			indexOf()、lastIndexOf() 返回数组下标
		迭代方法:
			every() 接收一个回调函数,每一项都返回true则返回true
			filter() 接收一个回调函数,该函数返回true的项组成的数组
			forEach() 接收一个回调函数,没有返回值
			map() 接收一个回调函数,返回每次函数调用的结果组成的数组(必须要有return)
			some() 接收一个回调函数,改函数任一项返回true,则返回true
		归并方法:
			reduce()、reduceRight()
			reduce(): 从数组的第一项开始，逐个遍历到最后,接收2个参数,一个在每一项上调用的函数和作为归并基础的初始值(可选的)
				传给reduce()和reduceRight()的函数接收4个参数：前一个值,当前值,项的索引,数组对象.
				这个函数`返回的任何值`都会`作为第一个参数`自动传给下一项.
				第一次迭代发生在数组的第二项上,因此,第一个参数是数组的第一项,第二个参数是数组的第二项.
			eg: var values = [1,2,3,4,5];
				  var sum = values.reduce(function(prev, cur, index, array) {
						return prev + cur;
					});
					alert(sum); // 15

      温故而知新:
        a(b())
        // 先调用b函数，b函数的返回值再作为a函数的参数，进而再执行a函数.
        a()()
        // 先调用a函数，a函数返回另一个函数体，然后再进行这个函数体的调用,
        // 也就是说 执行的是a函数返回的函数


	Date
		Date.now() 当前时间(ie8及以下不支持)
		理解
			var start = +new Date()  // 使用+操作符获取Date对象的时间戳
			dosomething
			var stop = +new Date()
				result = stop - start;
		方法
			getTime()
			getDate()
			getDay()
			getHours()
			...
			
	Function
		理解1:
			每个函数都是Function类型的实例
			由于函数是对象,因此函数名也是一个指向函数对象的指针
			
		理解2:
			1. function sum(num1, num2) { return num1 + num2 }
			2. var sum = function(num1, num2) { return num1 + num2 }
			3. var sum = new Function("num1", "num2", "return num1 + num2");
			函数声明: 解析器率先读取
			函数表达式: 则必须等到解析器执行到它所在的代码行才能被解释执行

		理解3:
			函数内部属性
				2个特殊的对象: arguments、this
				arguments对象有一个callee属性, 指向拥有arguments对象的函数 （递归用）
				函数对象的属性caller, 保存着调用当前函数 的函数的引用，如果是在全局作用域中调用当前函数，值为null

				arguments.callee
				arguments.caller
				函数.caller

		理解4:
			函数的属性和方法
				length、prototype
				call()、apply()

				length 表示接收的参数的个数
				prototype toString()、valueOf()


	基本包装类型
		引用类型与基本类型的主要区别就是对象的生存周期 JS高级程序设计P119
			
		Boolean
		Number
			toFixed(2)
			10.toExponential(1) // 1.0e+1

		String
			字符方法
				charAt() 'hello world'.charAt(1) // 'e'
				charCodeAt() 'hello world'.charCodeAt(1) // '101'
				stringValue[] 'hello world'.stringValue[1] // 'e'
			操作方法
				concat() 'hello '.concat('world'); // 'hello world'
				+	 'hello ' + 'world' // 'hello world'

				slice()     (下标开始, 下标之前) 'abcdefgh'.slice(2,5) 返回 "cde"
				substr()    (下标开始, 下标结束-截取个数) 'abcdefgh'.substr(2,5) 返回 "cdefg"
				subString() (下标开始, 下标之前) 'abcdefg'.substring(2,5) 返回"cde"

        区别:
          substring是以两个参数中较小一个作为起始位置，较大的参数作为结束位置。
            'abcdefg'.substring(5,2) 返回 'cde'
          接收负数时候都有区别，且IE对substr接收负值的处理有错，它会返回原始字符串。(很少见到接收负数的，暂时不做具体区别)
			位置方法
				indexOf()
				lastIndexOf()
			
			trim()	创建字符串副本,删除前置及后缀的所有空格,然后返回结果

			大小转换方法
				toLowerCase()
				toLocaleLowerCase()
				toUpperCase()
				toLocaleUpperCase()

			字符串模式匹配方法
				match() 只接收一个参数, 正则表达式 or RegExp对象，返回一个数组
				search() 参数与match参数相同，返回字符串中第一个匹配项的索引，如果没有返回-1 eg: 'cat, bat, sat, fat'.search(/at/); // 1
				replace() 接收2个参数 第一个参数RegExp对象 or 字符串,第二个参数字符串 or 函数
				split() 基于指定分隔符将字符串分割成多个子字符串，并将结果放在一个数组中, 分隔符可以是字符串 or RegExp对象， 可以接收第二个参数指定数组大小,不改变原数组

				localeCompare() 比较两个字符串,返回 -1 0 1;
						'b'.localeCompare('c'); // -1
						'b'.localeCompare('b'); // 0
						'b'.localeCompare('a'); // 1

				formCharCode() 接收一个或多个字符串编码，将它们转换成字符串 String.formCharCode(104, 101, 108, 108, 111); // 'hello'
			
	Global对象
		URI编码方法
			encodeURI() 整个URI(http://www.wrox.com/illegal value.htm)
			encodeURIComponent() 对非标准字符进行编码
			decodeURI()
			decodeURIcomponent()

		eval()

		Math
			ceil()
			floor()
			round()
			random()
			



# 事件

Javascript和HTML之间的交互是通过事件实现的
## 事件流

从页面接收事件流的顺序
### 事件冒泡

IE事件流叫做事件冒泡
### 事件捕获

老版本浏览器不支持，有特殊需求再用。

## DOM事件流

事件捕获阶段
处于目标阶段
事件冒泡阶段

### 事件处理程序

响应某个事件的函数叫事件处理程序（或事件监听）

DOM0级事件处理程序 -> 元素的方法 this引用当前元素

DOM2级事件处理程序 -> addEventListener() removeEventListener()
接收3个参数，要处理的事件名、事件处理的程序、布尔值(true->捕获,false->冒泡)
```javascript
var btn = document.getElementById('myBtn');
btn.addEventListener("click", function(){
  alert(this.id);
},false)
```
### IE事件处理程序

attachEvent() datachEvent()
接收2个参数，事件名称、事件处理函数
```javascript
btn.addEventListener("click", function(){
btn.attachEvent("onclick", function(){
  alert('clicked')
})
```
```
IE中使用attachEvent()与使用DOM0级方法的主要区别是事件处理程序的作用域
在使用DOM0级方法-> 事件处理程序会在其所属元素的作用域内运行，
在使用attachEvent()方法 -> 事件处理程序会在全局作用域中运行，this === window (ps: 事件对象也在window window.event)
```

### 两者特点

都能添加多个处理程序，IE事件处理程序先处理后面的函数

### 跨浏览器的事件处理程序 (兼容写法)

P354 JavaScript高级程序设计
```javascript
var EventUtil = {
  addHandler: function(element, type, handler) {
    if (element.addEventListener) {
      element.addEventListener(type, handler, false);
    } else if (element.attachEvent) {
      element.attachEvent('on' + type, handler);
    } else {
      element["on" + type] = handler;
    }
  },
  removeHandler: function(element, type, handler) {
    if (element.addEventListener) {
      element.removeEventListener(type, handler, false);
    } else if (element.attachEvent) {
      element.detachEvent('on' + type, handler);
    } else {
      element["on" + type] = null;
    }
  }
}

```

### 事件对象

#### DOM事件对象 e

e.bubbles 事件是否冒泡
e.stopProgapation() 取消事件进一步捕获或冒泡 bubbles为true可以使用
e.cancelable 是否取消事件默认行为
e.preventDefault() 取消事件默认行为 cancelable为true可以使用
e.target 事件目标

阻止链接跳转 -> preventDefault
处理按钮 -> stopProgapation

### IE中的事件对象

window.event

cancelBubble 默认值false, 设置为true可以取消事件冒泡 -> stopProgapation
returnValue 默认值true，设置为false可以取消事件默认行为 -> preventDefault
srcElement 事件的目标 -> target


window.event.srcElement === this

### 跨浏览器的事件对象(事件对象兼容)

```javascript
var EvenUtil = {
  getEvent: function(event) {
    return event ? event || window.event
  },
  getTarget: function(event) {
    return event.target || event.srcEvent;
  },
  preventDefault: function(event) {
    if (event.preventDefault) {
      event.preventDefault();
    } else {
      event.returnValue = false;
    }
  },
  stopPropagation: function(event) {
    if (event.stopPropagation) {
      event.stopPropagation()
    } else {
      event.cancelBubble = true;
    }
  }
}

```

### 事件类型

load
  1. 页面完全加载完后在window上触发
  2. 所有框架都加载完毕时在框架集上触发
  3. 当图像加载完毕时在<img>上触发
  4. 当嵌入内容加载完毕时在<object>元素上触发

  5. 一些非标准方式支持load, script元素触发load以便开发人员确定动态加载Javascript文件是否完毕 (ie9+支持)
  
unload
  1. 文档被完全卸载后触发
  2. 用户从一个页面切换到另一个页面触发
  这个时间最多的的情况是清除引用，避免内存泄漏


resize
  窗口调整到一个新的高度或宽度触发 ie9+支持

scroll
  在window上触发，实际表示的是页面中相应元素的变化
  混杂模式下，通过body元素的scrollLeft和scrollTop来监控

blur
  失去焦点 不冒泡

focus
  获得焦点 不冒泡

### 鼠标与滚轮事件

mousedown
  任意鼠标按钮按下触发，不能通过键盘触发

mouseleave
  元素上方的光标 -> 元素范围外触发 不冒泡

mousemove
  鼠标指针在元素内部移动时触发

mouseout
  元素上方的光标 -> 移入另一个元素时触发

mouseover
  光标在元素外部，首次移到另一个元素边界时触发

mouseenter mouseleave
  鼠标事件都会冒泡，也都可以被取消

--------------------------------华丽的分割线--------------------------------

## 客户区坐标位置 P370 (相对于整个浏览器)

视口: (能浏览的页面内容,整个浏览器窗口除去属性栏的下面部分)
clientX 视口水平坐标
clientY 视口垂直坐标


## 页面坐标位置 (相对于整个页面)

pageX
pageY
鼠标光标在页面中的位置
因此，坐标是从页面本身计算
而非视口的左边和右边计算

页面没有滚动的情况下,
pageX === clientX
pageY === clientY

IE8之前版本不支持事件对象上的页面坐标，而是通过使用客户区坐标和滚动信息计算的
document.body(混杂模式) 或 document.documentElement(标准模式)
的scrollLeft和scrollTop属性计算

```javascript
var div = document.getElementById('myDiv');
EventUtil.addHandler(div,"click", function(event){
  event = EventUtil.getEvent(event);
  var pageX = event.pageX;
      pageY = event.pageY;
  if (pageX === undefined) {
      pageX = event.clientX + (document.body.scrollLeft || document.documentElement.scrollLeft);
  }
  if (pageY === undefined) {
    pageY = event.clientY + (document.body.scrollTop || document.documentElement.scrollTop)
  }
  alert("Page coordinates:" + pageX + ',' + pageY);
})

```

## 屏幕坐标位置 (相对于整个电脑屏幕)

screenX 
screenY


## 实际开发

clientheight
![clientheight](https://img-blog.csdn.net/20170207205650930?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2FuZ2p1bjUxNTk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### offsetWidth
![offsetWidth](https://img-blog.csdn.net/20170207205905401?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2FuZ2p1bjUxNTk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### scrollHeight 
![scrollHeight](https://img-blog.csdn.net/20170207210025028?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2FuZ2p1bjUxNTk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```javascript
// 实际开发  整个页面的高度 == 滚动条滚动的高度(当前滚动条位置) + 视口高度
  getScrollHeight: () => { // (获取的是 整个页面的高度)
    let scrollHeight = 0;
    let bodyScrollHeight = 0;
    let documentScrollHeight = 0;
    if (document.body) {
      bodyScrollHeight = document.body.scrollHeight;
    }
    if (document.documentElement) {
      documentScrollHeight = document.documentElement.scrollHeight;
    }
    scrollHeight = (bodyScrollHeight - documentScrollHeight > 0) ? bodyScrollHeight : documentScrollHeight;
    return scrollHeight;
  },
  getWindowHeight: () => { // 浏览器视口的高度
    let windowHeight = 0;
    if (document.compatMode === 'CSS1Compat') {
      windowHeight = document.documentElement.clientHeight;
    } else {
      windowHeight = document.body.clientHeight;
    }
    return windowHeight;
  },
  getPageScroll: () => { // 获取当前滚动条位置 (获取的是整个页面滚动条的当前位置)
    let x;
    let y;
    // debugger;
    if (window.pageYOffset) {
      // all except IE
      y = window.pageYOffset;
      x = window.pageXOffset;
    } else if (document.documentElement && document.documentElement.scrollTop) {
      // IE 6 Strict
      y = document.documentElement.scrollTop;
      x = document.documentElement.scrollLeft;
    } else if (document.body) {
      // all other IE
      y = document.body.scrollTop;
      x = document.body.scrollLeft;
    }
    return { X: x, Y: y };
  },
  setPageScroll: (y) => { // 设置页面纵向滚动条
    if (document.body) {
      // all other IE
      document.body.scrollTop = y;
    } else if (document.documentElement && document.documentElement.scrollTop) {
      // IE 6 Strict
      document.documentElement.scrollTop = y;
    } else if (window.pageYOffset) {
      // all except IE
      window.pageYOffset = y;
    }
  },

```
### 参考资料

[1](https://www.cnblogs.com/nanshanlaoyao/p/5964730.html)
[2](https://www.cnblogs.com/momo798/p/5923371.html)

--------------------------------华丽的分割线--------------------------------


## 修改键


## 相关元素

mouseover mouseout
这两个事件都会涉及把鼠标指针从`一个元素`的**边界**移动到`另一个元素`的**边界**之内
```
比如，一个例子，页面上显示一个div元素。
如果鼠标指针一开始位于这个div元素上，然后移出了这个元素，
那么久会在div元素上触发mouseout事件，相关元素是body元素
同时，
body元素上回触发mouseover事件，而相关元素变成了div
```

### 鼠标按钮


## 更多事件信息

offsetX 光标相对于目标元素边界的X坐标
offsetY 光标相对于目标元素边界的Y坐标

## 键盘与文本事件

keydowm 用户按下键盘上`任意键`触发，按住不放则重复触发此事件 
keypress  用户按下键盘上`字符键`触发，按住不放则重复触发此事件
keyup 用户释放键盘上的键触发

三者关系:

用户按了一下键盘上的字符键，
首先触发keydown,
接着触发keypress,
最后触发keyup

keydown和keypress都是在文本框中发生变化之前触发，
keyup是在文本框已经发生变化之后被触发

如果用户按下一个非字符键，则先触发keydown，然后是keyup

## 键码 p380

发生在keydown和keyup
event.keyCode = 值(一个代码,与ASCII码中对应的小写字母或数字的编码相同)

## 字符编码

发生在keypress

查编码之用属性 charCode或者keypress
```javascript
function (event) {
  if (typeof event.charCode == 'number' ) {
    return event.charCode;
  } else {
    return event.keypress
  }
}

```

## DOM3变化

不再包含charCode属性，新增key、char
key 取代 keyCode,
  字符键按下时，key的值就是相应的文本字符，如"k","M"
  非字符键按下,key值就是相应键的名,如"Shift","Down"
char
  字符键按下同key
  非字符键按下，值为null

IE9支持key，不支持char

综上： 不推荐使用key char


## textInput事件(代替keypress)

在可编辑区域中输入字符，就会触发这个事件
区别
  1. 任何焦点元素可以触发keypress，但只有可编辑区域才能触发textInput事件
  2. textInput只有在用户按下能输入实际字符的键才会被触发，
     keypress事件则是在按下哪些能够影响文本显示的键也会触发(比如退格键)


## 变动事件

### 删除节点

removeChild() 或 replaceChild()

### 插入节点

appendChild()、replaceChild()、insertBefore()


# 内存和性能

### 事件委托 P402

对事件处理程序过多，解决问题方案就是事件委托。利用事件冒泡，只制定一个事件处理程序，就可以管理某一类型的所有事件，click事件一直会冒到document层次

适合采用事件委托的技术
  click、mousedown、mouseup、keydown、keyup、keypress

### 移出事件处理程序

每当事件处理程序指定给元素时，运行中的浏览器代码与支持页面交互的Javascript代码之间会建立一个连接。连接越多，页面执行越慢
在不需要的时候，移出事件处理程序

两种情况会造成上面的问题:
  1. 文档中移出带有事件处理程序的元素，用到replaceChild,removeChild,innerHTML
    知道某个元素将被移出，最好是手工处理程序(btn.onclick = null)，
    或者用事件委托避免这种问题的出现
  2. 卸载页面的时候。 比如两个页面来回切换，也可能是点击刷新按钮，内存的滞留的对象会增加，因为事件处理程序占用的内存没有被释放。
    卸载页面前调用onunload移出所有的事件处理程序(onload加载的所有程序都用onunload移出)
    或者用事件委托技术避免这种问题出现
