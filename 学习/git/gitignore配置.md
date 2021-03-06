## Git的.gitignore 配置

.gitignore 配置文件用于配置不需要加入版本管理的文件，配置好该文件可以为我们的版本管理带来很大的便利，以下是个人对于配置 .gitignore 的一些心得。

### 配置语法：

- 以斜杠“/”开头表示目录；
- 以星号“*”通配多个字符；
- 以问号“?”通配单个字符
- 以方括号“[]”包含单个字符的匹配列表；
- 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；

### 示例：

#### 规则1：
	
	fd1/*
	说明：忽略目录 fd1 下的全部内容；
	注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；

#### 规则2：
	
	/fd1/*
	说明：忽略根目录下的 /fd1/ 目录的全部内容；

#### 规则3：

	/*
	!.gitignore
	!/fw/bin/
	!/fw/sf/
	
	说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；

 