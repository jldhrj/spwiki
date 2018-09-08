转载自 http://www.wjl58.club/index.php/2018/04/13/hello-world/

1：使用qq邮箱发送方法

1. 去qq邮箱开启smtp服务
![img](http://www.wjl58.club/wp-content/uploads/2018/04/9_LV1N3XAPWH0@E6J.png)

![img](http://www.wjl58.club/wp-content/uploads/2018/04/DPY56ND2KQJRUSYT.png)

![img](http://www.wjl58.club/wp-content/uploads/2018/04/VVYTKB3RZF9SYO@W_IC0B.png)

开启这两个服务

![img](http://www.wjl58.club/wp-content/uploads/2018/04/@1KRTLNUW9G4YTXJT7.png)

生成授权码一会要用

![img](http://www.wjl58.club/wp-content/uploads/2018/04/8IY0WRNYDAP_IM0G.png)

2：以下功能需要开启

//邮件设置———————————————————————–
`$System_Config[‘mailDriver’] = ‘smtp’; // mailgun or smtp or sendgrid` 选择发送邮件的方式

//邮箱验证
`$System_Config[‘enable_email_verify’]=‘true’; //`是否启用注册邮箱验证码

3：配置smtp参数，以下为smtp格式填写正确即可发送邮箱（以qq邮箱为例子vu等不封端口的服务器可用）
```
\# smtp
$System_Config[‘smtp_host’] = ‘smtp.qq.com’;

$System_Config[‘smtp_username’] = ‘你的qq号@qq.com’;

$System_Config[‘smtp_port’]= ‘465’;

$System_Config[‘smtp_name’] = ‘发送名’;

$System_Config[‘smtp_sender’] = ‘你的qq号@qq.com’;

$System_Config[‘smtp_passsword’] = ‘刚才生成的授权码’;

$System_Config[‘smtp_ssl’] = ‘true’;
```
再来个例子
```
$System_Config['smtp_host'] = ''; //SMTP服务器地址

$System_Config['smtp_username'] = ''; //发信人邮件地址

$System_Config['smtp_port'] = ''; //SMTP服务器端口

$System_Config['smtp_name'] = ''; //发信人昵称

$System_Config['smtp_sender'] = ''; //发信人邮件地址

$System_Config['smtp_passsword'] = ''; //发信人邮箱登录密码/授权码

$System_Config['smtp_ssl'] = ''; //SMTP服务器SSL设置
```
这样就可以发信了来张示例图

![img](http://www.wjl58.club/wp-content/uploads/2018/04/7IT018O_BLJ3LR26E33.png)


重点如果qq失败并且确定自己服务器的端口没被封看这里登
录自己的qq邮箱你会看到有这条信息，去qq安全中心把
邮箱登录保护开关一下即可，使用gmail的把允许所有应用打开

![img](http://www.wjl58.club/wp-content/uploads/2018/04/O060Z1AHP925W2N-169x300.png)


4：接下来重点讲下谷歌服务器如何配置（博主在这块吃尽了苦头）

1，gcp封锁了所有端口25，465,587(自家邮箱这个端口没封)
2，gcp封了所有除自家（gmail）以外的所有邮箱
3，使用gcp发邮箱必须使用gmail邮箱，而且必须使用587端口
   我之前gmail为啥不行原来是使用了465端口的原因。
4，如果使用587端口，是除了gmail以外的邮箱照样失败，587端口gcp只给自家邮箱放开了
如果你服务器端口没被封25，465，587，那你用哪个邮箱都无所谓
接下来正式开始gcp gmail的教程
1：进入gmail跟qq一样把smtp打开，都大同小异就不一一复述了
gmail如下填即可，一定要记住使用587端口（gcp）

![img](http://www.wjl58.club/wp-content/uploads/2018/04/FZS8FMV8IVU3MTE36AFO-300x203.png)

（此图直接借用群友Ui）


2：配置样板
```
# smtp
`$System_Config['smtp_host'] = 'smtp.gmail.com';`

`$System_Config['smtp_username'] = '你的gmail邮箱@gmail.com';`

`$System_Config['smtp_port'] = '587';`

`$System_Config['smtp_name'] = '发送名与qq邮箱一样';`

`$System_Config['smtp_sender'] = '你的gmail邮箱@gmail.com';`

`$System_Config['smtp_passsword'] = '你的gmail邮箱密码';`

`$System_Config['smtp_ssl'] = 'true';`
```
现在应该就可以发了，来试试![img](http://www.wjl58.club/wp-content/uploads/2018/04/ZPN1F4J9HT1R_CL-169x300.png)

自此gcp谷歌服务器也终于可以发邮件了，完结撒花！！！！


另外一个可以用mailgun 在gcp服务器上发邮件
就这样设置吧如果想知道怎么样注册网上应该有教程 必须要用mailgun其他的没有测试过 https://www.mailgun.com/

smtp

$System_Config['smtp_host'] = 'smtp.mailgun.org';

$System_Config['smtp_username'] = 'mailgun smtp的邮箱';

$System_Config['smtp_port'] = '2525'; 必须得是2525其他都不可以！！！！

$System_Config['smtp_name'] = '就是你网站的名字吧';

$System_Config['smtp_sender'] = 'mailgun smtp的邮箱';

$System_Config['smtp_passsword'] = '你的mailgun Smtp 密码';

$System_Config['smtp_ssl'] = 'false'; 必须得是false不知道为什么感觉很怪

## Sendgrid 使用smtp发送邮件

点控制面板左边的Sender Authentication，按照提示在DNS解析商那边添加CNAME验证域名
```
$System_Config['smtp_host'] = 'smtp.sendgrid.net';
$System_Config['smtp_username'] = 'sendgrid用户名（就是登录sendgrid网页时候填的那个）';
$System_Config['smtp_port'] = '465';
$System_Config['smtp_name'] = '填下面你填在Reply to的邮箱';
$System_Config['smtp_sender'] = '就随便起个名（比如admin.rutou.com）';
$System_Config['smtp_passsword'] = 'sendgrid密码（就是你登陆sendgrid网页的密码）';
$System_Config['smtp_ssl'] = 'true';
```
点控制面板左边的Senders,再点右边的Create New Sender
![img](https://puu.sh/BrPsL/2524615018.jpg)