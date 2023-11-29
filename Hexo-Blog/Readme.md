---
date: 2022-07-23 13:52:53.913384
lastmod: 2023-02-18 18:00:40.280960
title: Readme
---
## quick start

### download the reposity

```bash
git clone git@github.com:DingSJ101/hexo_blog.git
```

### start in one command

```bash
cd hexo_blog && docker-compose -f docker-compose.yml  up --force-recreate --build -d
```

### change the configuration

1. `dockerfiles/nodejs/start.sh`

   修改`https://gitee.com/starry101/docs.git` 为自己的markdown文件仓库

2. `nginx/hexo.conf`

   2.1 修改http配置，取消nginx测试部分的注释

   2.2 修改ssl证书配置为自己的域名相关文件，如果没有证书就需要注释443端口部分的整个server

   ```bash
   #     server_name starry101.top www.starry101.top;	
   #     ssl_certificate starry101.top_bundle.crt;
   #     ssl_certificate_key starry101.top.key;
   ```

3. `blog/_config.yml` 和 `blog/_config.yml` 主题的相关配置。

4. 放开80，443和4000端口

## 启动流程

1. 通过`docker-compose`启动`docker-compose.yml`，拉起`hexo_nginx`和`hexo_nodejs两个容器

   1.1 `hexo_nginx`由`dockerfiles/nginx/Dockerfile`配置，负责监听宿主机端口，并配置博客的路由

   1.2 `hexo_nodejs`由`dockerfiles/nodejs/Dockerfile`配置，运行hexo环境

2. `hexo_nodejs`容器入口为`dockerfiles/nodejs/start.sh`，自动下载文件仓库、安装插件，并使用hexo进行生成

   > 注：
   >
   > `start.sh`文件中使用`webhook-cli --port 4000 --hooks hooks.json --verbose`监听4000端口，当文档仓库进行`git pull`后，会重启容器并重新完成第2步中的内容，具体配置见后文
   >
   > 可以使用`hexo server -p 4000`命令替换，此时访问4000端口即可进入博客主页



## 配置webhook

### 工作流程

当我们`git pull`到文件仓库后，会触发仓库的webhook并发送一个post请求给远程HTTP URL ，我们通过在服务器上4000端口监听该请求。

通过使用`webhook-cli`，配置`hooks.json`文件，当接收到特定请求后，触发`hooks.json`中绑定的脚本，强制杀死容器入口重新，使得容器重新启动。

### 配置

以Gitee为例：

![image-20230218174437255](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230218174438.png)

URL中可以根据自己的需求修改鉴权方式，方便起见，这里没有配置信息验证，所有的请求都会触发。



## 配置gittalk评论区

