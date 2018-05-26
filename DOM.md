# 关于DOM

>  文档对象模型 (DOM) 是HTML和XML文档的编程接口。它提供了对文档的结构化的表述，并定义了一种方式可以使从程序中对该结构进行访问，从而改变文档的结构，样式和内容。DOM 将文档解析为一个由节点和对象（包含属性和方法的对象）组成的结构集合。简言之，它会将web页面和脚本或程序语言连接起来。
> 
> 推荐文章：https://harttle.land/2015/10/01/javascript-dom-api.html

## node和element的区别

> Basically an Element is specified by an HTML Tag whereas a Node is any Object in the DOM, so an Element is a Node but a Node can also include Text Nodes in the form of whitespace, comments, text characters or line breaks.
> 
> 来自回答https://stackoverflow.com/questions/31097016/whats-the-difference-between-nextelementsibling-vs-nextsibling?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa

````
<div id="start"></div>
Me
<p>Hi</p>

console.log(document.getElementById('start').nextSibling); // "\nMe\n"
console.log(document.getElementById('start').nextSibling.nextSibling); // "<p>

console.log(document.getElementById('start').nextElementSibling);// "<p>"
````

## DOM操作

* 获取节点
  * document.documentElement     返回文档的根节点< html> 
  * document.body     < body> 
  * document.activeElement 返回当前文档中被击活的标签节点(ie)
  * event.fromElement        返回鼠标移出的源节点(ie) 
  * event.toElement       返回鼠标移入的源节点(ie) 
  * event.srcElement     返回激活事件的源节点(ie) 
  * event.target         返回激活事件的源节点(firefox) 

* 获取相关节点，例如当前节点为node：
  * 返回父节点：node.parentNode, node.parendElement, 
  * 返回所有子节点：node.childNodes（包含文本节点及标签节点）,node.children 
  * 返回第一个子节点：node.firstChild 
  * 返回最后一个子节点： node.lastChild 
  * 返回同属上一个子节点：node.nextSibling 
  * 返回同属下一个子节点：node.previousSibling 

* 节点操作
  * 添加、删除子元素：ele.appendChild(el); ele.removeChild(el);
  * 替换子元素：ele.replaceChild(el1, el2);
  * 插入子元素：parentElement.insertBefore(newElement, referenceElement);

* 属性操作
  * 获取一个{name, value}的数组：var attrs = el.attributes;
  * 获取、设置属性：var c = el.getAttribute('class'); el.setAttribute('class', 'highlight');
  * 判断、移除属性：el.hasAttribute('class'); el.removeAttribute('class');
  * 是否有属性设置：el.hasAttributes();   

* 查询
  * 返回当前文档中第一个类名为 "myclass" 的元素：var el = document.querySelector(".myclass");
  * 返回一个文档中所有的class为"note"或者 "alert"的div元素：var els = document.querySelectorAll("div.note, div.alert");
  * 获取元素
    * var el = document.getElementById('xxx');
    * var els = document.getElementsByClassName('highlight');
    * var els = document.getElementsByTagName('td');

### differences between methods to access DOM node
> 被腾讯老爹坑过的一道题。
1. querySelector() querySelectorAll(): 返回类型分别是node类型的object 和 非活动 的nodelist
2. getElementById()：返回类型是Element类型的Object
3. getElementsByClassName() getElementByTagName(): 返回类型是 活动 的HTMLCollection
4. getElementsByName(): 返回类型是 活动 的nodelist
5. getElementsByTagNameNS(Namespace, name): 返回类型是 活动 的nodelist。但是没怎么用过。 element.namespace似乎已经过时。

* 注意点
  * nodelist 和 HTMLCollection 都是类似数组，可以直接用list[index]的格式对其操作。
  * HTMLCollection，从定义上是一直活动的。对DOM的更新操作会对其造成影响。
  * nodelist则不一定。
  * 活动和非活动的nodelist在获取之后都可以对原DOM进行数据修改。修改之后该元素的变化也可以读取到。但如果添加/删除新元素则无法察觉。

### dom operation
````
var a = document.getElementById("dom");
      del_space(a); //清理空格
      var b = a.childNodes; //获取a的全部子节点；
      var c = a.parentNode; //获取a的父节点；
      var d = a.nextSibling; //获取a的下一个兄弟节点
      var e = a.previousSibling; //获取a的上一个兄弟节点
      var f = a.firstChild; //获取a的第一个子节点
      var g = a.lastChild; //获取a的最后一个子节点
      
parent(expr) //找父亲节点，可以传入expr进行过滤，比如$("span").parent()或者$("span").parent(".class")

jQuery.parents(expr) //类似于jQuery.parents(expr),但是是查找所有祖先元素，不限于父元素

jQuery.children(expr) //返回所有子节点，这个方法只会返回直接的孩子节点，不会返回所有的子孙节点

jQuery.contents() //返回下面的所有内容，包括节点和文本。这个方法和children()的区别就在于，包括空白文本，也会被作为一个jQuery对象返回，children()则只会返回节点

jQuery.prev() //返回上一个兄弟节点，不是所有的兄弟节点

jQuery.prevAll() //返回所有之前的兄弟节点

jQuery.next() //返回下一个兄弟节点，不是所有的兄弟节点

jQuery.nextAll() //返回所有之后的兄弟节点

jQuery.siblings() //返回兄弟姐妹节点，不分前后

jQuery.find(expr)  //跟jQuery.filter(expr)完全不一样。jQuery.filter()是从初始的jQuery对象集合中筛选出一部分，而jQuery.fi
````
### more operation
````
// get reference to DOM element
var el = document.querySelector(".main-content");
 
//----Adding a class------
 
/* jQuery */
$(el).addClass("someClass");
 
/* native equivalent */
el.classList.add("someClass");
 
//----Removing a class-----
 
/* jQuery */
$(el).removeClass("someClass");
 
/* native equivalent */
el.classList.remove("someClass");
 
//----Does it have class---
 
/* jQuery */
if($(el).hasClass("someClass"))
 
/* native equivalent */
if(el.classList.contains("someClass"))
````


## 一些题目

> innerHTML vs outerHTML

DOM元素的innerHTML, outerHTML, innerText, outerText属性的区别也经常被面试官问到， 比如对于这样一个HTML元素：< div>content< br/></ div>。

  * innerHTML：内部HTML，content< br/>；
  * outerHTML：外部HTML，< div>content< br/></ div>；
  * innerText：内部文本，content ；
  * outerText：内部文本，content ；

上述四个属性不仅可以读取，还可以赋值。outerText和innerText的区别在于outerText赋值时会把标签一起赋值掉，另外xxText赋值时HTML特殊字符会被转义。 