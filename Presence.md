# 在线监控
***实时监控用户和设备的上线以及下线情况***
想第一时间知道谁连接到你的设备上，从而更深入了解您的用户动态。云巴的实时监控服务可以为您的每一台设备提供以下功能：

 - 用户状态变化的通知（上线或者下线）
 - 一个实时更新的订阅特定频道的用户（别名）列表
 - 一个实时更新的频道列表


我们的实时监控做些什么

 1. 实时跟踪用户上线和下线
 2. 监控设备的连接状态
 3. 对于聊天室、游戏大厅、和机器之间的交流至关重要


特色应用
1 使用情况： 实时监控用户人数以及设备占用状况
2 上线，下线通知： 以便及时更新所有客户端的连接状态
3 全球化，大规模：遍布全球的同步服务器基于云巴消息系统 保证用户在全球每一个角落体验到云巴为您带来的服务。

##**核心APIs**

 ```
 get_state()```**查看在线状态**
 ```
 函数使用方法：
```
yunba.get_state(alias, function (data) {
console.log('get state succ');
```
注：alias 为查询用户的别名。 *function(data)*中传递回的参数有 *success*、*data、error_msg*。其中，查询成功 *success* 为 *true* 否则为 *false*，*data* 表示在线状态，*success* 为 *false* 时 *error_msg* 有效。
  ```
  get_alias_list( )```**获取所有订阅该频道用户的别名**
 ```
 函数使用方法：
```
yunba.get_alias_list(topic, function (success, data)
      {
            if (success) {
            var aliasList = "";
            data.alias.forEach(function (alias) {
            aliasList = aliasList + alias + "  ";
        });
```
注：**topic** 为订阅的频道。***Function(success, data)***传递回的参数有 success、data.alias、error_msg。查询成功 success 为 true 否则为 false，data.alias 为订阅的 alias 列表，类型 List，success 为 true 时有效，success 为 false 时 error_msg 有效。


---
