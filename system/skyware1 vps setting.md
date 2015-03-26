##新的vps 环境配置

### 初始配置
- 挂载data磁盘，并进行4k优化
- 更新yum源，并yum update 更新了内核
- 配置IPtables防火墙，开放22 9999 8080 1883四个端口

### 安装nginx
  安装nginx,修改nginx.conf
  设置开机启动;
  
### 安装php

  安装php55w,php-fpm,php扩展,修改php.ini，修改php-fpm.conf
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
    mv to /usr/local/
    init.d/redis
    vi /etc/redis.conf
    chkconfig redis on

###安装mosquitto
    更新yum源，详见http://mosquitto.org/download/
    yum install mosquitto
    chkconfig mosquitto on
    配置mosquitto
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

	yum install libuuid-devel
    make and make install ok
    
#### 链接库文件

	ln -s /usr/local/lib/libmosquitto.so.1 /usr/lib/libmosquitto.so.1
	ln -s /usr/local/lib64/libssl.so.1.0.0  /usr/lib/libssl.so.1.0.0
	ln -s /usr/local/lib64/libcrypto.so.1.0.0 /usr/lib/libcrypto.so.1.0.0
	
	ldconfig 

### 安装mosquitto-php扩展
	wget from github
	./configure
	make && make install
	
### 安装swoole-php扩展
    wget swoole
    tar -zxvf swoole
    cd swoole-src
    phpize
    ./configure
    make && make install

### 安装posix扩展
    yum install php5w-posix
    
### 安装zimg
