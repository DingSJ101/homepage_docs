---
abbrlink: 809983c5
title: Readme
categories:
  - Hexo-Blog
---
# quick start 

```bash
##########33 docker-compose.yml
version: '3'
services:
  # nginx:
  #   restart: always
  #   build: ./dockerfiles/nginx
  #   ports:
  #     - "80:80"
  #     - "443:443"
  #   volumes:
  #     - "./blog:/var/www/blog"
  #     - "./ssl/certs:/var/www/ssl/certs"
  #     - "./nginx/conf.d:/etc/nginx/conf.d"
  #   # command: /bin/bash /start.sh
  #   env_file:
  #     - docker.env
  #   extra_hosts:
  #     - "raw.githubusercontent.com:199.232.96.133"
  #   container_name: "nginx"
  nodejs:
    restart: always
    build: ./dockerfiles/nodejs
    ports:
      - "4000:4000"
    volumes:
      - "~/workspace/hexo_blog:/var/www/blog"
      - "/etc/localtime:/etc/localtime"
      - "~/.ssh/id_rsa:/root/.ssh/id_rsa"

########### init.sh
#!/bin/bash

# start project with one file
cd ~/workspace
sudo rm -r hexo_blog
# mkdir hexo_blog
# cd hexo_blog
# git init
# # git config --replace-all --local user.name "DingSJ101"
# # git config --replace-all  --local user.email "1018966798@qq.com"
# git remote add origin https://gitee.com/dsj_ws/hexo_blog.git
# git pull origin master
# git submodule update --init --recursive
# git clone https://gitee.com/dsj_ws/hexo_blog.git hexo_blog  #部署使用
# git clone git@gitee.com:starry101/hexo_blog.git
git clone git@github.com:DingSJ101/hexo_blog.git  # 容器开发使用
cd hexo_blog


echo "### Stoping containers ..."
docker-compose down

echo "### Starting v ..."
docker-compose -f docker-compose.yml  up --force-recreate --build -d
```

