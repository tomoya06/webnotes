数据流stream
分可读数据流/可写数据流/双向数据流
fs/http/等地方都用的是数据流的继承
http://javascript.ruanyifeng.com/nodejs/stream.html


事件：
stream.on('data'/'end'/'error'/etc.

pipe方法：
````
fs.createReadStream('wow.txt') 
    .pipe(zlib.createGzip())
    .pipe(fs.createWriteStream('wow.gz')); 
````
可以把可读数据流流向可写数据流。

另一个例子
````
var fs = require('fs'); 
console.log(process.argv[2], '->', process.argv[3]); 
var readStream = fs.createReadStream(process.argv[2]); 
var writeStream = fs.createWriteStream(process.argv[3]); 
readStream.on('open', function () { 
    readStream.pipe(writeStream); }); 
    readStream.on('end', function () { writeStream.end(); 
});
````
