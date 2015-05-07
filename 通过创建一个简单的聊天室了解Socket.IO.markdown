# 通过创建一个简单的聊天室了解Socket.IO

传统来讲,用传统编程语言写一个聊天工具有一定的难度例如LAMP（Linux+Apache+MySql+PHP）PHP，不是十分容易上手，也让很多的初学者望而生畏从而放弃了开发。然而，在这篇指南中,我们将利用当前最流行的Node.js接合Socket.io来创建一个简单的聊天工具,最重要的是,它几乎不需要什么专业的编程知识,十分容易上手。
##  准备工作
Node.js 框架Express

在实验开始之前,我们首先要确定我的nodejs以及它的开发框架Express已经成功安装.
nodejs 在官方网站上下载 https://nodejs.org/
npm （nodejs packetmanager）,是一个NodeJS包管理和分发工具已经集成在软件中。通过 ```npm install express``` 安装框架Express 

安装完成后，可以通过下列指令来进行检验是否成功
```
node-v
express-v
```

---
安装成功后,我们首先可以通过建立一个app.js来入手，开始我们的程序。
```
var app = require('express')();
var http = require('http').Server(app);

app.get('/', function(req, res){
  res.send('<h1>Hello world</h1>');
});

http.listen(3000, function(){
  console.log('listening on port:3000');
});
```
注释:
>* Express初始化一个功能handler来应用到Http服务器
>* 定义路径handler用来调用文件或者相应的路径
>* Http服务器侦听服务端口号为3000
运行程序 ``` node app.js ```


在浏览器的地址栏输入: http://localhost:3000

##客户端index.html
创建HTML文件
这就是我们简易的聊天窗口
```
<!doctype html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      * { margin: 0; padding: 0; box-sizing: border-box; }
      body { font: 13px Helvetica, Arial; }
      form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
      form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }
      form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }
      #messages { list-style-type: none; margin: 0; padding: 0; }
      #messages li { padding: 5px 10px; }
      #messages li:nth-child(odd) { background: #eee; }
    </style>
  </head>
  <body>
    <ul id="messages"></ul>
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>
  </body>
</html>
```

##**整合Socket.IO**
Socket.IO有两部分组成
>* **socket.io** : 一个整合了(部署在)Nodejs HTTP服务器的socke 服务器
>* **socket.io.client**: 在浏览器端载入的客户端

在此,我们先需要安装socket.io模块

> npm install --save socket.io

根据之前创建的app.js文件进行修改
```
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);
app.get('/', function(req, res){
  res.sendfile('index.html');
});
io.on('connection', function(socket){
  console.log('a user connected');
});
http.listen(3000, function(){
  console.log('listening on port:3000');
});
```
**代码说明**
> io.on('connection',function(socket));

**监听客户端的连接,回调函数会传递本次连接的socket**


现在,我们需要在index.html中标签</body>之前添加一小段代码用来加载Socket.IO
```
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io();
</script>
```

####服务器端程序 app.js
```
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  socket.on('chat message', function(msg){
    io.emit('chat message', msg);
  });
});

http.listen(3000, function(){
  console.log('listening on port:3000');
});
```
代码详解

 - **监听事件**(监听客户端发送的信息)
 ```socket.on('String',function(data));```

 >*  connect:连接成功
 
 >*  connecting:正在连接
 
 >*  disconnect: 断开连接
 

 

 - **发送事件**广播消息(emit the event)
 ```socket.emit('String',data);```




>* 给所有客户端广播消息```io.sockets.emit("msg",{data:"hello, all"});```

>* 给除了自己以外的客户端广播消息
```io.sockets.emit("msg",{data:"hello,everyone"});```

成果展示
经过这个小型的聊天应用的创建,相信您对Socket.IO也有了初步的认识了吧。







