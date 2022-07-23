---
abbrlink: '60e45118'
title: Django部署
categories:
  - 后端
  - Django
---
# nginx + uwsgi

```bash
sudo apt-get install nginx
sudo iptables -I INPUT -p tcp --dport 8001 -j ACCEPT #开放防火墙端口
wget https://projects.unbit.it/downloads/uwsgi-2.0.20.tar.gz

## sudo iptables -I INPUT -p tcp --dport 22 -j ACCEPT #开放防火墙端口

apt-get install python3-setuptools
apt-get install python3-dev
python3 setup.py install
# no libpython3.9
    ## wget -t 100 -c https://www.python.org/ftp/python/3.9.7/Python-3.9.7.tgz
    ## cd Python-3.9.7/
    ## ./configure --prefix=/tmp/Python
    ## make -j4
    ## make install
    ## cp /tmp/Python/lib/libpython3.9.a ~/miniconda3/lib/python3.9/config-3.9-x86_64-linux-gnu/

# 配置 nginx
cd /etc/nginx/sites-available
vim default
## 在server{}内增加
    server_name 175.24.167.6;
    error_log       /var/log/nginx/web_error.log;
      location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to displaying a 404.
            # try_files $uri $uri/ =404;
            include  uwsgi_params;#通过uwsgi转发请求
                    uwsgi_pass  127.0.0.1:8000;  
        }
    location /static {# 访问静态资源
       alias /home/dsj/workspace/ai-care/static;
    }
##
sudo nginx -t
sudo service nginx restart

# 配置 uwsgi
cd ai-care
vim uwsgi.ini
## add
    [uwsgi]
    chdir = /home/dsj/workspace/ai-care
    module = ai-care.wsgi:application
    socket = 127.0.0.1:8000 
    master = True         
    daemonize = /home/dsj/workspace/ai-care/run.log
## 
vim __init__.py
## add
    import pymysql
    pymysql.install_as_MySQLdb()
##
uwsgi --ini ../uwsgi.ini
## sudo fuser -k 8000/tcp #杀死该端口进程

```

## 运维

### log

/home/dsj/workspace/ai-care/run.log

```verilog
invalid request block size: 21573 (max 4096)...skip
# 直接使用http访问，非法
# 配置的是socket访问，限定了数据包大小
```

/var/log/nginx/error.log

/var/log/nginx/web_error.log

/var/log/nginx/access.log



### 命令

```bash
lsof -i:8000 # 查看端口使用
sudo fuser -k 8000/tcp #杀死该端口进程
    lsof -i:8080：查看8080端口占用
    lsof abc.txt：显示开启文件abc.txt的进程
    lsof -c abc：显示abc进程现在打开的文件
    lsof -c -p 1234：列出进程号为1234的进程所打开的文件
    lsof -g gid：显示归属gid的进程情况
    lsof +d /usr/local/：显示目录下被进程开启的文件
    lsof +D /usr/local/：同上，但是会搜索目录下的目录，时间较长
    lsof -d 4：显示使用fd为4的进程
    lsof -i -U：显示所有打开的端口和UNIX domain文件
ps aux | grep nginx
sudo netstat -anp | grep 端口号  # 显示 tcp，udp 的端口和进程等相关情况

```

1..

