##zimg 服务搭建

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
    
