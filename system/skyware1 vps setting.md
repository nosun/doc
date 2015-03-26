##新的vps 环境配置

- 1、挂载data磁盘，并进行4k优化
- 2、更新yum源，并yum update 更新了内核
- 3、配置IPtables防火墙，开放22 9999 8080 1883四个端口
- 4、安装nginx php55w mysql，并设置开机启动，修改nginx，php-fpm，php.ini 等配置文件，为mysql配置初始密码。
- 5、下载并安装jdk1.7.0_65 版本，rpm 版本，用rpm -ivh 命令进行安装，安装完之后配置/etc/profile文件，修改path info
- 6、将data文件夹设置为数据文件夹，log，backup，www都设在此下。
- 7、安装redis
    wget
    make
    make install
    mv to /usr/local/
    init.d/redis
    vi /etc/redis.conf
    chkconfig redis on

- 8、安装mosquitto
    更新yum源，详见http://mosquitto.org/download/
    yum install mosquitto
    chkconfig mosquitto on
    配置mosquitto
    
- 9、安装swoole-php扩展
    wget swoole
    tar -zxvf swoole
    cd swoole-src
    phpize
    ./configure
    make && make install

- 11、安装posix扩展
    yum install php5w-posix
    
- 10、安装mosquitto-php扩展
    

- 12、安装zimg