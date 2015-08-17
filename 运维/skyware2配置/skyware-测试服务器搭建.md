## Skyware2 测试

### 初始配置
- 挂载data磁盘，并进行4k优化
- 更新yum源，并yum update 更新内核
- 配置IPtables防火墙，开放22 9999 8080 1883四个端口

### 安装nginx
	
	yum install nginx
	chkconfig nginx on
  
### 安装php

	rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm
	yum install php55w php55w-fpm php55w-opcache php55w-devel php55w-gd php55w-mbstring php55w-mcrypt php55w-mysql php55w-pdo php55w-process php55w-xml php55w-pecl-memcache
	chkconfig php-fpm on
  
### 安装mysql

	yum install mysql mysql-server
	yum install mysql mysql-server  #输入Y即可自动安装,直到安装完成
	/etc/init.d/mysqld start #启动MySQL
	chkconfig mysqld on #设为开机启动
	cp /usr/share/mysql/my-medium.cnf /etc/my.cnf #拷贝配置文件（注意：如果/etc目录下面默认有一个my.cnf，直接覆盖即可）
	mysql_secure_installation
	
回车，根据提示输入Y，输入2次密码，回车，根据提示一路输入Y.
MySql密码设置完成，重新启动 MySQL：

	service mysqld restart
  
###安装redis
    wget http://download.redis.io/releases/redis-3.0.3.tar.gz
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

#### ssl 库安装

	wget http://www.openssl.org/source/openssl-1.0.1i.tar.gz
	tar zxvf openssl-1.0.1i.tar.gz
	cd openssl-1.0.1i
	./config shared --prefix=/usr/local --openssldir=/usr/ssl
	make && make install 

#### uuid 库安装

	yum install libuuid-devel 

### 编译安装mosquitto

	wget http://mosquitto.org/files/source/mosquitto-1.4.2.tar.gz
	tar -zxvf mosquitto-1.4.2.tar.gz 
	cd mosquitto-1.4.2
	make && make install
    
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


### 替换配置文件
将/etc文件夹下的所有配置文件更新至服务器上
- redis
- mosquitto
- php
- mysql
- nginx
- sysctl
    
### 部署云服务


### 部署接口服务


### 部署管理后台

### 安装数据库


### 域名解析
t3.skyware.com.cn 管理后台
c3.skyware.com.cn 接口

### 设置工作目录/data
将data文件夹设置为数据文件夹，log，backup，www都设在此下。
