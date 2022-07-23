---
abbrlink: a477be03
title: nginx
categories: uncategorized
---
## install

```bash
sudo apt-get install nginx
sudo iptables -I INPUT -p tcp --dport 8001 -j ACCEPT #开放防火墙端口
sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT #开放防火墙端口
sudo iptables -I INPUT -p tcp --dport 443 -j ACCEPT #开放防火墙端口
sudo fuser -k 80/tcp

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
```

```bash
## command 
nginx -t # 验证配置
nginx -c "path" # 使用配置文件
nginx -s reload # 载入配置
```

nginx配置路径

```bash
/etc/nginx
├── conf.d
│   └── nginx.conf
├── fastcgi.conf
├── fastcgi_params
├── koi-utf
├── koi-win
├── mime.types
├── modules-available
├── modules-enabled
│   ├── 50-mod-http-geoip.conf -> /usr/share/nginx/modules-available/mod-http-geoip.conf
│   ├── 50-mod-http-image-filter.conf -> /usr/share/nginx/modules-available/mod-http-image-filter.conf
│   ├── 50-mod-http-xslt-filter.conf -> /usr/share/nginx/modules-available/mod-http-xslt-filter.conf
│   ├── 50-mod-mail.conf -> /usr/share/nginx/modules-available/mod-mail.conf
│   └── 50-mod-stream.conf -> /usr/share/nginx/modules-available/mod-stream.conf
├── nginx.conf
├── proxy_params
├── scgi_params
├── sites-available
│   └── default
├── sites-enabled
│   └── default -> /etc/nginx/sites-available/default
├── snippets
│   ├── fastcgi-php.conf
│   └── snakeoil.conf
├── uwsgi_params
└── win-utf

```

nginx.conf

 ![在这里插入图片描述](D:%5CAPPlications%5CTypora%5Cpicturecopy%5C20210428152140590.png)


```yml
# nginx.conf
## 全局配置
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

## 工作模式配置
events {
        worker_connections 768;
}

## http配置
http {
        ## Basic Settings        
        ## SSL Settings
        ## Logging Settings
        ## Gzip Settings
        ## Virtual Host Configs
        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
#
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
server {
        #SSL 访问端口号为 443
        listen 443 ssl;
        #填写绑定证书的域名
        server_name ding-sj.com;
        #证书文件名称
        ssl_certificate ding-sj.com_bundle.crt;
        #私钥文件名称
        ssl_certificate_key ding-sj.com.key;
        ssl_session_timeout 5m;
        #请按照以下协议配置
        ssl_protocols TLSv1.2 TLSv1.3;
        #请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
        location / {
           #网站主页路径。此路径仅供参考，具体请您按照实际目录操作。
           #例如，您的网站运行目录在/etc/www下，则填写/etc/www。
            root html;
            uwsgi_pass 127.0.0.1:8000;
            index  index.html index.htm;
        }
    }

```

### main全局配置

```yml
user www-data; # nginx worker进程运行用户以及用户组，默认nobody账号运行
worker_processes  auto;   # 服务器并发进程数量
pid /var/run/nginx.pid; # 进程文件的位置
include /etc/nginx/modules-enabled/*.conf;
error_log /var/log/nginx/error.log info; #全局错误日志级别，debug|info|notice|warn|error|crit
```



### events工作配置

```yml
event{
	worker_connections  1024;  #最大连接数，需灵活配置，和worker processes共同决定
}
```



### http配置

包括http块和server块：

```yaml
http{
	## http settings
	# ...
	## server settings
	server{	 # 服务器主机配置（虚拟主机、反向代理等）
		# ...
	}
}
```



#### http

```yaml
## http settings 
include       mime.types;      #文件扩展名与文件类型映射表
default_type  application/octet-stream;  # 访问到未定义的扩展名的时候，就默认为下载该文件
# 负载均衡
upstream my_server {
	ip_hash; # 按访问ip的hash结果分配，固定访问一个后端服务器
	server 127.0.0.1:8000 weight=5; # 权重weight,(default = 1),分配到的概率
	server 127.0.0.1:8001 down; # 该主机暂停服务
	server 127.0.0.1:8002 max_fails=3; # 失败最大次数，超过失败最大次数暂停服务
	server 127.0.0.1:8003 fail_timeout=20s; # 如果请求受理失败，暂停指定的时间之后重新发起请求
}
## Basic Settings
sendfile on; # 将文件的回写过程交给数据缓冲去去完成，而不是放在应用中完成，提升性能
tcp_nopush on; # 在一个数据包中发送所有的头文件，而不是一个一个单独发
tcp_nodelay on; # 不缓存数据，而是一段一段发送，如果数据的传输有实时性的要求的话可以配置它
keepalive_timeout 65; # 连接超时时间(s)
client_header_timeout 10; # 请求头的超时时间
client_body_timeout 10; # 请求体的超时时间
send_timeout 10; # 客户端响应超时时间，如果客户端两次操作间隔超过这个时间，服务器就会关闭这个链接
client_header_buffer_size 32k; #上传文件大小限制
# server_tokens off; # 关闭nginx版本提示
# server_names_hash_bucket_size 64;
# server_name_in_redirect off;
# types_hash_max_size 2048; # hash容量，值越大消耗内存越多，散列key冲突率会降低，检索速度更快
include /etc/nginx/mime.types; # 包含另一个文件的指令
default_type application/octet-stream; # 指定默认处理的文件类型可以是二进制

## SSL Settings
#ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
#ssl_prefer_server_ciphers on; # 协商加密算法时，优先使用服务端的加密套件，而不是客户端浏览器的加密套件

## Logging Settings
access_log /var/log/nginx/access.log; # 访问日志
error_log /var/log/nginx/error.log; # 错误日志

## Gzip Settings
gzip on; # 采用gzip压缩的形式发送数据。这将会减少我们发送的数据量
# gzip_vary on; # 是否传输gzip压缩标志
# gzip_proxied any; # 允许或者禁止压缩基于请求和响应的响应流。any，意味着将会压缩所有的请求。
# gzip_comp_level 6; # 压缩级别
# gzip_buffers 16 8k; # 压缩缓冲区
# gzip_http_version 1.1;
# gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript; # 设置需要压缩的数据格式。

## Virtual Host Configs
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
server{}
```



#### server

包括server部分和location部分

```yaml
## server settings
server{	 # 服务器主机配置（虚拟主机、反向代理等）
	## server part
	# ...
	## location part
	location /{ # 路由配置（虚拟目录等）
		# ...
	}
	location /route{
		# ...
	}
}
```

##### server part

```yaml
listen       80 default_server;       #  服务监听的端口号，可选参数default_server
server_name  localhost www.aaa.com;  # 访问域名，空格分隔
# ssl证书配置见文章 https://typecodes.com/web/lnmppositivessl.html
ssl_certificate /etc/nginx/ssl/typecodes.crt;
# ssl密钥文件见文章 https://typecodes.com/web/lnmppositivessl.html
ssl_certificate_key /etc/nginx/ssl/typecodes.key;
ssl_session_cache shared:SSL:20m;
ssl_session_timeout 10m;
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA:!CAMELLIA;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #enables TLSv1, but not SSLv2, SSLv3 which is weak and should no longer be used.
ssl_prefer_server_ciphers on;
add_header Alternate-Protocol 443:npn-spdy/3.1; # 开启spdy功能
add_header Strict-Transport-Security "max-age=31536000; includeSubdomains;"; # 严格的https访问
charset        utf-8; 
access_log     logs/access.log; # 指定该虚拟主机服务器中的访问记录日志存放路径
log_format access '$remote_addr - $remote_user [$time_local] "$request" ' '$status $body_bytes_sent "$http_referer" ' '"$http_user_agent" $http_x_forwarded_for'; #日志格式

location / {} # 虚拟路由
```
##### location part

```yaml
location / {     #表示匹配访问根目录
    root   /data/; # 根目录路径
    index  index.html index.htm; # 不指定访问具体资源时，默认展示的资源文件列表
    # autoindex  on; # 开启目录列表访问,列出当前目录下的资源
}
location /route1 {
	proxy_pass http://localhost:8888; # 反向代理
	proxy_set_header X-real-ip $remote_addr; 
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; #通过X-Forwarded-For获取用户真实IP
	proxy_set_header Host $http_host;
	client_max_body_size 10m; # 允许客户端请求的最大单文件字节数
	client_body_buffer_size 128k; # 缓冲区代理缓冲用户端请求的最大字节数，
	proxy_connect_timeout 90; # nginx跟后端服务器连接超时时间(代理连接超时)
	proxy_send_timeout 90; # 后端服务器数据回传时间(代理发送超时)
	proxy_read_timeout 90; # 连接成功后，后端服务器响应时间(代理接收超时)
	proxy_buffer_size 4k; # 设置代理服务器（nginx）保存用户头信息的缓冲区大小

}
location /route2 {
	include uwsgi_params; # uwsgi 
	uwsgi_pass localhost:8888
}
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ #图片缓存时间设置
{
	expires 10d;
}
# 设置wordpress和typecho博客中，插件目录无法直接访问php或者html文件
location ~ .*/plugins/.*\.(php|php5|html)$ {
	deny  all;
}
#访问favicon.ico时不产生日志
location = /favicon.ico {
	access_log off;
}
#设置40系列错误的应答文件为40x.html
error_page  400 401 402 403 404  /40x.html;
location = /40x.html {
    root   html;
    index  index.html index.htm;
}
```

路由匹配 `location [=|~|~*|^~] /uri/ { … }`，常规路由按照长度依次匹配，`^~`按顺序匹配，成功后停止搜索

~*表明不区分大小写

~区分大小写匹配

^~常规正则串