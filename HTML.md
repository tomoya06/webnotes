
# HTML

## 基本知识点

### DOCTYPE

* < !DOCTYPE> 声明位于HTML文档中的第一行，处于 < html> 标签之前。告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。分严格模式-混杂模式，以doctype中声明的DTD（document type definition）相关。
  * 严格模式/标准模式：就是以W3C标准来解析文档
  * 混杂模式/兼容模式：以浏览器自己的方式来解析文档
* 如何触发两种模式：
  * 触发严格模式：声明正确的DTD即可。
  * 触发混杂模式：不声明DTD，或者在doctype加入XML声明。
* 两种模式的区别：
  * 盒模型不同：严格模式为content-box(width=content-width)，混杂模式为border-box(width=padding+border+content-width)
  * 设置行内元素的高宽：严格模式下不能设置，混杂模式可以设置
  * 设置百分比：严格模式下一个元素的高度由其内容来决定。如果父元素没有设置高度，子元素设置百分比是无效的。
  * 水平居中：严格模式下：使用margin：0 auto；混杂模式下，用text-align

* HTML5 不基于 SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为（让浏览器按照它们应该的方式来运行）；

### meta标签

* 提供有关该页面的员信息。不会被显示，但会被浏览器读取。可用于标识页面描述、关键词、作者、最后更改或其他元信息，会被搜索引擎使用到。
* HTML5 introduced a method to let web designers take control over the viewport (the user's visible area of a web page), through the < meta> tag

* 必须属性：
  * content
* 可选属性：
  * http-equip
    * 包括 **content-type/X-UA-Compatible/refresh**/expires/set-cookie
  * name
    * 包括author/description/keywords/generator/revised/**viewport**...
  * scheme
> 关于属性的更多描述见 http://www.w3school.com.cn/tags/tag_meta.asp

````
<meta name="viewport" content="width=device-width, initial-scale=1.0">
// 页面宽度与屏幕宽度等宽。初始放大倍数为屏幕宽度1倍。
<meta name="viewport" content="maximum-scale=1">
// 防止放大
````

### 标签杂项

#### label
* < label>标签来定义表单控制间的关系,当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。e.g.:这个例子点击label会触发选中对应for的id的元件。

````
<label for='name'>Name</label>
<input type='text' id='name'></text>
````

#### iframe

* iframe会阻塞主页面的Onload事件；
* iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。最好是通过javascript动态给iframe添加src属性值，这样可以绕开以上两个问题。

## HTML5 新特性

### 新元素：

* 表单：
    < input> 加入新类型，如type=email|url|number|range|date|time|week|month|datetime|datetime-local...
    < output>

* 页面元素：
  * < video>
  * < audio>
  * < canvas>

