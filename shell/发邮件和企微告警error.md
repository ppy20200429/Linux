### linux下使用自带mail发送邮件
> 具体步骤如下：
>> 1.需要安装mailx工具，mailx是一个小型的邮件发送程序   yum install mailx

>> 2.编辑配置文件   vim /etc/mail.rc 

>>> 具体配置如下：

    set from=xxxx@126.com
    
    set smtp=smtp.126.com
    
    set smtp-auth-user=xx@126.com
    
    set smtp-auth-password=xxx
    
    set smtp-auth=login
    
    ---说明
    
    from：对方收到邮件时显示的发件人
    
    smtp：指定第三方发邮件的smtp服务器地址
    
    set smtp-auth-user：第三方发邮件的用户名
    
    set smtp-auth-password：用户名对应的密码,有些邮箱填的是授权码
    
    smtp-auth：SMTP的认证方式，默认是login，也可以改成CRAM-MD5或PLAIN方式

