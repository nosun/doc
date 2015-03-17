##新的vps 环境配置

- 1、挂载data磁盘，并进行4k优化
- 2、更新yum源，并yum update 更新了内核
- 3、配置IPtables防火墙，开放22 9999 8080 1883四个端口
- 4、安装nginx php55w mysql，并设置开机启动，修改nginx，php-fpm，php.ini 等配置文件，为mysql配置初始密码。
- 5、下载并安装jdk1.7.0_65 版本，rpm 版本，用rpm -ivh 命令进行安装，安装完之后配置/etc/profile文件，修改path info
- 6、下载并安装最新版本的tomcat，删除webapp下的初始项目时将云平台的m文件夹从skyware1上拷贝过来，数据库也做了拷贝动作，不知道什么原因没能正常运行9999端口，不过tomcat正常运行了。
- 7、将data文件夹设置为数据文件夹，log，backup，www都设在此下。