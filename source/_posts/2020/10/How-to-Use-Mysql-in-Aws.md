---
title: How to Use Mysql in Aws
date: 2020-10-28 11:48:29
categories: 技术
tags: 指导
---
Aws is very famous product.Is very very popular in Japan.
This part we all practise mysql in Aws .
__This is Amazon product Many people not know yet.__
[Aws site in Japan](https://aws.amazon.com/jp/)

<!-- more -->

## 安装数据库
``` sql
sudo yum install php-mysql php-mbstring php-gd
# php博客
# wget http://ja.wordpress.org/latest-ja.zip
# 安装mariadb
sudo yum install mariadb mariadb-server
# 启动mariadb
sudo systemctl start mariadb
# 登陆
mysql -u root -p
# 创建用户
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
## EC2 里面的mysql使用mysqlbench连接
```
Connection Name:myblogs
Connection Method:Stand TCP/IP over SSH
SSH Hostname:公网ip
SSH Username:ec2-user
SSH key File:pem文件
# 其他默认
```
