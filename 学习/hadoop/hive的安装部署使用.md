## hive

### 学习资源

http://www.imooc.com/learn/387

### hive的安装

#### hive 嵌入模式
使用 derby数据库 作为元数据存储，一般用于演示使用

#### hive 本地模式
元数据信息被存储在mysql数据库中
mysql数据库和hive运行在同一台物理机器上

#### hive远程模式
元数据信息被存储在远程mysql数据库中

### hive 远程模式安装

- 解压缩tar包
- 设置hive path
- 配置hive

### hive cli

#### hive 交互模式

	show tables --查看表列表；
	show functions -- 查看函数列表；
	！cliear  -- 清除屏幕
	desc tablename -- describe 查看表
	dfs -ls /user; -- 查看hdfs文件
	dfs -lsr /user; -- 递归查询hdfs文件
	！+ cmd -- 可以执行shell命令
	source /root/my.sql -- 执行sql脚本

#### hive -S 静默模式

### hive web interface

#### web 界面
- 端口号 9999
- 启动方式 #hive --Service hwi &
- 通过浏览器访问： http://ip:9999/hwi

#### 安装web界面

- 下载 apache-hive-source包
- 解压缩 tar.gz包
- 进入hwi目录,打war包

		jar cvfM0 hive-hwi-1.2.1.war -C web/ .

- 拷贝war包到hive/lib的目录下
- 修改配置文件

		<property>
	    <name>hive.hwi.listen.host</name>
	    <value>0.0.0.0</value>
	  </property>
	  <property>
	    <name>hive.hwi.listen.port</name>
	    <value>9981</value>
	    <description/>
	  </property>
	  <property>
	    <name>hive.hwi.war.file</name>
	    <value>lib/hive-hwi-1.2.1.war</value>
	  </property>

- 启动网页管理工具

		hive --service hwi& // 加 & 为守护进程

- 如果出现运行错误，可能是缺少jre的包

		拷贝jdk/lib/tools.jar /hive/lib 目录下


### hive 的管理

hive --service hiveserver
启动一个 Thrift Server，能够使用远程管理功能。

### hive的数据类型
#### 基本数据类型
- tinyint/smallint/int/bigingt: 整数类型	
- float/double ：浮点数类型
- string/varchar/char: 字符串
		
		create table person (pid int,pname string, married boolean,salary double)

		create table test1(vname varchar(20),cname char(20));

		desc test1

#### 复杂数据类型
- array: 数组类型，由一系列

		create table student (sid int,sname string,grade array<float>))

		{1,Tom,[80,90,74]}

- map:集合类型，包含 key->value，可以通过key 访问value

		create table student1 (sid int,sname string,grade map<string,float>)

		{1,Tom,['大学语文'，80]}

		
		create table student2 (sid int,sname string,grade array<map<string,float>>)

		{1,Tom,[<'大学语文'，80>,<'数学'，'90'>]}

- 结构类型

		create table student3 (sid int,info struct<name:string,age:int,sex:string>);

 string,grade array<map<string,float>>)

		{1,Tom,[<'大学语文'，80>,<'数学'，'90'>]}

#### 时间类型

- Date：从hive 0.12.0 开始支持      和时区相关
- Timestamps：从hive 0.8.0 开始支持 unix时间戳

### hive的数据存储

- 基于Hdfs
- 没有专门的数据存储格式
- 存储结构主要包括：数据库、文件、表、视图
- 可以直接加载文本文件(.txt文件等)
- 创建表时，指定hvie数据的列分隔符与行分隔符

#### 表

##### Table 内部表
- 与数据库中的table的概念是类似的
- 每个table在hive中都有一个相应的目录存储数据
- 所有的table数据都保存在这个目录
- 删除表时，元数据和数据都会被删除

###### 创建一张表
		create table t1(tid int,tname string,age int)
###### 创建一张新表，指定物理位置
		create table t2(tid int,tname string,age int) location '/mytable/hive/t2';
###### 创建空表，指定分隔符
		create table t3(tid int,tname string, age int) row format delimited fields terminated by ',';
###### 查询例子表		
		select * from sample_data;

		create table t4 as select * from sample_data;
###### 创建一个表，无分隔符
		hdfs dfs -cat /usr/hive/warehouse/t4/00000_0（无分隔符）
###### 创建一个表，指定风格符
		create table t5 row format delimted fields terminated by ',' as select * from sample_data;
###### 增加一列
		alter table t1 add columns(english int);

###### 删除表
		drop table t1;

#### Partition 分区表
- 分区表对应于数据库的Partition 列的密集索引
- 在hive中，表中的一个分区对应于表下的一个目录，所有的partition的数据都存储在对应的目录中。
- 分区字段索引，查询速度快。

		create table partition_table (sid int sname string) partitioned by (gender string) row format delimited fields terminated by ',';

		insert into table partition_table partition(gender='F') select sid,sname from sample_data where gender='F'

		explain select * from sample_data where gender = 'F';

#### External Table 外部表
- 指向已经在hdfs中存在的数据，可以创建partition
- 他和内部表在元数据的组织上是相同的，而实际数据的存储有较大差异
- 外部表只是一个过程，加载数据和创建表同时完成，并不会移动到数据仓库目录中，只是与外部数据建立了一个连接，当删除一个外部表时，仅仅删除该链接。

		external table external_student(sid int,sname string,age int) row format delimited fields terminated by ',' location '/input'

表和文件是关联关系。

#### Bucket Table 桶表

桶表是对数据进行哈希取值，然后放到不同文件中存储。

	create table bucket_table1(sid int,sname string,age int) clustered by(sname) into 5 buckets

### 视图

视图是一个虚表，是一个逻辑概念，可以跨越多张表
操作视图和操作表的方式是完全一致的
视图是不存数据的，它建立在已有表的基础上，视图依赖于建立的这些表为基表
视图最大的好处就是可以简化复杂的查询


