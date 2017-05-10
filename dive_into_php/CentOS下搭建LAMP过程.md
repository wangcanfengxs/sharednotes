#CentOS下搭建LAMP过程:
[TOC]

##1.安装httpd(Apache2)
     yum install httpd httpd-devel
##2.开启httpd
     service httpd start
##3.访问测试
外网访问不了，考虑防火墙的原因。

centos 7关闭防火墙：

	systemctl stop firewalld.service #停止
	systemctl disable firewalld.service #禁用
之前的版本：

	service iptables stop #停止
	chkconfig iptables off #禁用



##4.安装mysql
###(1)下载mysql的repo源
     wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
###(2)安装mysql-community-release-el7-5.noarch.rpm包
     sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
     
 安装这个包后，会获得两个mysql的yum repo源
 
	/etc/yum.repos.d/mysql-community.repo
	/etc/yum.repos.d/mysql-community-source.repo
   
###(3)安装mysql
     sudo yum install mysql-server
     根据步骤安装就可以了，不过安装完成后，没有密码，需要重置密码。
###(4)重置密码
     重置密码前，首先要登录
     mysql -u root
     登录时有可能报这样的错：ERROR 2002 (HY000): Can‘t connect to local MySQL server through     socket ‘/var/lib/mysql/mysql.sock‘ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：
     sudo chown -R openscanner:openscanner /var/lib/mysql
     然后，重启服务：service mysqld restart
接下来登录重置密码：

	$ mysql -u root
	mysql > use mysql;
	mysql > update user set password=password(‘123456‘) where user=‘root‘;
	mysql > exit;
重启服务，顺利登录

##5.安装php
（默认版本时5.4.16）

	yum install php php-devel
##6.验证安装完成
 在/var/www/html/下建立一个php文件

##7.安装php扩展
	yum install php-mysql
安装完成之后，重启httpd
