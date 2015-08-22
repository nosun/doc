## swoole  扩展学习

### 1、安装swoole扩展，用composer的方式
服务器上composer之前安装过，设置好composer.json，然后composer install 就好了。
    发现composer的命令还不是很熟悉，还需要学习一下。
    
###2、swoole框架文件布局比较深，在vendor/matyhtf/swoole_framework/下
不知道标准的框架布局应该是怎样的，用CI框架，感觉文件布局非常清晰，而swoole框架用composer安装之后，感觉很深，加上外层的webroot，就有三层是多余的。如果将swoole_framework移到外层，不晓得对类库的管理是不是会受到影响。
    
###3、运行server.php
将example目录下的app_server的例子作为server.php放在swoole_framework根目录下，运行，即启动了基于swoole的mvc框架。

###4、基于server服务，进一步了解其工作原理

####4.1 server.php
1、require /libs/lib_config.php
2、webserver::create(swoole.ini)
3、$server->setAppPath
4、$server->setDocumentRoot
5、$server->setLogger
6、$server->run

####4.2 swoole.ini
$config 配置文件 server 段
host = "0.0.0.0" 
port = 9502
max_request = 2000
worker_num = 8
webroot = 'http://127.0.0.1:8888'
document_root = "/data/wwwroot/"
process_rename = 1
keepalive = 1
gzip_open = 1
user = www-data
expire_open = 1
####4.1 lib/lib_config.php

####4.2 /Swoole/Swoole.php

####4.2 /Swoole/Loader.php


####4.2 Swoole/Protocol/WebServer.php
####4.3 Swoole/Protocol/Base.php

