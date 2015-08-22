##Mosquitto 安装及配置

###Mosquitto的安装
根据mosquitto官网上的的提示，在/etc/etc/yum.repos.d/ 增加 repo文件，如下：

	[home_oojah_mqtt]
	name=mqtt (CentOS_CentOS-6)
	type=rpm-md
	baseurl=http://download.opensuse.org/repositories/home:/oojah:/mqtt/CentOS_CentOS-6/
	gpgcheck=1
	gpgkey=http://download.opensuse.org/repositories/home:/oojah:/mqtt/CentOS_CentOS-6/repodata/repomd.xml.key
	enabled=1


Download the repository config file for your CentOS version from below and copy it to /etc/yum.repos.d/ You’ll now be able to install and keep mosquitto up to date using the normal package management tools.
	
The available packages are: mosquitto, mosquitto-clients, libmosquitto1, libmosquitto-devel, libmosquittopp1, libmosquittopp-devel, python-mosquitto.

使用yum 方式安装，安装完成之后测试mosquitto_sub命令时提示错误：

	error while loading shared libraries: libmosquitto.so.1: cannot open shared object file: No such file or directory

原因是libmosquitto的库文件链接问题，依照网上的方法，增加软链接：

	# 创建链接
	sudo ln -s /usr/local/lib/libmosquitto.so.1 /usr/lib/libmosquitto.so.1
	# 更新动态链接库
	sudo ldconfig

###Mosquitto的配置
参考： http://blog.csdn.net/xukai871105/article/details/39252653
