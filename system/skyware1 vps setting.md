##新的vps 环境配置

### 初始配置
- 挂载data磁盘，并进行4k优化
- 更新yum源，并yum update 更新了内核
- 配置IPtables防火墙，开放22 9999 8080 1883四个端口

### 安装nginx
  安装nginx,修改nginx.conf
  设置开机启动;
  
### 安装php

  安装php55w,php-fpm,php扩展
  设置php-fpm开机启动;
  
### 安装mysql
  yum 安装mysql,设置开机启动，为mysql配置初始密码。
  
### jdk
  之前考虑使用java的云平台版本，因此下载安装了jdk 和tomcat，目前未使用
  下载并安装jdk1.7.0_65 版本，rpm 版本，用rpm -ivh 命令进行安装，安装完之后配置/etc/profile文件，修改path info

### 设置工作目录/data
将data文件夹设置为数据文件夹，log，backup，www都设在此下。

###安装redis
    wget
    make
    make install
    将四个可执行文件redis-server、redis-benchmark、redis-cli和redis.conf 分别拷贝到/usr/redis下，和/etc下
	  cp redis-server  /usr/redis
	  cp redis-benchmark /usr/redis
	  cp redis-cli  /usr/redis
	  cp redis.conf  /etc
    init.d/redis
    vi /etc/redis.conf 将redis 设置为后台运行;
    chkconfig redis on
#### 解决redis启动的三个错误
- 启动错误  
    1.WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.  
    2.WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
    
- 解决方法  
    第一个警告两个方式解决(overcommit_memory)
    1. echo "vm.overcommit_memory=1" > /etc/sysctl.conf
    2. echo 1 > /proc/sys/vm/overcommit_memory  不需要启机器就生效
    第二个警告解决
    1. echo 511 > /proc/sys/net/core/somaxconn

###安装mosquitto

    更新yum源，详见http://mosquitto.org/download/
    yum install mosquitto
    chkconfig mosquitto on
    
    因为yum安装少libmosquitto.so.1库，导致php扩展无法安装，mosquitto_sub无法使用，因此重新做了编译安装。

    wget http://www.openssl.org/source/openssl-1.0.1i.tar.gz
	tar zxvf openssl-1.0.1i.tar.gz
	cd openssl-1.0.1i
	./config shared --prefix=/usr/local --openssldir=/usr/ssl
	make && make install 
	
#### ssl.h 的问题
yum系统中的ssl库有问题，或者说因为是未编译安装的，因此系统会少很多扩展库，从而不能为其他需要这个包的软件服务。
zimg的编译过程中需要这个，mosquitto的编译过程中也需要这个。

	wget http://www.openssl.org/source/openssl-1.0.1i.tar.gz
	tar zxvf openssl-1.0.1i.tar.gz
	cd openssl-1.0.1i
	./config shared --prefix=/usr/local --openssldir=/usr/ssl
	make && make install 
	
#### uuid 的问题
编译时遇到uuid/uuid.h: No such file or directory？  
你需要先安装uuid库, 用于生成全局唯一ID, uuid库是e2fsprogs包工具的一部分；

	yum install libuuid-devel
    make and make install ok
    
#### 链接库文件

	ln -s /usr/local/lib/libmosquitto.so.1 /usr/lib/libmosquitto.so.1
	ln -s /usr/local/lib64/libssl.so.1.0.0  /usr/lib/libssl.so.1.0.0
	ln -s /usr/local/lib64/libcrypto.so.1.0.0 /usr/lib/libcrypto.so.1.0.0
	
	ldconfig 

### 安装php-redis 扩展
    
    wget https://github.com/phpredis/phpredis/archive/2.2.7.tar.gz
    phpize
    ./configure
    make && make install
    在/etc/php.d/ 下增加扩展加载 redis.ini文件
    其中写入 extension=redis.so

### 安装mosquitto-php扩展
	wget from github
	./configure
	make && make install
    在/etc/php.d/ 下增加扩展加载 mosquitto.ini文件
    其中写入 extension=mosquittto.so
	
### 安装swoole-php扩展
    wget swoole
    tar -zxvf swoole
    cd swoole-src
    phpize
    ./configure
    make && make install
    在/etc/php.d/ 下增加扩展加载 swoole.ini文件
    其中写入 extension=swoole.so    

### 安装posix扩展
    yum install php5w-posix
    
### 安装zimg
    详见 zimg 服务搭建;
    期间遇到zimg无法上传图片的问题，原因是imageMagic库的依赖没有安装，光编译imageMagic是不行的，于是yum了一下imageMagic，然后重新编译了imageMagic.
    设置 zimg 开机自启动;
    
### 优化系统内核参数
    修改sysctl.conf
    
    net.ipv4.tcp_syncookies=1
    net.ipv4.tcp_max_syn_backlog=81920
    net.ipv4.tcp_synack_retries=3
    net.ipv4.tcp_syn_retries=3
    net.ipv4.tcp_fin_timeout = 30
    net.ipv4.tcp_keepalive_time = 300
    net.ipv4.tcp_tw_reuse = 1
    net.ipv4.tcp_tw_recycle = 1
    net.ipv4.ip_local_port_range = 20000 65000
    net.ipv4.tcp_max_tw_buckets = 200000
    net.ipv4.route.max_size = 5242880
    
    net.core.wmem_default = 8388608
    net.core.rmem_default = 8388608
    net.core.rmem_max = 16777216
    net.core.wmem_max = 16777216
    net.unix.max_dgram_qlen =100
    vm.overcommit_memory=1
    
### 设置Reids.conf
    daemonize yes
    
### 设置Mosquitto.conf
    做了部分设置，详情见mosquitto.conf

### 设置Nginx配置文件
    yun.conf
    c1.conf

### 设置My.cnf
    /etc/my.cnf

### 设置php.ini
    /etc/php.ini

### 设置php-fpm参数
    php-fpm.d/www.conf

### 设置Swoole server参数
    /www/c1.skyware.com.cn/Server/bin

### 设置web服务参数
    /www/c1.skyware.com.cn/app的参数
    
### 域名解析
### 登录重定向


