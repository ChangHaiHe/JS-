##React性能优化

###React生命周期图
![React生命周期图](https://segmentfault.com/img/bVLyCB?w=2803&h=2945)

当 props 和 state 发生变化时，React会根据shouldComponentUpdate方法来决定是否重新渲染整个组件。
  
React的性能优化就是围绕shouldComponentUpdate方法（SCU）来进行的:
  1. 缩短SCU方法的执行时间(或者不执行)。
  2. 没必要的渲染，SCU应该返回false。


##React性能分析工具

* Web通用工具：Chrome DevTools

* React特色工具：Perf
  > Perf 是react官方提供的性能分析工具。Perf最核心的方法莫过于Perf.printWasted(measurements)了，该方法会列出那些没必要的组件渲染。很大程度上，React的性能优化就是干掉这些无谓的渲染。
(PS: TodoItem中的SCU方法，使用的是浅比较，也可以使用PureComponent代替。实际项目中，往往需要使用复杂的深比较，可以考虑使用Immutable.js)



