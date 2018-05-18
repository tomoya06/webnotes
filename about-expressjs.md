由一道练习引出的问题。

下述代码中，发现若使用两次res.send会报错，提示Can't set headers after they are sent.

首先要注意的是res.send不是原生request的方法，而是express中res对象的响应方法之一。
````
app.get('/', function(req, res) {
    if (req.query.q) {
        res.send(req.query.q);
    }
    res.end('===');
})
````
在express的文档中还有其他的响应方法：
http://expressjs.com/zh-cn/guide/routing.html

文档中还提到，可以 res.status(404).send('xxx');
