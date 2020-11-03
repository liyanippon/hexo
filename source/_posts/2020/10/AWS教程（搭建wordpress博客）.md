---
title: AWS教程（搭建wordpress博客）
date: 2020-10-31 13:53:45
categories: 技术
tags: 指导
---

自己使用aws搭建wordpress的主要过程
<!-- more -->


## EC2
ssh登录（macos）
chmod 400 ~/.ssh/mykeypair.pem
ssh -i ~liyan/.ssh/tomosta.pem ec2-user@52.198.102.128
ssh -i ~liyan/.ssh/javaweb-docker-key.pem ec2-user@18.183.135.53

http://liyanippon.com

更新系统 sudo yum update,sudo yum update
安装web服务器 sudo yum install httpd
设置固定IP 52.198.102.128
启动 sudo systemctl start httpd
结束 sudo systemctl stop httpd
重启 sudo systemctl restart httpd
状态 sudo systemctl status httpd

## php服务器测试

进入 cd /var/www/html
新建文件 sudo vim index.html
保存后可查看
sudo vim sum.php

```php
<?php
$sum =10 +1;
echo $sum;
?>
```
安装php
sudo yum install php

## 安装数据库

sudo yum install php-mysql php-mbstring php-gd
php博客
wget http://ja.wordpress.org/latest-ja.zip
安装mariadb
sudo yum install mariadb mariadb-server
启动mariadb
sudo systemctl start mariadb
登陆
mysql -u root -p
创建用户

```sql
# 设置密码
update mysql.user set password=password('root') where user='root';
flush privileges;
# 创建用户
create user 'wordpress'@'localhost' identified by 'admin123456';
create database wordpressdb;
grant all privileges on wordpressdb.* to 'wordpress'@'localhost';
flush privileges;
show databases;
```
登陆  mysql -u root -p
sudo cp -R wordpress/* /var/www/html
sudo amazon-linux-extras install php7.2 -y
再次重启服务器，看到首页
sudo cp wp-config-sample.php wp-config.php

## 配置数据库连接

修改文件 wp-config.php
```php
define( 'DB_NAME', 'wordpressdb' );
define( 'DB_USER', 'wordpress' );
define( 'DB_PASSWORD', '123456' );
```
## 测试

测试登录

| 用户名 | 权限 | 邮箱 | 密码 |
| :-----| :---: | :----: | :----: |
| liyanippon | 管理员 | liyanippon@outlook.com | hahaha |
| sakichan | 编辑者 | jiangjiang@yahoo.co.jp | hahaha |

## 文件传输（ec2）

ssh -i path_to_pemkey hostname@public_ip *//建立链接* 
mkdir upload  *//创建接受文件的文件夹* 
chmod 777 upload 
**上传文件**
scp -i ~liyan/.ssh/tomosta.pem Documents/Develop/DevelopFile/python/python-test3.py ec2-user@52.198.102.128:/home/ec2-user/upload
**下载文件**
scp -i ~liyan/.ssh/tomosta.pem ec2-user@52.198.102.128:/home/ec2-user/upload/python-test3.py .

## VPC

VPC设置(还有一些问题，以后修改)
+ tag：javaweb-docker-test
+ ipv4: 192.168.1.0/24
sunet设置
+ tag: javaweb-docker-subnet
+ Availability zone: ap-northeast-1a
+ ipv4 cidr:192.168.1.0/25
internet-gw:设置
+ tag：javaweb-docker-igw
vpc-attach设置
+ ec2: 172.31.10.98

## Docker
安装
+ sudo docker run --rm -d -p 80:80 hello-docker-ec2
启动服务
+ sudo service docker start

## Springboot
ec2 默认只有80可以使用
AWS EC2设置安全组开放端口
找到已经运行的实例，拖到最右边，点击【安全组】下面那个link
依次点击【入站】，【编辑】
点击【添加规则】
安装maven
sudo yum install maven
编译并运行
mvn spring-boot:run

更改httpd的端口号（tomcat），ngnix保持不变
+ find /etc/httpd/ -name *conf
+ /etc/httpd/conf.d/php.conf
+ /etc/httpd/conf.modules.d/10-php.conf
+ /etc/httpd/conf/httpd.conf

eclipse springboot 打包
在项目的pom.xml中配置相关的内容
```xml
<build>    	
	<finalName>api</finalName>        
	<plugins>            
		<plugin>                
			<groupId>org.springframework.boot</groupId>               
			 <artifactId>spring-boot-maven-plugin</artifactId>            
		</plugin>        
	</plugins>    
</build>
```

## AWS RDB 使用
+ DB インスタンス識別子:database-demo
+ マスターユーザー名:admin
+ パスワード:admin123456
+ linux连接数据库命令(需要在一个vpc下) 需要开放端口
mysql -h database-blog.cbakleqdftvx.ap-northeast-1.rds.amazonaws.com -P 3306 -u admin -p

## 建站步骤
+ 域名申请
+ 域名设置
+ 安全证书申请
+ 配置负载均衡
  安全证书使用(博客代码有问题，暂时不使用证书)
+ Rout53 创建记录 
  - www	
  - cname
  - 值 cert-manager-blog-alb-281286114.ap-northeast-1.elb.amazonaws.com

## Ngnix使用

启动服务
>sudo nginx -c /etc/nginx/nginx.conf

停止服务
>ps -ef | grep nginx
kill -TERM 主进程号
kill -9 主进程号 
修改sudo vim /etc/nginx/nginx.conf
