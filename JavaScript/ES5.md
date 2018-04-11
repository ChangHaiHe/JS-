
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
			slice() 接收1-2个参数, 返回起始和结束为止之间的项,不包含结束位置,(不影响原数组). 可以接收负数，没有返回空数组
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
			

