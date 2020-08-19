> &

当在前台运行某个作业时，终端被该作业占据；可以在命令后面加上& 实现后台运行。

例如：sh test.sh & 

适合在后台运行的命令有find、费时的排序及一些shell脚本。在后台运行作业时要当心：需要用户交互的命令不要放在后台执行，因为这样你的机器就会在那里傻等。

不过，作业在后台运行一样会将结果输出到屏幕上，干扰你的工作。如果放在后台运行的作业会产生大量的输出，最好使用下面的方法把它的输出重定向到某个文件中：

command  >  out.file  2>&1  & 

这样，所有的标准输出和错误输出都将被重定向到一个叫做out.file 的文件中。

PS：当你成功地提交进程以后，就会显示出一个进程号，可以用它来监控该进程，或杀死它。(ps -ef | grep 进程号 或者 kill -9 进程号）

> nohup 

使用&命令后，作业被提交到后台运行，当前控制台没有被占用，但是一但把当前控制台关掉(退出帐户时)，作业就会停止运行。nohup命令可以在你退出帐户之后继续运行相应的进程。nohup就是不挂起的意思

( no hang up)。该命令的一般形式为：

nohup command &

如果使用nohup命令提交作业，那么在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中，除非另外指定了输出文件：

nohup command > myout.file 2>&1 &

使用了nohup之后，很多人就这样不管了，其实这样有可能在当前账户非正常退出或者结束的时候，命令还是自己结束了。所以在使用nohup命令后台运行命令之后，需要使用exit正常退出当前账户，这样才能保证命令一直在后台运行。

ctrl + z  可以将一个正在前台执行的命令放到后台，并且处于暂停状态。

Ctrl+c   终止前台命令。

jobs  查看当前有多少在后台运行的命令。 

jobs -l  选项可显示所有任务的PID，jobs的状态可以是running, stopped, Terminated。但是如果任务被终止了（kill），shell 从当前的shell环境已知的列表中删除任务的进程标识。

例子1：

```shell
[root@bigdatafat026087 PreLoanAuditJob]# jobs -l
[1]+   990 Running                 nohup sh xbsp_error_monitor.sh > xbsp_error_monitor.out 2>&1 &
[root@bigdatafat026087 PreLoanAuditJob]# 
```
例子2：

```shell
nohup sh tiaoe_error_monitor.sh > tiaoe_error_monitor.out 2>&1 &

nohup sh xbsp_error_monitor.sh > xbsp_error_monitor.out 2>&1 &

```

> 2>&1解析

command >out.file 2>&1 &

>> command>out.file是将command的输出重定向到out.file文件，即输出内容不打印到屏幕上，而是输出到out.file文件中。

>> 2>&1 是将标准出错重定向到标准输出，这里的标准输出已经重定向到了out.file文件，即将标准出错也输出到out.file文件中。最后一个&， 是让该命令在后台执行。

>> 试想2>1代表什么，2与>结合代表错误重定向，而1则代表错误重定向到一个文件1，而不代表标准输出；换成2>&1，&与1结合就代表标准输出了，就变成错误重定向到标准输出.
