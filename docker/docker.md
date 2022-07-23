---
abbrlink: f255ffad
title: docker
categories:
  - docker
---
### 仓库、镜像和容器

- **镜像**

  Docker镜像里包含了已打包的应用程序及其所依赖的环境。它包含应用程序可用的文件系统和其他元数据，如镜像运行时的可执行文件路径。

- **镜像仓库**

  Docker镜像仓库用于存放Docker镜像，以及促进不同人和不同电脑之间共享这些镜像。当编译镜像时，要么可以在编译它的电脑上运行，要么可以先上传镜像到一个镜像仓库，然后下载到另外一台电脑上并运行它。某些仓库是公开的，允许所有人从中拉取镜像，同时也有一些是私有的，仅部分人和机器可接入。

- **容器**

  Docker容器通常是一个Linux容器，它基于Docker镜像被创建。一个运行中的容器是一个运行在Docker主机上的进程，但它和主机，以及所有运行在主机上的其他进程都是隔离的。这个进程也是资源受限的，意味着它只能访问和使用分配给它的资源（CPU、内存等）。

### 打包、分发和部署

**打包**：就是把你软件运行所需的依赖、第三方库、软件打包到一起，变成一个安装包
**分发**：你可以把你打包好的“安装包”上传到一个镜像仓库，其他人可以非常方便的获取和安装
**部署**：拿着“安装包”就可以一个命令运行起来你的应用，自动模拟出一摸一样的运行环境，不管是在 Windows/Mac/Linux。

### 镜像加速源

| 镜像加速器          | 镜像加速器地址                          |
| ------------------- | --------------------------------------- |
| Docker 中国官方镜像 | https://registry.docker-cn.com          |
| DaoCloud 镜像站     | http://f1361db2.m.daocloud.io           |
| Azure 中国镜像      | https://dockerhub.azk8s.cn              |
| 科大镜像站          | https://docker.mirrors.ustc.edu.cn      |
| 阿里云              | https://<your_code>.mirror.aliyuncs.com |
| 七牛云              | https://reg-mirror.qiniu.com            |
| 网易云              | https://hub-mirror.c.163.com            |
| 腾讯云              | https://mirror.ccs.tencentyun.com       |

```bash
## images
docker images	# 查看镜像
docker search image_name # 搜索镜像
docker pull 镜像名称:版本号  # 拉取镜像
docker rmi 镜像名称:版本号  # 删除镜像
docker image prune -a # 删除未绑定容器的镜像
docker rmi -f  `docker images | grep '<none>' | awk '{print $3}'`  # 删除tag为none的镜像

vim /var/lib/docker/image/overlay2/repositories.json  # 镜像信息
cd /var/lib/docker/containers # 容器位置

docker ps	# 查看正在运行的容器
docker ps -a	# 查看所有容器

## 使用
docker run -d -p 1337:1337    --network kong-net   --name konga  -e "NODE_ENV=production"  \
	-e "DB_ADAPTER=postgres"   -e "DB_URI=postgresql://kong:kong@172.0.0.1:5432/konga"         pantsel/konga # 创建容器
# -rm	在容器退出时自动清理容器内部的文件系统	在Docker容器退出时,默认容器内部的文件系统仍然被保留,以方便调试并保留用户数据.
# -i	保持容器运行
# -p	端口映射
# -e	传递环境变量
# -t	为容器重新分配一个伪输入终端
# -d	以守护进程模式运行容器，后台运行
# -it	创建一个交互式容器，退出后容器容器停止运行
# -id	创建一个守护容器；退出后容器不停止运行
docker exec -it 容器ID  /bin/bash	# 进入容器
docker start 容器ID  # 启动容器
docker stop 容器ID  # 停止容器
docker rm 容器ID
docker rm `docker ps -aq` # 删除所有容器
docker inspect 容器ID # 查看容器详细信息

## 网络
docker network ls  # 查看docker下的网络列表
docker network inspect network_name # 查看单个网络详细信息
docker network create networkname	# 创建网络,不指定网络驱动时,默认创建的是bridge网络.
docker network rm networkname

## 日志
docker logs [OPTIONS] CONTAINER_ID
    # Options参数
    --details: 显示更多的信息
    --follow(-f): 跟踪实时日志
    --since string: 显示自某个timestamp之后的日志，或相对时间，如40m（即40分钟）
    --tail string: 从日志末尾显示多少行日志， 默认是all
    --timestamps(-t): 显示时间戳
    --until string: 显示自某个timestamp之前的日志，或相对时间，如40m（即40分钟
## 仓库
docker login
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname


### 提取文件打包
tar  --exclude=/proc --exclude=/sys --exclude=/base_img.tgz  -cvpzf /base_img.tgz  /
docker cp jupyter:/base_img.tgz .
cat base_img.tgz | docker import - ubuntu
### 

docker commit container_id image:tagname
docker save -o filename.tar image_name
docker load --input filename.tar

## 配置
vim  /etc/docker/daemon.json
# 镜像信息
{
    "registry-mirrors": ["https://registry.docker-cn.com"],
    "live-restore": true
}


## docker build
## 分层构建
## Dockerfile
FROM image1 as pre_image
# ..
FROM image2
COPY --from=pre_image /image1_folder /image2_folder
```

## Docker Compose

https://www.cnblogs.com/minseo/p/11548177.html

```bash
Usage:
  docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help
  docker-compose -f a.yml -f b.yml up
  docker-compose stop [options][Service]
Options:
  -f, --file FILE             指定使用的 Compose 模板文件(default: docker-compose.yml)可以多次指定
  -p, --project-name NAME     指定项目名称，默认将使用所在目录名称作为项目名。
  -H, --host HOST             Daemon socket to connect to
Commands:
  config             验证 Compose 文件格式是否正确，若正确则显示配置，若格式错误显示错误原因。
  images             列出 Compose 文件中包含的镜像
  ps                 列出项目中目前的所有容器
  kill               Kill containers
  port               Print the public port for a port binding
  build              Build or rebuild services
  create             Create services
  pause              Pause services
  restart            Restart services
  start              Start services
  stop               Stop services
  up                 尝试自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一系列操作。
	-d				后台启动
	--no-color 		不使用颜色来区分不同的服务的控制台输出。
	--no-deps 		不启动服务所链接的容器。
	--force-recreate 强制重新创建容器，不能与 --no-recreate 同时使用。
	--no-recreate 	如果容器已经存在了，则不重新创建，不能与 --force-recreate 同时使用。
	--no-build 		不自动构建缺失的服务镜像。⑦：
	-t | --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）。
  down               停止用up命令所启动的容器并移除网络
```

### docker-compose.yml

```yml
version: '3'
services:
  webapp1:
    build: ../
    container_name: my-web-container
    ports:
     - "5000:5000"
    volumes:
     - ../src:/opt/src
     #挂载数据卷的默认权限是读写（rw），可以通过ro指定为只读。 
     - ../src:/opt/src:ro
  webapp2:
  	build:
      #包含Dockerfile文件的目录路径，或者是git仓库的URL。
      context: ./dir 
      dockerfile: Dockerfile-alternate
      #构建镜像的参数，环境变量只能在构建过程中访问，两种写法
      # YAML布尔值（true，false，yes，no，on，off）必须用引号括起来，以便解析器将它们解释为字符串。
      args:
        buildno: 1
        - passwd=2
  redis:
    image: "redis:3.0.7"
    # 覆盖容器启动后默认执行的命令
    command: bundle exec thin -p 3000
    # 指定容器退出后的重启策略为始终重启,always | unless-stopped | on-failure | "no"
    # 指定为always时，容器总是重新启动。 如果退出代码指示出现故障错误，则on-failure将重新启动容器。
    start: always
    environment:
      - RACK_ENV=development
      - SHOW=true
      - SESSION_SECRET
	# 将PID模式设置为主机PID模式。 打开了容器与主机操作系统之间的共享PID地址空间。 
	# 使用此标志启动的容器将能够访问和操作裸机的命名空间中的其他容器，反之亦然。
	# 即打开该选项的容器可以相互通过进程 ID 来访问和操作。
    pid: "host"
    # 配置 DNS 服务器。可以是一个值，也可以是一个列表。
    dns: 8.8.8.8
```



restart指定容器退出后的重启策略为始终重启。该命令对保持服务始终运行十分有效，在生产环境中推荐配置为 always 或者 unless-stopped。restart: always

```yaml
version: "3"
# 跨多个服务并重用挂载卷，在顶级volumes关键字中命名挂在卷
services:
  db:
    image: db
    volumes:
      - data-volume:/var/lib/db
  backup:
    image: backup-service
    volumes:
      - data-volume:/var/lib/backup/data
volumes:
  data-volume:
```





