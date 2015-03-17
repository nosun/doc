###用Jekyll+github制作个人博客 ###
在本地的虚拟机上安装总是有些问题，在阿里云的vps上安装一路很顺利。
安装参考 https://ruby-china.org/wiki/install_ruby_guide

#### 安装RVM
	curl -L https://get.rvm.io | bash -s stable
	安装完成后，重新进入下shell，运行
	rvm -v 可以查看到版本信息

#### 用RVM 安装 RUBY
- rvm list known 查看可以安装的版本
- rvm install 2.0.0 安装2.0.0版本

#### 设置Ruby版本
- rvm 2.0.0 --default
- ruby -v 查看ruby版本
- gem -v 查看gem版本

#### 安装jekyll
- gem install jekyll 
系统会安装许多依赖的包，自动安装。


###用jekyll配置站点
- 建立站点

	进入监理站点的目录
	jekyll new www.site.com 生成新的jekyll目录
	cd www.site.com 
	jekyll serve 站点运行了
	curl 127.0.0.1:4000 可以看到首页。

- 配合nginx使用

	log_format   xxx.me  '$remote_addr - $remote_user [$time_local] "$request" '
             '$status $body_bytes_sent "$http_referer" '
             '"$http_user_agent" $http_x_forwarded_for';
	server
        {
                listen       80;
                server_name xxx.me;
                index index.html;
                root /home/wwwroot/xxx.me/_site;
                access_log  /home/wwwlogs/xxx.me.log  xxx.me;
        }

	需要注意的是，jekyll的根目录是./_site ，不是./ 

- 配置文件 

	./_config_yaml 详情查看 http://jekyllrb.com/docs/configuration/
	./_posts 中存放markdown 文档

- 更新站点

	jekyll server -w

- 中文支持

	jekyll默认的markdown解析器maruku对中文的支持不是很好，所以改用RDiscount。
	
	安装rdiscount：
	
	gem install rdiscount
	设置rdiscount：
	
	在_config.yml文件中，添加一行：
	
	markdown: rdiscount


jekyll安装：http://hack0nair.me/2013-10-31-config-jekyll-on-centos/
ruby安装：https://ruby-china.org/wiki/install_ruby_guide