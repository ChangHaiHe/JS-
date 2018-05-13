##CSS3
	不定宽高垂直居中
	方案一： 绝对定位
		.wraper{
			position:absolute;
			top:50%;
			left:50%;
			-webkit-transform:translate(-50%,-50%);
		}
	方案二：弹性盒模型
		.wraper{
			display:-webkit-flex;
			justify-content:center; //水平居中
			align-items:center; //垂直居中
		}
	方案三：
		postion: absolute;
        top: 50%;
        left: 50%;
        margin-top: -(0.5 * 自身高度)px;
        margin-left: -(0.5 * 自身宽度)px;

  方案四:
    用表格




	
	超过字数...
		.fn-text-overflow {
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
		}

	calc();

	阴影
	第几个儿子
	兄弟选择器
	三角


--------


## CSS3的一些重点

兄弟子...选择器

背景和边框

弹性盒模型

栅栏布局

动画

offsettop
函数的递归


### 选择器

`兄弟选择器`
div+p 选择所有紧接着<div>元素之后的<p>元素

`属性选择器`
* [attribute] 选择所有带有target属性元素
* [target=-blank] 选择所有使用target="-blank"的元素
* [title~=flower] 选择标题属性包含单词"flower"的所有元素
* [lang|=en] 选择一个lang属性的起始值="EN"的所有元素

`:选择器`
:root	选择文档的根元素
input:focus 选择具有焦点的输入元素
input:checked 选择每个选中的输入元素 (值还有disabled 、enabled)
:not(p) 选择每个并非p元素的元素


p:first-letter 选择每一个`<P>`元素的第一个字母
p:first-line 选择每一个`<P>`元素的第一行
p:first-child 指定只有当`<p>`元素是其父级的第一个子级的样式。
p:before 在每个`<p>`元素之前插入内容
p:after 在每个`<p>`元素之后插入内容
p:lang(it) 	选择p元素之后的每一个ul元素


a[src^="https"] 选择每一个src属性的值以"https"开头的元素
a[src$=".pdf"] 选择每一个src属性的值以".pdf"结尾的元素
a[src*="runoob"] 	选择每一个src属性的值包含子字符串"runoob"的元素


p:first-of-type 选择每个p元素是其父级的第一个p元素
p:last-of-type 选择每个p元素是其父级的最后一个p元素
p:only-of-type 选择每个p元素是其父级的唯一p元素
p:only-child 选择每个p元素是其父级的唯一子元素
p:nth-child(2) 选择每个p元素是其父级的第二个子元素
p:nth-last-child(2) 选择每个p元素的是其父级的第二个子元素，从最后一个子项计数
p:nth-of-type(2) 选择每个p元素是其父级的第二个p元素
p:nth-last-of-type(2) 择每个p元素的是其父级的第二个p元素，从最后一个子项计数
p:last-child 	选择每个p元素是其父级的最后一个子级。


## 单位

em 一般浏览器字体大小默认为16px，则2em == 32px;
vw (viewpoint width)视窗宽度，1vw=视窗宽度的1%
vh
vmin
vmax
rem 根元素（html）的 font-size
% 

### others
in	英寸 (1in = 96px = 2.54cm)


## 边框、圆角、阴影 超出文字...
border-radius: 5px 5px 5px 5px;
  四个值: 第一个值为左上角，第二个值为右上角，第三个值为右下角，第四个值为左下角。

box-shadow: 10px 10px 5px #888888;
  阴影水平偏移值（可取正负值）； 阴影垂直偏移值（可取正负值）；阴影模糊值；阴影颜色

border-image: url(border.png) 30 30 round;

div {
  white-space:nowrap; 
  width:200px; 
  overflow:hidden;
  border:1px solid #000000; 
  text-overflow:ellipsis
}


`box-sizing`属性在一个元素的 width 和 height 中包含 padding(内边距) 和 border(边框)。
  1. 不使用 box-sizing (W3C -> width是内容的宽度，整个盒子div的宽度还要包括div的padding和border的宽度)
    实际宽: width(宽) + `padding(内边距)` + `border(边框)` = 元素实际宽度
    实际高: height(高) + `padding(内边距)` + `border(边框)` = 元素实际高度
    结论: 元素真实展示的高度与宽度会更大(因为元素的`边框`与`内边距`也会计算在 width/height 中)。
  2. 使用 box-sizing: border-box; (IE -> width 是盒子的实际宽度，包括padding和border在内)
    padding(内边距) 和 border(边框) 也包含在 width 和 height 中
    ps: 另外在设置padding的时候实际宽度就不再计算padding和border的宽度

  结论: 
    IE盒模型和W3c盒模型的区别主要是: IE的盒模型宽不包括padding和border, 而W3C的标准盒模型宽包括padding和border

##弹性盒模型

弹性容器(Flex container)和弹性子元素(Flex item)组成。

弹性容器设置 display 属性的值为 flex 或 inline-flex将其定义为弹性容器。

  1. flex-direction 顺序指定了弹性子元素在父容器中的位置。
      row：横向从左到右排列（左对齐），默认的排列方式。
      row-reverse
      column：纵向排列。
      column-reverse

  2. justify-content 应用在弹性容器上，把弹性项沿着弹性容器的主轴线（main axis）对齐
      flex-start: 弹性项目向行头紧挨着填充。这个是默认值
      flex-end：
      center：
      space-between：第1个弹性项的外边距和行的main-start边线对齐，而最后1个弹性项的外边距和行的main-end边线对齐
      space-around：弹性项目平均分布在该行上，两边留有一半的间隔空间(比如是20px,首尾两边和弹性容器之间留有一半的间隔（1/2*20px=10px）。)
      ![效果图展示](http://www.runoob.com/wp-content/uploads/2016/04/2259AD60-BD56-4865-8E35-472CEABF88B2.jpg)

  3. align-items 弹性盒子元素在侧轴（纵轴）方向上的对齐方式。
      flex-start
      flex-end
      center
      baseline
      stretch: 属性值为'auto', 同时会遵照'min/max-width/height'属性的限制。
      
  4. flex-wrap 用于指定弹性盒子的子元素换行方式。
      nowrap: 默认， 弹性容器为单行。该情况下弹性子项可能会溢出容器(不换行)
      wrap: 弹性容器为多行,弹性子项溢出的部分会被放置到新行
      wrap-reverse

  5. align-content 用于修改 flex-wrap 属性的行为，类似于 align-items, 但它不是设置弹性子元素的对齐, 设置各个行的对齐。
      stretch: 默认。各行将会伸展以占用剩余的空间。
      flex-start
      flex-end 
      center 
      space-between
      space-around

弹性子元素属性
  排序
  * order: -1、0、1;
      <integer>：用整数值来定义排列顺序，数值小的排在前面。可以为负值。

  对齐
  * 设置"margin"值为"auto"值，自动获取弹性容器中剩余的空间， 可以使弹性子元素在弹性容器的两上轴方向都完全集中。
      ps: 在第一个弹性子元素上设置了 margin-right: auto; 。 它将剩余的空间放置在元素的右侧：

  完美的居中
  * 设置 margin: auto; 可以使得弹性子元素在两上轴方向上完全居中:
  ```javascript 
    <style>
      .flex-container {
        display: flex;
        width: 400px;
        height: 350px;
        border:1px solid #f00
      }
      .flex-item {
        background-color: #0f9;
        width: 75px;
        height: 75px;
        margin: auto;
      }
    </style>
  
    <div class="flex-container">
      <div class="flex-item">Perfect centering!</div>
    </div>

  ```
  * align-self 于设置弹性`元素自身`在侧轴（纵轴）方向上的对齐方式。
      auto
      flex-start
      flex-end
      center
      baseline
      stretch

  * flex
      auto: 计算值为 1 1 auto
      initial: 计算值为 0 1 auto
      none：计算值为 0 0 auto
      inherit：从父元素继承
      [ flex-grow ]：定义弹性盒子元素的扩展比率。
      [ flex-shrink ]：定义弹性盒子元素的收缩比率。
      [ flex-basis ]：定义弹性盒子元素的默认基准值。


## 其他属性

min-height: 设置元素的最低高度 (css2)
  100px: 元素的最小高度
  100%: 基于包含它的块级对象的百分比最小高度。
  inherit: 从父元素继承 min-height 属性的值。
min-width (css2)

max-height
max-width

overflow-x
overflow-y

calc()
  width: calc(100% - 10px);


## IE条件判断 css Hack

<!--[if <keywords>? IE <version>?]>  // <!--[if gt IE 6]>
  HTML代码块
<![endif]-->

  keywords
    大于: gt（greater than）
    大于或等于: gte（greater than or equal）
    小于: lt（less than）
    小于或等于: lte（less than or equal）
    非指定版本: !

  version
    注意: IE10及以上版本已将条件注释特性移除，使用时需注意。

  [cssHack文档](http://www.css88.com/book/css/hack/selectors.htm)



## 动画
