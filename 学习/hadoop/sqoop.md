## sqoop

###

一简介

Sqoop是一个用来将Hadoop和关系型数据库中的数据相互转移的工具，可以将一个关系型数据库（例如 ： MySQL ,Oracle ,Postgres等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。

二特点

Sqoop中一大亮点就是可以通过hadoop的mapreduce把数据从关系型数据库中导入数据到HDFS。

三 Sqoop 命令

Sqoop大约有13种命令,和几种通用的参数(都支持这13种命令)，这里先列出这13种命令。
接着列出Sqoop的各种通用参数,然后针对以上13个命令列出他们自己的参数。Sqoop通用参数又分Common arguments,Incremental import arguments,Output line formatting arguments,Input parsing arguments,Hive arguments,HBase arguments,Generic Hadoop command-line arguments,下面一一说明:
1.Common arguments
通用参数,主要是针对关系型数据库链接的一些参数

四  sqoop命令举例

1）列出mysql数据库中的所有数据库
sqoop list-databases –connect jdbc:mysql://localhost:3306/ –username root –password 123456


2)连接mysql并列出test数据库中的表
sqoop list-tables –connect jdbc:mysql://localhost:3306/test –username root –password 123456
命令中的test为mysql数据库中的test数据库名称 username password分别为mysql数据库的用户密码


3)将关系型数据的表结构复制到hive中,只是复制表的结构，表中的内容没有复制过去。
sqoop create-hive-table –connect jdbc:mysql://localhost:3306/test
–table sqoop_test –username root –password 123456 –hive-table
test
其中 –table sqoop_test为mysql中的数据库test中的表 –hive-table
test 为hive中新建的表名称


4)从关系数据库导入文件到hive中
sqoop import –connect jdbc:mysql://localhost:3306/zxtest –username
root –password 123456 –table sqoop_test –hive-import –hive-table
s_test -m 1


5)将hive中的表数据导入到mysql中,在进行导入之前，mysql中的表
hive_test必须已经提起创建好了。
sqoop export –connect jdbc:mysql://localhost:3306/zxtest –username
root –password root –table hive_test –export-dir
/user/hive/warehouse/new_test_partition/dt=2012-03-05


6）从数据库导出表的数据到HDFS上文件
./sqoop import –connect
jdbc:mysql://10.28.168.109:3306/compression –username=hadoop
–password=123456 –table HADOOP_USER_INFO -m 1 –target-dir
/user/test


7）从数据库增量导入表数据到hdfs中
./sqoop import –connect jdbc:mysql://10.28.168.109:3306/compression
–username=hadoop –password=123456 –table HADOOP_USER_INFO -m 1
–target-dir /user/test  –check-column id –incremental append
–last-value 3

五 Sqoop原理（以import为例）

Sqoop在import时，需要制定split-by参数。Sqoop根据不同的split-by参数值来进行切分,然后将切分出来的区域分配到不同map中。每个map中再处理数据库中获取的一行一行的值，写入到HDFS中。同时split-by根据不同的参数类型有不同的切分方法，如比较简单的int型，Sqoop会取最大和最小split-by字段值，然后根据传入的num-mappers来确定划分几个区域。 比如select max(split_by),min(split-by) from得到的max(split-by)和min(split-by)分别为1000和1，而num-mappers为2的话，则会分成两个区域(1,500)和(501-100),同时也会分成2个sql给2个map去进行导入操作，分别为select XXX from table where split-by>=1 and split-by<500和select XXX from table where split-by>=501 and split-by<=1000。最后每个map各自获取各自SQL中的数据进行导入工作。

六mapreduce job所需要的各种参数在Sqoop中的实现

1) InputFormatClass
com.cloudera.sqoop.mapreduce.db.DataDrivenDBInputFormat

2) OutputFormatClass
1)TextFile
com.cloudera.sqoop.mapreduce.RawKeyTextOutputFormat
2)SequenceFile
org.apache.hadoop.mapreduce.lib.output.SequenceFileOutputFormat
3)AvroDataFile
com.cloudera.sqoop.mapreduce.AvroOutputFormat


3)Mapper
1)TextFile
com.cloudera.sqoop.mapreduce.TextImportMapper                 
2)SequenceFile
com.cloudera.sqoop.mapreduce.SequenceFileImportMapper        

3)AvroDataFile
com.cloudera.sqoop.mapreduce.AvroImportMapper

4)taskNumbers
1)mapred.map.tasks(对应num-mappers参数)   
2)job.setNumReduceTasks(0);

这里以命令行:import –connect jdbc:mysql://localhost/test  –username root –password 123456 –query “select sqoop_1.id as foo_id, sqoop_2.id as bar_id from sqoop_1 ,sqoop_2  WHERE $CONDITIONS” –target-dir /user/sqoop/test -split-by sqoop_1.id   –hadoop-home=/home/hdfs/hadoop-0.20.2-CDH3B3  –num-mappers 2


1）设置Input
DataDrivenImportJob.configureInputFormat(Job job, String tableName,String tableClassName, String splitByCol)

a)DBConfiguration.configureDB(Configuration conf, String driverClass,     String dbUrl, String userName, String passwd, Integer fetchSize)
1).mapreduce.jdbc.driver.class com.mysql.jdbc.Driver
2).mapreduce.jdbc.url  jdbc:mysql://localhost/test              
3).mapreduce.jdbc.username  root
4).mapreduce.jdbc.password  123456
5).mapreduce.jdbc.fetchsize -2147483648

b)DataDrivenDBInputFormat.setInput(Job job,Class<? extends DBWritable> inputClass, String inputQuery, String inputBoundingQuery)
1)job.setInputFormatClass(DBInputFormat.class);                 
2)mapred.jdbc.input.bounding.query SELECT MIN(sqoop_1.id), MAX(sqoop_2.id) FROM (select sqoop_1.id as foo_id, sqoop_2.id as bar_id from sqoop_1 ,sqoop_2  WHERE  (1 = 1) ) AS t1
3)job.setInputFormatClass(com.cloudera.sqoop.mapreduce.db.DataDrivenDBInputFormat.class);
4)mapreduce.jdbc.input.orderby sqoop_1.id
c)mapreduce.jdbc.input.class QueryResult
d)sqoop.inline.lob.length.max 16777216

2）设置Output
ImportJobBase.configureOutputFormat(Job job, String tableName,String tableClassName)
a)job.setOutputFormatClass(getOutputFormatClass());               
b)FileOutputFormat.setOutputCompressorClass(job, codecClass);
c)SequenceFileOutputFormat.setOutputCompressionType(job,CompressionType.BLOCK);
d)FileOutputFormat.setOutputPath(job, outputPath);


3）设置Map
DataDrivenImportJob.configureMapper(Job job, String tableName,String tableClassName)
     a)job.setOutputKeyClass(Text.class);
     b)job.setOutputValueClass(NullWritable.class);
c)job.setMapperClass(com.cloudera.sqoop.mapreduce.TextImportMapper);

4）设置task number
JobBase.configureNumTasks(Job job)
mapred.map.tasks 4
job.setNumReduceTasks(0);

七 大概流程

1.读取要导入数据的表结构，生成运行类，默认是QueryResult，打成jar包，然后提交给Hadoop

2.设置好job，主要也就是设置好以上第六章中的各个参数

3.这里就由Hadoop来执行MapReduce来执行Import命令了，

1）首先要对数据进行切分，也就是DataSplit
DataDrivenDBInputFormat.getSplits(JobContext job)

2）切分好范围后，写入范围，以便读取
DataDrivenDBInputFormat.write(DataOutput output) 这里是lowerBoundQuery and  upperBoundQuery

3）读取以上2）写入的范围
DataDrivenDBInputFormat.readFields(DataInput input)

4）然后创建RecordReader从数据库中读取数据
DataDrivenDBInputFormat.createRecordReader(InputSplit split,TaskAttemptContext context)

5）创建Map
TextImportMapper.setup(Context context)

6）RecordReader一行一行从关系型数据库中读取数据，设置好Map的Key和Value，交给Map
DBRecordReader.nextKeyValue()

7）运行map
TextImportMapper.map(LongWritable key, SqoopRecord val, Context context)
最后生成的Key是行数据，由QueryResult生成，Value是NullWritable.get()

八 总结

通过这些,了解了MapReduce运行流程.但对于Sqoop这种切分方式感觉还是有很大的问题.比如这里根据ID范围来切分,如此切分出来的数据会很不平均,比如min(split-id)=1,max(split-id)=3000，交给三个map来处理。那么范围是(1-1000),(1001-2000),(2001-3000).而假如1001-2000是没有数据，已经被删除了。那么这个map就什么都不能做。而其他map却累的半死。如此就会拖累job的运行结果。这里说的范围很小，比如有几十亿条数据交给几百个map去做。map一多，如果任务不均衡就会影响进度。看有没有更好的切分方式？比如取样？如此看来，写好map reduce也不简单!、