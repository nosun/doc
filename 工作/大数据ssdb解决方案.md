## 大数据问题

### 数据工作流程

- 设备上传数据，存入ssdb
- 计划任务，每天执行一次，从ssdb读取数据，做出分析，并写入mysql
- 管理后台对数据进行图表绘制。

### 需要解决的问题

海量日志如何存储，并每日进行分析，设定目标为10万台设备，每台设备5s钟写入一条数据，均每秒钟 2万 条数据写入。

日志结构

	date time mac cmd [key-value，key-value，key-value]；

#### 如何存储

- 数据存储方案
- 数据存储效率
- 数据备份
- 旧数据分离

#### 如何检索

- 数据查询效率


#### 方案选择

- ssdb解决方案
- hadoop解决方案

### ssdb解决方案

#### 存储
- 使用hset 将设备传过来的数据进行存储，格式如下：

		key:   ds_mac_cmd_date_time
		value: key->value key->value

- 使用 zset 建立集合，集合的名字为 ds_mac_cmd_date，每个设备，每类指令，每天有一个集合。

		name  - zset 的名字.
		key   - zset 中的 key.
		score - 整数, key 对应的权重值

		key的结构 ：  time
 
		例如：         "201508111208",
		score 类似这样：201508111208

这样，每个集合的数据不超过 86400/5 = 17280

#### 检索

比如，需要分析一个月内的某设备的数据。

##### 首先使用zlist，检索一定时间范围内的zset，当然根据规则直接遍历也可以。

	ssdb->zlist('a', 'z', 10);

##### 然后，对每个zset，查出特定key。

	ssdb->zkeys() 查出这一天所有的。

##### 最后，使用multi_hget查出每个key的值，后面的事情，就需要程序处理了。
	
	ssdb->multi_hget('h', array('k1', 'k2'));

### hadoop解决方案

#### hadoop 安装
下载 hadoop，建议使用1.2.*的 http://hadoop.apache.org/

		wget http://mirrors.hust.edu.cn/apache/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz
		tar -zxvf hadoop-1.2.1.tar.gz //解压缩
		mv hadoop-1.2.1 /opt // 移动文件夹到 opt下，hadoop的执行文件和配置文件

#### hadoop 配置
Hadoop配置文件在conf目录下，代码开发分为了core，hdfs和map/reduce三部分，配置文件也分了三个core-site.xml、hdfs-site.xml、mapred-site.xml。core-site.xml和hdfs-site.xml是站在HDFS角度上配置文件；core-site.xml和mapred-site.xml是站在MapReduce角度上配置文件。

- 修改hadoop-evn.sh 

		echo $JAVA_HOME      // 获取java_home path
		vi /opt/hadoop-1.1.1/conf/hadoop-env.sh    // 修改hadoop 配置文件
	    export JAVA_HOME = /usr/java/jdk1.7.0_60  // 修改 java_home path 配置文件

- 修改core-siet.xml
		
		<configuration>
		    <property>
		        <name>hadoop.tmp.dir</name>
		        <value>/data/hadoop</value>
		    </property>
		    <property>
		        <name>dfs.name.dir</name>
		        <value>/data/hadoop/name</value>
		    </property>
		     <property>
		         <name>fs.default.name</name>
		         <value>hdfs://182.92.148.183:9000</value>
		     </property>
		
		</configuration>  

	- 修改hdfs-site.xml 

		<configuration>
		     <property>
		         <name>dfs.data.dir</name>
		         <value>/data/hadoop/data</value>
		     </property>
		    <property>
		        <name>dfs.replication</name>
		        <value>1</value>
		    </property>
		</configuration>

	- 修改mapred-site.xml
		
			<configuration>
			     <property>
			          <name>mapred.job.tracker</name>
			          <value>182.92.148.183:9001</value>
			     </property>
			</configuration>

	- 修改/etc/profile, 增加 HADOOP_HOME 输出
	- 使得 profile生效，source /etc/profile		

#### 启动 hadoop
- 格式化HDFS文件系统
		hadoop namenode -format
- 启动hadoop
		./bin/start-all.sh

首先启动namenode 接着启动datanode1，datanode2，…，然后启动secondarynamenode。再启动jobtracker，然后启动tasktracker1，tasktracker2，…。

启动 hadoop成功后，在 Master 中的 tmp 文件夹中生成了 dfs 文件夹，在Slave 中的 tmp 文件夹中均生成了 dfs 文件夹和 mapred 文件夹。

我这里没有配置集群，只是单机使用。

### 安装flume

#### 下载flume

	wget http://apache.fayea.com/flume/1.6.0/apache-flume-1.6.0-src.tar.gz
	tar -zxvf apache-flume-1.6.0-src.tar.gz 
	mv apache-flume-1.6.0-src /opt/flume-1.6.0

#### 配置flume

主要是配置jdk的路径

	cp conf/flume-env.sh.template conf/flume-env.sh
	vi conf/flume-env.sh
	
	JAVA_HOME=/usr/lib/jvm/java-7-oracle
	
输入命令，验证安装是否正确

	/home/hadoop/flume-1.5.0-bin/bin/flume-ng version
	
#### 配置agent

- flume的一些核心概念：

		Agent	使用JVM 运行Flume。每台机器运行一个agent，但是可以在一个agent中包含多个sources和sinks。
		Client	生产数据，运行在一个独立的线程。
		Source	从Client收集数据，传递给Channel。
		Sink	从Channel收集数据，运行在一个独立线程。
		Channel	连接 sources 和 sinks ，这个有点像一个队列。
		Events	可以是日志记录、 avro 对象等

- 配置device_log_tail.conf

		a1.sources = r1
		a1.sinks = k1
		a1.channels = c1
		
		# Describe/configure the source
		a1.sources.r1.type = exec
		a1.sources.r1.channels = c1
		a1.sources.r1.command = tail -F /data/log/device.log
		
		# Describe the sink
		a1.sinks.k1.type = logger
		
		# Use a channel which buffers events in memory
		a1.channels.c1.type = memory
		a1.channels.c1.capacity = 1000
		a1.channels.c1.transactionCapacity = 100
		
		# Bind the source and sink to the channel
		a1.sources.r1.channels = c1
		a1.sinks.k1.channel = c1
- 运行 flume agent

		bin/flume-ng agent --conf conf --conf-file conf/device_log_tail.conf --name a1 -Dflume.root.logger=INFO,console

#### 存储
#### 检索
#### 参考资料
- http://blog.csdn.net/baiyangfu_love/article/details/8096088
- http://idoall.org/home.php?mod=space&uid=1&do=blog&id=550

[2015-08-12 16:3

