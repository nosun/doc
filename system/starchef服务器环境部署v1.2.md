#starchef 服务器环境部署

    version: 1.2.1
    author:  nosun
    date:    2015-04-01
    log:     修正zimg安装部分错误

## 一、基础环境
**系统环境：**centos 64位系统  
**安装软件：**redis缓存服务器、zimg图片服务器、java jdk、tomcat服务器、mysql数据库

## 二、软件及数据
### 1、软件
软件位于 src目录下,包含软件如下:
- jdk-7u60-linux-x64.rpm
- apache-tomcat-7.0.54.tar.gz 
- redis-2.8.19.tar.gz
- openssl-1.0.1i.tar.gz        
- cmake-3.0.1.tar.gz
- libevent-2.0.21-stable.tar.gz
- nasm-2.11.06.tar.gz
- libjpeg-turbo-1.3.1.tar.gz
- libwebp-0.4.2.tar.gz
- ImageMagick-6.9.0-5.tar.gz
- libmemcached-1.0.18.tar.gz
- zimg.rar

以上软件，可以用提供的包，也可以从网上下载，jdk，apache，redis 用最新的版本即可，下面的一些包主要是zimg（图片服务器）依赖的包，需要注意版本，只要比当前版本新即可，主要是yum源的版本太旧，因此需要编译安装。

### 2、部署文件
位于 file 目录下，包含文件如下：
- starchef.zip          mysql数据库文件
- img.tar.gz            zimg下的图片初始数据
- starchef.war          starchef web war包

### 3、脚本
位于 init.d 目录下, 包含文件如下：
- zimg   zimg开机自启动的脚本
- redis  redis开机自启动的脚本
- tomcat tomcat开机自启动的脚本

### 4、配置文件
位于 conf 目录下，包含文件,
- zimg.lua  zimg 配置文件

## 二、软件安装

### 1、约定
- /usr/local/src 作为软件默认源文件存放和编译的目录
- /usr/local 如果不做特殊要求，则作为软件默认安装的目录
- 将附件 /soft 下的安装包都拷贝到 /usr/local/src 下

### 2、JDK安装和环境变量配置

本环境用的是jdk-7u60-linux-x64.rpm
	- 进入目录：cd /usr/local/src
	- 安装： rpm -ivh jdk-7u60-linux-x64.rpm
	
	修改环境变量：
	- 进入:vi /etc/profile 在文件末尾添加
		JAVA_HOME=/usr/local/src/jdk1.7.0_60
		CLASSPATH=.:$JAVA_HOME/lib/tools.jar
		PATH=$JAVA_HOME/bin:$PATH
	    export JAVA_HOME CLASSPATH PATH
	- 保存退出：wq!
	- 使之生效：source /etc/profile
	- 检查配置：echo $JAVA_HOME

### 3、Tomcat
    解压：tar apache-tomcat-7.0.54.tar.gz
    移动：mv apache-tomcat-7.0.54 /usr/local/tomcat  #移动目录
    配置：vi /usr/local/tomcat/conf/server.xml  #编辑
        
    Connector port="8686" protocol="HTTP/1.1"    
    端口设置，默认为8080；注意：防火墙需要开放相应端口

#### 添加启动该脚本
    
启动脚本 init.d 目录下，放至/etc/init.d/下，脚本 tomcat，启动脚本中涉及jdk地址与tomcat地址，请根据实际的情况检查一下。
    
	chmod 755 /etc/rc.d/init.d/tomcat  #添加执行权限  
	chkconfig --add tomcat  #添加服务  
	chkconfig tomcat on  #设置开机启动  
	
### 3、redis
Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库。

	解压缩：tar -zxvf redis-2.8.19.tar.gz
	编译安装：make && make install
	进入/usr/local/src/redis-2.6.17/src，将四个可执行文件redis-server、redis-benchmark、redis-cli和redis.conf 拷贝到 /usr/redis 下
	mkdir -p /usr/redis
	cp redis-server  /usr/redis
	cp redis-benchmark /usr/redis
	cp redis-cli  /usr/redis
	cp redis.conf  /usr/redis

#### 为redis添加开机启动项脚本
	
	启动脚本见 init.d 下redis,放至/etc/init.d/下
    chkconfig redis on  为脚本增加执行权限
	
	修改 /usr/local/redis/redis.conf
	
	将daemonize修改为yes	

	service redis start  #启动


### 4、 yum 安装 mysql

    yum install mysql mysql-server   #询问是否要安装，输入Y即可自动安装,直到安装完成
    chkconfig mysqld on   #设为开机启动
    cp /usr/share/mysql/my-medium.cnf   /etc/my.cnf  #拷贝配置文件
    service mysqld start
    
    mysql_secure_installation
    回车，根据提示输入Y
    输入2次密码，回车
    根据提示一路输入Y
    最后出现：Thanks for using MySQL!
    MySql密码设置完成，重新启动 MySQL：
    service mysqld restart    #重启

### 5、zimg安装步骤

zimg是一个具有图片处理功能的图片存储服务。 安装过程中遇到问题，可以参考官方文档 http://zimg.buaa.us/documents/install/

**安装配置完成之后，需要配置外网端口，配置文件/usr/local/zimg/bin/conf/zimg.lua 需要做相应的更改** （详见附件conf 目录有 zimg.lua文件）


#### 基础环境

- yum groupinstall development tool  
一键安装所需的编译环境
- yum install ImageMagick && yum remove ImageMagick
安装ImageMagick的依赖包，但是删除ImageMagick，因为版本太旧。

#### 安装 openssl

 	解压缩：tar zxvf openssl-1.0.1i.tar.gz
	进入目录：cd openssl-1.0.1i
	预编译：./config shared --prefix=/usr/local --openssldir=/usr/ssl
	安装：make && make install 

#### 安装 cmake

	解压缩：tar xzvf cmake-3.0.1.tar.gz 
	进入目录：cd cmake-3.0.1
	预编译：./bootstrap --prefix=/usr/local 
	安装：make && make install 

#### 安装 libevent

	解压缩：tar zxvf libevent-2.0.21-stable.tar.gz
	进入目录：cd libevent-2.0.21-stable
	预编译：./configure --prefix=/usr/local 
	安装：make && make install 

#### 安装 nasm

	解压缩：tar zxvf nasm-2.11.06.tar.gz
	编译：./configure
	安装：make && make install
	
#### 安装 libjpeg-turbo ( recommend )
	
	解压缩：tar zxvf libjpeg-turbo-1.3.1.tar.gz
	进入目录：cd libjpeg-turbo-1.3.1
	预编译：./configure --prefix=/usr/local --with-jpeg8
	安装：make && make install	

#### 安装 webp

	解压缩：tar zxvf libwebp-0.4.2.tar.gz
	编译：./configure
	安装：make && make install

#### 安装 imagemagick

	解压缩：tar zxvf ImageMagick-6.9.0-5.tar.gz
	进入目录：cd ImageMagick-6.9.0-5
	预编译：./configure  --prefix=/usr/local 
	安装：make && make install 

#### 安装 libmemcached

	解压缩：tar zxvf libmemcached-1.0.18.tar.gz
	进入目录：cd libmemcached-1.0.18
	预编译：./configure -prefix=/usr/local 
	安装：make &&　make install 

#### 安装 zimg

	下载：git clone https://github.com/buaazp/zimg -b master --depth=1
	编译：make 
    mv /usr/local/src/zimg /usr/local/zimg

##### 设置开机启动脚本

    启动脚本见 init.d/zimg 放到 /init.d/下
    chmod +x zimg      #为脚本增加执行权限
	chkconfig zimg on  #设置开机启动
	zimg 使用 4869 端口，如果不用此端口，需要修改 zimg.lua配置文件

##### 更换配置文件
    配置文件在 /conf 下， 更换/usr/local/zimg/bin/conf/zimg.lua
    启动 zimg   service zimg start
    

### 三、文件布置

- starchef.zip          mysql数据库文件
- img.tar.gz            zimg下的图片初始数据
- starchef.war          starchef web war包

#### 1、mysql 数据库部署
脚本位于 file/starchef.zip

    unzip starchef.zip，得到 starchef.sql 文件
    
进入mysql-server
    mysql -u root -p 输入密码，进入mysql shell
    
输入 

    CREATE DATABASE starchef DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci
    use starchef # change database
    source starchef.sql # install database
    
#### 2、img资源文件

img.tar.gz 是图片用例的压缩包，压缩包解压缩后放到 /usr/local/zimg/bin/ 下，覆盖之前的img文件夹，主要是增加了测试图片用例。

#### 3、配置 web 服务器

将starchef.war 放置于 /usr/local/tomcat/webapps 下

修改数据库配置文件,路径为：/usr/local/tomcat/webapps/starchef/WEB-INF/classes/下

修改mysql配置文件：dbconfig.properties

    hibernate.dialect=org.hibernate.dialect.MySQLDialect
    validationQuery.sqlserver=SELECT 1
    jdbc.url.jeecg=jdbc:mysql://localhost:3306/starchef?useUnicode=true&characterEncoding=UTF-8   #localhost：3306 地址及端口
    jdbc.username.jeecg=root    #用户
    jdbc.password.jeecg=rensheng #密码
    jdbc.dbType=mysql
    hibernate.hbm2ddl.auto=none

修改 地址，端口，用户，密码；

修改redis配置文件：redis.properties

    redis.port=6379           #端口
    redis.poolSize=1200
    redis.host = 127.0.0.1    #地址
    redis.password=rensheng   #密码
    redis.timeout=3000

配置文件中的 地址，端口，密码请做相应的更改，redis的密码如果没有就空着。

redis 配置文件位于/usr/redis/redis.conf 可以不修改。

#### 4、 修改静态页面

静态页面位于/usr/local/tomcat/webapps/starchef/static下，主要是服务用到的一些内容页面，目前是用ajax方式呈现，动态获取服务器资源。

静态页面中涉及到 图片服务器地址，web服务api接口地址，因此需要根据分配的测试地址更换一下，（static文件夹将index.html1、index.html2、share1.html、share2.html中的url地址前段的ip和端口号，修改为自己对应得ip和端口号。图片地址修改为对应的ip和端口号） 

    cd /usr/local/tomcat/webapps/starchef/static

运行以下命令，其中tomcatserver：port 与imgserver:port需要做相应的更换处理。

    sed -i 's/192.168.1.50:8686/tomcatserver:port/g' `grep  192.168.1.50:8686 -rl ./`
    sed -i 's/192.168.1.50:4869/imgserver:port/g' `grep  192.168.1.50:4869 -rl ./`
