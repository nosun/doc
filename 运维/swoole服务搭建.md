##工作记录

- 安装php55w
   
     rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
            
    yum install php55w  php55w-bcmath php55w-cli php55w-common  php55w-devel php55w-fpm    php55w-gd php55w-imap  php55w-ldap php55w-mbstring php55w-mcrypt php55w-mysql   php55w-odbc   php55w-pdo   php55w-pear  php55w-pecl-igbinary  php55w-xml php55w-xmlrpc php55w-opcache php55w-intl php55w-pecl-memcache

- 查询当前目录下深度为1的文件大小统计结果。
	du -h --max-depth=1

- php扩展被安装到了哪里？

	find / -name "swoole.so" 

很快就找到了swoole.so被安装的位置,php扩展的安装位置在 /usr/lib64/php/modules/ 下，如果要升级自己编译的扩展，直接删除掉，然后重新编译就可以了。

- 用composer 安装swoole扩展  
    在/www/swoole/ 目录下，建立composer.json，写入
    
        {
        "require": {
            "matyhtf/swoole_framework": "dev-master"
            }
        }
        
    运行：
    
    composer install
    
