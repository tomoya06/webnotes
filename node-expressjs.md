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

4.
值得注意的是，app.send之后前端的显示就交给浏览器了，而不是服务器负责了，nodejs就不起作用了。

前后端的交互是通过一般的href跳转或者ajax请求来完成。

服务器向客户端的话就是websocket咯。


页面跳转方法总结：
````
window.location.href = '/destination';  //confirmed
top.location = '/des';                  //unsure
window.navigate = '/des';               //unsure

window.history.back(-1);
window.history.go(-1);
````
5.
nodejs中的文件路径：
````
__dirname: 总是返回被执行的 js 所在文件夹的绝对路径
__filename: 总是返回被执行的 js 的绝对路径
process.cwd(): 总是返回运行 node 命令时所在的文件夹的绝对路径
````



