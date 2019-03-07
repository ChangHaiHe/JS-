#简单自我介绍, 做过哪些项目, 使用哪些技术栈 ? 
  我叫李后琴，目前从事前端开发已有5年的时间，接触过angular,vue,react3大框架，

#Grunt和Gulp
Grunt和Gulp的工作方式是：在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，这个工具之后可以自动替你完成这些任务。
#webpack
Webpack的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个浏览器可识别的JavaScript文件。

#webpack的原理, loader 和 plugin 是干什么的? 有自己手写过么 ?
* webpack核心概念
  [entry] 一个可执行模块或库的入口文件。
  [chunk] 多个文件组成的一个代码块，例如把一个可执行模块和它所有依赖的模块组合和一个 chunk 这体现了webpack的打包机制。
  [loader] 文件转换器，例如把es6转换为es5，scss转换为css。
  [plugin] 插件，用于扩展webpack的功能，在webpack构建生命周期的节点上加入扩展hook为webpack加入功能。
* 打包原理
  识别入口文件，通过逐层识别模块依赖。（Commonjs、amd或者es6的import，webpack都会对其进行分析。来获取代码的依赖），
  webpack做的就是分析代码。转换代码，编译代码，输出代码，最终形成打包后的代码
* loader
  文件加载器，能够加载资源文件，并对这些文件进行一些处理，诸如编译，压缩等，最终打包到指定的文件中
* plugin
  在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果
  * plugin和loader的区别是什么？
    对于loader，它就是一个转换器，将A文件进行编译形成B文件，这里操作的是文件，比如将A.scss或A.less转变为B.css，单纯的文件转换过程
    plugin是一个扩展器，它丰富了wepack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务。

#如何看待前端框架选型 ? 
1⃣️首先看项目本身的需求，是pc项目，还是移动端项目，如果是pc项目可以考虑vue,移动项目考虑react
2⃣️要看下当前团队的技术能力，学习新框架的成本，后期维护的成本
3⃣️框架的优势劣势是什么
3⃣️框架的生态系统，是否有繁荣的生态供我们学习
4⃣️跨平台性，是否要同时支持移动端和pc端


#react和vue和angular的比较 ?
[angular]：双向数据绑定，指令都是n-, html, 学习成本高，适合复杂大型应用
[vue]：双向数据绑定， 指令是v-, ui库element-ui, html模版, 学习成本低，性能好，适合小型应用，跨平台优势差
[react]：单向数据流，jsx语法, ui库antd-design, 学习成本一般，适合跨平台应用,react native

#Vue和react的区别：
首先vue和react有很大的相似之处，都是前端mvvm框架，React与Vue也都是只有骨架，其他的功能如路由、状态管理等是框架分离的组件。
Vue2.0版本之前并没有使用虚拟dom，Vue.js(2.0版本)与React都可以使用虚拟dom，使得性能提高很多，不过都是虚拟dom，但是渲染方式有很大差别， vue在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树，而对于React而言，每当应用的状态被改变时，全部子组件都会重新渲染。
React与Vue最大的不同是模板的编写。Vue虽然也有虚拟dom，但鼓励去写近似常规HTML的模板。写起来很接近标准HTML元素，只是多了一些属性。React推荐所有的模板通用JavaScript的语法扩展——JSX书写。当然现在vue也可以使用JSX编写。
Vue比React出来的要晚一些，顺应了前端组件化的大潮，并且个人觉得借鉴了部分React的思想来实现其组件化思想。React主要用来实现一些大型的应用或者跨终端的应用，而vue相应的比较简单。如果使用模板编写组件，使用vue是比较好的。对于两个框架，各有各的优势，不过个人认为react的应用稍微广泛点，而且用react Native开发APP是非常方便的。

#vue的如何实现双向绑定的 ？
  核心原理：Object.defineProperty();
  defineProperty利用属性描述符get和set实现对象的观察者模式和订阅者模式，进而实现双向数据绑定，是vue的核心原理

  双向绑定实现：
  <body>
      <div id="app">
          <input type="text" id="txt">
          <p id="show-txt"></p>
      </div>
      <script>
          var obj = {}
          Object.defineProperty(obj, 'txt', {
              get: function () {
                  return obj
              },
              set: function (newValue) {
                  document.getElementById('txt').value = newValue
                  document.getElementById('show-txt').innerHTML = newValue
              }
          })
          document.addEventListener('keyup', function (e) {
              obj.txt = e.target.value
          })
      </script>
  </body>

#观察者模式实现?

#react virsualDOM 是什么? 如何实现? 说一下diff算法 ? 
  [React核心原理]是虚拟DOM和diff（差异化）算法，基于React进行开发时所有的DOM构造都是通过虚拟DOM进行，
                每当数据变化时，React都会重新构建整个DOM树，然后React将当前整个DOM树和上一次的DOM树进行对比，得到DOM结构的区别，然后仅仅将需要变化的部分进行实际的浏览器DOM更新。
  [虚拟DOM]：可以看作是一个使用javascript模拟了DOM结构的树形结构，这个树结构包含整个DOM结构的信息,不论是标签名称还是标签的属性或标签的子集.
            之前使用原生js或者jquery写页面的时候会发现操作DOM是一件非常麻烦的一件事情，往往是DOM标签和js逻辑同时写在js文件里，数据交互时不时还要写很多的input隐藏域，如果没有好的代码规范的话会显得代码非常冗余混乱，耦合性高并且难以维护。
            另外一方面在浏览器里一遍又一遍的渲染DOM是非常非常消耗性能的，常常会出现页面卡死的情况；所以尽量减少对DOM的操作成为了优化前端性能的必要手段，vdom就是将DOM的对比放在了js层，通过对比不同之处来选择新渲染DOM节点，从而提高渲染效率。
  [Diff算法]：react在实现这种算法前做了几个假设，
            一是Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计，
            二是拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构。
            三是对于同一层级的一组子节点，它们可以通过唯一 id 进行区分。
              在进行树结构比较时，React仅仅是一层一层的访问树结构，React比较两棵元素树的过程是同步的，当React比较到元素树中同一位置的元素节点时，如果前后元素的类型不同时,不论该元素是组件类型还是DOM类型的，那么以这个节点(React元素)为子树的所有节点都会被销毁并重新构建。不会再继续比较，性能提高很多。


#react从创建的销毁都会经历哪些生命周期（react15和react16+有较大变化，我更希望听到后者）
[生命周期]getDefaultProps/getInitialState/componentWillMount/componentDidMount/componentWillReceiveProps/shouldComponentUpdate/componentWillUpdate/componentDidUpdate
[react16新特性]react16采用了核心算法react fiber, react 是对核心算法的一次重新实现
  新周期：getDerivedStateFromProps 替换 componentWillReceiveProps，禁止了组件去访问 this.props，强制让开发者去比较 nextProps 与 prevState 中的值，以确保当开发者用到 getDerivedStateFromProps 这个生命周期函数时，就是在根据当前的 props 来更新组件的 state，而不是去做其他一些让组件自身状态变得更加不可预测的事情
        getSnapshotBeforeUpdate 替换 componentWillUpdate，getSnapshotBeforeUpdate会在最终的 render 之前被调用，也就是说在 getSnapshotBeforeUpdate 中读取到的 DOM 元素状态是可以保证与 componentDidUpdate 中一致的
  1⃣️使用Error Boundary处理错误组件
  2⃣️render方法新增返回类型
  3⃣️使用createPortal将组件渲染到当前组件树之外
  4⃣️支持自定义DOM属性
  5⃣️setState传入null时不会再触发更新
  6⃣️更好的服务器端渲染
  7⃣️新的打包策略


#http报文头部有哪些字段? 有什么意义 ? 
请求行（包含请求的方法、URL地址和协议版本）
请求头（Request Header）
  [Accept]指定客户端接收哪些类型的消息 
  [Accept-Language]指定接收的语言
  [Accept-Encoding]指定接收的编码格式
  [Accept-Charset]客户端可以处理的字符集类型
  [connection]指定为keep-alive
  [Content-Type]指示资源的MIME类型 media type
  [Authorizaion]请求消息头含有服务器用于验证用户代理身份的凭证
  [Cookie]是一个请求首部，其中含有先前由服务器通过 Set-Cookie  首部投放并存储到客户端的 HTTP cookies
请求正文
  客户端需要向服务器端传输数据，传输的数据存放在请求正文中



#移动端高清方案如何解决 ?
  1⃣️设置理想窗口viewport,
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  2⃣️rem适配方案
    <script>
      /* 设计稿是750,采用1：100的比例,用1rem表示100px,font-size为100 * (clientWidth / 750) */
      (function(doc, win) {
        var docEl = doc.documentElement,
          resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
          recalc = function() {
            var clientWidth = docEl.clientWidth;
            if (!clientWidth) return;
            docEl.style.fontSize = 100 * (clientWidth / 750) + 'px';
          };
        if (!doc.addEventListener) return;
        win.addEventListener(resizeEvt, recalc, false);
        doc.addEventListener('DOMContentLoaded', recalc, false);
      })(document, window); 
    </script>
  3⃣️用密集的媒体查询设置font-size
    <style>
      /* 设计稿是750,采用1：100的比例,用1rem表示100px,100*(100/750)=13.333 以min-width: 750px时font-size: 100px为基准，
      min-width每缩小100px,font-size就缩小13.3333px，如需更密集的媒体查询可以按照这个对照关系设置。*/
      @media screen and (min-width: 320px) {
          html {
              font-size: 42.6667px;
          }
      }
      @media screen and (min-width: 375px) {
          html {
              font-size: 50px;
          }
      }
      @media screen and (min-width: 425px) {
          html {
              font-size: 56.6667px;
          }
      }
      @media screen and (min-width: 768px) {
          html {
              font-size: 102.4px;
          }
      }
    </style>
  4⃣️用单位vw设置font-size, 1vw等于屏幕可视区宽度(的可视区域的百分之一。
    <style>
      /* 设计稿是750,采用1：100的比例,用1rem表示100px,font-size为100*(100vw/750) */
      html {
          font-size: 13.3334vw;
      }
    </style>
  5⃣️图片模糊问题，用多倍图片(@2x)
  6⃣️1px细线问题，
    [伪元素+transform]： 构建1个伪元素, border为1px, 再以transform缩放到50%。
    <style>
      /* 设计稿是750,采用1：100的比例,font-size为100*(100vw/750) */
      .border-1px {
          position: relative;
      }
      @media screen and (-webkit-min-device-pixel-ratio: 2) {
        .border-1px:before {
            content: " ";
            position: absolute;
            left: 0;
            top: 0;
            width: 100%;
            height: 1px;
            border-top: 1px solid #D9D9D9;
            color: #D9D9D9;
            -webkit-transform-origin: 0 0;
            transform-origin: 0 0;
            -webkit-transform: scaleY(0.5);
            transform: scaleY(0.5);
        }
      }
    </style>

#简述从网页输入url到网页展示的过程发生了哪些事情 ? （参考：https://www.jianshu.com/p/23b388f8e5aa）
  TCP传输层 IP网络层 HTTP应用层
1⃣️DNS域名解析[查找域名对应的ip地址]
  DNS(域名系统)，域名和ip地址相互映射的分布式数据库。简单理解就是域名与ip地址的对照表
  *dns负载均衡，在dns服务器中为同一个主机分配多个ip地址（也就是多台服务器），当进行dns解析时，通过算法根据主机的负载情况返回ip地址
  [优化]减少DNS解析的时间，适当减少要解析的域名个数（可以将页面资源发布到2-4个域名上）
2⃣️浏览器向web服务器发送http请求
  [拿到域名对应的ip地址后，浏览器会以一个随机的端口向服务器80/8080端口发起TCP连接请求。连接请求到达服务器端后，通过网卡，进入到内核的TCP/IP协议栈，还有可能要进入防火墙，进入web程序，最终建立TCP/IP链接]
  tcp三次握手，四次挥手
  [优化]将数据放到接近客户端的地方，可以减少网络往返时间
3⃣️浏览器永久重定向响应。 [重定向]将我输入的网址引向另一个网址
  [优化]如果服务器返回了跳转重定向（非缓存重定向），那么浏览器端就会向新的URL地址重新走一遍DNS解析和建立连接，所以应该避免不必要的重定向。
4⃣️服务器处理请求
  后端服务器从固定的端口接收到TCP报文后，开始对TCP报文进行处理，对HTTP协议进行解析，然后将相应的数据封装为HTTP Request对象，供上层使用
  [优化]复用：针对HTTP1.1的最好方法是启用长连接，同一客户端socket（浏览器可能会开多个端口并行请求）针对同一socket(域名+端口)后续请求都会复用一个TCP连接进行传输，直到关闭这个TCP连接。
5⃣️服务器返回HTTP响应 
  1XX，信息状态码，表示服务器已接收了客户端请求，客户端可以继续发送请求
  2XX，成功状态码，表示服务器已成功接收到请求并处理
    200客户端请求成功
    204成功，但不返回任何实体的主体部分
  3XX，重定向状态码，表示服务器要求客户端重定向
    301永久重定向，也就是原网址资源不可用了，搜索引擎在抓取新内容的同时，也会将旧网址重定向到新网址
    302暂时重定向，旧地址A的资源还在（仍然可以访问），这个重定向只是暂时的从地址A跳转到地址B，搜索引擎在抓取新内容而保存旧的网址
    303请求的资源存在着另一个URI，客户端应使用GET方法定向获取请求的资源
    304服务器内容没有更新，可以直接读取浏览器缓存
    307临时重定向
  4XX，客户端错误状态码
    400客户端有语法错误，不能被服务端所解析
    401请求未经授权
    403服务器收到请求，但拒绝提供服务
    404请求资源不存在
  5XX，服务器错误状态码
    500服务器发生不可预期的错误
    503 服务器当前不能够处理客户端的请求，在一段时间之后，服务器可能会恢复正常
5⃣️浏览器显示页面信息
  [重绘]当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color
  [回流]当render tree中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等改变而需要重新构建
  回流必将引起重绘，而重绘不一定会引起回流
  [优化] 减少和合并不必要的dom相关操作，如使用DocumentFragment、修改classname而不是各项style等，减少对重绘和重排的触发
        减少首次下载的文件数量大小，使用图片懒加载，js的按需加载等方式，也可以节省用户流量，甚至使用storage存储进行js、css文件的缓存
        使用PWA，让用户在没有得到数据时也能看到页面。
        对页面某些ajax请求数据进行storage存储。
        加载进度、骨架图、占位图等类似让用户感觉好一点的措施。
        及时更新升级服务器，优化措施依赖于服务器支持。

#PWA
渐进式网络应用，是一种可以提供类似于原生应用程序(native app)体验的网络应用程序(web app)。
PWA 可以用来做很多事。其中最重要的是，在离线(offline)时应用程序能够继续运行功能。这是通过使用名为 Service Workers 的网络技术来实现的。

#浏览器是如何工作的（http://www.cnblogs.com/mirror/archive/2013/04/05/3000517.html）


#SSR 和 客户端渲染有什么区别 ? 
[ssr]服务端渲染，用户请求服务器，服务器直接生成html内容并返回给浏览器。页面的内容是由 Server 端生成的。也就是说，你点击一下，他就会刷新一个，然后从后台请求新的页面数据。
  好处：前端耗时少（前端只负责将html进行展示），利于SEO
  坏处：网络传输数据量大，占用（部分、少部分）服务器运算资源，response 出的数据量会（稍）大点，模板改了前端的交互和样式什么的一样得跟着联动修改
[客户端渲染]前端渲染真正意义上的实现了前后端分离，前端只专注于UI的开发，后端只专注于逻辑的开发，前后端交互只通过约定好的API来交互，后端提供json数据，前端循环json生成DOM插入到页面中去。
  好处：网络传输数据量小（减少了服务器压力）
  坏处：前端耗时较多，不利于SEO
[优劣]
+ 客户端渲染不利于 SEO 搜索引擎优化
+ 服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的
+ 所以你会发现真正的网站既不是纯异步也不是纯服务端渲染出来的
+ 而是两者结合来做的
+ 例如京东的商品列表就采用的是服务端渲染，目的了为了 SEO 搜索引擎优化
+ 而它的商品评论列表为了用户体验，而且也不需要 SEO 优化，所以采用是客户端渲染

#浏览器事件有哪些过程? 为什么一般在冒泡阶段, 而不是在捕获阶段注册监听? addEventListener 参数分别是什么 ? 
js中时间执行的整个过程称之为事件流，分为三个阶段：事件捕获阶段，事件目标处理函数、事件冒泡

当某歌元素触发某个事件（如：click），顶级对象document发出一个事件流，顺着dom的树节点向触发它的目标节点流去，直到达到目标元素，这个层层递进，向下找目标的过程为[事件的捕获阶段]，此过程与事件相应的函数是不会触发的。到达目标函数，便会执行绑定在此元素上的，与事件相应的函数，即[事件目标处理函数阶段]。最后，从目标元素起，再依次往顶层元素对象传递，途中如果有节点绑定了同名事件，这些事件所对应的函数，在此过程中便称之为[事件冒泡]。

考虑浏览器的兼容性，低版本IE（IE8及以下版本）不支持捕获阶段

addEventListener可接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值
element.addEventListener(event,function,useCapture) //useCapture为true时表示在捕获阶段执行，为false时表示在冒泡阶段执行

#面向对象如何实现? 需要复用的变量 怎么处理 ? 
[面向对象的特点]
  [封装]对于一些功能相同或者相似的代码，我们可以放到一个函数中去，多次用到此功能时，我们只需要调用即可，无需多次重写。
        单例模式，工厂模式，构造函数模式，原型模式
  [继承]子类可以继承父类的属性和方法
  [多肽] 重载：严格意义上说js中没有重载的功能，不过我们可以通过判断函数的参数的不同来实现不同的功能来模拟重载。
        重写：子类可以改写父类的属性和方法

#移动端300ms延时的原因? 如何处理?
300 毫秒延迟的主要原因是解决双击缩放(double tap to zoom)。双击缩放，顾名思义，即用手指在屏幕上快速点击两次，iOS 自带的 Safari 浏览器会将网页缩放至原始比例。 那么这和 300 毫秒延迟有什么联系呢？ 假定这么一个场景。用户在 iOS Safari 里边点击了一个链接。由于用户可以进行双击缩放或者双击滚动的操作，当用户一次点击屏幕之后，浏览器并不能立刻判断用户是确实要打开这个链接，还是想要进行双击操作。因此，iOS Safari 就等待 300 毫秒，以判断用户是否再次点击了屏幕。 鉴于iPhone的成功，其他移动浏览器都复制了 iPhone Safari 浏览器的多数约定，包括双击缩放，几乎现在所有的移动端浏览器都有这个功能。
[解决]1⃣️添加viewpoint meta标签
     2⃣引入fastclick.js
     <script>
      if('addEventListener' in document) {
        document.addEventListener('DOMContentLoaded', function() {
          FastClick.attach(document.body);
        }, false);
      }
     </script> ️

#移动端如何实现下拉到底部 跟随移动 结束后回弹的动画? 

#移动端如何优化首页白屏时间过长 ?

#ES6 generator函数简述

#数组去重实现? 

#js浮点数运算不精确 如何解决?

#工作中最得意和出色的点, 头疼的点, 问题如何解决的

#简述公司node架构中容灾的实现 ?
#工作中最出色的点, 和你最头疼的问题 如何解决的 ?

#平时如何学习, 最近接触了解了哪些新的知识 ? 


