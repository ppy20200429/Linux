python'''
#!/bin/sh

# 监控-参数设置
type="线下跑批详版审批Job"
monitor_file="/www/PreLoanAuditJob/current/logs/error.log"
receive_addr="chenyulong@ppdai.com yangpeipei@ppdai.com"

# 如休眠10分钟,使用10m,每sleep_time去检测一次monitor_file文件
sleep_time=10s

# 发邮件和企微上限
notice_limit=2

# error数量阈值
error_limit=50

# 监控-邮件&企业微信处理
error_num=0
content=""
error_msg=""
current_ip=$(ifconfig | grep 'inet'| grep -v '127.0.0.1'  |grep -v 172 | cut -d: -f2 | awk '{ print $1}')

# 发送企业微信接口的相关参数

uuid=$(cat /proc/sys/kernel/random/uuid)
corpid="wx23640668e63d634a"
corpsecret="tvfr54g7upRzWX9Tqq5g60wAGr8MouX_8eBHIMCZ09s"
agentid=1000019
content=""
wechart_url="http://qcgaojing.ppdapi.com/api_model?mark=qcgaojing"

# 发送企业微信post接口
function send_enterprise_wechat(){
  uuid=$(cat /proc/sys/kernel/random/uuid)
  wechart_receive=${receive_addr/" "/"|"}
  return $(curl -H "Content-Type:application/json;charset=UTF-8" -X POST -d '{"uuid":"'${uuid}'", "corpid":"'${1}'","corpsecret":"'${2}'","agentid":1000019,"touser":"'${wechart_receive}'","content":"'${4}'"}' $5)

}



total_times=0
echo "$type monitor starting..."
while :
do
 error_num=$(grep -s -c 'ERROR' $monitor_file)
 if [[ "$error_num" -eq "" ]]
 then 
   error_num=0
 fi
 echo "错误记录数:$error_num"
 # error_num > error_limit 且 total_times < notice_limit时，发送邮件告警与企业微信
 if [ $error_num -gt $error_limit -a $total_times -lt $notice_limit ] 
 then
 
   # 发邮件
   error_msg=$(tail -n100 $monitor_file)
   content="$type-监控发现$error_num个错误,请尽快处理！部分错误如下：$error_msg"
   echo "$content" |  mail -s "$type-监控报警-$current_ip" $receive_addr
   
   # 发企业微信
   error_msg=$(tail -n1 $monitor_file)
   content="$type($current_ip)-监控发现$error_num个错误,请尽快处理!"
   send_enterprise_wechat "$corpid" "$corpsecret" $agentid "$content" "$wechart_url"
   echo "调用企微状态:$?"
   total_times=$(( $total_times + 1 ))
   echo "告警通知次数:$total_times"
 fi
 sleep $sleep_time
 # error_num <=error_limit 且 total_times >=notice_limit 说明错误信息已经处理，把控制次数计算器设为0
 if [[ $error_num -lt $error_limit || $error_num -eq $error_limit ]] && [[ $total_times -gt $notice_limit || $total_times -eq $notice_limit ]]
 then
   total_times=0
   echo "重置告警通知计数器为:$total_times"
 fi
done
echo "$type monitor end..."

'''
