- 下载composer  
	curl -sS https://getcomposer.org/installer | php
- 将composer.phar 移动到 /usr/local/bin/下，供全局调用。
- 安装  
	\# cd /root  
	\# composer global require "laravel/installer=~1.1"

安装这个需要php5.4.0以上，特别有对php5.3.17进行升级。

	rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm //更新源
	yum install yum-plugin-replace  安装yum replace插件
	yum replace php-common --replace-with=php54w-common //升级php
	

PHP Warning:  PHP Startup: swoole: Unable to initialize module

	Module compiled with module API=20090626
	PHP    compiled with module API=20100525
	These options need to match
	 in Unknown on line 0
	Changed current directory to /root/.composer
	./composer.json has been created
	Loading composer repositories with package information
	Updating dependencies (including require-dev)
	  - Installing symfony/console (v2.5.6)
	    Downloading: 100%         
	
	  - Installing guzzlehttp/streams (2.1.0)
	    Downloading: 100%         
	
	  - Installing guzzlehttp/guzzle (4.2.3)
	    Downloading: 100%         
	
	  - Installing laravel/installer (v1.1.3)
	    Downloading: 100%         
	
	symfony/console suggests installing symfony/event-dispatcher ()
	symfony/console suggests installing psr/log (For using the console logger)
	Writing lock file
	Generating autoload files

- 安装Laravel  
	\#cd /www/laravel  
	laravel new  study //创建一个叫 study 的目录, 此目录里面存放着全新安装的 Laravel 以及其依赖的工具包。
- 配置Laravel
	- /app/config/app.php  
	设置时区，权限，路径。

###安装必要的插件
- 安装Sentry插件构建登录等权限验证系统  
打开 ./composer.json ，变更为：  

	"require": {  
		"laravel/framework": "4.2.*",  
		"cartalyst/sentry": "2.1.4"  
	},  

然后，在项目根目录下运行命令

	composer update

- 安装way/generators  

	在 composer.json中增加：
	
	"require-dev": {
	    "way/generators": "~2.0"
	}

放在“require”的下面，运行 composer update

在 ./app/config/app.php 中 恰当的位置 增加配置：

	'Way\Generators\GeneratorsServiceProvider'

- 命令行中运行 php artisan
	\# php artisan
	Laravel Framework version 4.2.11
	
	Usage:
	  [options] command [arguments]
	
	Options:
	  --help           -h Display this help message.
	  --quiet          -q Do not output any message.
	  --verbose        -v|vv|vvv Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug.
	  --version        -V Display this application version.
	  --ansi              Force ANSI output.
	  --no-ansi           Disable ANSI output.
	  --no-interaction -n Do not ask any interactive question.
	  --env               The environment the command should run under.
	
	Available commands:
	  changes                      Display the framework change list
	  clear-compiled               Remove the compiled class file
	  down                         Put the application into maintenance mode
	  dump-autoload                Regenerate framework autoload files
	  env                          Display the current framework environment
	  help                         Displays help for a command
	  list                         Lists commands
	  migrate                      Run the database migrations
	  optimize                     Optimize the framework for better performance
	  routes                       List all registered routes
	  serve                        Serve the application on the PHP development server
	  tail                         Tail a log file on a remote server
	  tinker                       Interact with your application
	  up                           Bring the application out of maintenance mode
	  workbench                    Create a new package workbench
	asset
	  asset:publish                Publish a package's assets to the public directory
	auth
	  auth:clear-reminders         Flush expired reminders.
	  auth:reminders-controller    Create a stub password reminder controller
	  auth:reminders-table         Create a migration for the password reminders table
	cache
	  cache:clear                  Flush the application cache
	  cache:table                  Create a migration for the cache database table
	command
	  command:make                 Create a new Artisan command
	config
	  config:publish               Publish a package's configuration to the application
	controller
	  controller:make              Create a new resourceful controller
	db
	  db:seed                      Seed the database with records
	generate
	  generate:controller          Generate a controller
	  generate:migration           Generate a new migration
	  generate:model               Generate a model
	  generate:pivot               Generate a pivot table
	  generate:publish-templates   Copy generator templates for user modification
	  generate:resource            Generate a new resource
	  generate:scaffold            Scaffold a new resource (with boilerplate)
	  generate:seed                Generate a database table seeder
	  generate:view                Generate a view
	key
	  key:generate                 Set the application key
	migrate
	  migrate:install              Create the migration repository
	  migrate:make                 Create a new migration file
	  migrate:publish              Publish a package's migrations to the application
	  migrate:refresh              Reset and re-run all migrations
	  migrate:reset                Rollback all database migrations
	  migrate:rollback             Rollback the last database migration
	queue
	  queue:failed                 List all of the failed queue jobs
	  queue:failed-table           Create a migration for the failed queue jobs database table
	  queue:flush                  Flush all of the failed queue jobs
	  queue:forget                 Delete a failed queue job
	  queue:listen                 Listen to a given queue
	  queue:restart                Restart queue worker daemons after their current job.
	  queue:retry                  Retry a failed queue job
	  queue:subscribe              Subscribe a URL to an Iron.io push queue
	  queue:work                   Process the next job on a queue
	session
	  session:table                Create a migration for the session database table
	view
	  view:publish                 Publish a package's views to the application


