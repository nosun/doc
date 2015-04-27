## Homesteatd 开发环境配置

### 下载安装homestead.box
- 因为网速原因，先手动下载box，并将其放在d:/homestead/目录下
- 用git bash 进入d:/homestead下，输入命令：

> vagrant box add laravel/homestead ./homestead.box  
> box 添加成功

### 安装 Homestead
- git clone https://github.com/laravel/homestead.git Homestead

> 不是很明白这是什么？ 下载之后，进入Homestead文件夹，其中有个init.sh脚本，运行之。

	bash init.sh

此时Homestead.yaml 文件，将会被放置在你的 ~/.homestead 目录中。

### 配置 

#### 配置你的 Provider

provider: virtualbox

#### 配置你的 SSH 密钥
生成ssh密匙

ssh-keygen -t rsa -C "you@homestead"

> step1 输入sshkey所有生成的路径
> step2 输入密匙 2次

### 修改homestead.yaml
	修改 ssh 配置，共享目录设置，ip设置

	---
	ip: "192.168.11.10"
	memory: 2048
	cpus: 1
	provider: virtualbox
	
	authorize: d:/homestead/id_rsa.pub
	
	keys:
	    - d:/homestead/id_rsa
	
	folders:
	    - map: d:/homestead/data/code
	      to: /home/vagrant/Code
	
	sites:
	    - map: yun.app
	      to: /home/vagrant/Code/Laravel/public
	      hhvm: true
	databases:
	    - homestead
	
	variables:
	    - key: APP_ENV
	      value: local
	
	# blackfire:
	#     - id: foo
	#       token: bar
	#       client-id: foo
	#       client-token: bar
		
		# blackfire:
		#     - id: foo
		#       token: bar
		#       client-id: foo
		#       client-token: bar

**`important:`** 文件中的格式要非常注意，冒号，tab 可能回引起虚拟机启动错误。

### 启动虚拟机

进入 ~/Homestead 目录  

	vagrant up

### 进入虚拟机
- 设置root账号
	
	sudo passwd root	
	
- 下载laravel源码	

	composer global require "laravel/installer=~1.1"

- 修改环境变量

	vi /etc/enviroment
	source /etc/environment

- 生成laravel 项目

	cd /home/vagrant/Code
	laravel new laravel

