php 设计模式
- namespace 必须在文件首部；
- 自动载入类 __autoload();

	function __autoload($class)
{
	require __DIR__.'/'.$class.'.php';
}
	spl_autoload_register('autoload1');
	spl_autoload_register('autoload2');

- PSR-0
	- 命名空间必须与绝对路径一致
	- 类名首字母必须大写
	- 除入口文件外，其他的.php文件必须只有一个类，不能有其他的代码、

- 开发符合PSR-0规范的基础框架
	- 全部使用命名空间
	- 所有php文件必须自动载入，不能有include/require
	- 单一入口
- spl四种常用的数据结构
- 链式操作 重点在于类中的方法 return $this
- 魔术方法的使用：
	- __get
	- __set
	- __call
	- __callStatic
	- __toString
	- __invoke 

- 数据对象映射模式