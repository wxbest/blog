## 双引号单引号区别

双引号解释变量，单引号不解释变量
双引号里插入单引号，其中单引号里如果有变量的话，变量解释
双引号的变量名后面必须要有一个非数字、字母、下划线的特殊字符，或者用{}讲变量括起来，否则会将变量名后面的部分当做一个整体，引起语法错误
能使单引号字符尽量使用单引号，单引号的效率比双引号要高

------

## GET和POST提交方式的区别

GET产生一个TCP数据包；POST产生两个TCP数据包；
对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）
对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。
GET在浏览器回退时是无害的，而POST会再次提交请求
GET请求会被浏览器主动cache，而POST不会，除非手动设置
GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留
GET请求只能进行url编码，而POST支持多种编码方式
GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息

------

## 如何获取客户端的真实ip

$_SERVER['REMOTE_ADDR']或getenv('REMOTE_ADDR')
可以使用ip2long()转成数字

------

## include和require的区别

require是无条件包含，也就是如果一个流程里加入require，无论条件成立与否都会先执行require，当文件不存在或者无法打开的时候，会提示错误，并且会终止程序执行
include有返回值，而require没有(可能因为如此require的速度比include快)，如果被包含的文件不存在的化，那么会提示一个错误，但是程序会继续执行下去
注意:包含文件不存在或者语法错误的时候require是致命的，而include不是

------

## AJAX的优势是什么

ajax是异步传输技术，可以通过javascript实现，也可以通过JQuery框架实现，实现局部刷新，减轻了服务器的压力，也提高了用户体验

------

## 在程序的开发中，如何提高程序的运行效率

优化SQL语句，查询语句中尽量不使用select *，用哪个字段查哪个字段；
少用子查询可用表连接代替；
少用模糊查询；
数据表中创建索引；
对程序中经常用到的数据生成缓存；

------

## SESSION与COOKIE的区别

存储位置：session存储在服务器，cookie存储在浏览器
安全性：session安全性高于cookie
参考链接：<https://www.zhihu.com/questio...>

------

## isset和empty的区别

isset()函数 一般用来检测变量是否设置
若变量不存在则返回 FALSE 
若变量存在且其值为NULL，也返回 FALSE 
若变量存在且值不为NULL，则返回 TURE
empty()函数是检查变量是否为空
若变量不存在则返回 TRUE 
若变量存在且其值为""、0、"0"、NULL、、FALSE、array()、var $var; 以及没有任何属性的对象，则返回 TURE 
若变量存在且值不为""、0、"0"、NULL、、FALSE、array()、var $var; 以及没有任何属性的对象，则返回 FALSE

------

## 数据库三范式

第一范式：1NF是对属性的原子性约束，要求属性具有原子性，不可再分解；
第二范式：2NF是对记录的惟一性约束，要求记录有惟一标识，即实体的惟一性；
第三范式：3NF是对字段冗余性的约束，即任何字段不能由其他字段派生出来，它要求字段没有冗余。

------

## 主键、外键和索引的区别

定义:
主键--唯一标识一条记录，不能有重复的，不允许为空
外键--表的外键是另一表的主键, 外键可以有重复的, 可以是空值
索引--该字段没有重复值，但可以有一个空值*斜体文字*
作用:
主键--用来保证数据完整性
外键--用来和其他表建立联系用的
索引--是提高查询排序的速度
个数:
主键--主键只能有一个
外键--一个表可以有多个外键
索引--一个表可以有多个唯一索引

------

## 堆和栈的区别

栈是编译期间就分配好的内存空间，因此你的代码中必须就栈的大小有明确的定义； 
堆是程序运行期间动态分配的内存空间，你可以根据程序的运行情况确定要分配的堆内存的大小。