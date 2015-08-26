## Linux 常用命令

### 系统操作

- 查看linux 版本信息

> lsb_release -a

- 查看linux 内核信息

> cat /proc/version

## 抓 包

### tcpdump 抓包

	tcpdump -i eth1 host 106.37.103.250 and dst port 55555 -w /root/4.log
	

### shell

    #!/bin/bash
    PID=`ps aux|grep 进程名|grep master|awk '{print $2}'`
    kill -15 $PID
    echo "kill process : $PID"
    
在 Linux 上可用以下语句看了一下服务器的TCP状态(连接状态数量统计)： 

	netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}' 
	
	返回结果范例如下： 
	
	ESTABLISHED 1423 
	FIN_WAIT1 1 
	FIN_WAIT2 262 
	SYN_SENT 1 
	TIME_WAIT 962