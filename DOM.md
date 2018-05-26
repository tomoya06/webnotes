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



## 一些题目

> innerHTML vs outerHTML

DOM元素的innerHTML, outerHTML, innerText, outerText属性的区别也经常被面试官问到， 比如对于这样一个HTML元素：< div>content< br/></ div>。

  * innerHTML：内部HTML，content< br/>；
  * outerHTML：外部HTML，< div>content< br/></ div>；
  * innerText：内部文本，content ；
  * outerText：内部文本，content ；

上述四个属性不仅可以读取，还可以赋值。outerText和innerText的区别在于outerText赋值时会把标签一起赋值掉，另外xxText赋值时HTML特殊字符会被转义。 