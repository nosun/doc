Nginx 配置 HTTPS

###安装openssl
openssl是必须的软件，这里用yum进行安装

	#yum install openssl
	#yum install openssl-devel

###生成server.key

	#cd /usr/local/nginx/conf
	#openssl genrsa -des3 -out server.key 1024 //提示输入密码，密码用于加密key文件.
	#openssl req -new -key server.key -out server.csr
	#openssl rsa -in server.key -out server_nopwd.key
	#openssl x509 -req -days 365 -in server.csr -signkey server_nopwd.key -out server.crt

###nginx的配置文件

	server {
	    listen       443;
	    server_name  x.webbig.cn
	
	    ssl                  on;
	    ssl_certificate      server.crt;
	    ssl_certificate_key  server_nopwd.key;
	
	    ssl_session_timeout  5m;
	
	    ssl_protocols  SSLv2 SSLv3 TLSv1;
	    ssl_ciphers  ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
	    ssl_prefer_server_ciphers   on;
	
	    location / {
	        root   /www/x.webbig.cn;
	        index  index.php index.html;
	    }
	}




##背景知识

###OpenSSL

####SSL(Secure Socket Layer) 安全协议

SSL由 Netscape 公司首先提出，最初用在保护 Navigator 浏览器和 Web 服务器之间的 HTTP 通信 ( 即 HTTPS)
SSL被 IETF 吸收改进为 TLS(Transport Layer Security) 协议,也称为SSL/TLS协议

####SSL/TLS功能

SSL/TLS 协议位于 TCP 协议(HTTPS)和应用层协议之间（即在会话层或表示层），为传输双方提供：认证和加密

####OpenSSL
OpenSSL 是著名的开源SSL，是用 C 语言实现的，被广泛应用在基于 TCP/Socket 的网络程序中。

####Openssl生成证书的全过程

- 生成服务器端的key文件：server.key文件
openssl genrsa -des3 -out server.key 1024
需要输入密码，生成csr文件时会用到

- 生成服务器端的csr文件：server.csr文件
openssl req -new -key server.key -out server.csr
需要输入前面设置的密码，然后根据提示输入一系列信息，应该是用于认证用的。

- 生成服务器端的免密码server.key文件
openssl rsa -in server.key -out server_nopwd.key
需要输入前面设置的密码

- 生成服务器端的证书server.crt文件
openssl x509 -req -days 365 -in server.csr -signkey server_nopwd.key -out server.crt
	


