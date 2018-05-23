# differences between methods to access DOM node
被腾讯老爹坑过的一道题。

1。 querySelector() querySelectorAll()

返回类型分别是node类型的object 和 非活动 的nodelist

2。 getElementById() 

返回类型是Element类型的Object

3。 getElementsByClassName() getElementByTagName()

返回类型是 活动 的HTMLCollection

4。 getElementsByName()

返回类型是 活动 的nodelist

5。 getElementsByTagNameNS(Namespace, name)

返回类型是 活动 的nodelist。但是没怎么用过。 element.namespace似乎已经过时。

nodelist 和 HTMLCollection 都是类似数组，可以直接用list[index]的格式对其操作。
HTMLCollection，从定义上是一直活动的。对DOM的更新操作会对其造成影响。
nodelist则不一定。

活动和非活动的nodelist在获取之后都可以对原DOM进行数据修改。修改之后该元素的变化也可以读取到。

但如果添加/删除新元素则无法察觉。


# dom operation
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
