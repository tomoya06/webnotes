1. 

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

来自栈溢出网站的一条回答:
````
res.send

    res.send is only in Express js.
    Performs many useful tasks for simple non-streaming responses.
    Ability to automatically assigns the Content-Length HTTP response header field.
    Ability to provides automatic HEAD & HTTP cache freshness support.

    Practical explanation
        res.send can only be called once, since it is equivalent to res.write + res.end()

        Example

        app.get('/user/:id', function (req, res) {
            res.send('OK');
        });

for more details expressjs.com/en/api.html

res.write

    Can be called multiple times to provide successive parts of the body.

    Example

    response.write('<html>');
    response.write('<body>');
    response.write('<h1>Hello, World!</h1>');
    response.write('</body>');
    response.write('</html>');
    response.end();


````

2. 
在express中，要取得文件路径中的某个param，可以这样
````
app.get('/path/:id', (req, res) {
    var getid = req.params.id;
})
````

3. 
使用路由中间层时，注意在route.js中要导出module.exports = route供上层使用。




