## shell中$0,$?,$!等的特殊用法

### 概念	

	$0
	Shell本身的文件名

	$1～$n
	添加到Shell的各参数值。$1是第1参数、$2是第2参数…。

	$#
	添加到Shell的参数个数

	$*
	所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。

	$@
	所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。

	$?
	最后运行的命令的结束代码（返回值）

	$$
	Shell本身的PID（ProcessID）

	$!
	Shell最后运行的后台Process的PID

	$-
	使用Set命令设定的Flag一览

### 示例

#### 编辑脚本
	
	vi var.sh

	#!/bin/sh
	echo "number:$#"
	echo "scname:$0"
	echo "first :$1"
	echo "second:$2"
	echo "argume:$@"

#### 设置权限

	chmod a+x var.sh

##### 执行脚本

	# ./variable aa bb
	number:2
	scname:./variable
	first: aa
	second:bb
	argume:aa bb

##### 结果

	$# 是传给脚本的参数个数
	$0 是脚本本身的名字
	$1是传递给该shell脚本的第一个参数
	$2是传递给该shell脚本的第二个参数
	$@ 是传给脚本的所有参数的列表