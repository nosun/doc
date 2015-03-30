#starchef 服务器环境部署

###系统环境：centos 64位系统
###安装软件：redis缓存服务器、zimg图片服务器、jdk、tomcat服务器、mysql数据库

##JDK
Java 语言的软件开发工具包。

###安装和环境变量配置
	本环境用的是jdk1.6.0_25.tar直接进行解压配置环境变量即可
	- 进入目录：cd /usr/local/src
	- 解压：tar zxvf jdk1.6.0_25.tar
	修改环境变量：
	- 进入:vi /etc/profile 在done后面添加
		JAVA_HOME=/usr/local/src/jdk1.6.0_25
		CLASSPATH=.:$JAVA_HOME/lib/tools.jar
		PATH=$JAVA_HOME/bin:$PATH
		export JAVA_HOME CLASSPATH PATH
	- 保存退出： :wq
	- 重置：source /etc/profile
	- 检查配置：echo $JAVA_HOME

##Tomcat
Java web 服务容器，配置文件端口请根据测试环境需求更改为 40000

###安装tomcat
	- 下载：wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-7/v7.0.59/bin/apache-tomcat-7.0.59.tar.gz
	- 解压缩：tar zxvf apache-tomcat-7.0.59.tar.gz
	- 移动：mv apache-tomcat-7.0.59 /usr/local/tomcat  #移动目录
			/usr/local/tomcat/bin/startup.sh  #启动tomcat
			/usr/local/tomcat/bin/shutdown.sh #关闭tomcat
###修改tomcat配置文件
	cp /usr/local/tomcat/conf/server.xml  /usr/local/tomcat/conf/server.xmlbak  #备份原有配置文件
	vi /usr/local/tomcat/conf/server.xml  #编辑

	Connector port="8686" protocol="HTTP/1.1"    
	端口设置，默认为8080；注意：防火墙需要开放相应端口
  
###添加tomcat为系统服务，实现开机自启动
	cd /etc/rc.d/init.d  #进入目录
	
    启动脚本 见附件
    
	chmod 755 /etc/rc.d/init.d/tomcat  #添加执行权限  
	chkconfig --add tomcat  #添加服务  
	chkconfig tomcat on  #设置开机启动  
	
##redis
Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库。

	- 进入src目录：cd /usr/local/src
	- 下载：wget http://download.redis.io/releases/redis-2.6.17.tar.gz
	- 解压缩：tar -zxvf redis-2.617.tar.gz
	- 编译安装：make && make install
	- 进入/usr/local/src/redis-2.6.17/src
	- 将四个可执行文件redis-server、redis-benchmark、redis-cli和redis.conf
	  然后拷贝到一个目录下
	   mkdir /usr/redis
	  cp redis-server  /usr/redis
	  cp redis-benchmark /usr/redis
	  cp redis-cli  /usr/redis
	  cp redis.conf  /usr/redis
	  cd /usr/redis
	- 启动Redis服务：redis-server   redis.conf

###为redis添加开机启动项脚本
	
	启动脚本见附件
    为脚本增加执行权限
    chkconfig redis on
	
	修改 /usr/local/redis/redis.conf 配置文件 将daemonize修改为yes	

	service redis stop  #停止
	service redis start #启动
	ps aux |grep redis  #查看状态
	
#### 解决redis启动的三个错误
- 启动错误  
    1.WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.  
    2.WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
    
- 解决方法  
    第一个警告两个方式解决(overcommit_memory)
    1.  echo "vm.overcommit_memory=1" > /etc/sysctl.conf
    2.  echo 1 > /proc/sys/vm/overcommit_memory  不需要启机器就生效
    第二个警告解决
    1. echo 511 > /proc/sys/net/core/somaxconn
	

##zimg安装步骤

zimg是一个具有图片处理功能的图片存储服务。 安装过程中遇到问题，可以参考官方文档 http://zimg.buaa.us/documents/install/

**安装配置完成之后，需要配置外网端口，配置文件/usr/local/zimg/bin/conf/zimg.lua 需要做相应的更改** 本文附件分别有zimg.lua，/usr/local/zimg/bin/www/ 文件需要做替换。

###openssl

   	-下载：wget http://www.openssl.org/source/openssl-1.0.1i.tar.gz
 	-解压缩：tar zxvf openssl-1.0.1i.tar.gz
	- 进入目录：cd openssl-1.0.1i
	- 预编译：./config shared --prefix=/usr/local --openssldir=/usr/ssl
	- 安装：make && make install 

###cmake

	- 下载：wget http://www.cmake.org/files/v3.0/cmake-3.0.1.tar.gz
	- 解压缩：tar xzvf cmake-3.0.1.tar.gz 
	- 进入目录：cd cmake-3.0.1
	- 预编译：./bootstrap --prefix=/usr/local 
	- 安装：make && make install 

###libevent

	- 下载：wget http://cloud.github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz
	- 解压缩：tar zxvf libevent-2.0.21-stable.tar.gz
	- 进入目录：cd libevent-2.0.21-stable
	- 预编译：./configure --prefix=/usr/local 
	- 安装：make && make install 

###nasm
	- 下载：wget http://www.nasm.us/pub/nasm/releasebuilds/2.11.06/nasm-2.11.06.tar.gz
	- 解压缩：tar zxvf nasm-2.11.06.tar.gz
	- 编译：./configure
	- 安装：make && make install
	- 查看：nasm –version(查看是否安装成功)
	
###libjpeg-turbo ( recommend )
	- 下载：wget https://downloads.sourceforge.net/project/libjpeg-turbo/1.3.1/libjpeg-turbo-1.3.1.tar.gz
	- 解压缩：tar zxvf libjpeg-turbo-1.3.1.tar.gz
	- 进入目录：cd libjpeg-turbo-1.3.1
	- 预编译：./configure --prefix=/usr/local --with-jpeg8
	- 安装：make && make install	

###webp
	- 下载:wget http://ftp.jaist.ac.jp/pub/sourceforge/m/ms/msys2/REPOS/MINGW/Sources/mingw-w64-i686-libwebp-0.4.2-1.src.tar.gz(双重包，建议手动下载)
	- 解压缩：tar zxvf libwebp-0.4.2.tar.gz
	- 编译：./configure
	- 安装：make
			sudo make install

###imagemagick
	- 下载：wget http://www.imagemagick.org/download/ImageMagick-6.9.1-0.tar.gz
	- 解压缩：tar zxvf ImageMagick-6.9.1-0.tar.gz
	- 进入目录：cd ImageMagick-6.9.1-0
	- 预编译：./configure  --prefix=/usr/local 
	- 安装：make && make install 

###libmemcached
	- 下载：wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz
	- 解压缩：tar zxvf libmemcached-1.0.18.tar.gz
	- 进入目录：cd libmemcached-1.0.18
	- 预编译：./configure -prefix=/usr/local 
	- 安装：make &&　make install 

###Build zimg
	- 编译：wget https://github.com/buaazp/zimg/archive/v3.1.0.tar.gz
	- 解压缩：tar zxvf v3.1.0.tar.gz
	- 编译安装：make &&　make install 

###将zimg拷贝到/usr/local/下

    mv /usr/local/src/zimg-3.1.0 /usr/local/zimg

###设置开机启动脚本

    启动脚本见附件
    为脚本增加执行权限
	chkconfig zimg on  #设置开机启动

##mysql安装和设置

略

##附件说明

附件分别为
- init.d文件夹，存放有三个服务启动脚本，对应放在/etc/init.d/下；
- zimg文件夹，存放至 /usr/local/文件夹下，对应安装好的zimg文件夹，由于zimg默认开放 4869端口，图片服务需要此服务，请开放此端口，或者修改配置文件并开放相应的端口。