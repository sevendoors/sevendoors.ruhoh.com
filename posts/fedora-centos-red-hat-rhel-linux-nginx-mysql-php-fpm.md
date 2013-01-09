---
title: Fedora17/16,CentOS/Red Hat(RHEL)6.3/5.8 配置 LEMP
date: '2013-01-09'
description:
categories: '工具'
tags:
- LEMP
- Fedora
- Centos
- Red Hat
- Nginx
- MySQL
- PHP-FPM
---

##什么是LEMP？
LEMP(Linux,Nginx,MySQL,PHP) 相对于LAMP来说也不遑多让。

而对于新手来说，nginx配置更简单，基于这种原因，有人说Nginx比Apache强大。

当然，Nginx和Apache到底谁更优不是我能说得清楚的，很多大企业面临这个选择也是犯难，幸好，这不在本文的讨论范围。

##教你安装LEMP
关于Linux,Nginx,MySQL,PHP-FPM，官方文档都有详细的安装说明文档。

这个指南试图详尽地指导你在Fedora17/16/15/14,Centos 6.3/6.2/6.1/6/5.8,RedHat 6.3/6.3/6.1/6/5.8上面通过YUM的方式部署你的LEMP服务。

总得来说，可分为三步：

	1.安装Linux(这个过程网上已有大量的教程，并且现在的Linux版本安装过程相当友好)
	2.安装MySQL
	3.安装Nginx和PHP(PHP-FPM)

以上每一步，都是分开的。MySQ和Nginx还有PHP其实没有多大的依赖关系。

##安装MySQL5.5
MySQL 是一种关系型数据库，相当多的CMS是使用这种数据库的，几乎可以说，所有的开源PHP应用都是用MySQL，至少是支持的。比如国外的：drupal,joomla,wordpress.国内的：dedecms,帝国等等。

	如果你是升级版本，请一定记得备份你的数据库和Mysql配置文件，并且记得先运行下

	mysql_upgrade

###1.更改权限

	su -
	## 或者 ##
	sudo -i

###2.安装 Remi repository

Fedora

	## Remi Dependency on Fedora 18, 17, 16
	rpm -Uvh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm 
	rpm -Uvh http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm
	 
	## Fedora 18 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-18.rpm
	 
	## Fedora 17 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-17.rpm
	 
	## Fedora 16 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-16.rpm
	 
	## Fedora 15 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-15.rpm
	 
	## Fedora 14 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-14.rpm
	 
	## Fedora 13 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-13.rpm
	 
	## Fedora 12 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-12.rpm
	
CentOS 和 Red Had(RHEL)

	## Remi Dependency on CentOS 6 and Red Hat (RHEL) 6 ##
	rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
	 
	## CentOS 6 and Red Hat (RHEL) 6 ##
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
	 
	 
	## Remi Dependency on CentOS 5 and Red Hat (RHEL) 5 ##
	rpm -Uvh http://dl.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm
	 
	## CentOS 5 and Red Hat (RHEL) 5 ## 
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-5.rpm
	
###3.检查可用的MySQL版本

Fedora 18,17,16,15,14,13,12

	yum --enablerepo=remi list mysql mysql-server
	
CentOS 6.3/6.2/6.1/6/5.8 和 Red Hat(RHEL) 6.3/6.2/6.1/6/5.8

	yum --enablerepo=remi,remi-test list mysql mysql-server
	
如果正常的话，输出会像这个样子

	Loaded plugins: changelog, fastestmirror, presto, refresh-packagekit
	...
	remi                                                            | 3.0 kB     00:00     
	remi/primary_db                                                 | 106 kB     00:00     
	Available Packages
	mysql.i686                               5.5.29-1.fc14.remi                        @remi
	mysql-server.i686                        5.5.29-1.fc14.remi                        @remi
	

###4.升级或者安装MySLQ 5.5.29

Fedora 18,17,16,15,14,13,12

	yum --enablerepo=remi install mysql mysql-server


CentOS 6.3/6.2/6.1/6/5.8 和 Red Hat(RHEL) 6.3/6.2/6.1/6/5.8

	yum --enablerepo=remi,remi-test install mysql mysql-server
	
###5.开启MySQL服务并且配置其随系统启动

Fedora 18/17/16

	systemctl start mysqld.service ## use restart after update
	 
	systemctl enable mysqld.service
	
Fedora 15/14/13/12/11, CentOS 6.3/6.2/6.1/6/5.8 和 Red Hat(RHEL) 6.3/6.2/6.1/5.8

	/etc/init.d/mysqld start ## use restart after update
	## 或者 ##
	service mysqld start ## use restart after update
	 
	chkconfig --levels 235 mysqld on
	

###6.MySQL 安全安装
这一步很容易忽略，但是它尤为重要。你可以选择手动完成以下任务，也可以使用MySQL自带的安全检查步骤。

+ 设置（更改）ROOT密码
+ 删除匿名帐户
+ 不允许ROOT的远程登录
+ 移出test数据库
+ 重建权限表

使用MySQL自带的安全检查：

	/usr/bin/mysql_secure_installation

输出会是这个样子：

	NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MySQL
	      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!
	 
	 
	In order to log into MySQL to secure it, we\'ll need the current
	password for the root user.  If you\'ve just installed MySQL, and
	you haven\'t set the root password yet, the password will be blank,
	so you should just press enter here.
	 
	Enter current password for root (enter for none): 
	OK, successfully used password, moving on...
	 
	Setting the root password ensures that nobody can log into the MySQL
	root user without the proper authorisation.
	 
	Set root password? [Y/n] Y
	New password: 
	Re-enter new password: 
	Password updated successfully!
	Reloading privilege tables..
	 ... Success!
	 
	 
	By default, a MySQL installation has an anonymous user, allowing anyone
	to log into MySQL without having to have a user account created for
	them.  This is intended only for testing, and to make the installation
	go a bit smoother.  You should remove them before moving into a
	production environment.
	 
	Remove anonymous users? [Y/n] Y
	 ... Success!
	 
	Normally, root should only be allowed to connect from 'localhost'.  This
	ensures that someone cannot guess at the root password from the network.
	 
	Disallow root login remotely? [Y/n] Y
	 ... Success!
	 
	By default, MySQL comes with a database named 'test' that anyone can
	access.  This is also intended only for testing, and should be removed
	before moving into a production environment.
	 
	Remove test database and access to it? [Y/n] Y
	 - Dropping test database...
	 ... Success!
	 - Removing privileges on test database...
	 ... Success!
	 
	Reloading the privilege tables will ensure that all changes made so far
	will take effect immediately.
	 
	Reload privilege tables now? [Y/n] Y
	 ... Success!
	 
	Cleaning up...
	 
	 
	 
	All done!  If you\'ve completed all of the above steps, your MySQL
	installation should now be secure.
	 
	Thanks for using MySQL!


注意：如果你有特别的原因，不愿意使用MySQL自带的安全检查，你至少应该更改ROOT密码。

	mysqladmin -u root password [your_password_here]
	 
	## Example ##
	mysqladmin -u root password myownsecrectpass
	

###7.本地连接MySQL数据库

	mysql -u root -p
	 
	## 或者 ##
	mysql -h localhost -u root -p

###8.创建数据库，创建用户，允许远程连接
这个示例将使用以下参数

+ 数据库（名字）	=	webdb
+ 用户				=	webdb_user
+ 远程IP			=	10.0.15.25
+ 密码				=	password123
+ 权限				=	ALL

示例如下：

	## CREATE DATABASE ##
	mysql> CREATE DATABASE webdb;
	 
	## CREATE USER ##
	mysql> CREATE USER 'webdb_user'@'10.0.15.25' IDENTIFIED BY 'password123';
	 
	## GRANT PERMISSIONS ##
	mysql> GRANT ALL ON webdb.* TO 'webdb_user'@'10.0.15.25';
	 
	##  FLUSH PRIVILEGES, Tell the server TO reload the GRANT TABLES  ##
	mysql> FLUSH PRIVILEGES;
	
**远程连接**

1.编辑文件/etc/sysconfig/iptables:

	vim /etc/sysconfig/iptables

2.在最后加入这一行

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
	
3.重启防火墙

	service iptables restart
	## OR ##
	/etc/init.d/iptables restart
	
4.测试远程连接：

	mysql -h dbserver_name_or_ip_address -u webdb_user -p webdb
	
##安装Nginx和PHP-FPM
###1.更改用户权限

	sudo -i
	##或者##
	su -

###2.安装依赖的repositiory

Fedora 17/16/15/14 Remi repository

	## Remi Dependency on Fedora 17, 16
	rpm -Uvh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm 
	rpm -Uvh http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm
	 
	## Fedora 17 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-17.rpm
	 
	## Fedora 16 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-16.rpm
	 
	## Fedora 15 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-15.rpm
	 
	## Fedora 14 ##
	rpm -Uvh http://rpms.famillecollet.com/remi-release-14.rpm
	

CentOS 6.3/6.2/6.1/6/5.8 and Red Hat (RHEL) 6.3/6.2/6.1/6/5.8 Remi repository

	## Remi Dependency on CentOS 6 and Red Hat (RHEL) 6 ##
	rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
	 
	## CentOS 6 and Red Hat (RHEL) 6 ##
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
	 
	 
	## Remi Dependency on CentOS 5 and Red Hat (RHEL) 5 ##
	rpm -Uvh http://dl.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm
	 
	## CentOS 5 and Red Hat (RHEL) 5 ## 
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-5.rpm

CentOS 6.3/6.2/6.1/6/5.8 and Red Hat (RHEL) 6.3/6.2/6.1/6/5.8的话，还需要在yum源里添加nginx。
**创建文件/etc/yum.repos.d/nginx.repo，然后加入以下内容**
CentOS

	[nginx]
	name=nginx repo
	baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
	gpgcheck=0
	enabled=1
	
Red Hat (RHEL)

	[nginx]
	name=nginx repo
	baseurl=http://nginx.org/packages/rhel/$releasever/$basearch/
	gpgcheck=0
	enabled=1
	
###3.安装nginx,PHP5.4.10和PHP-FPM
Fedora 17/16/15/14, CentOS 6.3/5.8 and Red Hat (RHEL) 6.3/5.8
	
	yum --enablerepo=remi install nginx php php-fpm php-common
	
CentOS 6.3/5.8 and Red Hat (RHEL) 6.3/5.8

	yum --enablerepo=remi,remi-test install nginx php php-fpm php-common

###4.安装PHP5.4.10的模块

+ APC (php-pecl-apc) – APC缓存和加速
+ CLI (php-cli) – 命令行接口
+ PEAR (php-pear) – PHP的一个扩展库
+ PDO (php-pdo) – 数据库连接的抽象层的一个库
+ MySQL (php-mysql) – 使得PHP能使用mysql的一个模块
+ PostgreSQL (php-pgsql) – 使PHP能使用PostgreSQL的模块
+ MongoDB (php-pecl-mongo) – PHP的MongoDB驱动
+ SQLite (php-sqlite) – 使用SQLite V2 扩展
+ Memcache (php-pecl-memcache) – 可使用Memcached缓存系统的扩展
+ Memcached (php-pecl-memcached) –可使用Memcached缓存服务的扩展
+ GD (php-gd) – GD库
+ XML (php-xml) – XML库
+ MBString (php-mbstring) – mbstring库，可处理多国语言字符
+ MCrypt (php-mcrypt) – 标准mcrypt库，用于字符串加密

Fedora 17/16/15/14

	yum --enablerepo=remi install php-pecl-apc php-cli php-pear php-pdo php-mysql php-pgsql php-pecl-mongo php-sqlite php-pecl-memcache php-pecl-memcached php-gd php-mbstring php-mcrypt php-xml

CentOS 6.3/5.8 and Red Hat (RHEL) 6.3/5.8

	yum --enablerepo=remi,remi-test install php-pecl-apc php-cli php-pear php-pdo php-mysql php-pgsql php-pecl-mongo php-sqlite php-pecl-memcache php-pecl-memcached php-gd php-mbstring php-mcrypt php-xml

	
###5.停止httpd(Apache)服务，开启Nginx和PHP-FPM服务。
停止 httpd(Apache)

	/etc/init.d/httpd stop
	## OR ##
	service httpd stop
	
开启Nginx

	/etc/init.d/nginx start ## use restart after update
	## OR ##
	service nginx start ## use restart after update
	
开启PHP-FPM

	/etc/init.d/php-fpm start ## use restart after update
	## OR ##
	service php-fpm start ## use restart after update
	
###6.使Nginx和PHP-FPM随系统启动而启动
取消httpd的启动

	chkconfig httpd off
	
自动启动Nginx

	chkconfig --add nginx
	chkconfig --levels 235 nginx on

自动启动PHP-FPM

	chkconfig --add php-fpm
	chkconfig --levels 235 php-fpm on

###7.配置Nginx和PHP-FPM
例子中使用`testsite.local`作为站点，但是这不是正式的站点。
正式的站点是像`blog.mlskill.com`这个样子

	## public_html directory and logs directory ##
	mkdir -p /srv/www/testsite.local/public_html
	mkdir /srv/www/testsite.local/logs
	chown -R apache:apache /srv/www/testsite.local

非必须的步骤，添加日志到/var/log目录

	## public_html directory and logs directory ##
	mkdir -p /srv/www/testsite.local/public_html
	mkdir -p /var/log/nginx/testsite.local
	chown -R apache:apache /srv/www/testsite.local
	chown -R nginx:nginx /var/log/nginx
	
【注意】我在这里用户和用户组都使用Apache是因为PHP-FPM默认是以apache为用户的，你也可以自定义。

创建站点的nginx的配置文件

	mkdir /etc/nginx/sites-available
	mkdir /etc/nginx/sites-enabled
	
在/etc/nginx/nginx.conf文件中加入下面这行，在`include /etc/nginx/conf.d/*.conf`这行后面（在http的大括号里面）

	## Load virtual host conf files. ##
	include /etc/nginx/sites-enabled/*;
	
创建testsites.local虚拟站点文件

把下面的内容加入/etc/nginx/sites-available/testsite.local。这是最基础的站点配置。

	server {
	    server_name testsite.local;
	    access_log /srv/www/testsite.local/logs/access.log;
	    error_log /srv/www/testsite.local/logs/error.log;
	    root /srv/www/testsite.local/public_html;
	 
	    location / {
	        index index.html index.htm index.php;
	    }
	 
	    location ~ \.php$ {
	        include /etc/nginx/fastcgi_params;
	        fastcgi_pass  127.0.0.1:9000;
	        fastcgi_index index.php;
	        fastcgi_param SCRIPT_FILENAME /srv/www/testsite.local/public_html$fastcgi_script_name;
	    }
	}
	
将你的虚拟站点链接到/etc/nginx/sites-enabled已开启站点

	cd /etc/nginx/sites-enabled/
	ln -s /etc/nginx/sites-available/testsite.local
	service nginx restart

将testsite.local作为域名加入/etc/hosts文件

	127.0.0.1               localhost.localdomain localhost testsite.local

**如果你使用另外一台机器作为你的Nginx服务器，把它的公有IP加入/etc/hosts**

	10.0.2.19				wordpress

###8.测试
创建一个测试文件 /srv/www/testsite.local/publick_html/index.php

	<?php 
	    echo phpinfo();
	?>
	
	
在你的浏览器地址栏输入 http://testsite.local/ 你就能看到你的PHP信息了。


**远程连接**

1.编辑文件/etc/sysconfig/iptables:

	vim /etc/sysconfig/iptables

2.在最后加入这一行

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
	
3.重启防火墙

	service iptables restart
	## OR ##
	/etc/init.d/iptables restart
	
4.测试远程连接：
在浏览器打开你的域名，`http://your.domain`


预告：
1.Nginx和PHP-FPM的详细设置和加速方面的设置。
2.APC的设置和注意事项。
