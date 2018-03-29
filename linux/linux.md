# 填空

- 1、在Linux系统中，以 **文件** 方式访问设备。
- 2、Linux内核引导时，从文件 **/etc/fstab** 中读取要加载的文件系统
- 3、Linux文件系统中每个文件用 **i节点** 来标识
- 4、全部磁盘块由四个部分组成，分别为: **引导块、专用块、i节点块、数据存储块**
- 5、前台起动的进程使用:** ctrl+c** 禁止
- 6、安装Linux系统对硬盘分区时，必须有两种分区类型：**文件系统 和 交换分区。**
- 7、网络管理的重要任务是 **监控 和 控制**
- 8、内核分为 **文件管理系统、I/O管理系统 、内存管理系统 和进程管理系统** 等四个子系统。

------

# 系统

## 1、Linux开机启动过程？

- 1）主机加电自检，加载BOLS硬件信息
- 2）读取MBR的引导文件（grub，lilo）
- 3）引导linux内核
- 4）运行第一个进程init（进程号永远为1）
- 5）进入相应的运行级别
- 6）运行终端，输入用户名和密码

## 2、Linux系统缺省的运行级别

```
0.关机
1.单机用户模式 
2.字符界面的多用户模式（不支持网络）
3.字符界面的多用户模式
4.未分配使用 
5.图形界面的多用户模式 
6.重启
```

## 3、Linux系统是由那些部分组成？

Linux系统内核，shell，文件系统和应用程序四部分组成

## 4、硬链接和软链接有什么区别？

- 1）硬链接不可以跨分区，软件链可以跨分区
- 2）硬链接指向一个i节点，而软链接则是创建一个新的i节点
- 3）删除硬链接文件，不会删除原文件，删除软链接文件，会把原文件删除

## 5、如何规划一台Linux主机，步骤是怎样？

- 1、确定机器是做什么用的，比如是做web、db、还是游戏服务器
- 2、确定好之后，就要定系统需要怎么安装，默认安装哪些系统、分区怎么做
- 3、需要优化系统的哪些参数，需要创建哪些用户等等的

## 6、查看系统当前进程连接数？

```
netstat -an | grep ESTABLISHED | wc -l
```

## 7、如何在/usr目录下找出大小超过10MB的文件?

```
find /usr -type f -size +10240k
```

## 8、添加一条到192.168.3.0/24的路由，网关为192.168.1.254？

```
route add -net 192.168.3.0/24 netmask 255.255.255.0 gw 192.168.1.254
```

## 9、如何在/var目录下找出90天之内未被访问过的文件?

```
find /var \! -atime -90
```

## 10、如何在/home目录下找出120天之前被修改过的文件?

```
find /home  -mtime +120
```

## 11、在整个目录树下查找文件“core”，如发现则无需提示直接删除它们。

```
find / -name core -exec rm {} \;
```

## 12、有一普通用户想在每周日凌晨零点零分定期备份/user/backup到/tmp目录下，该用户应如何做?

```
crontab -e
0 0 * * 7 /bin/cp /user/backup /tmp
```

## 13、每周一下午三点将/tmp/logs目录下面的后缀为*.log的所有文件rsync同步到备份服务器192.168.1.100中同样的目录下面，crontab配置项该如何写：

```
00 15 * * 1 rsync -avzP /tmp/logs/*.log root@192.168.1.100:/tmp/logs
```

## 14、找到/tmp/目录下面的所有名称以"_s1.jpg"结尾的普通文件，如果其修改日期在一天内，则将其打包到/tmp/back.tar.gz文件中

```
find /tmp -type f -name ".*_sj.jpg" -mtime 1|xarges tar zxf /tmp/back.tar.gz
```

## 15、配置mysql服务器的时候，配置了auto_increment_increment=3，请问这里的3意味着什么？

auto_increment是用于主键自动增长的，从3开始增长，3表示自增的起始值

## 16、详细说明keepalived的故障切换工作原理

这种故障切换是通过VRRP协议来实现的，主节点会按一定的时间间隔发送心跳信息的广播包，告诉备节点自己的存活状态信息，当主节点发生故障时，备节点在一段时间内就收到广播包，从而判断主节点出现故障，因此会调用自身的接管程序来接管主节点的IP资源及服务，当主节点恢复时，备节点会主动释放资源，恢复到接管前的状态，从而来实现主备故障切换

------

# 安全

## 1、防火墙有几张表几条链？

4张表，5条链

## 2、一台Linux系统初始化环境后需要做一些什么安全工作？

- 1、添加普通用户登陆，禁止root用户登陆，更改SSH端口号
- 2、服务器使用密钥登陆，禁止密码登陆
- 3、开启防火墙，关闭SElinux，根据业务需求设置相应的防火墙规则
- 4、装fail2ban这种防止SSH暴力破击的软件
- 5、设置只允许公司办公网出口IP能登陆服务器（看公司实际需要）
- 6、设置nginx_waf模块防止SQL注入
- 7、把Web服务使用www用户启动，更改网站目录的所有者和所属组为www
- 8、修改历史命令记录的条数为10条

## 3、什么叫CC攻击？什么叫DDOS攻击？怎么预防CC攻击和DDOS攻击？

简介：

- CC攻击主要是用来攻击页面的，模拟多个用户不停的对你的页面进行访问，从而使你的系统资源消耗殆尽
- DDOS攻击中文名叫分布式拒绝服务攻击，指借助服务器技术将多个计算机联合起来作为攻击平台，来对一个或多个目标发动DDOS攻击，
  攻击即是通过大量合法的请求占用大量网络资源，以达到瘫痪网络的目的

预防：

- 防CC/DDOS攻击这些只能是用硬件防火墙做流量清洗，将攻击流量引入黑洞
  流量清洗这一块，主要是买ISP服务商的防攻击的服务就可以，机房一般有空余流量，

我们一般是买服务，毕竟攻击不会是持续长时间

## 4、什么是网站数据库注入？怎么过滤与预防网站数据库注入？

### 简介：

- 由于程序员的水平及经验参差不齐，大部分程序员在编写代码的时候，没有对用户输入数据的合法性进行判断，
- 应用程序存在安全隐患。用户可以提交一段数据库查询代码，根据程序返回的结果，获得某些他想得知的数据，这就是所谓的SQL注入。
- SQL注入是从正常的WWW端口访问，而且表面看起来跟一般的Web页面访问没什么区别，如果管理员没查看日志的习惯，可能被入侵很长时间都不会发觉。

### 过滤与预防：

数据库网页端注入这种，可以考虑使用nginx_waf做过滤与预防

------

# 网络

## 写出如何给apache增加virtualhost，让访问[http://www.test.com和http](http://www.test.xn--comhttp-bs4l/)://www.test.cn的时候，都打开/var/www/html目录下面的文件：

```
<VirtualHost *:80>
    ServerAdmin admini@abc.com
    DocumentRoot "/var/www/html"
    ServerName www.test.com
    ServerAlias  test.cn
    ErrorLog "logs/bbs-error_log"
    CustomLog "logs/bbs-access_log" common
</VirtualHost>
```

## 用一条命令显示本机eth0网卡的IP地址，不显示其它字符

```
#方法一：
ifconfig eth0|grep inet|awk -F ':' '{print $2}'|awk '{print $1}'
#方法二
ifconfig eth0|grep "inet addr"|awk -F '[ :]+' '{print $4}' 
#方法三：
ifconfig eth0|awk -F '[ :]+' 'NR==2 {print $4}' 
#方法四：
ifconfig eth0|sed -n '2p'|sed 's#^.*addr:##g'|sed 's# Bc.*$##g'
#方法五：
ifconfig eth0|sed -n '2p'|sed -r 's#^.*addr:(.*)  Bc.*$#\1#g'
#方法六(centos7也适用)：
ip addr|grep eth0|grep inet|awk '{print $2}'|awk -F '/' '{print $1}'
```

## 写出一个curl命令，访问指定服务器61.135.169.121上的如下URL：<http://www.baidu.com/s?wd=test>，访问的超时时间是20秒：

```
curl --connect-timeout 20 http://61.135.169.121/s?wd=test
```

## 用netstat命令配合其他shell命令，按照源IP统计所有到80端口的ESTABLISHED状态链接的个数，输出结果类似（第一列为连接数，第二列为IP）：

```
[root@ ~]# netstat -an|grep ESTABLISHED
tcp        0     52 139.224.199.85:22           101.47.33.86:51763          ESTABLISHED 
tcp        0      0 139.224.199.85:45368        106.11.68.13:80             ESTABLISHED 
[root@ ~]# netstat -an|grep ESTABLISHED|grep ":80"
tcp        0      0 139.224.199.85:45368        106.11.68.13:80             ESTABLISHED ```

```

netstat -an|grep ESTABLISHED|grep ":80"|awk 'BEGIN{FS="[[:space:]:]+"}{print $4}'

```
说明：FS 是字段分隔符,简单的可以用多个awk过滤。

如果需要进行整理并排序的话，完整命令如下
```

netstat -an|grep ESTABLISHED|grep ":80"|awk 'BEGIN{FS="[[:space:]:]+"}{print $4}'|sort|uniq -c|sort -nr

```
---
# shell编程
## 用Shell编程，判断一文件是不是字符设备文件，如果是将其拷贝到 /dev 目录下。
```

# !/bin/bash

read -p "Input file name: " FILENAME
if [ -c "FILENAME"];then　　cpFILENAME"];then　　cpFILENAME /dev
fi

```
## 设计一个shell程序，添加一个新组为class1，然后添加属于这个组的30个用户，用户名的形式为stdxx，其中xx从01到30。
```

# !/bin/bash

groupadd class1
for((i=1;i<31;i++))
do

```
    if [ $i -le 10 ];then
            useradd -g class1 std0$i
    else
            useradd -g class1 std$i
    fi
```

done

```
## 编写shell程序，实现自动删除50个账号的功能。账号名为stud1至stud50。
```

# !/bin/bash

for((i=1;i<51;i++))
do

```
            userdel -r stud$i
```

done

```
## 写一个sed命令，修改/tmp/input.txt文件的内容。
要求：(1) 删除所有空行；
(2) 一行中，如果包含"11111"，则在"11111"前面插入"AAA"，在"11111"后面插入"BBB"，比如：将内容为0000111112222的一行改为：0000AAA11111BBB2222

```

[root@~]# cat -n /tmp/input.txt

```
 1  000011111222
 2
 3  000011111222222
 4  11111000000222
 5
 6
 7  111111111111122222222222
 8  2211111111
 9  112222222
10  1122
11
```

# 删除所有空行命令

[root@~]# sed '/^$/d' /tmp/input.txt
000011111222
000011111222222
11111000000222
111111111111122222222222
2211111111
112222222
1122
插入指定的字符
[root@~]# sed 's#(11111)#AAA1BBB#g' /tmp/input.txt
0000AAA11111BBB222
0000AAA11111BBB222222
AAA11111BBB000000222
AAA11111BBBAAA11111BBB11122222222222
22AAA11111BBB111
112222222
1122

```

```