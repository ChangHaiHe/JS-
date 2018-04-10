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

----------
## Class
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

----------

## Promise

理解:
  Promise是一个对象，用来传递异步操作的消息
  三种状态 Padding（进行中）
          Resolved（已完成，又称Fulfilled）
          Reject（已失败）
  缺点: 一旦新建，立即执行，无法取消
        处于Pending状态，无法得知进展到哪个阶段

  1. then
    * 定义在Promise.prototype上的
    * 方法可以接受两个回调函数作为参数, 第一个是成功回调，第二个是失败回调
    * 作用是为Promise实例添加改变时的回调函数.
    * 返回的是一个新的Promise实例(注意: 不是原来那个Promise实例)

  2. catch
    * 制定发生错误时的回调
    ```javascript
      p.then((val) => console.log('fullfilled', val))
       .catch((err) => console.log('rejected:', err));

      // 等同于

      p.then((val) => console.log('fullfilled', val))
       .then(null, (err) => console.log('rejected:', err));

    ```
    * 一般不要在then方法中定义Rejected状态的回调函数(即then的第二个参数),而应该总使用catch方法
    ```javascript
      promise
        .then(function(data) {
           console.log('success');
        })
        .catch(function(err) {
          console.log('err');
        })
    ```
    * catch方法返回的还是一个Promise对象，因此后面还可以接着调用then方法

  3. Promise.all()
    * 用于将多个Promise实例包装成一个新的Promise实例
    > var p = Promise.all([p1,p2,p3]);
    > 1. 当p1,p2,p3状态都变成已完成(Fullfilled), P的状态才会变成Fullfilled, 此时p1,p2,p3的返回值组成一个数组，传递给p的回调
    > 2. 只要p1,p2,p3中有一个被Rejected，P的状态就变成Rejected, 此时第一个被Rejected的实例的返回值会被传递给p的回调
    > ```javascript
    >
    > ```

        var promise = [2,3,4,5,7,11,13].map((id) => {
          return getJson("/post/"+id+".json");
        });
        
        Promise.all(promise).then((posts) => {
          ...
        }).catch((reason) => {
          ...
        })

      ```
    * 该方法接受一个数组作为参数(实际上是只要具有Iterator接口的都可以), p1,p2,p3都是Promise的实例, 如果不是实例，则先调用Promise.resolv()将参数转化为Promise实例
      ```

  4. Promise.race()
    * 同样是将多个Promise实例包装成一个新的Promise实例
    > var p = Promise.all([p1,p2,p3]);
      1. p1,p2,p3中有一个率先改变状态,p的状态就跟着改变。那个率先改变的Promise实例的返回值，就传递给p的回调函数。

    * 参数与Promise.all方法一样，如果不是Promise实例，也先调用Promise.resolve方法，然后进一步处理

  5. Promise.resolve()
    > Promise.resolve('foo')
    // 等价
    new Promise(resolve => resolve('foo'))

    * 该方法的参数不是具有then方法的对象(又称thenable对象), 则返回一个新的Promise对象，且状态为Resolved
  
  6. Promise.reject()
    > Promise.reject(reason)方法也会返回一个新的Promise实例,状态为Rejected. Promise.reject方法的参数reason会被传递给实例的回调函数

两个有用的附加方法
  7. done()
    > 总是处于回调链的尾端, 保证跑出任何可能出现的错误.

    asyncFunc()
      .then(f1)
      .catch(f2)
      .then(f2)
      .done();

    其实现代码
    ```javascript
      Promise.prototype.done = function (onFulfilled, onRejected) {
        this.then(onFullfilled, onRejected).catch((reason) => {
           <!-- 抛出一个全局错误 -->
           setTimeout(() => { throw reason}, 0)
        });
      };

    ```

  8. finally()
    > 不管对象最后状态如何都会执行的操作。
    与done最大的区别: 它接受一个普通的回调函数作为参数， 该函数不管怎样都会必须执行
    
    eg: 
      // 服务器使用Promise处理请求, 然后使用finally方法关掉服务器
      server.listen(0)
        .then(function() {
          ...
        })
        .finally(server.stop);

    实现原理
    ```javascript
      Promise.prototype.finally = function (callback) {
        let P = this.constructor;
        return this.then(
          value => P.resolve(callback()).then(() => value),
          reason => p.resolve(callback()).then(() => { throw reason })
        );
      };
    <!-- 无论Promise是Fulfilled还是Rejected,都会执行回调函数callback -->
    ```


## 数组的扩展

Array.from()
  > 将两类对象转为真正的数组: 类似数组的对象 || 可遍历(有iterable)的对象(包括数据结构Set和Map)
  > > 接收第二个参数, 作用类似于数组的map方法, 用来对每个元素进行处理, 将处理后的值放入返回的数组.
  > ```javascript
  >   Array.from(arrayLike, x => x * x);
  >   // 等价
  >   Array.from(arrayLike).map(x => x * x);
  > ```

    Array.from([1,2,3]).map(x => x * x); // [1,4,9]

  ```


  document.querySelectorAll() 返回一个类似数组的对象

所谓类似数组的对象, 本质特征只有一点，必须有length属性.

Array.of()
  > 将一组值转换为数组
  > Array.of(3,11,8) // [3,11,8]


数组实例的find() 和 findIndex()

  find:
    > 实例的find方法，用于招数第一个复合条件的数组成员
      参数是一个回调函数，所有数组成员依次执行该回调函数，知道找出第一个返回值为true的成员，返回该成员

    [1,4,-5,10].find((n) => n < 0) // -5

  findIndex:
    > 返回第一个符合条件的数组成员的位置,所有成员都不符合条件，则返回-1.
    [1,5,10,15].findIndex(function(value, index, arr) {
      return value > 9
    })
    // 2

    另外
      [NaN].indexOf(NaN) // -1
      [NaN].findIndex(y => Object.is(NaN, y)) // 0

数组的fill()
  
  fill
    > 用于填充数组
    ['a','b','c'].fill(7)  // [7,7,7]
    
    理解: 
      用于空数组的初始化很方便
      还可以接受第二个第三个参数,用于指定填充的起始位置和结束位置
        eg: ['a','b','c'].fill(7,1,2) // ['a',7,'b']

    
数组实例的 includes()

  Array.prototype.includes()
    [1,2,3].includes(2) // true
    [1,2,3].includes(4) // false

    [NaN].indexOf(NaN) // -1
    [NaN].includes(NaN) // true


数组的空位

  ES6明确将空位转换为 undefined

  for...of 会遍历空位
  eg: 
    let arr = [,,];
    for(let i of arr) {
      console.log(1)
    }
    // 1
    // 1
  entries() 遍历键值对
  [...[,'a'].entries()] // [[0, undefined], [1, 'a']]

  ```



# Set

是一种新的数据结构，类似数组，组员值是唯一的，没有重复值



###去重

```javasc
var s = new Set()
[2,3,5,4,5,2,2].map(x => s.add(x))
for(i of s) {console.log(i)} // 2 3 4 5
```

```javascript
var set = new Set([1,2,3,3,4,4])
[...set] // [1,2,3,4]
set.size // 4
```

NaN 等于自身



### 属性和方法

```
Set.prototype.constructor
Set.prototype.size
```

#### 操作方法

```
add(value)	增加某个值
delete(value)	删除某个值
has(value): 返回布尔值，是否有该成员
clear(): 清楚所有成员
```

####Array.from

```javasc
将Set结构转换为数组
Array.from(new Set([1,2,3,1,2,5,3,4,5])) // [1,2,3,4,5]
```

#### 遍历方法

```
keys() 遍历健名
values() 遍历键值
entries()
forEach()
注意: Set结构没有健名，所以 健名===健值 
```

```
Set.prototypes[Symbol.iterator] === Set.prototypes.values
意味着: 可以省略values方法,直接用 for...of 循环遍历Set
```

### WeakSet

结构和Set类似

区别

* WeakSet成员只能是对象
* 对象都是弱引用(**垃圾回收机制不考虑WeakSet对该对象的引用** -> WeakSet是不可遍历)

WeakSet是一个构造函数，所以可以用new命令创建WeakSet数据结构


p156




