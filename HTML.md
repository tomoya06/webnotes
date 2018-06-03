
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

* 图像方面 canvas
* 位置方面 Geolocation
* 存储方面 localStorage,sessionStorage
* 多任务方面 webworker
* 持久连接方面websocket
* 离线响应请求方面 service-worker
* 语义化标签 header，nav,artical,section,footer
* 表单控件 calendar,time,color,email,search,date

## 常见问题

* data-xxx 属性的作用是什么？

  * HTML5 data- ：定义和用法
    * data-* 属性用于存储页面或应用程序的私有自定义数据。
    * data-* 属性赋予我们在所有 HTML 元素上嵌入自定义 data 属性的能力。
    * 存储的（自定义）数据能够被页面的 JavaScript 中利用，以创建更好的用户体验（不进行 Ajax 调用或服务器端数据库查询）。
  * data-* 属性包括两部分：
    * 属性名不应该包含任何大写字母，并且在前缀 "data-" 之后必须有至少一个字符
    * 属性值可以是任意字符串

> 注释：用户代理会完全忽略前缀为 "data-" 的自定义属性。这里的data-前缀就被称为data属性，其可以通过脚本进行定义，也可以应用CSS属性选择器进行样式设置。数量不受限制，在控制和渲染数据的时候提供了非常强大的控制。
> 
> 见JavaScript中的dataset操作


````
<div class="oop-data-test" data-obj='{"uid":"007","name":"hacker","age":"unkown","address":"UFO"}'>
</div>

//js

// data-obj='{"uid":"007","name":"hacker","age":"unkown","address":"UFO"}'
// Object (must be , data-obj=`{"key":"value"}`)

let test = document.querySelector('[data-obj*="uid"');​​​​​​​
let data_obj = JSON.parse(test.dataset.obj);​​​​​​​
````

* 标准模式 / 怪异模式
  * 判断方法：

````
alert(window.top.document.compatMode);
// BackCompat 怪异模式
// CSS1Compat 标准模式
````