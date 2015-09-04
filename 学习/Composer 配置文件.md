## Composer 学习

	http://www.udpwork.com/item/10716.html

### 来看看Composer.json文件吧

#### 一、根目录包
根目录包就是在你的项目的根目录由composer.json定义的包，主要是由composer.json来定义你的项目的依赖。

某些字段只能在根目录包中使用，比如config字段，只有根目录包能定义自己的配置。依赖包中的config字段是被忽略的，所以config 字段是root-only的。

#### 二、composer.json 文件中的各个字段

- name  
包的名子，由vendor名和项目名组成，用 / 分隔。
发布包的时候需要填。

- description  
对包的简短的描述，通常是一行的长度。
发布包的时候需要填。

- version  
包的版本。格式必须是x.y.z，选择性后缀 -dev、-alphaN, -betaN,-RCN

- type  
包的类型，默认是library
包类型用于定制安装逻辑。如果你的包的安装需要一些特殊的逻辑，你可以定义一个定制的类型。它可以是一个symfony-bundle 的类型，或者wordpress-plugin类型。这些类型将被特定对项目所用，他们将提供安装器来安装这些类型的包。

- keywords  
一个与包相关的关键词数组。用于包的搜索和过滤。
可选
- homepage  
项目的网站URL

- time  
版本发布时间，必须是YYYY-MM-DD 格式

- licence  
包的许可证，可以是字符串或者字符串组，可选，但是建议加上。

- author  
包的作者。是个对象数组。

- support    
各种关于该项目如何获取支持的信息，包含这些属性：
email，issues，forum，wiki，irc，source

- Package links    
依赖包的映射表，由包名映射版本约束。如：

	{
	    "require": {
	        "monolog/monolog": "1.0.*"
	    }
	}

	- require:列出依赖包
	- require-dev：列出开发这个包所以来的包
	- conflict：列出包会和哪些包发生冲突
	- replace：列出哪些包要被这个包替代
	- provide：推荐的包列表

- suggest  
建议一些能让这个包工作的更好或者得到增强的包列表。

- autoload  
提供给php autoloader的自动加载映射，目前支持
- PSR-0  
在psr-0 键名下，定义一个命名空间到路径的映射表，相对于包的根目录。
请注意，命名空间的声明得以\\结尾，确保自动加载器正确响应。
PSR-0 的引用可以在安装或更新时声称的文件中查看：
vendor/composer/autoload_namespaces

	例子：
	
	{
	    "autoload": {
	        "psr-0": {
	            "Monolog\\": "src/",
	            "Vendor\\Namespace\\": "src/",
	            "Vendor_Namespace_": "src/"
	        }
	    }
	}

- classmap  
classmap 的引用可以在安装或更新时生成的文件中查看：
vendor/composer/autoload_classmap.php

你可以给任何不支持 PSR-0 的库用 classmap 生成器实现自动加载。配置上只要指定类所在的目录或文件即可：
	
	{
	    "autoload": {
	        "classmap": ["src/", "lib/", "Something.php"]
	    }
	}

- files  
如果你确定需要在任何请求中都加载某些文件，你可以使用 files 自动加载机制。对于那些包中有些 PHP 函数但不能自动加载时特别有用。例如：

	{
	    "autoload": {
	        "files": ["src/MyLibrary/functions.php"]
	    }
	}

