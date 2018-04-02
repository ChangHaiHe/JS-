## JS性能优化


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


##Others

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






##相关文档
参考:https://yj1028.me/article/%7BJS%7D%20Javascript%20%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.html?t=1508304605465
