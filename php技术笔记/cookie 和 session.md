### 为什么使用cookie和session

因为http是无状态协议，除了本身所有存储在全局变量的信息外，该环境不会保存任何信息。为了在每次会话之间传递信息，使用session和cookie

##关于cookie

###1、什么时候会使用cookie	

​    1｝比如实现用户自动登录

​    2）保存session_id	如果使用了session http响应头每次请求浏览器都会带有cookie,cookie中的session_id是和服务器相应session的关键

###2、使用cookie的原理

​    setcookie()函数向客户端发送一个 HTTP Cookie请求头

​    使用前提：必须在任何其他输出发送前使用setcookie()

​    返回值：成功返回true,失败返回false

###3、如何使用cookie

​    cookie的语法：setcookie(name,value,expire,path,domain,secure)

| 参数     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| name     | 必需。规定 cookie 的名称。                                   |
| value    | 必需。规定 cookie 的值。                                     |
| expire   | 可选。规定 cookie 的有效期。默认关闭浏览器失效               |
| path     | 可选。规定 cookie 的服务器路径。                             |
| domain   | 可选。规定 cookie 的域名。cookie不能跨域使用，但是可以在同一个域的子域名下使用 |
| secure   | 可选。规定是否通过安全的 HTTPS 连接来传输 cookie。           |
| httpOnly | 默认为false,可选为true。php>5.6 httponly是微软对cookie做的扩展，这个主要是解决用户的cookie可能被盗用的问题。 |

​    ###使用cookie需要注意的问题：

​    1)设置cookie后如何使用？

​    setcookie('ip',$_SERVER['REMOTE_ADDR'],time()+10,'/');

​    var_dump($_COOKIE);

​    第一次打印cookie显示empty	因为第一次访问当前页面，请求已经发送到浏览器，http头并没有设置cookie，设置cookie的操作是在后面程序执行的时候才去做的。也就是你再次提交页面的时候，http头会带有cookie

​    第二次打印的时候，展示cookie的值。

​    2)当cookie自动过期后会怎么样？

​    if(!isset($_COOKIE['ip'])){

​    	setcookie('ip',$_SERVER['REMOTE_ADDR'],time()+10,'/');

​    }

​    var_dump($_COOKIE);

​    像这样给cookie赋值，当超过生存时间后，打印cookie,结果为 empty

​    重新给cookie赋值后超过生存时间，又会变成empty。

​    3)如何销毁一个cookie

​    源码中清清楚楚的显示“if (value && value_len == 0)”，当“value_len”为0时，“sprintf(cookie, 

​    "Set-Cookie: %s=deleted; expires=%s", name, dt);”会发送删除cookie的http头给浏览器。

​    为cookie设置一个过期时间，cookie内的键值对就会消失（常用）

​    重新为当前键赋值为空，当前键值对都会消失

​    销毁当前变量也可以实现销毁cookie

​    4)使用cookie存储用户信息时httpOnly的实现

​    什么是httponly？

​    很多时候都是在cookie中存储一个sessionID，服务器来识别该用户，那么安全隐患也就引申而出了，只要获得这个cookie，就可以取得别人的身份，特别是管理员等高级权限帐号时，危害就大了，而XSS就是在别人的应用程序中恶意执行一段JS以窃取用户的cookie。如何保障我们的Cookie安全呢？Cookie都是通过document对象获取的，我们如果能让cookie在浏览器中不可见就可以了，那HttpOnly就是在设置cookie时接受这样一个参数，一旦被设置，在浏览器的document对象中就看不到cookie了。而浏览器在浏览网页的时候不受任何影响，因为Cookie会被放在浏览器头中发送出去(包括Ajax的时候)，应用程序也一般不会在JS里操作这些敏感Cookie的，对于一些敏感的Cookie我们采用HttpOnly，对于一些需要在应用程序中用JS操作的cookie我们就不予设置，这样就保障了Cookie信息的安全也保证了应用。

​    document.cookie	可以获得cookie信息

​    如何设置httpOnly：

​    方法一：在php_ini中修改配置session.cookie_httponly = 1；

​    方法二：ini_set("session.cookie_httponly", 1)

​    方法三：session_set_cookie_params(0, NULL, NULL, NULL, TRUE);

​    方法四：setcookie("abc", "test", NULL, NULL, NULL, NULL, TRUE); 

##关于session

###1、解释什么是session依赖于cookie

​     'PHPSESSID' => string '51oobhf4j6id7n8e67ou48oh00' (length=26)//这是在设置了session之后，var_dump($_COOKIE得到的session_id)；

​    这时候，如果认为的关闭浏览器中的cookie,如果在请求头(url)继续添加PHPSESSID='****'，session仍然可以使用的。

###2、解释session的生存时间。

​    session的生命周期是间隔的，从创建时，开始计时如在20分钟，没有访问session，那么session生命周期被销毁。但是，如果在20分钟内（如在第19分钟时）访问过session，那么，将重新计算session的生命周期。

###3、修改session的生存时间

####方法一：开启session后设置

setcookie(session_name(),session_id(),'/','时间',null,null,ture)//设置session生存时间，并将cookie设置为httpOnly。

session_name()函数	PHPSESSID

session_id()函数	51oobhf4j6id7n8e67ou48oh00

开启session后当前session_name()和session_id()就确定了。

####方法二：开启session前设置

$lifeTime = 24 * 3600; 

session_set_cookie_params($lifeTime); 

session_start();

$_SESSION["admin"] = true; 

####方法三：在php_ini修改

修改php.ini文件中的gc_maxlifetime变量就可以延长session的过期时间了：（例如，我们把过期时间修改为86400秒）

session.gc_maxlifetime = 86400

然后，重启你的web服务（一般是apache）就可以了。

####4、销毁session

#####场景一：删除某个session值可以使用PHP的unset函数，删除后就会从全局变量$_SESSION中去除，无法访问。

session_start();$_SESSION['name'] = 'jobs';unset($_SESSION['name']);echo $_SESSION['name']; //提示name不存在

#####场景二：如果要删除所有的session，可以使用session_destroy函数销毁当前session，session_destroy会删除所有数据，但是session_id仍然存在。

```php
session_start();

if(!isset($_SESSION['ip'])){

	_SESSION['ip'] = _SERVER['REMOTE_ADDR'];

	var_dump($_SESSION);//session的值已经存在了。

}else{

	session_destroy();

	var_dump($_SESSION);//这个页面访问session仍然有值

}

```



值得注意的是，session_destroy并不会立即的销毁全局变量$_SESSION中的值，只有当下次再访问的时候，$_SESSION才为空，因此如果需要立即销毁$_SESSION，可以使用unset函数。

session_start();$_SESSION['name'] = 'jobs';$_SESSION['time'] = time();unset($_SESSION);session_destroy(); var_dump($_SESSION); //此时已为空，程序会报错Notice错误Notice: Undefined variable: _SESSION

场景三：如果需要同时销毁cookie中的session_id，通常在用户退出的时候可能会用到，则还需要显式的调用setcookie方法删除session_id的cookie值。