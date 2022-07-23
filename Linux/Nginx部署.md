---
abbrlink: a1dff2a4
title: Nginx部署
categories:
  - Linux
---
## 基于域名配置

```nginx
# 域名 www.fly.com 的子域名
## /etc/nginx/sites-avaliable/a.conf
server {
        listen 80;
        server_name a.fly.com;
        
        location / { 
                root /data/web-a/dist;
                index index.html;
        }
}
## /etc/nginx/sites-avaliable/b.conf
server {
        listen 80;
        server_name b.fly.com;
        
        location / { 
                root /data/web-b/dist;
                index index.html;
        }
}
```

## 基于端口配置

```nginx
# 域名 www.fly.com 的子域名
## /etc/nginx/sites-avaliable/a.conf
server {
        listen 8000;
        location / { 
                root /data/web-a/dist;
                index index.html;
        }
}
### nginx 80端口配置 （监听a二级域名）
server {
        listen  80;
        server_name a.fly.com;
        location / {
                proxy_pass http://localhost:8000; #转发
        }
}

## /etc/nginx/sites-avaliable/b.conf
server {
        listen 8001;
        location / { 
                root /data/web-b/dist;
                index index.html;
        }
}

# nginx 80端口配置 （监听b二级域名）
server {
        listen  80;
        server_name b.fly.com;
        location / {
                proxy_pass http://localhost:8001; #转发
        }
}

```

## 语法

```nginx
listen address[:port] [default_server] [setfib=number] [backlog=number] [rcvbuf=size] [sndbuf=size] [deferred]
    [accept_filter=filter] [bind] [ssl]; #配置监听的IP地址
listen port[default_server] [setfib=number] [backlog=number] [rcvbuf=size] [sndbuf=size] [accept_filter=filter] 
    [deferred] [bind] [ipv6only=on|off] [ssl]; #配置监听端口
listen unix:path [default_server]  [backlog=number] [rcvbuf=size] [sndbuf=size] [accept_filter=filter] 
    [deferred] [bind] [ssl]; #配置 UNIX Domain Socket
	# 1 listen *:80 | *:8080 #监听所有80端口和8080端口
    # 2 listen  IP_address:port   #监听指定的地址和端口号
    # 3 listen  IP_address     #监听指定ip地址所有端口
    # 4 listen port     #监听该端口的所有IP连接
```

```nginx
server_name   name ...;
server_name 123.com www.123.com
server_name *.123.com www.123.*
server_name ~^www\d+\.123\.com$;
```

```nginx
location [ = | ~ | ~* | ^~] uri {
 }
# = ：用于不含正则表达式的 uri 前，要求请求字符串与 uri 严格匹配，如果匹配成功，就停止继续向下搜索并立即处理该请求。
# ~：用于表示 uri 包含正则表达式，并且区分大小写。
# ~*：用于表示 uri 包含正则表达式，并且不区分大小写。
# ^~：用于不含正则表达式的 uri 前，要求 Nginx 服务器找到标识 uri 和请求字符串匹配度最高的 location 后，立即使用此 location 处理请求，而不再使用 location 块中的正则 uri 和请求字符串做匹配。
# 注意：如果 uri 包含正则表达式，则必须要有 ~ 或者 ~* 标识。
```

```nginx
proxy_pass URL; # 设置被代理服务器的地址。可以是主机名称、IP地址加端口号的形式
proxy_pass  http://www.123.com/uri;
```

```nginx
index  filename ...;  # 设置网站的默认首页。
# 用户在请求访问网站时，请求地址可以不写首页名称；第二个是可以对一个请求，根据请求内容而设置不同的首页
```





