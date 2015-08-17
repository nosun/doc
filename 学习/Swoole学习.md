##swoole学习

###swoole安装
####教程地址：[http://wiki.swoole.com/wiki/page/6.html](http://wiki.swoole.com/wiki/page/6.html "编译安装swoole")
###安装过程
- 下载swoole 1.7.4 stable版本，解压缩，并编译安装  
	cd /usr/local/src  
	wget https://github.com/swoole/swoole-src/archive/  swoole-1.7.4-stable.tar.gz  
	tar -zxvf swoole-1.7.4-stable.tar.gz  
	cd  swoole-1.7.4-stable  
	phpize  (由于没有安装php-devel报错，yum 安装)
	./configure  （由于没有安装gcc报错，yum 安装gcc）
	make   
	sudo make install  
- 在/etc/php.d/目录下，增加swoole.ini
- 重新启动php-fpm   
	service php-fpm reload
- 检查扩展是否安装成功
	php -m（查看扩展列表）
- demean模式关闭的方法
	ps -aux |grep "##.php" 查看demean进程
	kill -9  pid 关闭子进程
	kill -15 pid 直接关闭守护进程
- 外网调试，要注意打开防火墙端口。
- 升级php5.3.3 到php5.4  
	rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm //更新源  
	yum install yum-plugin-replace  安装yum replace插件  
	yum replace php-common --replace-with=php54w-common //升级php  
- 重新编译安装swoole
	目录：/user/local/etc   
	下载：swoole最新 stable版本1.7.8  
    so文件安装至 /usr/lib64/php/modules/
    增加swoole.ini 文件至 /etc/php.d/ 目录下


###swoole 聊天室
- 半桶水，泽泽，主要讲了swoole内置http服务器的基本实现，以及性能。

###tcpip协议
- 链路层，网络层，传输层，应用层
- NAT网络映射：由于ip地址不够导致，采用NAT技术。
- socket/内核/网卡：伯克利大学设计了很多api，被广泛采用：内核将应用层的内存数据写入网卡，并将网卡传入的数据拷贝到应用层的内存，Read/Write中的内存地址，内核为每一个TCP连接创建文件描述符，Accept/close创建文件描述符，关闭连接。操作socketAPI。内核负责内存和网卡之间的数据交换。

###udp协议
- 最大不超过64k
- 包式协议，有消息边界，一次必然是一个包
- 不保证可靠性，数据包有肯能个会丢失
- 不保证顺寻，多个数据包的顺序有可能颠倒

	<?php 
	$server =new swoole_server('127.0.0.1',9502,SWOOLE_PROGRESS,SWOOLE_SOCK_UDP);
	$server->set(array('work_num'=>4,'dispatch_mode'=>2));
	$server->on("receive",function($serv,$fd,$from_id,$data){
		$serv->send($fd,'Swoole:'.$data,$from_id);

	})
	$server->start();

	netcat 可以取代telnet 做一些客户端网络测试。

- udp服务器的适用场景
	- 允许小部分数据丢失，如统计和日志
	- 实时性要求高，比如视频流，丢一部分帧关系不大。
	- 如果程序自行保证可靠性（丢失重传，包序列），可以无视UDP存在的问题。
- TCP服务器协议设计
	- 流式协议，没有消息边界。客户端向服务器发送1次数据，可能会被服务器多次收到，也可能客户端发送多次数据一次性全部收到。
	- 保证传输的可靠性，顺序。
	- TCP有拥塞控制，所以数据包可能会延迟。
-  SWOOLE提供了2种同工的TCP协议解析
	-  EOF检测
	-  固定包头+包体（解决了数据的合并和拆分），不支持变长字节长度。包头内有一个固定字节的长度。



