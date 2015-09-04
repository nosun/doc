## find 查找最近修改过的文件

find ./ -mtime 0：返回最近24小时内修改过的文件。

find ./ -mtime 1 ： 返回的是前48~24小时修改过的文件。而不是48小时以内修改过的文件。

那怎么返回10天内修改过的文件？find还可以支持表达式关系运算，所以可以把最近几天的数据一天天的加起来：

find ./ -mtime 0 -o -mtime 1 -o -mtime 2 ……虽然比较土，但也算是个方法了。

还有没有更好的方法，我也想知道。

另外， -mmin参数-cmin / - amin也是类似的。

### Find应用实例

查找最近30分钟修改的当前目录下的.php文件

	find . -name '*.php' -mmin -30

查找最近24小时修改的当前目录下的.php文件

	find . -name '*.php' -mtime 0

查找最近24小时修改的当前目录下的.php文件，并列出详细信息

	find . -name '*.inc' -mtime 0 -ls

查找当前目录下，最近24-48小时修改过的常规文件。

	find . -type f -mtime 1

查找当前目录下，最近1天前修改过的常规文件。

	find . -type f -mtime +1

参考 http://www.cnblogs.com/wanqieddy/archive/2012/05/07/2487761.html