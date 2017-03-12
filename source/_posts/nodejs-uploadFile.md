---
title: nodejs实现简单的文件上传
date: 2017-03-11 21:24
tags: nodejs,multiparty
---

上传文件，三种方式
-------------
#### 1、上传文件直接作为body上传服务器，用fs模块读取
```javascript
req.pipe(fs.createWriteStream(filename))
```
#### 2、使用BASE64编码后上传，用buffer直接读取
```javascript
Buffer.from(body.content, 'base64')
```
#### 3、multipart/form-data，对浏览器的兼容性较好，请求体如图

![浏览器抓包图](http://omc3jlwz8.bkt.clouddn.com/d81cf01a-b38d-4b56-a72d-4db5313da6c9.jpg)
<br>
需要使用multiparty模块进行解析
<br>
安装
```javascript
npm install multiparty
```

解析上传的请求题
```javascript
var multiparty = require('multiparty');
var http = require('http');
var util = require('util');
http.createServer(function(req, res) {
    if (req.url === '/upload' && req.method === 'POST') {
    // parse a file upload
        var form = new multiparty.Form();
        form.parse(req, function(err, fields, files) {
            res.writeHead(200, {'content-type': 'text/plain'});
            res.write('received upload:\n\n');
            res.end(util.inspect({fields: fields, files: files}));
        });
        return;
    }
    // show a file upload form
    res.writeHead(200, {'content-type': 'text/html'});
    res.end(
        '<form action="/upload" enctype="multipart/form-data" method="post">'+
        '<input type="text" name="title"><br>'+
        '<input type="file" name="upload" multiple="multiple"><br>'+
        '<input type="submit" value="Upload">'+
        '</form>'
    );
}).listen(8080);
```