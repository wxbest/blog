require_once('class.phpmailer.php');

require_once('class.smtp.php');

$mail = new PHPMailer();

$mail->CharSet ='utf-8'; //编码要指定 

$mail->Encoding = 'base64';

$mail->SMTPSecure = "ssl";  //这步是关键

$mail->IsSMTP(); // 代表我们使用SMTP的方式

$mail->Port=465;  //使用465端口

$mail->Host = "smtp.163.com"; // SMTP servers  //腾讯的就是这样的地址

$mail->SMTPAuth = true; // turn on SMTP authentication

$mail->SMTPDebug  = false;  

$mail->Username = "haorenergou@163.com"; // SMTP 你的邮箱用户名

$mail->Password = "haorenergou"; // SMTP 口令

$mail->From = "haorenergou@163.com"; 

$mail->FromName = "你的大佬"; 

$to = "531432012@qq.com"; 

$mail->AddAddress($to); 

$mail->Subject = "phpmailer测试标题"; 

$mail->Body = "<h1>phpmail演示</h1>这是php点点通（<font color=red>www.phpddt.com</font>）对phpmailer的测试内容"; 

//$mail->AltBody = "To view the message, please use an HTML compatible email viewer!"; //当邮件不支持html时备用显示，可以省略 

//$mail->WordWrap = 80; // 设置每行字符串的长度 

//$mail->AddAttachment("f:/test.png"); //可以添加附件 

$mail->IsHTML(true); 

$mail->Send(); 

使用前必须打开163邮箱的SMTP

发送邮件可以在163邮箱查看

接收不到邮件原因是：

1）对方的邮箱地址是不是填写错误（数字或者字母少了一位，com写成了con）；

2）邮件是否在对方的垃圾箱中，可以让对方检查一下垃圾箱；

3）最不可能的情况，邮件服务器出现异常。

要是仍然出现问题，建议重新发送一封邮件，或者发送到对方的其他邮箱尝试一下。