## 架构层：

 ##  一、url访问

1、url路由的访问

访问路由http://localhost/index.php/Index/BlogTest/Read,或者路由

[http://localhost/index.php/index/blogtest/read](http://localhost/index.php/Index/BlogTest/Read)效果是一样的。linux严格区分大小写，如果

lnmp环境遇到找不到路由的时候注意检查大小写。

2、关闭URL中控制器和操作名的自动转换

'url_convert' => false,

3、如何访问驼峰方法t像BlogTest	方法 	url就要写	blog_test  在关闭自动转换大小写后仍

有效

 ## 二、api友好

1、返回指定的数据类型

返回json格式：

return json(['data'=>$data,'code'=>1,'message'=>'操作完成']);

返回xml格式：

return xml(['data'=>$data,'code'=>1,'message'=>'操作完成']);

返回jsonp格式

return jsonp(['data'=>$data,'code'=>1,'message'=>'操作完成']);

2、错误调试

由于 API 开发不方便在客户端进行调试， ThinkPHP5 ? Trace调试功能支持 Socket 在内的方式，可实现远程开发调试。

设置方式：

'app_trace' => true,

'trace' => [

'type' => 'socket',

// socket服务器

'host' => 'slog.thinkphp.cn',

],