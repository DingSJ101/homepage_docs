---
abbrlink: 21380a8
title: Mysql配置
categories:
  - Linux
---
Mysql

## 安装

```bash
# https://downloads.mysql.com/archives/community/
wget https://cdn.mysql.com/archives/mysql-8.0/mysql-8.0.20-linux-glibc2.12-x86_64.tar.xz
tar -xvf mysql-8.0.20-linux-glibc2.12-x86_64.tar.xz
sudo mv mysql-8.0.20-linux-glibc2.12-x86_64 /usr/local/mysql # 移动到usr
cd /usr/local/mysql
mkdir data
sudo groupadd mysql
sudo useradd -g mysql mysql
sudo chown -R mysql.mysql /usr/local/mysql/
cd bin
sudo ./mysqld --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data/ --initialize
sudo vi /etc/my.cnf
## 修改如下
[mysqld]
      basedir = /usr/local/mysql   
      datadir = /usr/local/mysql/data
      socket = /usr/local/mysql/mysql.sock
      character-set-server=utf8
      port = 3306
      sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
[client]
      socket = /usr/local/mysql/mysql.sock
      default-character-set=utf8
## 注释mysqld_safe
cd ..
sudo cp -a ./support-files/mysql.server /etc/init.d/mysql #添加mysqld服务到系统
sudo chmod +x /etc/init.d/mysql
chkconfig --add mysql
## sudo vi /etc/apt/sources.list
## deb http://archive.ubuntu.com/ubuntu/ trusty main universe restricted multiverse
## sudo apt-get update 
## sudo apt-get install sysv-rc-conf
## sudo sudo cp /usr/sbin/sysv-rc-conf /usr/sbin/chkconfig

service mysql start #启动mysql服务 
service mysql status #查看启动状态 
sudo ln -s /usr/local/mysql/bin/mysql /usr/bin # 将mysql命令添加到服务  
```

## 登录

```bash
mysql -uroot -p #用临时密码登录mysql    wAIOuHQwM7!M
```

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456'; #修改root密码
flush privileges;  #使密码生效
use mysql;
update user set host='%' where user='root';
flush privileges;
exit;
```

```bash
sudo firewall-cmd --add-port=3306/tcp --permanent #开放防火墙端口,加载生效
sudo firewall-cmd --reload
```



