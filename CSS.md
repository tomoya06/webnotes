# CSS

## 盒子模型

*  border-box / content-box。可以用box-sizing切换

## 选择符

1. id选择器（ # myid）
2. 类选择器（.myclassname）
3. 标签选择器（div, h1, p）
4. 相邻选择器（h1 + p）
5. 子选择器（ul > li）
6. 后代选择器（li a）
7. 通配符选择器（ * ）
8. 属性选择器（a[rel = "external"]）
9. 伪类选择器（ a:hover, li:nth-child，　:not(.class) ）

### 优先级

* 同权重: 内联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）。 
* !important >  id > class > tag
* important 比 内联优先级高

## flex

> 详见阮一峰的介绍：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

* 对容器：

  * flex-direction: row | row-reverse | column | column-reverse 项目主轴排列方向 
  * flex-wrap: nowrap | wrap | wrap-reverse 项目换不换行
  * flex-flow: < flex-direction> || < flex-wrap> 以上两项的合并
  * justify-content: flex-start | flex-end | center | space-between | space-around  项目在主轴上的对齐方式
  * align-items: flex-start | flex-end | center | baseline | stretch 项目在垂直主轴方向上的对齐方式
  * align-content: flex-start | flex-end | center | space-between | space-aroud | stretch 项目有换行时多根主轴之间的对齐方式

* 对项目：

  * order: < integer>
    数值越小排列越靠前。默认为0
  * flex-grow: < integer>
    定义项目的放大比例。默认为0。即如果存在剩余空间，也不放大。
  * flex-shrink: < integer>
    定义项目的缩小比例。默认为1。即如果空间不足，该项目将缩小。
  * flex-basis: < length> | auto
    定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
  * flex: none | [< flex-grow> < flex-shrink> < flex-basis>]
    属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
  * align-self：auto | flex-start | flex-end | center | baseline | stretch
    允许单个项目有不同的align-items属性。

## display 属性

* block         | 块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
* none          | 缺省值。象行内元素类型一样显示。
* inline        | 行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
* inline-block  | 默认宽度为内容宽度，可以设置宽高，同行显示。
* list-item   	| 象块类型元素一样显示，并添加样式列表标记。
* table       	| 此元素会作为块级表格来显示。
* inherit     	| 规定应该从父元素继承 display 属性的值。

## position

* absolute | 生成绝对定位的元素，相对于值不为 static的第一个父元素进行定位。
* fixed （老IE不支持）| 生成绝对定位的元素，相对于浏览器窗口进行定位。
* relative | 生成相对定位的元素，相对于其正常位置进行定位。
* static | 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right z-index 声明）。
* inherit |	规定从父元素继承 position 属性的值。

## 常见兼容性/布局问题

* 【兼容】png24位的图片在iE6浏览器上出现背景，
  > 解决方案是做成PNG8.

* 【兼容】浏览器默认的margin和padding不同。
  > 解决方案是加一个全局的*{margin:0;padding:0;}来统一。

* 【兼容】IE下,可以使用获取常规属性的方法来获取自定义属性,也可以使用getAttribute()获取自定义属性;Firefox下,只能使用getAttribute()获取自定义属性。
  > 解决方法:统一通过getAttribute()获取自定义属性。

* 【兼容】Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示
  > 可通过加入 CSS 属性 -webkit-text-size-adjust: none; 解决。

* 【布局】超链接访问过后hover样式就不出现了 被点击访问过的超链接样式不在具有hover和active了。
  > 解决方法是改变CSS属性的排列顺序:L-V-H-A :  a:link {} a:visited {} a:hover {} a:active {}

* 【布局】li与li之间有看不见的空白间隔/display:inline-block中的元素之间存在间隙
  > 行框的排列会受到中间空白（回车\空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占据空间，所以会有间隔。
  > 解决：font-size=0 / 不换行 / 使用注释标签 / 把右书名号放下一行 / margin负值

* 【兼容】【布局】visibility: collapse的作用
  > 对于普通元素visibility:collapse;会将元素完全隐藏,不占据页面布局空间,与display:none;表现相同。如果目标元素为table,visibility:collapse;将table隐藏，但是会占据页面布局空间. 仅在Firefox下起作用,IE会显示元素,Chrome会将元素隐藏,但是占据空间。

* 【布局】position / display / margin / float 特性叠加影响
  > 详细参见：https://blog.csdn.net/qq_28506819/article/details/72864581 
  > 
  > 另外这篇文章还提到下面containing block以及BFC（blocking formatting context）的问题：https://blog.csdn.net/qq_28506819/article/details/72866748

* 【布局】外边距合并
  > 也是跟上一条的BFC相关，可以参考上条连接。简单来说就是对相邻/包含的块元素，上下margin会合并，选取一个最大值。

* 【布局】浮动元素引起的问题和解决方法：
  * 父元素的高度无法被撑开，影响与父元素同级的元素
  * 与浮动元素同级的非浮动元素会跟随其后
  * 若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构 
  > 解决方法见 https://blog.csdn.net/taotao_web/article/details/78082522。清除原理在于对父元素添加伪元素::after，对其设置空内容，然后clear:both
  * 设置元素浮动之后，该元素的display值变成block

* 【布局】媒体查询/响应式布局
  * 原理是根据页面宽度来调整布局。此概念於2010年5月由國外著名網頁設計師Ethan Marcotte所提出。
````
@media print {
  body { font-size: 10pt; }
}

@media screen {
  body { font-size: 13px; }
}

@media screen, print {
  body { line-height: 1.2; }
}

@media only screen and (max-width: 500px) {
  .gridmenu {
    width:100%;
  }

  .gridmain {
    width:100%;
  }

  .gridright {
    width:100%;
  }
}
````

* 【布局】字体中使用偶数字号多于奇数字号的原因
> 其实奇数字号也不是不可以用。详见https://blog.csdn.net/jian_xi/article/details/79346477

* 【优化】CSS提高性能方法
  * 关键选择器（key selector）。选择器的最后面的部分为关键选择器（即用来匹配目标元素的部分）；
  * 如果规则拥有 ID 选择器作为其关键选择器，则不要为规则增加标签。过滤掉无关的规则（这样样式系统就不会浪费时间去匹配它们了）；<==这条注意一下
  * 提取项目的通用公有样式，增强可复用性，按模块编写组件；增强项目的协同开发性、可维护性和可扩展性;
  * 使用预处理工具或构建工具（gulp对css进行语法检查、自动补前缀、打包压缩、自动优雅降级）；
  > 更多前端优化方法参见 https://github.com/zwwill/blog/issues/1

* 【布局】【震惊】css 元素的竖向百分比设定是相对于容器的高度吗？
  > 如果是height的话，是相对于容器高度，如果是padding-height,margin-height则是相对于容器的宽度。

* 【布局】全屏滚动的原理和实现
  > TODO
  * https://blog.csdn.net/Rita_jing/article/details/78236768
  * https://blog.csdn.net/yinkaihui/article/details/51169916

* 【布局】视差滚动
  > TODO
  * https://www.jianshu.com/p/e24951a65fd7
  * http://www.zhangxinxu.com/wordpress/2015/03/css-only-parallax-effect/

* 【布局】line-height
  > 用于设置多行元素的空间量，比如文本。对于块级元素，它指定元素行盒（line boxes）的最小高度。对于非替代的inline元素，它用于计算行盒（line box）的高度。

* 【布局】px / em / % / (CSS3分界线) / rem / vw / vh / vm 的区别
  > 参见这篇讲的很清楚了 https://www.jianshu.com/p/82f02af17e78

* 【布局】解决页面使用overflow: scroll在iOS上滑动卡顿的问题
  > 加入-webkit-overflow-scrolling: touch 启动硬件加速

* 【布局】不同照片格式的区别
  > 详见http://www.cnblogs.com/diligenceday/p/4472035.html

* 【优化】style标签写在body前与后有什么区别
  * 写在head标签中利于浏览器逐步渲染（resources downloading->CSSOM+DOM->RenderTree(composite)->Layout->paint）。具体渲染过程请参考https://blog.csdn.net/wozaixiaoximen/article/details/50640954##1
  * 写在body标签后由于浏览器以逐行方式对html文档进行解析，当解析到写在尾部的样式表（外联或写在style标签）会导致浏览器停止之前的渲染，等待加载且解析样式表完成之后重新渲染，在windows的IE下可能会出现FOUC现象（即样式失效导致的页面闪烁问题）
  
* 【布局】换行问题
  * word-break：word-break will create a break at the exact place where text would otherwise overflow its container (even if putting an entire word on its own line would negate the need for a break).
    * normal / break-word / break-all / keep-all
  * overflow-wrap：overflow-wrap will only create a break if an entire word cannot be placed on its own line without overflowing.
    * normal / break-word
  * <pre>换行方法
    * word-break: break-word // 非FF
    * white-space: pre-wrap  // FF

## Containing block 包含块

> 在 CSS2.1 中，很多框的定位和尺寸的计算，都取决于一个矩形的边界，这个矩形，被称作是包含块( containing block )。 一般来说，(元素)生成的框会扮演它子孙元素包含块的角色；我们称之为：一个(元素的)框为它的子孙节点建造了包含块。包含块是一个相对的概念。
> 
> 每个框关于它的包含块都有一个位置，但是它不会被包含块限制；它可以溢出(包含块)。包含块上可以通过设置 'overflow' 特性达到处理溢出的子孙元素的目的。

### 判定

* position: absolute：先找到其祖先元素中最近的 position 值不为 static 的元素，然后再判断：

  1. 若此元素为 inline 元素，则 containing block 为能够包含这个元素生成的第一个和最后一个 inline box 的 padding box (除 margin, border 外的区域) 的最小矩形；

  2. 否则,则由这个祖先元素的 padding box 构成。

  3. 如果都找不到，则为 initial containing block，即html。

* position: static/relative：简单说就是它的父元素的内容框（即去掉padding的部分）

* position: fixed：一律为initial containing block，即根元素(html)

## css3新特性

### css3新增伪类(:)和伪元素(::)/别的一些有趣的伪类

* p:first-of-type
* p:last-of-type
* p:only-of-type
* p:only-child
* p:nth-child(2)
* p:nth-last-child(n)
* p:nth-of-type(n)
* p:nth-last-of-type(n)
* p:not(n)
* :enabled :disabled 控制表单控件的禁用状态。
* :checked 单选框或复选框被选中。
* p::selection 被聚焦的元素
* ::after 在元素之前添加内容,也可以用来做清除浮动。
* ::before 在元素之后添加内容
* ---
* :-webkit-autofill 被浏览器自动补全的伪类（chrome/safari支持）


> 注意：对于css3之前已经有的伪元素，单冒号和双冒号的写法作用是一样的。不过也就::selection是css3新增的。双冒号是css3引入的。

### 字体相关

* @font-face:

````
@font-face { 
font-family: BorderWeb; 
src:url(BORDERW0.eot); 
} 
.border { FONT-SIZE: 35px; COLOR: black; FONT-FAMILY: "BorderWeb" } 
````

* word-break: break-word | break-all | keep-all | normal

* text-overflow: clip | ellipsis | "-" < or other string > 裁剪 / 用省略标记断尾 / 用特定字符断尾

* text-decoration: 包括

  * text-fill-color
  * text-stroke-color
  * text-stroke-width

* multi-column layout: 包括
  * column-count
  * column-rule
  * column-gap

### 装饰相关

* color/border支持透明度。比如rgba/hsla

* gradient 渐变。有线性渐变和radial径向渐变

* shadow阴影 text-shadow/box-shadow
* reflect反射 仅有webkit-box-reflect

* background 背景

  * background-clip: border-box/no-clip | padding-box | content-box
  * background-origin : border-box | padding-box | content-box
  * background-size: contain | cover | 100px 100px | 50% 100%
  * background-break: continuous | bounding-box | each-box

* 盒子模型

````
//html
<div class="boxcontainer"> 
    <div class="item"> 
        1 
    </div> 
    <div class="item"> 
        2 
    </div> 
    <div class="item flex2"> 
        3 
    </div> 
    <div class="item flex"> 
        4 
    </div> 
</div> 

//css
.boxcontainer { 
    width: 1000px; 
    display: -webkit-box; 
    display: -moz-box; 
    -webkit-box-orient: horizontal; 
    -moz-box-orient: horizontal; 
} 

.item { 
    background: #357c96; 
    font-weight: bold; 
    margin: 2px; 
    padding: 20px; 
    color: #fff; 
    font-family: Arial, sans-serif; 
}
.flex { 
    -webkit-box-flex: 1; 
    -moz-box-flex: 1; 
} 

.flex2 { 
    -webkit-box-flex: 2; 
    -moz-box-flex: 2; 
}
````
    
* transition / animation

* transform

````
@-webkit-keyframes anim1 { 
    0% { 
        Opacity: 0; 
        Font-size: 12px; 
    } 
    100% { 
        Opacity: 1; 
        Font-size: 24px; 
    } 
} 
.anim1Div { 
    -webkit-animation-name: anim1 ; 
    -webkit-animation-duration: 1.5s; 
    -webkit-animation-iteration-count: 4; 
    -webkit-animation-direction: alternate; 
    -webkit-animation-timing-function: ease-in-out; 
}
````
