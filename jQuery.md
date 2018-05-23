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
