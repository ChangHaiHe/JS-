
#CSS优先级算法如何计算？
!important > 行内样式 > #id > .class > tag > * > 继承 > 默认

#盒模型
width+height => padding => border => margin
页面渲染时，dom 元素所采用的 布局模型。可通过box-sizing进行设置。根据计算宽高的区域可分为：
  content-box (W3C 标准盒模型) //测量width和height属性只包括的内容，不包括border, margin, 或者padding。
  border-box (IE 盒模型)  //width和height属性包括padding和border，不包括margin
  padding-box //width和height属性包括padding的大小，不包括border和margin
  inherit //指定box-sizing属性的值，应该从父元素继承


#flex布局简单阐述原理，列举项目中的应用，是否存在兼容性问题，如何解决
[原理]：浏览器的布局方式是：沿着主轴的方向依次排列，如果要换行，则沿着交叉轴的方向进行换行，每行代表一条轴线
       flex布局是围绕父元素和轴来进行布局的
       Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性，
       采用flex布局的元素，称为flex容器，容器默认存在2根轴，水平的主轴和垂直的交叉轴，项目默认沿主轴的方向排列
[案例]：水平垂直居中，一侧固定一侧
      .container {
        display: flex;
        .sidebar {
          width: 100px;
        }
        .content {
          flex: 1;
        }
      }
[兼容性]：通过加上各种前缀
        webkit内核的浏览器，必需加上 -webkit 前缀
        .box{
          display: box;              /* OLD - Android 4.4- */
          display: -webkit-box;      /* OLD - iOS 6-, Safari 3.1-6 */
          display: -moz-box;         /* OLD - Firefox 19- (buggy but mostly works) */
          display: -ms-flexbox;      /* TWEENER - IE 10 */
          display: -webkit-flex;     /* NEW - Chrome */
          display: flex;             /* NEW, Spec - Opera 12.1, Firefox 20+ */
        }

#flex布局
  * display: flex;
  * display: -webkit-flex;  /* Safari */
    * 主轴方向：flex-direction: row|row-reverse|column|column-reverse;（左到右|右到左|上到下|下到上）
    * 水平对齐：justify-content: flex-start|flex-end|center|space-between|space-around; (左对齐|右对齐|居中|2端对齐|中间对齐)
    * 垂直对齐： align-items: flex-start|flex-end|center|baseline|stretch; (上对齐|下对齐|垂直居中|项目第一行文字的基线对齐|占满)
    * 多根轴线对齐： align-conent: flex-start|flex-end|center|space-between|space-around|stretch;
    * 换行： flex-wrap: nowrap|wrap|wrap-reverse (不换行|换行第一行在上方|换行第一行在下方)
    * flex: 0 1 auto; (flex: flex-grow flex-shrink flex-basis)
    * flex-grow: 0; 等分剩余空间，0不等分，1按比例等分，如果一个为2，其他都为1，其占据的剩余空间是其他的2倍
    * flex-shrink: 1; 空间不足时项目缩小，0不缩小, 1按比例缩小
    * flex-basis: auto; 在分配多余空间前,计算主轴是否有多余空间，auto项目本来大小

# 水平垂直居中：
  * 未知宽高水平垂直居中
    <div class="parent">
      <div class="child">hello world</div>
    </div>
    方法一：父元素display:table; 子元素：display:table-cell，
            优势，父元素可以动态改变高度，劣势：table属性容易造成多次reflow,IE8以下不支持
        .parent{
          display: table;
        }
        .parent .child {
          display: table-cell;
          vertical-align: middle;
          text-align: center;
        }
    方法二：利用空元素或伪类，优势：兼容性好
        .parent {
          position: absolute;
          width: 100%;
          height: 100%;
          text-align: center;
          background: #92b922;
        }
        .child {
          background: #d93168;
          display: inline-block;
          color: #fff;
          padding: 20px;
        }
        .parent:after {
          display: inline-block;
          content: '';
          width: 0px;
          height: 100%;
          vertical-align: middle;
        }
    方法三：绝对定位+伪类, transform兼容性差，IE9以下不支持
        .parent{
          position:relative;
          width:300px;
          height:300px;
          background:#92b922;
        }
        .child{
          position:absolute;
          top:50%;
          left:50%;
          color:#fff;
          background:red;
          transform:translate(-50%,-50%);
        }
    方法四：flex布局，兼容性不好，IE支持性很差
      display: flex;
      align-items: center;
      justify-content: center;

#图片文字

#什么是CSS预处理器并介绍相关特性
less/sass/stylus (.less/.scss)
css预处理器是用一种专门的编程语言，进行 Web 页面样式设计，然后再编译成正常的 CSS 文件，以供项目使用。CSS 预处理器为 CSS 增加一些编程的特性，无需考虑浏览器的兼容性问题
[特性]：1⃣️less变量以@开头, sass变量以$开头
        @orange: #feb914；
        header {
            background-color: @orange;
        }

        $orange: #feb914；
        header {
            background-color: $orange;
        }
      2⃣变量作用域， less以最近一次定义的为准，sass以距离最近的为准
      3⃣️⃣️嵌套 
      4⃣️混入
        .roundedCorners(@radius: 5px){
          -moz-border-radius: @radius;
          -webkit-border-radius: @radius;
          border-radius: @radius;
        }
        /*定义的类应用到另个一个类中*/
        ＃header {
          .roundedCorners; 
        }
        #footer {
          .roundedCorners(10px);
        }
      5⃣️函数， less中不能自定义函数，sass可以
        @function pxToRem($px) {
          @return $px / 2;
        }
        body{
          font-size: pxToRem(32px);
        }

#display:none与visibility:hidden的区别
  display：none 不显示对应的元素，在文档布局中不再分配空间（回流+重绘）
  visibility：hidden 隐藏对应元素，在文档布局中仍保留原来的空间（重绘）


# 优雅降级，渐进增强
渐进增强（Progressive Enhancement）：一开始就针对低版本浏览器进行构建页面，完成基本的功能，然后再针对高级浏览器进行效果、交互、追加功能达到更好的体验。

优雅降级（Graceful Degradation）：一开始就构建站点的完整功能，然后针对浏览器测试和修复。比如一开始使用 CSS3 的特性构建了一个应用，然后逐步针对各大浏览器进行 hack 使其可以在低版本浏览器上正常浏览。

#em, rem, px
  em:相对父元素  rem:相对根元素  px:像素

#rem.js
  function initRem(doc, win) {
    var html = document.getElementsByTagName("html")[0];
    var fs = parseInt(getStyle(html, 'fontSize'));

    function getStyle(obj, attr) {
      if (obj.currentStyle) {
        return obj.currentStyle[attr];
      }
      else {
        return getComputedStyle(obj, false)[attr];
      }
    }

    function setUnitA() {
      document.documentElement.style.fontSize =
        document.documentElement.clientWidth / 10 / fs * 100 + "%";
    }

    var h = null;
    window.addEventListener("resize", function () {
      clearTimeout(h);
      h = setTimeout(setUnitA, 300);
    }, false);
    setUnitA();
  }

  initRem(document, window)

#媒体查询
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="format-detection" content="telephone=no">

  CSS : @media only screen and (max-device-width:480px) {/css样式/}

#怎么让Chrome支持小于12px 的文字？
p{font-size:10px;-webkit-transform:scale(0.8);} //0.8是缩放比例

#让页面里的字体变清晰，变细用CSS怎么做？
-webkit-font-smoothing在window系统下没有起作用，但是在IOS设备上起作用-webkit-font-smoothing：antialiased是最佳的，灰度平滑。

#js数据类型
5种基本类型：Number，String，Boolean，Undefined，Null
复杂数据类型：Object

引用类型有：Object,Array,Function,Data等
es6新增数据类型，symbol

#基本类型和引用类型的区别
1⃣️声明变量时不同的内存分配，
  基本类型是存储在栈中的简单数据段，
  引用类型是存储在堆中的对象。存储在变量处的是一个指针，指向存储对象的内存地址

2⃣️基本类型按值访问，引用类型按引用访问（首先得到这个对象在堆内存中的地址，然后再按照这个地址去获得这个对象中的值）
3⃣️复制变量时，基本类型是独立的，改变其中一个不影响另一个。引用类型复制的是内存地址，2个变量指向的是堆内存中的同一个对象，改变一个会影响另一个
4⃣️参数传递不同，基本类型只是把变量里的值传递给参数，之后互不影响。
  引用类型把对象在堆内存中的地址传递给这个变量，传递内存地址，函数对这个参数内部的修改会体现在外部

#检测数据类型
typeof, instanceof, constructor, Object.prototype.toString.call()

typeof 'aa';  //string
[] instanceof Array;  //true  (instanceof要求左边的运算数是一个对象，右边的运算数是对象类的名字或构造函数)
[] instanceof Object;  //true 
[].constructor(); //[]
Object.prototype.toString.call([]); //[Object, Array]
Object.prototype.toString.call({}); //[Object, Object]
Object.prototype.toString.call(new Date()); //[Object, Date]
Object.prototype.toString.call(new Function()); //[Object, Function]
Object.prototype.toString.call(new RegExp()); //[Object, RegExp]


#vue、react、angular
* Vue.js
一个用于创建 web 交互界面的库，是一个精简的 MVVM。它通过双向数据绑定把 View 层和 Model 层连接了起来。实际的 DOM 封装和输出格式都被抽象为了Directives 和 Filters

* AngularJS
是一个比较完善的前端MVVM框架，包含模板，数据双向绑定，路由，模块化，服务，依赖注入等所有功能，模板功能强大丰富，自带了丰富的 Angular指令

* react
React 仅仅是 VIEW 层是facebook公司。推出的一个用于构建UI的一个库，能够实现服务器端的渲染。用了virtual dom，所以性能很好。


#说下闭包，闭包用多了会内存泄漏，还有什么也会内存泄漏？
闭包是指有权访问另一个函数作用域中变量的函数
闭包的特性：
  函数内嵌套函数
  内部函数可以引用外层的参数和变量
  参数和变量不会被垃圾回收机制回收

使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。在js中，函数即闭包，只有函数才会产生作用域的概念。

内存泄漏： 1⃣️使用递归，2⃣️setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏


#作用域链
作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的
简单的说，作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期


#JavaScript原型，原型链 ? 有什么特点？
每个对象都会在其内部初始化一个属性，就是prototype(原型).
当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的（原型链）的概念
instance.constructor.prototype = instance.__proto__

特点：JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变

#原型，构造函数，实例
[原型]：一个简单的对象，用于实现对象的属性继承
[构造函数]：可以通过new来新建一个对象的函数
[实例]：通过构造函数和new创建出来的对象，便是实例。实例通过_ptoto_指向原型，通过constructor指向构造函数
实例._proto_ = 构造函数.prototype = 原型
原型.constructor = 实例.constructor = 构造函数


#javascript如何实现继承
  es5: 主要通过prototype实现继承。在js中，继承通常指的是原型链继承，也就是通过指定原型，并可以通过原型链继承原型上的数学和方法
  最优化[圣杯模式]
  var inherit = (function(c, p){
    var F = function(){};
    return function(c, p){
      F.prototype = p.prototype;
      c.prototype = new F();
      c.uber = p.prototype;
      c.prototype.constructor = c;
    }
  })()

  es6: 通过class/extends 实现继承

#对象的拷贝
  * 浅拷贝: 把父对象的属性，全部拷贝给子对象，实现继承
          如果父对象的属性等于数组或另一个对象，那么实际上，子对象获得的只是一个内存地址，而不是真正拷贝，因此存在父对象被篡改的可能
          [Object.assign], [展开运算符]
      function extendCopy(p) {
        var c = {};
        for(var i in p){
          c[i] = p[i];
        }
        c.uber = p;
        return c;
      }

  * 深拷贝: 就是能够实现真正意义上的数组和对象的拷贝 (jquery使用的就是这种继承方法)
          (JSON.parse(JSON.stringify(obj)): 性能最快
            具有循环引用的对象时，报错
            当值为函数、undefined、或symbol时，无法拷贝)
      function deepCopy(p, c){
        var c = c || {};
        for(var i in p){
          if(typeof p[i] === 'object) {
            c[i] = (p[i].constructor === Array) ? [] : {};
            deepCopy[p[i], c[i]];
          }else {
            c[i] = p[i];
          }
        }
        return c;
      }

#谈下对this对象的理解
this总指向函数的直接调用者
如果有new关键字，this指向new出来的那个对象
在事件中，this指向触发这个事件的对象。特殊的是，ie中的attachEvent中的this总指向全局对象window


#事件代理（事件委托）
主要是利用事件冒泡，把原本需要绑定的事件委托给父元素，让父元素担当事件监听的职务，点击子元素时通过e.target 或 e.srcElement判断
优点：1，可以节省大量内存空间  2，可以实现当新增子对象时无需对其绑定


#事件模型
冒泡型事件：当你使用事件冒泡时，子级元素先触发，父级元素后触发
捕获型事件：当你使用事件捕获时，父级元素先触发，子级元素后触发
DOM事件流：同时支持两种事件模型：捕获型事件和冒泡型事件
阻止冒泡：在W3c中，使用stopPropagation()方法；在IE下设置cancelBubble = true
阻止捕获：阻止事件的默认行为，例如click - <a>后的跳转。在W3c中，使用preventDefault（）方法，在IE下设置window.event.returnValue = false


## 请描述一下 cookies，sessionStorage 和 localStorage 的区别？
存储大小：
cookie数据大小不能超过4k
sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大

有期时间：
cookie 设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭
sessionStorage 数据在当前浏览器窗口关闭后自动删除
localStorage 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据

cookie是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）
cookie数据始终在同源的http请求中携带（即使不需要），记会在浏览器和服务器间来回传递
sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存


## 从浏览器地址栏输入url到显示页面的步骤
浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图像等）；
浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM）；
载入解析到的资源文件，渲染页面，完成

#15. 防抖与节流(lodash)
防抖与节流函数是一种最常用的 高频触发优化方式，能对性能有较大的帮助。
防抖 (debounce): 将多次高频操作优化为只在最后一次执行，通常使用的场景是：用户输入，只需再输入完成后做一次输入校验即可。
  function debounce(fn, wait, immediate) {
    let timer = null
    return function() {
      let args = arguments
      let context = this

      if (immediate && !timer) {
        fn.apply(context, args)
      }

      if (timer) clearTimeout(timer)
      timer = setTimeout(() => {
        fn.apply(context, args)
      }, wait)
    }
  }

节流(throttle): 每隔一段时间后执行一次，也就是降低频率，将高频操作优化成低频操作，通常使用场景: 滚动条事件 或者 resize 事件，通常每隔 100~500 ms执行一次即可。

  function throttle(fn, wait, immediate) {
    let timer = null
    let callNow = immediate
    
    return function() {
      let context = this,
          args = arguments

      if (callNow) {
          fn.apply(context, args)
          callNow = false
      }

      if (!timer) {
        timer = setTimeout(() => {
          fn.apply(context, args)
          timer = null
        }, wait)
      }
    }
  }


## HTTP的几种请求方法用途
get: 产生一个TCP数据包, 对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
post: 产生两个TCP数据包。而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

GET参数通过URL传递，POST放在Request body中。GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。GET请求会被浏览器主动cache，而POST不会，除非手动设置。GET在浏览器回退时是无害的，而POST会再次提交请求。

HTTP的底层是TCP/IP。所以GET和POST的底层也是TCP/IP，也就是说，GET/POST都是TCP链接。GET和POST能做的事情是一样一样的。你要给GET加上request body，给POST带上url参数，技术上是完全行的通的。 

其他还有：put/head/delete/options/trace/connect


## HTTP状态码及其含义
* 1XX：信息状态码
    100 Continue 继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
* 2XX：成功状态码
    200 OK 正常返回信息
    201 Created 请求成功并且服务器创建了新的资源
    202 Accepted 服务器已接受请求，但尚未处理
* 3XX：重定向
    301 Moved Permanently 请求的网页已永久移动到新位置。
    302 Found 临时性重定向。
    303 See Other 临时性重定向，且总是使用 GET 请求新的 URI。
    304 Not Modified 自从上次请求后，请求的网页未修改过。
* 4XX：客户端错误
    400 Bad Request 服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
    401 Unauthorized 请求未授权。
    403 Forbidden 禁止访问。
    404 Not Found 找不到如何与 URI 相匹配的资源。
* 5XX: 服务器错误
    500 Internal Server Error 最常见的服务器端错误。
    503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）


## es6/es7
1⃣️let,const 块级作用域，不存在变量提升，var 代码块外也能调用，有变量提升(无论变量声明在何处，都会被视为声明在函数的最顶部，（不在函数内即在全局zuo yon）)
2⃣️模版字符串 [``和${}, 换行不需要用反斜杆]
3⃣️箭头函数和函数默认值
  继承上下文this
  省略return
  返回仅有一个表达式的时候可以省略return
4⃣️解构赋值
5⃣️...展开运算符，取出对象/数组的某一部分或合并对象/数组
6⃣️import/export
  1.当用export default people导出时，就用 import people 导入（不带大括号）
  2.一个文件里，有且只能有一个export default。但可以有多个export。
  3.当用export name 时，就用import { name }导入（记得带上大括号）
  4.当一个文件里，既有一个export default people, 又有多个export name 或者 export age时，导入就用 import people, { name, age } 
  5.当一个文件里出现n多个 export 导出很多模块，导入时除了一个一个导入，也可以用import * as example
7⃣️promise, 用同步的方法去写异步代码
    setTimeout(function() {
      console.log(1)
    }, 0);
    new Promise(function executor(resolve) {
      console.log(2);
      for( var i=0 ; i<10000 ; i++ ) {
        i == 9999 && resolve();
      }
      console.log(3);
    }).then(function() {
      console.log(4);
    });
    console.log(5);

    // 2，3，5，4，1
8⃣️generator, yield: 暂停代码, next(): 继续执行代码
    // 生成器
    function *createIterator() {
        yield 1;
        yield 2;
        yield 3;
    }

    // 生成器能像正规函数那样被调用，但会返回一个迭代器
    let iterator = createIterator();

    console.log(iterator.next().value); // 1
    console.log(iterator.next().value); // 2
    console.log(iterator.next().value); // 3

9⃣️async/await: 是generator的语法糖， babel中是基于promise实现。
  [generator需要加.next()后执行，async/await会在上一个执行完后，自动执行下一个]

🔟

#es7新特性：
1⃣️Array.prototype.includes()方法，含2个参数，第二个指的是从哪个索引开始
  ['a', 'b', 'c', 'd'].includes('b', 1);
  ['a', 'b', 'c', 'd'].indexOf('b');
  区别indexOf(), indexOf能取到索引位置，includes不行。 indexOf不能判断是否包含NaN,includes可以
2⃣️求幂运算符（**），借鉴了python,ruby
  3 ** 2  //9
  Math.pow(3, 2)  //9


## 如何进行网站性能优化
content方面:
  减少HTTP请求：合并文件、CSS精灵、inline Image
  减少DNS查询：DNS缓存、将资源分布到恰当数量的主机名
  减少DOM元素数量

Server方面:
  使用CDN
  配置ETag
  对组件使用Gzip压缩

css方面:
  将样式表放到页面顶部
  不使用CSS表达式
  使用<link>不使用@import

Javascript方面:
  将脚本放到页面底部
  将javascript和css从外部引入
  压缩javascript和css
  删除不需要的脚本
  减少DOM访问

图片方面:
  优化图片：根据实际颜色需要选择色深、压缩
  优化css精灵
  不要在HTML中拉伸图片


## 前端需要注意哪些seo:
合理的title、description、keywords：搜索对着三项的权重逐个减小，title值强调重点即可，重要关键词出现不要超过2次，而且要靠前，不同页面title要有所不同；
description把页面内容高度概括，长度合适，不可过分堆砌关键词，不同页面description有所不同；keywords列举出重要关键词即可
语义化的HTML代码，符合W3C规范：语义化代码让搜索引擎容易理解网页
重要内容HTML代码放在最前：搜索引擎抓取HTML顺序是从上到下，有的搜索引擎对抓取长度有限制，保证重要内容一定会被抓取
重要内容不要用js输出：爬虫不会执行js获取内容
少用iframe：搜索引擎不会抓取iframe中的内容
非装饰性图片必须加alt
提高网站速度：网站速度是搜索引擎排序的一个重要指标

#如何解决跨域问题?
jsonp、 iframe、window.name、window.postMessage、服务器上设置代理页面

#谈谈你对webpack的看法
WebPack 是一个模块打包工具，你可以使用WebPack管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包Web开发中所用到的HTML、Javascript、CSS以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，webpack有对应的模块加载器。webpack模块打包器会分析模块间的依赖关系，最后 生成了优化且合并后的静态资源

#你觉得jQuery源码有哪些写的好的地方
jquery源码封装在一个匿名函数的自执行环境中，有助于防止变量的全局污染，然后通过传入window对象参数，可以使window对象作为局部变量使用，好处是当jquery中访问window对象的时候，就不用将作用域链退回到顶层作用域了，从而可以更快的访问window对象。同样，传入undefined参数，可以缩短查找undefined时的作用域链
jquery将一些原型属性和方法封装在了jquery.prototype中，为了缩短名称，又赋值给了jquery.fn，这是很形象的写法
有一些数组或对象的方法经常能使用到，jQuery将其保存为局部变量以提高访问速度
jquery实现的链式调用可以节约代码，所返回的都是同一个对象，可以提高代码效率

#Ajax原理
Ajax的原理简单来说是在用户和服务器之间加了—个中间层(AJAX引擎)，通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。使用户操作与服务器响应异步化。这其中最关键的一步就是从服务器获得请求数据
Ajax的过程只涉及JavaScript、XMLHttpRequest和DOM。XMLHttpRequest是ajax的核心机制

  * 1. 创建连接
  var xhr = null;
  xhr = new XMLHttpRequest()
  * 2. 连接服务器
  xhr.open('get', url, true)
  * 3. 发送请求
  xhr.send(null);
  * 4. 接受请求
  xhr.onreadystatechange = function(){
    if(xhr.readyState == 4){
      if(xhr.status == 200){
        success(xhr.responseText);
      } else { // fail
        fail && fail(xhr.status);
      }
    }
  }

#冒泡排序
每次比较相邻的2个数，如果后一个比前一个小，换位置
var arr = [3, 1, 4, 6, 5, 7, 2];
function bubbleSort(arr){
  for(var i = 0; i < arr.length; i++){
    if(arr[i+1] < arr[i]){
        var temp;
        temp = arr[i];
        arr[i] = arr[i+1];
        arr[i+1] = temp;
    }
  }
  return arr;
}

bubbleSort(arr);


#快速排序
采用二分法，取出中间数，数组每次和重睑术
var arr = [3, 1, 4, 6, 5, 7, 2];
function quickSort(arr){
  if(arr.length == 0){
    return [];
  }
  var cIndex = Math.floor(arr.length/2);
  var c = arr.splice(cIndex, 1);
  var l = [];
  var r = [];
  for(var i = 0; i < arr.length; i++){
    if(arr[i] < c){
      l.push(arr[i]);
    }else{
      r.push(arr[i])
    }
  }
  return quickSort(l).contact(c, quickSort(r));
}

quickSort(arr);

#谈谈你对重构的理解
在不改变外部行为的情况下,简化结构，添加可读性

#什么样的前端代码是好的
高复用低耦合，这样文件小，好维护，而且好扩展。

#平时如何管理你的项目？
先期团队必须确定好全局样式（globe.css），编码模式(utf-8) 等；

编写习惯必须一致（例如都是采用继承式的写法，单样式都写成一行）；

标注样式编写人，各模块都及时标注（标注关键样式调用的地方）；

页面进行标注（例如 页面 模块 开始和结束）；

CSS跟HTML 分文件夹并行存放，命名都得统一（例如style.css）；

JS 分文件夹存放 命名以该JS功能为准的英文翻译。

图片采用整合的 images.png png8 格式文件使用 - 尽量整合在一起使用方便将来的管理
