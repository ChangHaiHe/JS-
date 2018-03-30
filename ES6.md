## 对象的扩展

	Object.is() 比较两值是否严格相等 eg: Object.is('foo', 'foo)  // true
		+0 不等于-0  NaN等于NaN
	Object.assign()	讲源对象上所有枚举属性复制到目标对象上()
		为对象添加属性、添加方法、克隆对象、合并多个对象、为属性指定默认值
	
	属性的可枚举性
		Object.getOwnPropertyDescriptor() 获取属性的描述对象
			Object.getOwnPropertyDescriptor(obj, 'foo');
				let obj = { foo: 123 }
				{
					value: 123,
					writable: true,
					enumerable: true,
					configurable: true
				}
		
	ES5 忽略 enumerable 为false的属性
		for...in	遍历自身和继承可枚举属性
		Object.keys()	返回对象自身所有可枚举属性的键名
	ES6 ...同上
		Object.assign() 只复制自身可枚举属性
		Reflect.enumerate() 返回所有for...in 循环会遍历的属性
		所有Class的原型的方法都是不可枚举的

	
	属性的遍历
		for...in
		Object.keys()
		Object.getOwnPropertyNames(obj) 返回一个数组，包含所有可枚举属性，不包含Symbol属性
		Object.getOwnPropertySymbols(obj) 返回一个数组，包含所有Symbol属性
		Object.ownKeys(obj) 返回一个数组, 返回自身所有属性
		Reflect.enumerate(obj) 返回一个Iterator对象, 遍历自身继承和所有可枚举属性,不含Symbol属性,与for...in相同


	__proto__ 用来读取或设置当前对象的prototype对象
	Object.setPrototypeOf() (写操作) 与__proto__相同,用于设置一个对象的prototype对象(ES6正式推荐设置原型对象的方法)
		Object.setPrototypeOf(obj, prototype) eg: Object.setPrototypeOf({}, null);
	Object.getPrototypeOf() (读操作) 读取对象的prototype对象
	Object.create() (生成操作)

	__proto__调用的是 Object.prototype.__proto__


	对象的扩展运算符
		Rest参数
			理解:
				rest参数是浅复制, 如果一个键的值是复合类型的值(数组，对象，函数),那么rest参数复制的是这个值的引用,而不是这个值的副本
				rest不会复制继承原型对象的属性
			用法:
				用于取出参数对象的所有可遍历属性, 复制到当前对象中
					eg: let z = { a:3, b: 4};
							let n = { ...z };
							n // { a: 3, b: 4 }
				合并两个对象


##Class
	理解:
		Class 定义一个类
		constructor() 是构造方法	
		this 关键字代表实例对象
		
		构造函数的prototype属性在ES6上的"类"上继续存在, 事实上, 类的所有方法都定义在类的prototype属性上
		在类的实例上调用方法, 其实就是调用原型上的方法.

		Object.assign() 可以一次性向类添加多个方法

		类的内部定义的所有方法都是不可枚举的 ES6标准入门第二版P242

		类的属性名可以采用表达式

Class的继承
	Class ColorPoint extends Point{}
		通过extends关键字继承了Point类的所有属性和方法
		super关键字，指代父类的实例(父类的this对象)

	ES5的继承思路
		先创造子类的实例对象this,然后再将父类的方法添加到this上(Parent.call(this))
	ES6的继承思路
		先创造父类的实例对象this(所以必须先调用super方法),然后再用子类的构造函数修改this

	prototype And __proto__
		es5中，每一个对象都有__proto__属性, 指向对应的构造函数的prototype属性
		Class作为构造函数的语法糖,同时有prototype属性和__proto__属性,因此同时存在两条继承链
			子类的__proto__属性表示构造函数的继承,总是指向父类
			子类的prototype属性的__proto__属性表示方法的继承,总是指向父类的prototype属性
			eg:
				Class A {}
				Class B extends A {}
				B.__proto__ === A // true
				B.prototype.__proto__ === A.prototype // true
			理解:
				1. 作为一个对象,子类B的原型(__proto__属性)是父类A (理解为B是A的实例, 继承了A身上的所有属性和方法, ps: A构造函数只有属性,只有一个constructor方法, 其余的方法都在A的原型上)
				2. 作为一个构造函数, 子类B的原型(prototype属性)是父类的实例 (理解为继承了A原型上的所有属性和方法  ps: A的原型上只有方法)

	extends
		上面例子A只要是有一个prototype属性的函数,就能被B继承. 由于函数是对象, 函数都有prototype属性,因此,A可以是任意函数.
		三种特殊情况
			1. Class A extends Object { }
				A.__proto__ === Object //true
				A.prototype.__proto__ === Object.prototype
				ps: A就是构造函数Object的复制, A的实例就是Object的实例
			2. Class A { }
				A.__proto__ === Function.prototype // true
				A.prototype.__proto__ === Object.prototype // true
				ps: A作为一个基类(不存在任何继承)就是一个普通函数, 所以直接继承Function.prototype
						A调用后返回一个空对象(Object实例), 所以A.prototype.__proto__指向构造函数(Object)的prototype属性
							ps2: A的实例.__proto__ === A.prototype, 而原型链继续向上走, A.prototype.__proto__ === Object.prototype
									 注意区分一下究竟是A,还是A的实例
			3. Class A extends null { }
				A.__proto__ === Function.prototype // true
				A.prototype.__proto__ === undefined // true
				ps: A也是一个普通函数,所以直接继承Function.prototype.但是，A调用后返回的对象不继承任何方法,所以它的__proto__指向Function.prototype
						ps2： A的实例.__proto__ === A.prototype, A是构造函数: A.prototype.__proto__ === undefined;
				eg: Class C extends null {
							constructor() {
								return Object.create(null)
							}
						}
	Object.getPrototypeOf(ColorPoint) === Point // true
	ps: 可从子类上获取父类, 因此可以用来判断一个类是否继承了另一个类


	思考其他:
		function test () {}
		test.__proto__ === Function.prototype // true
		test.prototype.__proto__ === Object.prototype // true
		理解:
			test是普通函数,直接继承Function.prototype
			test是函数,函数是对象,对象都是Object的实例, 函数都有prototype, 所以test.prototype.__proto__ === Object.prototype 

	实例的__proto__属性
		子类实例的__proto__属性的__proto__属性，指向父类实例的__proto__属性
		子类的原型的原型是父类的原型

		Class ColorPoint extends Point { }

		var p1 = new Point(2, 3);
		var p2 = new ColorPoint(2, 3, 'red');

		p2.__proto__ === p1.proto__ //false
		p2.__proto.__proto__ === p1.__proto__ // true

		因此，可以通过子类实例的__proto__.__proto__属性修改父类实例的行为.

		p2.__proto__.__proto__.printName = function() { console.log('Ha') };
		p1.priName() // 'Ha'













