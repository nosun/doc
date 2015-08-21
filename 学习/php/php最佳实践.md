## php最佳实践

### 需要搞清楚几个问题

- 如何组织文件夹结构
- 如何组织类库与框架
- 如何正确使用composer
- 如何正确的命名，规范的写代码


### php最佳实践
#### php 版本选择
- php 版本选择 php5.5+
- php 内置web服务器

### php 代码风格
- psr-0 已经废弃，推荐使用psr-4
- psr-1  文件 命名
	- 文件：标签，字符编码，
	- 空间名，类名
	- 类常量，属性，方法
- psr-2 排版，格式
	- 源文件
	- 行
	- 缩进
	- 关键字，true，false，null
	- 命名空间 和导入声明
	- 类，属性，方法
	- 控制结构，闭包
- psr-3 日志
	- 基础
	- 消息
- psr-4
	-  命名空间
		-  必须有一个顶级命名空间 vendor name
		-  可有多个子命名空间
		-  应该有一个终止类名
		-  下划线没有特殊含义
		-  字母可以任何大小写组合
		-  类名大小写敏感
	-  自动载入
		-  命名空间，自命名空间对应目录
		-  目录与命名空间大小写匹配
		-  文件名与终止类名大小写匹配
	-  自动载入器的实现不可抛出任何异常，不可引发任何等级的错误，也不应有返回值；

### 自动载入
PHP5提供了类的自动装载(autoload)机制。autoload机制可以使得PHP程序有可能在使用类时才自动包含类文件，而不是一开始就将所有的类文件include进来，这种机制也称为lazy loading。

	<?php
		 function __autoload($classname) {
		  require_once ($classname . “class.php”);
		 }
		 
		 $person = new Person(”Altair”, 6);
		 var_dump ($person);
	 ?>

当php运行 需要某个类文件，而类文件不存在时，就会调用alutoload function，试图加载该类。

- 建议不适用 require_once 和 include_once
- spl_autoload('class','extension');
- spl_autoload_functions
- spl_autoload_extensions(".php");
- spl_autoload_register ([ callable $autoload_function [, bool $throw = true [, bool $prepend = false ]]] )
- spl_autoload_call
- spl_autoload_unregister
- spl_classes

###
