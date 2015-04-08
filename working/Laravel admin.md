#Laravel Admin

## 框架构建
系统采用 Laravel5.0 为基础进行开发，前端采用 ACE 模板框架  

- bugbar https://github.com/barryvdh/laravel-debugbar amazing  
- auth 系统默认   
- acl  https://github.com/Zizaco/entrust/tree/laravel-5
- debug barryvdh/laravel-debugbar


##安装Laravel

composer create-project laravel/laravel

##配置Laravel

- 命名空间
php artisan app:name Yun

- config/app.php
timezone and site_url

- storage write enable
chown -R nginx.nginx yun

- Nginx 配置

	location /{
		try_files $uri $uri/ /index.php?$query_string;
	}

- 缓存配置
将配置文件生成缓存，以保障程序运行时提升速度
php artisan config:cache

- 数据库配置
.env 文件，修改mysql数据库配置

### Laravel artisan

- php artisan app:name Yun
设置App 命名空间

- php artisan migrate:install
在数据库中建立一个migrations 的表，用来记录migrate的操作日志

- php artisan migrate
执行database 下migrations下的php脚本

- php artisan migrate:reset 
database 清空，migrations 表清空

- php artisan make:migrate create_temp_table --create temp
会在database/magrations 下建立一个php操作文件，为进一步生成数据库做准备，可以是任何一个数据库操作。

- php artisan config:cache
将配置文件生成缓存，以保障程序运行时提升速度

- php artisan up/down 
站点启动或者维护，模板位于 resources/views/errors/503.blade.php

- php artisan fresh
去除默认的脚手架，也就是auth模块

### 路由

- HomeController@index
前往修改默认的首页样式


### 增加认证模块

认证的配置文件放在 config/auth.php 里，
app 文件夹包含了一个使用默认 Eloquent 认证驱动的 App\User 模型
密码 字段至少 60个字符宽度
user 数据库表包含一个 remember_token 长度为100 的string类型，可接受null 字段

###两个默认的控制器
- AuthorController 处理用户注册和登录
- PasswordController 处理用户密码重置
- 视图放在 /resources/views/auth 下
#### 用户注册

要修改用户新注册时所用到的表单字段，可以修改 App\Services\Registrar 类，此类负责验证和建立应用的新用户
Registrar 的 validator 方法包含增加新用户时的验证规则，而Registrar 的create 方法负责在数据库中建立一条新的user记录，可以自由修改，Registrar方法时通过AuthenticatesAndRegisterUsers trait 中的 AuthController 调用的。

#### 手动认证
如果不想使用预设的AuthController ，可以直接使用Laravel的身份验证来管理用户认证。

	attempt方法：方法接受由键值对组成的数组作为第一个参数，password 的值会先进行哈希，数组中的其他的值会被用来查询数据表中的用户。
	如果认证成功，attempt 将会返回 true，否则返回 false

	intended 方法会重定向到用户尝试访问的url，其值会在进行认证过滤之前先被存起来，也可以给这个方法传入一个预设的URI，放置重定向的网址无法使用

#### 特定条件验证用户
	if (Auth::attempt(['email' => $email, 'password' => $password, 'active' => 1]))
	{
    	// The user is active, not suspended, and exists.
	}
	
#### 判断用户是否已经验证
	if (Auth::check())
	{
	    // The user is logged in...
	}
#### 认证用户并且【记住】

	if (Auth::attempt(['email' => $email, 'password' => $password], $remember))
	{
	    // The user is being remembered...
	}

#### 用户登出

	Auth::logout();

#### 认证事件

当 attempt 方法被调用时，auth.attempt 事件 会被触发，当用户认证成功并且登录了，auth.login 事件会被触发

#### 保护路由

路由中间件 只允许通过认证的用户访问指定的路由，Laravel默认提供了auth 中间件，放在 app\http\Middleware\
Authenticate.php ，需要将其加到路由定义中

	Route::get('profile',['middleware'=>'auth',function(){
		// only authenticated users may enter
	}])

	Route::get('profile',['middleware'=>'auth','users' => 'ProfileController@show']);

#### Http 基本认证
Http 基本认证提供了一个快速的方式来认证用户而不需特定设置一个登录页面，类似apache的授权认证。

## Auth 系统设定
- php artisan make:model Admin
会在app根目录下建立一个 Admin.php 和在database/migrations 下建立一个数据库操作文件

- 修改 model Admin
使用 Auth trait，使用contracts\auth Authenticable and CanResetPassword 接口，extend Model类
设置 table，fillable，hidden

- 修改Yun\Services\Registrar.php
设置验证规则，插入规则

- 修改Yun\Middleware\RedirectIfAuthenticated 
修改重定向的地址为后台地址。

- 修改Yun\Controllers\WelcomeController.php
修改构造函数，去除权限验证。

## 角色，权限，用户系统
https://github.com/Zizaco/entrust/tree/laravel-5

### Installation

In order to install Laravel 5 Entrust, just add 

    "zizaco/entrust": "dev-laravel-5"

to your composer.json. Then run `composer install` or `composer update`.

Then in your `config/app.php` add 

    'Zizaco\Entrust\EntrustServiceProvider'
    
in the providers array and

    'Entrust' => 'Zizaco\Entrust\EntrustFacade'
    
to the `aliases` array.

### Configuration

Set the property values in the `config/auth.php`.
These values will be used by entrust to refer to the correct user table and model.

You can also publish the configuration for this package to further customize table names and model namespaces.  
Just use `php artisan vendor:publish` and a `entrust.php` file will be created in your app/config directory.

### User relation to roles

Now generate the Entrust migration:

```bash
php artisan entrust:migration
```

It will generate the `<timestamp>_entrust_setup_tables.php` migration.
You may now run it with the artisan migrate command:

```bash
php artisan migrate
```

After the migration, four new tables will be present:
- `roles` &mdash; stores role records
- `permissions` &mdash; stores permission records
- `role_user` &mdash; stores [many-to-many](http://laravel.com/docs/4.2/eloquent#many-to-many) relations between roles and users
- `permission_role` &mdash; stores [many-to-many](http://laravel.com/docs/4.2/eloquent#many-to-many) relations between roles and permissions

### 测试角色，权限，用户，路由

#### 建立角色
    $owner = new Role();
    $owner->name         = 'super';
    $owner->display_name = 'super admin'; // optional
    $owner->description  = 'super'; // optional
    $owner->save();
    
    $admin = new Role();
    $admin->name         = 'admin';
    $admin->display_name = 'company administrator'; // optional
    $admin->description  = 'company administrator'; // optional
    $admin->save();

#### 给用户添加角色

    $user = User::where('username', '=', 'nosun')->first();
    $user->roles()->attach($admin->id); // id only

#### 添加权限

    $createPost = new Permission();
    $createPost->name         = 'create-post';
    $createPost->display_name = 'Create Posts'; // optional
    // Allow a user to...
    $createPost->description  = 'create new blog posts'; // optional
    $createPost->save();
    
    $editUser = new Permission();
    $editUser->name         = 'edit-user';
    $editUser->display_name = 'Edit Users'; // optional
    // Allow a user to...
    $editUser->description  = 'edit existing users'; // optional
    $editUser->save();

#### 给角色添加权限

    $admin->attachPermission($createPost);
    // equivalent to $admin->perms()->sync(array($createPost->id));
    
    $owner->attachPermissions(array($createPost, $editUser));
    // equivalent to $owner->perms()->sync(array($createPost->id, $editUser->id));
    
#### 测试权限，角色

    $user->hasRole('owner');   // false
    $user->hasRole('admin');   // true
    $user->can('edit-user');   // false
    $user->can('create-post'); // true

#### 测试路由
在Yun\Http\routes.php 末尾增加规则

    Entrust::routeNeedsRole('admin/*', 'admin');

## 建立基础的数据表
- yun_admin
- yun_company
- yun_device
- yun_product
- yun_user
- yun_admin_log
- yun_app
- yun_app_version
- yun_file
- yun_device_module
- yun_device_module_soft

### 建立基础的模型
- Admin
- App
- AppVersion
- Company
- Device
- DeviceModule
- DeviceModuleSoft
- File
- Permission
- Role
- User

### 建立基础的控制器
- Admin
    - RoleController
    - UserController
    - PermissionController
    - ProductController
    - FileController
    - DeviceController
    - AppController
    - AdminController
- Front
    - WelcomeController
- Auth
    - AuthController
    - PasswordController          
### 建立设置权限
与模型对应
- Admin
- App
- AppVersion
- Company
- Device
- DeviceModule
- DeviceModuleSoft
- File
- Permission
- Role
- User

### 建立路由


### 建立连接


### 建立视图



### 前端进行压缩
使用glup工具

