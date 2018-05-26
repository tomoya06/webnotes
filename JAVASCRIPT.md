
# Javascript

## 类型和传参

* 基本数据类型：undefined / null / boolean / string / number / object / symbol(es6)

  * 基本类型：undefined / null / boolean / string / symbol
  * 引用类型：object，具体包括object / array / date / regexp / function
  * 两种类型区别在于储存位置不同：
    * 原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储； 
    * 引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体

* 类型检测
  * typeof：经常用来检测一个变量是不是最基本的数据类型。结果包括number/string/object/boolean/undefined/function。
  > typeof null ==> object

  * instanceof：用来判断某个构造函数的 prototype 属性所指向的对象是否存在于另外一个要检测对象的原型链上。也就是用来判断一个引用类型的变量具体是不是某种类型的对象。对基本类型没有作用，因为基本类型没有原型链。

  * 另外还可以使用constructor / toString 来检测。参考 https://harttle.land/2015/09/18/js-type-checking.html

* 注意点
  * 对JS来说，基本类型是传值引用，引用类型是穿共享调用。传值调用本质上传递的是变量的值的拷贝。传共享调用本质上是传递对象的指针的拷贝，其指针也是变量的值。所以传共享调用也可以说是传值调用。

## 内置对象

* 标准内置对象的分类：详见 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects
  * 值属性：返回一个简单值，没有自己的属性和方法
    * Infinity
    * NaN
    * undefined
    * null
  * 函数属性：全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。
    * eval() uneval()
    * isFinite() isNaN()
    * parseFloat() parseInt()
    * decodeURI() decodeURIComponent() encodeURI() encodeURIComponent()
    * escape() unescape()
  * 基本对象：基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。
    * Object Function Boolean Symbol Error 
    * 各种Error
  * 数字和日期对象
    * Number Math Date
  * 字符串
    * String RegExp
  * 集合对象
    * Array
    * Map Set WeakMap WeakSet
  * 其他
    * arguments

## javascript的面向对象

别说了去看阮一峰吧 http://javascript.ruanyifeng.com/oop/basic.html

### 原型和原型链
  * prototype在最初设计JavaScript的时候是为了放置共享的方法和属性用的。
  * Object.prototype是所有对象的根原型。Object.prototype.__ proto__=null。
  * Contructor；prototype对象有一个constructor属性，默认指向prototype对象所在的构造函数。
    * constructor属性的作用是，可以得知某个实例对象，到底是哪一个构造函数产生的。
    * 给某一构造函数的prototype重新赋值时，会导致原先的prototype被切断引用，也就导致了原先的constructor不复存在。所以要修改prototype的话也要跟着修改constructor以防出错。

  * 获取原型的方法：注意，1）只有浏览器用；2）改了prototype而没有重新赋值constructor可能会报错；3）比较稳。
    * obj.__ proto__
    * obj.constructor.prototype
    * Object.getPrototypeOf(obj)

  * 对象的拷贝：需要保证以下两点
    * 新对象与元对象有相同的原型
    * 系对象与元对象有相同的实例属性

  * 如何实现继承：更多参考阮一峰 http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html
    * 构造继承
    * 原型继承
    * 实例继承
    * 拷贝继承

````
function Animal() {
  this.species = "Animal";
}
function Cat(name, color) {
  this.name = name;
  this.color = color;
}

// constructor inherit 
function Cat(name, color) {
  Animal.apply(this, arguments);
  //...
}

// prototype inherit
//...
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;

````

  ````
  function P() {}
  P.prototype.constructor === P //==> true
  ````
  * 参考文献
    * http://javascript.ruanyifeng.com/oop/prototype.html
    * http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html

### this

  * this总是指向函数的直接调用者（而非间接调用者）；如果有new关键字，this指向new出来的那个对象；
  * 如果this所在的方法不在对象的第一层，这时this只是指向当前一层的对象，而不会继承更上面的层。之后就直接到全局了。
  * 绑定this的方法
    * func.call(thisValue, arg1, arg2, ...) 
      * 调用对象的原生方法
    * func.apply(thisValue, [...args]) 
      * 找出数组最大元素：Math.max.apply(null, [10, 2, 4]);  //==> 10
      * 将数组的空元素变成undefined：Array.apply(null, [2, , 4]); //==> [2. undefined, 4]
      * 将类似数组的对象（比如arguments）转换成标准数组：Array.prototype.slice.apply(Array, arguments); 
    * func.bind(thisValue)
  * 参考文献 http://javascript.ruanyifeng.com/oop/this.html

### 作用域链 / 闭包

* 全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节。当需要从局部函数查找某一属性或方法时，如果当前作用域没有找到，就会上溯到上层作用域查找，直至全局函数，这种组织形式就是作用域链。

* 闭包：简单来说是一个嵌套的function。把一个函数的内部函数暴露出来，让接受了这个暴露的内部函数的函数可以读到原来那个函数的内部属性。或者可以用于封装一类函数。
> 参考 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures https://web.archive.org/web/20100825182303/http://www.felixwoo.com/archives/247 和http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html

### 创建对象的方式

* 直接用大括号或者function
* 工厂模式：工厂内部新建一个实例并交给客户
* 构造函数模式：new新建一个实例
* 原型模式：用function创建一个没有属性的对象，把共享的属性和方法交给prototype托管
* 混合模式：构造函数模式和原型模式的结合。
* 其他详见 http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html

## 常见问题

* window对象和document对象
  * window 对象表示一个包含DOM文档的窗口，其 document 属性指向窗口中载入的 DOM文档 。在标签浏览器中，每个标签具有自己的 window 对象（如果你在开发扩展，浏览器窗口也是一个独立的 window ）
  * Document 接口提供了一些在浏览器服务中作为页面内容入口点而加载的一些页面，也就是 DOM 树。

* 【事件】事件处理：冒泡和防止默认行为
  * 防止冒泡
  ````
  function myfn(e){
      window.event? window.event.cancelBubble = true : e.stopPropagation();
  }
  ````

  * 防止默认行为
  ````
  function myfn(e){
      window.event? window.event.returnValue = false : e.preventDefault();
  }
  ````
  > return false：javascript的return false只会阻止默认行为，而是用jQuery的话则既阻止默认行为又防止对象冒泡。


## ES6 专题

### map & set
  * set vs WeakSet：两个都是不重复的值的集合，与数组类似，但weakset成员只能是对象。WeakSet不能遍历，因为成员都是弱引用。
  * map vs WeakMap：键值对的集合，类似于对象，但键的范围不仅限于字符串。weakmap的键只能接受对象（null除外。null本身也不是引用类型）；weakmap键名所指向的对象不计入垃圾回收机制。
> 详见阮一峰 http://es6.ruanyifeng.com/#docs/set-map


### class

* ES6 要求，子类的构造函数必须执行一次super函数。

* prototype / __proto__：大多数浏览器的 ES5 实现之中，每一个对象都有__proto__属性，指向对应的构造函数的prototype属性。Class 作为构造函数的语法糖，同时有prototype属性和__proto__属性，因此同时存在两条继承链。

  * 子类的__proto__属性，表示构造函数的继承，总是指向父类。
  * 子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。

## HTML5带来的新接口

### 储存

* storage：有globalStorage localStorage sessionStorage 三种。主要用于客户端的持久化数据存储。localStorage/sessionStorage 不会自动把数据发给服务器，仅在本地保存。

  * globalStrage: 不属于标准。Firefox独有。可以跨域存储数据
  * localStorage: 主要针对同一个域下面数据的存取。
  * sessionStorage: 生命周期为一个session。刷新标签页时数据保留，但是当标签页关闭时，sessionStorage 内存储的数据会消失。

````
globalStorage['developer.mozilla.org'].username = "John"; 
localStorage.username = "John"; 
sessionStorage.username = "John"; 
sessionStorage.setItem("username", "John");
````

* cookie: 是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）。cookie数据始终在同源的http请求中携带（即使不需要），也就是会在浏览器和服务器间来回传递。更多介绍可以看：http://bubkoo.com/2014/04/21/http-cookies-explained/.


#### 存储大小：

* cookie数据大小不能超过4k。
* sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

​#### 有期时间：
​
* localStorage: 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据；
* sessionStorage: 数据在当前浏览器窗口关闭后自动删除。
* cookie: 设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭.


### navigator.online属性

### getElementsByCLassName() 

> 竟然是HTML5的？？
>
> 返回HTMLCollection类型。更多DOM操作见另一个笔记。

### History对象

> 引入history.pushState()和history.replaceState()两个方法。它们允许在无页面跳转的情况下添加和修改 History 实体，即操作浏览器历史，并且改变当前页面的 URL（pushState 用于向 history 添加当前页面的记录，而 replaceState 和 pushState 的用法完全一样，唯一的区别就是它用于修改当前页面在 history 中的记录）。同时，这些方法还会和“window.onpopstate” 事件一起工作。

````
var stateObj = { myVar: "myValue" }; 
var stateObj2 = { myVar: "myValue2" }; 
history.pushState(stateObj, "history page", "hisPage.html"); 
history.pushState(stateObj, "history page 2", "hisPage2.html"); 

window.onpopstate = function(e) { 
    console.log(e.state); 
};
````

还可以看看“onpageshow”和“onpagehide”，他们对应于“onload”和“onunload”事件。页面加载时默认触发“onload”事件，但是如果页面是从“往返缓存”里面加载的，则触发“onpageshow”等事件。

### 跨域通信

#### window.postMessage()

> window.postMessage() 方法可以安全地实现跨源通信。通常，对于两个不同页面的脚本，只有当执行它们的页面位于具有相同的协议（通常为https），端口号（443为https的默认值），以及主机  (两个页面的模数 Document.domain设置为相同的值) 时，这两个脚本才能相互通信。window.postMessage() 方法提供了一种受控机制来规避此限制，只要正确的使用，这种方法就很安全。

````
//iframe1 
window.parent.frames[1].postMessage(message, 'http://dm1.com'); 
 
//iframe2 
if (typeof window.addEventListener != 'undefined') { 
    window.addEventListener('message', messageHandler, false); 
} else if (typeof window.attachEvent != 'undefined') { 
    window.attachEvent('onmessage', messageHandler); 
} 
 
function messageHandler(e){ 
    if(event.origin !== http://dm1.com') return; 
    var data = e.data; 
    ...... 
}
````

### Notifications 桌面提醒接口

不多说了直接上代码。来自阮一峰。

````
if(window.Notification && Notification.permission !== "denied") {
    Notification.requestPermission(function(status) {
        var n = new Notification('通知标题', { body: '这里是通知内容！' }); 
    });
}


var n = new Notification("Hi!");

// 手动关闭
// n.close();

// 自动关闭
n.onshow = function () { 
    setTimeout(n.close.bind(n), 5000); 
}
````

### 离线缓存

首先要通过 '< html manifest="html5demo.manifest" lang="en">' 引入清单文件。

一个例子：https://yanhaijing.com/html/2014/12/28/html5-manifest/

### 文件操作接口 FileReader FileWriter

> 另外可以参考阮一峰的文件上传总结http://www.ruanyifeng.com/blog/2012/08/file_upload.html

### websocket

````
var socket = new WebSocket(location); 
socket.onopen = function(event) { 
    socket.send(“Hello, WebSocket”);  // ”postMessage” 
} 
socket.onmessage = function(event) { alert(event.data); } 
socket.onclose = function(event) { alert(“closed”); }
````

### 地理位置

````
function getLocation(){
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition);
    } else {
        console.log("Geolocation is not supported");
    }
}
function showPosition(position){ 
    console.log("Latitude: " + position.coords.latitude + "<br />Longitude: "
        + position.coords.longitude);
}

getLocation();
````

### 拖拽

### Web Worker

> Web Worker的目的，就是为JavaScript创造多线程环境，允许主线程将一些任务分配给子线程。在主线程运行的同时，子线程在后台运行，两者互不干扰。等到子线程完成计算任务，再把结果返回给主线程。

详细介绍参考阮一峰的讲解 http://javascript.ruanyifeng.com/htmlapi/webworker.html

关键点：主程序通过worker构建函数构建worker，要用worker.postMessage(msg)来唤醒worker启动，worker内设置listener监听这个msg，监听到之后开始执行，执行完又通过self.postMessage(cmsg)来回应主程序。主程序也要设置一个listener，听到回复之后做动作。

TODO

http1.1 vs http 2.0, http状态🐴
网站安全 CROS XSS
继承
prototype __proto__ apply() call() bind()