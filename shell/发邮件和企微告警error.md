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

>> 3.测试
    [root@001 ~]# mail -s "hesaucaq" 83xx@qq.com < /etc/passwd
    
    [root@001 ~]# echo "测试邮件" | mail -s "测试" 83xx@qq.com
    
    以上已经实现了发邮件功能！！！
    
    如遇：554 DT:SPM 发送的邮件内容包含了未被网易许可的信息，或违背了网易的反垃圾服务条款，可以自己邮箱发给自己！
    
 1)  无邮件正文

mail -s "主题"  收件地址

例如： mail -s "测试"  1968089885@foxmail.com


2) 有邮件正文

 mail -s "主题"  收件地址< 文件(邮件正文.txt)
 
例如： mail -s "邮件主题"  1968089885@foxmail.com < /data/findyou.txt

 echo "邮件正文" | mail -s 邮件主题  收件地址
 
例如： echo "邮件正文内容" | mail -s "邮件主题"  1968089885@foxmail.com

 cat 邮件正文.txt | mail -s 邮件主题  收件地址 
 
例如： cat  /data/findyou.txt | mail -s "邮件主题"  1968089885@foxmail.com

   
3)  带附件

 mail -s "主题"  收件地址  -a 附件 < 文件(邮件正文.txt) 
 
例如： mail -s "邮件主题"  1968089885@foxmail.com -a /data/findyou.tar.gz < /data/findyou.txt
