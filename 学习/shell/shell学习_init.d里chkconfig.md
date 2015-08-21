## init.d里chkconfig

### 一、什么是INIT
  init是Linux系统操作中不可缺少的程序之一。 
  所谓的init进程，它是一个由内核启动的用户级进程。 
  内核自行启动（已经被载入内存，开始运行，并已初始化所有的设备驱动程序和数据结构等）之后，就通过启动一个用户级程序init的方式，完成引导进程。所以,init始终是第一个进程（其进程编号始终为1）。 
  内核会在过去曾使用过init的几个地方查找它，它的正确位置（对Linux系统来说）是/sbin/init。如果内核找不到init，它就会试着运行/bin/sh，如果运行失败，系统的启动也会失败。 
### 二、运行级别

　　那么，到底什么是运行级呢？ 
　　简单的说，运行级就是操作系统当前正在运行的功能级别。这个级别从1到6 ，具有不同的功能。 
　　不同的运行级定义如下：

　　# 0 - 停机（千万不能把initdefault 设置为0 ） 
　　# 1 - 单用户模式 # s init s = init 1 
　　# 2 - 多用户，没有 NFS 
　　# 3 - 完全多用户模式(标准的运行级) 
　　# 4 - 没有用到 
　　# 5 - X11 多用户图形模式（xwindow) 
　　# 6 - 重新启动 （千万不要把initdefault 设置为6 ） 

　　这些级别在/etc/inittab 文件里指定。这个文件是init 程序寻找的主要文件，最先运行的服务是放在/etc/rc.d 目录下的文件。在大多数的Linux 发行版本中，启动脚本都是位于 /etc/rc.d/init.d中的。这些脚本被用ln 命令连接到 /etc/rc.d/rcn.d 目录。(这里的n 就是运行级0-6) 

### 三、chkconfig 命令 

不像DOS 或者 Windows，Linux 可以有多种运行级。常见的就是多用户的2,3,4,5，很多人知道 5 是运行 X-Windows 的级别，而 0 就 是关机了。

运行级的改变可以通过 init 命令来切换。例如，假设你要维护系统进入单用户状态，那么，可以使用 init 1 来切换。

在 Linux 的运行级的切换过程中，系统会自动寻找对应运行级的目录/etc/rc[0-6].d下的K 和 S 开头的文件，按后面的数字顺序，执行这些脚本。对这些脚本的维护，是很繁琐的一件事情，Linux 提供了chkconfig 命令用来更新和查询不同运行级上的系统服务。
 
语法为： 

	chkconfig --list [name] 
	chkconfig --add name 
	chkconfig --del name 
	chkconfig [--level levels] name 
	chkconfig [--level levels] name 

chkconfig 有五项功能：添加服务，删除服务，列表服务，改变启动信息以及检查特定服务的启动状态。 
chkconfig 没有参数运行时，显示用法。如果加上服务名，那么就检查这个服务是否在当前运行级启动。如果是，返回 true，否则返回 false。 

#### --level 选项可以指定要查看的运行级而不一定是当前运行级。 

如果在服务名后面指定了on，off 或者 reset，那么 chkconfig 会改变指定服务的启动信息。

on 和 off 分别指服务在改变运行级时的 启动和停止。

reset 指初始化服务信息，无论有问题的初始化脚本指定了什么。 

对于 on 和 off 开关，系统默认只对运行级 3，4，5有效，但是 reset 可以对所有运行级有效。

指定 --level 选项时，可以选择特 定的运行级。 

需要说明的是，对于每个运行级，只能有一个启动脚本或者停止脚本。当切换运行级时，init 不会重新启动已经启动的服务，也不会再 次去停止已经停止的服务。 

选项介绍： 

#### --level levels 

指定运行级，由数字 0 到 7 构成的字符串，如： 

--level 35 表示指定运行级3 和5。 

要在运行级别3、4、5中停运 nfs 服务，使用下面的命令：

	chkconfig --level 345 nfs off 

#### --add name 

这个选项增加一项新的服务，chkconfig 确保每个运行级有一项 启动(S) 或者 杀死(K) 入口。如有缺少，则会从缺省的init 脚本自动 建立。 

#### --del name 

用来删除服务，并把相关符号连接从 /etc/rc[0-6].d 删除。 

#### --list name 

列表，如果指定了name 那么只是显示指定的服务名，否则，列出全部服务在不同运行级的状态。 
运行级文件 

每个被chkconfig 管理的服务需要在对应的init.d 下的脚本加上两行或者更多行的注释。 

第一行告诉 chkconfig 缺省启动的运行级以及启动和停止的优先级。如果某服务缺省不在任何运行级启动，那么使用 - 代替运行级。
 
第二行对服务进行描述，可以用 跨行注释。 

例如，random.init 包含三行： 

	# chkconfig: 2345 20 80 
	# description: Saves and restores system entropy pool for 
	# higher quality random number generation. 

表明 random 脚本应该在运行级 2, 3, 4, 5 启动，启动优先权为20，停止优先权为 80。 

设置自启动服务
	
	chkconfig --level 345 nfs on 

###示例
 
	#!/bin/bash 
	# 
	# httpd Startup script for the Apache HTTP Server 
	# 
	# chkconfig: 235 98 98 
	# description: Apache is a World Wide Web server. 
	# It is used to serve HTML files and CGI. 
	# processname: httpd 
	# Source function library. 
	. /etc/rc.d/init.d/functions 
	# See how we were called. 

	case "$1" in 
		start) 
			echo "Starting Apache Server..." 
			/usr/apache/bin/apachectl start 
			;; 
		stop) 
			echo "Stopping Apache Server..." 
			/usr/apache/bin/apachectl stop 
			;; 
		restart) 
			echo "Restarting Apache Server..." 
			/usr/apache/bin/apachectl restart 
			;; 
		status) 
			statusproc /usr/apache/bin/httpd 
			;; 
		*) 
			echo "Usage: $0 {start|stop|restart|status}" 
			exit 1 
			;; 
	esac 
	exit 0