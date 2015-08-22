# API 方案

## 方案 Lumen + Dingo + Oauth2.0

### 安装
很慢，很艰难

- First, download the Lumen installer using Composer:
	
	composer global require "laravel/lumen-installer=~1.0"

- 进入web目录，安装lumen

	lumen new test

- 或者直接composer 安装

	composer create-project laravel/lumen test

### 基本设定

- 配置文件

与laravel有一组配置文件不同的是，lumen只有一个配置文件.env

- APP secret Key

lumen安装完毕之后，你需要在.env配置文件中设置一个32位的随机字符串,如果你是复制的.env.example，你需要立刻设置一下，否则你的session以及其他加密的数据都会有不安全。

- 其他设定

lumen不需要其他的设定就已经可以开始进行愉快的开发了，不过你也可能需要为lumen设定数据库或者缓存等，请随意。

- Pretty URLs
在Nginx下，增加这样一句

	location / {
	    try_files $uri $uri/ /index.php?$query_string;
	}

如果使用Homestead，Pretty URLs是自动实现的。

### 环境配置说明

- .env文件可能随使用环境的不同而有所不同，于是可能会可能存在多个env。
- lumen使用phpdotenv类库，全新安装的lumen会默认包含一个.env.example文件，如果是通过composer安装的，系统会自动生成一个.env文件，否则，需要你手动重命名一个。
- 所有的变量会被加载如一个$_ENV的变量中，你可以使用env helper取出这些变量。
- 你可以自由的根据你的应用修改环境变量，考虑到多人协同开发，可以保留一份.env.exmaple，.evn文件不要设定在版本控制范围之内。必要的设定可以增加“place-holder”，这样其他的开发者就不会随意改动。

### 环境参数调用

	$environment = APP::environment();  //通过APP facade 方式获得环境变量
	$environment = app()->environment();//通过app()方法获取环境变量
	$value = config('app.timezone'); // 通过config方法获取环境变量
	config(['app.timezone' => 'America/Chicago']);// 通过config 方法设定环境变量


### 测试一下

- 通过浏览器访问： http://t.app // 我这里用的是homestead,看到了lumen的首页
- 压力测试：ab -n 1000 -c 10 http://t.app/

	服务器配置为：1核心，2G内存，虚拟机，hhvm。

	Concurrency Level:      10
	Time taken for tests:   7.873 seconds
	Complete requests:      1000
	Failed requests:        0
	Total transferred:      1583000 bytes
	HTML transferred:       1422000 bytes
	Requests per second:    127.01 [#/sec] (mean)
	Time per request:       78.732 [ms] (mean)
	Time per request:       7.873 [ms] (mean, across all concurrent requests)
	Transfer rate:          196.35 [Kbytes/sec] received

	如果是纯php 无框架的 hello world，压测结果如下：

	Concurrency Level:      10
	Time taken for tests:   1.099 seconds
	Complete requests:      1000
	Failed requests:        0
	Total transferred:      163000 bytes
	HTML transferred:       27000 bytes
	Requests per second:    909.75 [#/sec] (mean)
	Time per request:       10.992 [ms] (mean)
	Time per request:       1.099 [ms] (mean, across all concurrent requests)
	Transfer rate:          144.81 [Kbytes/sec] received

性能大概为裸测的1/8，性能损失在所难免。

### 安装 dingo

	composer require dingo/api:0.9.*@dev

### 安全认证方式的选择

- JWT
Json Web Token，是php的一个类库，用于给用户返回一个token，并能够设定作用域。

- Oauth
依据Oauth2.0标准的php类库，主要用于实现含有第三方的授权，授权模式有多种，不过因为目前不存在第三方的问题，实质上超出我们的使用场景，本项目所需要的仅仅是一个便于维护的token。 


## 规划接口服务

### Restful API 最佳实践
- 命名：资源 + 操作，遵循RESTFUL API 设计原则
- 版本：大版本在URL中，小版本在header中
- 限速：HTTP状态码429，需要调研。
- 鉴权：Oauth2.0,或者其他方案。
- HTTPS：毫无例外，永远都要使用SSL。
- 文档：文档应该容易找到，公开，及时更新，更新后使用邮件进行通知。
- 更新：在POST操作以后，返回201 created 状态码，并且包含一个指向新资源的url作为返回头。
- 格式：回复是Json
- GZIP：开启gzip
- 状态码：

		200 ok  - 成功返回状态，对应，GET,PUT,PATCH,DELETE.
		201 created  - 成功创建。
		304 not modified   - HTTP缓存有效。
		400 bad request   - 请求格式错误。
		401 unauthorized   - 未授权。
		403 forbidden   - 鉴权成功，但是该用户没有权限。
		404 not found - 请求的资源不存在
		405 method not allowed - 该http方法不被允许。
		410 gone - 这个url对应的资源现在不可用。
		415 unsupported media type - 请求类型错误。
		422 unprocessable entity - 校验错误时用。
		429 too many request - 请求过多。

### Oauth 学习

### 参考
- 阮一峰：理解OAuth 2.0 http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html
- SeCloud：帮你深入理解Oauth2.0 http://blog.csdn.net/seccloud/article/details/8192707
- phphub：oauth资源整理 https://phphub.org/topics/639

### OAuth的参与实体：

+ RO (resource owner)

资源所有者，对资源具有授权能力的人，一般指用户。  

+ RS (resource server):

资源服务器，它存储资源，并处理对资源的访问请求。如Google资源服务器，它所保管的资源就是用户Alice的照片。

+ Client

第三方应用，它获得RO的授权后便可以去访问RO的资源。如网易印像服务。

+ AS (authorization server)

授权服务器，它认证RO的身份，为RO提供授权审批流程，并最终颁发授权令牌(Access Token)。为了便于协议的描述，这里逻辑上把AS与RS区分开来；在物理上，AS与RS的功能可以由同一个服务器来提供。


### 我的场景

- APP：Client，app客户端
- API：RS，资源服务器，提供资源
- 用户：RO，用户，操作授权行为的人
- 授权：AS，授权服务，提供token，管理token。

用户授权给APP访问API的权限，需要向授权服务器申请Token，APP获取Token后，具备访问用户资源的权限。

### 社会化账号场景
- 用户：RO
- 腾讯：RS，资源提供者
- 授权Client：第三方嵌入的授权Client，给用户显示授权页面，让用户输入自身的账号，从而获取该用户在腾讯服务器上的用户资料。
- 某论坛：第三方的服务，嵌入Client，获取通过Oauth Client 向腾讯获取用户信息，记录到自身的服务器中。
- 腾讯Oauth系统：AS，腾讯授权服务器，通过权鉴，为Client发放Token。

### Oauth授权流程


#### 权鉴方案



#### 限速方案
防止CC攻击方案
512509058


#### 通用字段
- token
- appid
- 时间戳

# 20150710

### lumen + dingo/api


#### 框架压力测试
	
	ab -n 1000 -c 10 http://t.app/

- CI2.2 86
- CI3.0 70
- Lumen 80
- 裸Echo 900
- slim2.6.2 157
- Lumen + Dingo 23 没有数据库查询
- Laravel 5（yun.app）
- c1服务器 36 （线上，vps，环境不同）

#### git 学习资料

	http://igit.linuxtoy.org/contents.html