##php数组学习##

###关联数组###

- 在使用下标时，如果是关联数组，一定要使用引号，不要使用常量名称；
- 数组的自动增长下标，默认是从0开始的，自动增长的都是出现过的最大值加1；
- 关联数组的字符串下标，不会影响下标的排列规则。

###list()###

- 作用：将数组中的元素转为变量使用，配合explode函数使用很棒。
- 等号左边使用list()函数，等号右边只能是一个数组
- 数组中有几个元素，在list()中就有几个参数，而且参数必须是变量（新声明的自定义变量）
- 只能是索引数组（下标连续），转为变量，是按下标0开始的。
- 可以在list()参数中通过，空项选择性的接收数组中的元素。

### each()###

- each()是一个函数，参数就是一个数组作为参数，返回的值，也是一个数组
- 返回的值是一个数组，数组固定有4个元素，下标是固定的，索引信息，值，key，值。
- each()只处理当前的元素将当前的元素转为数组信息，处理完后，指针向下一个元素移动。
- 如果指针已经在结束为止了，如果使用each()获取元素，返回false。

### list(),each(), while组合使用 ###
	
	while(list($k,$value)=each($group)){
		echo $k;
		echo $value;
	}

指针返回起始位置 reset($arr);
类似的指针移动函数有 prev(),next(),end();

###系统超全局变量 ###

- $_SERVER		默认有若干个服务器变量
- $_GET
- $_POST		name[] 这种方式可以post数组
- $_SESSION
- $_COOKIE		
- $_FILES    	HTTP文件上传变量
- $_ENV
- $_REQUEST
- $GLOBALS		所有的全局变量值，建议关闭

###数组的键值操作###

- array_value(array $input) 返回input 数组中的所有值并给其建立数字索引
- array_keys(array $input[,mixed $search_value [,bool $strit]]) 返回数组健名
- in_array(mixed $needle, array $haystack) 查询数组是否存在某个值，成功则返回真
- array_search(mixed $needle, array $haystack) 查询数组是否存在某个值，成功则返回第一个匹配的键值
- array_key_exists(mixed $key, array $search) 检查给定的键名是否存在于数组中。
- isset() 检查数组中是否有对应的键名键值存在，如果无此键或键值为null，则返回false。
- array_flip(array $arr) 将数组的键名与键值交换。注意同值交换后，作为键替换的问题，以及值必须是字符串或者整数。
- array_reverse(array $arr [, bool $preserve_keys ]) 将元素反过来，第二个参数为真，则保留原来的键值
	

###统计数组元素的个数与唯一性###

- count(mixed $var [,int $mode]) 计算数组中的单元数目或者对象中的属性个数，若字符串返回1；mode 维度，默认为0 只统计1层。
- array_count_values(array $arr) 统计同样value出现的次数。
- array_unique() 移除数组中重复的值，第一次出现的留下，再次出现的删掉
- 

###使用回调函数处理数组###
- array_filter(array $input [,callback $callback]) 按照规则过滤数组。
- array_walk(array $input, callback $funcname [, mixed $userdata])对数组中的每个成员应用用户函数。
- array_map(callback $callback, array $array [, array $...]) 将回调函数作用到给定的数组单元，处理多个数组。

###冒泡法排序数组###
- 插入排序，选择排序，冒泡排序，时间复杂度相似，所以全学实际意义不大，在实际测试中，3000个数组元素进行，这三种排序都要花费80s左右，而快速排序只需要8s。
- 将相邻两个数作比较，小的上升，大的下降，最终将一组数据进行排序。

###快速排序###
- 二叉法排序：使用递归的方式，采用二叉法的思路对数组进行排序。
###数组排序函数###
- sort-- 对数组升序排序
- rsort-- 对数组降序排序
- ksort -- 对数组按照键名排序
- krsort-- 对数组按照键名逆向排序
- asort-- 对数组进行排序并保持索引关系（关联数组排序）
- arsort--对数组进行逆向排序并保持索引关系
- natsort-- 用“自然排序”算法对数组进行排序
- natcasesort-- 自然排序，不区分大小写 针对字符串与数字组合时，字符串按照默认，而数字从小到大排序的方式，比如file1.txt，file2.txt，file11.txt，file12.txt。
- usort-- 使用用户自定义函数对数组进行排序
- uasort--使用用户自定义函数对数组进行排序，并保持索引关系
- uksort-- 使用用户自定义比较函数对数组中的键名进行排序
- array_multisort-- 对多个数组或多维数组进行排序

###拆分，合并，组合数组###
- array_slice() 从数组中取出一部分 负数代表从尾部开始计数 eg. array_slice($arr,-2,2);
- array_splice() 从原始数组中删除一部分
- arrat_combine() 合并两个数组，其中一个作为键，一个作为值，要求两个数组的个数对应，类型合适。
- array()+array() 下标相同会覆盖
- array_merge($arr,$arr,...) 合并多个数组；
- array_intersect() 求数组的交集;
- array_diff() 求数组的差集；

###数组与数据结构###
- 栈：后进先出，栈顶，栈底，压入数据，弹出数据，like弹匣子；
- 队列：先进先出，队首，队尾；
- 模拟栈：array_push($zhan,"1");压入栈，array_pop($zhan)，弹出一个元素，模拟栈的数据结构；
- 模拟队列：array_unshift($dl,"1");排队进入, array_pop($dl),弹出一个元素，模拟队列数据结构;

###其他的数组处理函数###
- array_rand() 从数组中随即取出一个下标;
- shuffle() 打乱一个数组，对原数组进行操作，和洗牌的场景较像。
- array_sum() 计算数组中的所有元素的合计;
- range() 建立一个包含指定范围单元的数组;$arr=range(0,10);通过设置步长值可以实现跳数。
- array_fill 用给定的数据填充一个数组;

###可以整理下php的很多小方法###
