##vagrant使用小结

###vagrant安装及环境配置
- windows下安装下载最新版本的vagrant  
	https://www.vagrantup.com/downloads

- 下载安装最新版的virtualbox  
	下载安装 VirtualBox ：https://www.virtualbox.org/

- 下载需要使用的 box  
    http://www.vagrantbox.es/ 由于万恶的GFW，速度比较慢

- 环境设定
	- 重启电脑。
	- 在本地创建好工作目录，并在命令行下切换到对应目录，git bash就挺好用，dos也可以。

###vagrant基本命令
- 增加新的box，在工作目录下，将下载的box文件导入到vagrant中  
	vagrant box add base CentOS-6.3-x86_64-minimal.box
	
- 工作目录下初始化box
	vagrant init
操作之后，在目录中会生成对应的Vagrantfile。通过文本编辑器打开Vagrantfile可以进行一些进一步的常用配置。

- 启动虚拟机 vagrant up
- 关闭虚拟机 vagrant halt
- 暂停虚拟机 vagrant suspend
- 恢复虚拟机 vagrant resume
- 删除虚拟机 vagrant destroy
- 进入虚拟机 vagrant ssh

###vagrant配置文件
- 默认盒子

	config.vm.box = "ubuntu"

- 网络映射

Vagrant的网络有三种模式

	1、较为常用是端口映射，就是将虚拟机中的端口映射到宿主机对应的端口直接使用 ，在Vagrantfile中配置：
	
		config.vm.network :forwarded_port, guest: 80, host: 8080
	
	guest: 80 表示虚拟机中的80端口， host: 8080 表示映射到宿主机的8080端口。
	
	2、如果需要自己自由的访问虚拟机，但是别人不需要访问虚拟机，可以使用private_network，并为虚拟机设置IP ，在Vagrantfile中配置：
	
		config.vm.network :private_network, ip: "192.168.1.104"
	
	192.168.1.104 表示虚拟机的IP，多台虚拟机的话需要互相访问的话，设置在相同网段即可
	
	3、如果需要将虚拟机作为当前局域网中的一台计算机，由局域网进行DHCP，那么在Vagrantfile中配置：
	
		config.vm.network :public_network

- 目录映射

默认情况下，当前的工作目录，会被映射到虚拟机的 /vagrant 目录，当前目录下的文件可以直接在 /vagrant 下进行访问，当然也可以在通过 ln 创建软连接，如

	config.vm.synced_folder "wwwroot/", "/var/www"

前面的参数 “wwwroot/” 表示的是本地的路径，这里使用对于工作目录的相对路径，这里也可以使用绝对路径，比如： “d:/www/”，后面的参数 “/var/www” 表示虚拟机中对应映射的目录。


- 运行脚本

不是必须，但是如果有需要在启动时运行一些脚本，可以编辑脚本。


###我的vagrant盒子

- centos6.4

	端口 22 映射到宿主机 2222
	端口 80 映射到宿主机 80
	ip地址 nat 转换
	共享  g:/data  /data
	web服务 /data/www

- yum源更新为阿里云的源
	mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
	wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
	yum clean all
	yum makecache
	yum -y update 更新yum源


- 安装php及php扩展
	yum install php54w php54w-fpm php54w-devel 
	php54w-mysql php54w-gd libjpeg* php54w-imap php54w-ldap php54w-odbc php54w-pear php54w-xml php54w-xmlrpc php54w-mbstring php54w-mcrypt  php54w-bcmath php54w-mhash libmcrypt

- 编译安装xdebug扩展，便于调试。

- 安装nginx v1.6.2 mysql5.1.73
  为了保证调试时静态文件的实时更新，不被虚拟机缓存，需要修改 nginx.conf ，把里面的 “sendfile on” 修改为 “sendfile off”。

- 安装git

- 准备配合phpstorm使用