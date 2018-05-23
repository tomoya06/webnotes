1.
防止事件冒泡：
````
    $("#outC").click(function(event){  
        return false;  
    });   
    // or
    $("#outC").click(function(event){  
        event.stopPropagation();  
    });  
````


2.
注意，使用箭头函数会改变函数体内this的作用域，即把this改为window。

这对于dom操作是没意义的。所以还是要用回function
