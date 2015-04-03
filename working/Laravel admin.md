#Laravel Admin

## 框架构建
系统采用 Laravel5.0 为基础进行开发，前端采用 ACE 模板框架
- bugbar https://github.com/barryvdh/laravel-debugbar amazing
    
        maximebf/debugbar suggests installing kriswallsmith/assetic (The best way to manage assets)
        maximebf/debugbar suggests installing predis/predis (Redis storage)
        尚未做


- auth   
- acl    https://github.com/kodeine/laravel-acl/

## 新建模型
### 后台管理员模型

php artisan make:model yun_admin



## 功能模块
### 用户与登录
- 使用系统默认的Auth模块
- 后台路由加入auth中间件
- 用artisan生成用户表
- 在后台建立User模型

### 权限
- 修改composer.json，加入kodeine/laravel-acl模块，composer update进行安装
- 详情见https://github.com/kodeine/laravel-acl/wiki/Installation
- 数据库增加权限相关表
- 用户模型增加权限类使用
- Http kernel
###用户，角色，日志，基本信息

#### 设备管理
#### 用户管理
#### 留言反馈
#### 数据报表