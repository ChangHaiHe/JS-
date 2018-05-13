## JS性能优化

### 一
代码层面的优化手段

  变量提升
```javascript
    for(var i=0,l=arr.length; i<l; i++){
      ...
    }
    // 最好可以
    var i=0, l = arr.length;
    for(; i<l; i++){
        ...
    }
```
  循环
    for循环的速度要比forEach快那么一点
  直接访问arguments
    任何函数在执行时都会创建arguments类数组，这个是无法避免的，对arguments的转化往往会增大浏览器的负担

  数组的操作
    直接访问数组的index往往比做 pop 或者 shift 要快.
    直接通过array[index]=val添加数组元素比push，更有效.
    很多时候，如果可以直接改变原数组就能得到需要的结果，就不要新建一个数组然后去push结果值

  加速Object的访问速度
    通过 Object.freeze(obj), 可以对obj的访问进行加速, 这个优化仅限于Chrome

  优化执行顺序与代码体积
    避免http请求，直接将js embed在页面中输出
    使用 async 对加载进行优化 (推荐做法)
    通过Webpack-Bundle-Analyzer分析依赖的必要性
    拆块引入
        一个库，一个插件，你可能只需要用到其中的某个部分，而不是整个包，那么拆块引入可以很大程度上减少代码体积
    深度优化打包速度
        通过cache 和 HappyPack的手段可充分利用cpu资源，加快打包速度。


### 二
  ##### 谨慎使用闭包
    闭包会造成更多的内存开销，同时IE下还会造成内存泄露。

  ##### 缓存对象成员(减少使用全局变量)
    在同一个函数中，如果存在多次读取同一个对象成员，可以在局部函数中保存对象，减少查找。
  ```javascript
    function getWindowWH() {
      var elBody = document.getElementsByTagName('body')[0];
        return {
          width: elBody.offsetWidth,
          height: elBody.offsetHeight
      }
    }
  ```

  ##### 减少DOM 操作
  ```javascript
    function innerHTMLLoop() {
      var content = '';
      for (var count = 0; count < 10000; count++){
          content += 'a';
      }
      document.getElementById("idName").innerHTML += content;     
    }

  ```
  ##### 重绘(repaints)与重排(reflows) -> 使用文档碎片
    当页面布局和几何属性改变时就需要＂重排＂
    避免在修改样式的过程中使用 offsetTop, scrollTop, clientTop, getComputedStyle() 这些属性, 它们都会刷新渲染队列
    > 最小化重绘和重排, 尽量一次处理
      使元素脱离文档流(隐藏元素)
      使用 documentFragment
      将原始元素拷贝到一个脱离文档的节点中, 修改副本, 完成后再替换原始元素)

  ##### 避免使用构造器 eval() Function()

  ##### 事件委托
    当有很多元素需要绑定事件的时候，我们一个一个的去绑定事件是有代价的的，元素越多应用程序越慢。事件绑定不但占用了处理时间，并且追踪事件需要更多的内存，有时候很多元素是不需要，或者是用户不会点击的，所以我们需要使用事件委托来解决没有必要的资源消耗。
    例子： 我们需监听li的click事件，通过冒泡事件来获取点击的对象。

    ```javascript
      <ul>
          <li index='1'>1</li>
          <li index='2'>2</li>
          <li index='3'>3</li>
      </ul>

      var ul = document.getElementById('ul');
      ul.addEventListener('click', function(e) {
        var now_index = e.target.getAttribute('index');
        ...
      })

    ```

  ### 尾调用优化
    es6


## Others

  webpack与requirejs区别

    1. 
      首先，Webapck的模块加载器在编译阶段就已经将依赖分析好了， 而RequireJS的依赖顺序需要在浏览器端才能得到解析，所以Webpack的调用速度比RequireJS要快，因为依赖都是按照索引存储在数组中的，全闭包贮存，通过id可以直接得到引用

    2.
      使webpackJsonp被修改，被攻击，也不影响现有的页面逻辑，照常执行，但是RequireJS 则在全局暴露了 define, require 等多个API，通过require甚至可以将定义的包导出，查看源码

    3.
      webpack 提供了从打包工具到模块化的一致性体验，而requirejs 打包则需要依赖rjs，并且不太容易和gulp等自动化工具进行整合

    4.
      由于加载代码布局和全闭包贮存，webpack的打包在UglifyJS的压缩下变得更加彻底, 打包的体积更小

    5.  
      loader 生态的问题

    6. xxx




##相关文档
参考:[{JS} Javascript 性能优化](https://yj1028.me/article/%7BJS%7D%20Javascript%20%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.html?t=1508304605465)
[JS 一些优化性能的小细节](https://juejin.im/post/58fdcdc31b69e60058a29444)
[Yahoo团队经验：网站性能优化的34条黄金法则](http://www.ha97.com/2710.html)




## 前端性能优化

1. 尽量减少HTTP请求次数
  CSS Sprites
  合并文件是通过把所有的脚本放到一个文件中来减少HTTP请求的方法(css合并)
2. 减少DNS查找次数 DNS
3. 避免跳转
4. 可缓存的AJAX、优化AJAX
  使用GET来完成AJAX请求
5. 推迟加载内容 （把样式表置于顶部、JS脚本放在页面的最后）
5. 预加载 
6. 减少DOM元素数量
7. 不要出现404错误
8. 使用内容分发网络 CDN
9. 减小Cookie体积
  * 去除不必要的coockie
  * 使coockie体积尽量小以减少对用户响应的影响
  * 注意在适应级别的域名上设置coockie以便使子域名不受影响
  * 设置合理的过期时间。较早地Expire时间和不要过早去清除coockie，都会改善用户的响应时间。





