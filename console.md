# JavaScript 控制台 Demo


##**主要API介绍**

在进行实验之前，首先要介绍三个简单易用的API，正是由于这些应用程序接口的使用使得用户能够方便地从频道接收和发送信息。


### Subscribe( )——加入并且接受从一个或多个订阅频道的消息,想要接收一个频道的信息，首先你得使用subscribe()的方法订阅这个频道，然后使用回调函数设接收这个频道发出的信息
### Publish( )—— 发送数据到频道中用户可以使用publish（）方法向所订阅的频道发布消息
### Unsubscribe( )——离开频道，取消订阅

对于绝大多数应用程序，这就是您用来建立实时应用程序所需要的基本API。



## 1. 准备开始
**调试工具**

注： 如果需要在浏览器内进行程序调试，Chrome浏览器自带JavaScript控制台，火狐浏览器Firefox需在附加组件中安装Firebug 插件。

**获取appkey**
只需要三步就可以获得属于你自己的Appkey
登录我们的官方网站（http://yunba.io/）注册我们的云巴账号，并且创建一个应用。
注意： 应用包名称应符合标准(一级包名为com，二级包名为xx（可以是公司也可以是个人），三级包名根据应用进行命名，四级包名为模块名或层级名)

## 2.初始化  Initialize
首先，引入所依赖的 Socket.io，
由于我们的JavaScript SDK 是基于Socket.io API，所以第一步要与Socket.io 服务器建立连接。

**那么，为什么要使用Socket.io ?**
Socket.IO 是在 WebSocket 基础上开发的一种基于 HTTP 协议的长连接通讯方式，使用起来跟原生的 Socket 一样方便， 特别是在 Web App 开发中，使用得越来越多。相对于原生 SDK（iOS,Android ），基于 Socket.io API 的 SDK 代码体积小，开发周期短，适合为小众语言快速开发 SDK。

```
<script type="text/javascript" src="socket.io.js"></script>
```
 紧接着引入 Yunba JavaScript SDK
```
<script type="text/javascript" src="yunba-1.0.1.js"></script>
```
利用刚刚获得appkey创建 Yunba 实例
```
var yunba_demo = new Yunba({
server: 'sock.yunba.io', port: 3000, appkey: '5535ee**********814e11163'});
```
使用刚刚获得的appkey初始化并连接服务器
完整代码如下(用户只需替换自己的appkey即可)：
```
<script type="text/javascript" src="./socket.io.js"></script>
<script type="text/javascript" src="./yunba-1.0.1.js"></script>
<script type="text/javascript">
  var yunba_demo = new Yunba({
    server: 'sock.yunba.io', port: 3000, appkey: '5535ee**********814e11163'});
  yunba_demo.init(function (success) {
    if (success) {
      yunba_demo.connect(function (success, msg) {
        if (success)
          console.log('successful connection!');
        else
          console.log(msg);
      });
    }
  });
</script>
```
将上述代码复制到一个空的文件中在你的桌面保存，命名为Yunba_demo.html
在Google Chrome浏览器打开这个文件打开文件后，开启JavaScript控制台（JavaScript Console）[Ctrl+Shift+J]
你会发现如右图所示，控制台中显示：（successful connection!）成功连接上服务
## 3 订阅频道 Subscribe a Topic
若要想接收从一个频道发送的消息，首先你得订阅这个频道
利用Subscribe（）方法订阅属于自己的频道并且用set_message_cb() 设置收到消息时调用的回调函数来接收消息。
将下面的代码复制到控制台中如右图所示，确认。你便成功订阅了频道“Yunba”
```
yunba_demo.subscribe({'topic':'yunba'},
function (success, msg) {
  if (success)
    console.log('你已成功订阅频道');
  else
    console.log(msg);
});
yunba_demo.set_message_cb(function (data) {
  console.log('Topic:' + data.topic +
',Msg:' + data.msg);
});
```
## 4.发布消息 Publish a Message
在成功订阅频道之后，下面要做的便是将你的消息用Publish（）的方法发送到刚刚订阅的频道上去。



复制下面的代码到控制台中，回车执行指令。得到确认消息“消息发布成功”你便成功地向频道“Yunba”发送了消息“你好！Yunba”
```
yunba_demo.publish({'topic': 'my_topic',
'msg': '你好！Yunba'},
  function (success, msg) {
    if (success)
      console.log('消息发布成功');
else
console.log(msg);
});
```
## 5.取消订阅 UnSubscribe the topic
当你厌烦了这个频道，不想再接收来自这个频道的消息，可以利用unsubscribe()方法来取消订阅。
复制下面的代码到控制台中，按回车键执行代码。
```
yunba.unsubscribe({'topic':yunba}, function (success, data) {
  if (success) {
 console.log(‘已成功离开频道’)
}
else {s
alert(data);
}
});
```
控制台显示：Object{success：true}已成功离开频道。
