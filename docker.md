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
docker images	# 查看镜像
docker search image_name # 搜索镜像
docker pull 镜像名称:版本号  # 拉取镜像
docker rmi 镜像名称:版本号  # 删除镜像

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

## sudo iptables -I INPUT -p tcp --dport 233 -j ACCEPT #开放防火墙端口

```

## opengauss

```bash
docker search opengauss
docker pull enmotech/opengauss


docker run --name opengauss --privileged=true -d -e GS_PASSWORD=123@QWEasd  -p 15432:5432 -u root -v ~/workspace/opengauss:/var/lib/opengauss enmotech/opengauss:latest
docker run --name opengauss --privileged=true -id -e GS_PASSWORD=123@QWEasd  -p 15432:5432 -u root -v ~/workspace/opengauss1:/var/lib/opengauss enmotech/opengauss:2.0.1 

# GS_PASSWORD
    ## 在你使用openGauss镜像的时候，必须设置该参数。该参数值不能为空或者不定义。该参数设置了openGauss数据库的超级用户omm以及测试用户gaussdb的密码。openGauss安装时默认会创建omm超级用户，该用户名暂时无法修改。测试用户gaussdb是在entrypoint.sh中自定义创建的用户。
    ## openGauss镜像配置了本地信任机制，因此在容器内连接数据库无需密码，但是如果要从容器外部（其它主机或者其它容器）连接则必须要输入密码。
    ##openGauss的密码有复杂度要求，需要：密码长度8个字符及以上，必须同时包含英文字母大小写，数字，以及特殊符号
# GS_NODENAME
	## 指定据库节点名称 默认为gaussdb
# GS_USERNAME
	## 指定数据库连接用户名 默认为gaussdb
# GS_PORT
	## 指定数据库端口，默认为5432。
```

## jupyter notebook

```bash
##容器构建
docker pull dingsj101/jupyter:base
pip install jupyter_contrib_nbextensions
conda install -c conda-forge jupyter_nbextensions_configurator
jupyter contrib nbextension install --user

docker run --name jupyter --privileged=true -d -it  -p 8888:8888 -p 233:22 -v ~/workspace/jupyter:/root/workspace dingsj101/base_jupyter:1.1
docker exec -it jupyter /bin/bash  
	/etc/init.d/ssh start
    ## python
        In [1]: from notebook.auth import passwd
        In [2]: passwd()	
            'argon2:$argon2id$v=19$m=10240,t=10,p=8$71pQ6/9WSu0X6+2fHVuGwQ$k0Rwe5xvYbqDevrXIejIS0TTh+wotpZIR8GqhKkoZ50'
    root@4662ea008cc2:~/.pip# jupyter notebook --generate-config
            Writing default config to: /root/.jupyter/jupyter_notebook_config.py
    ## 到配置文件/root/.jupyter/jupyter_notebook_config.py中修改以下参数

    c.NotebookApp.allow_remote_access = True #将远程访问设置成True
    c.NotebookApp.ip='*' #绑定所有地址，即所有IP 地址都可以访问
    c.NotebookApp.open_browser = False #启动后是否在浏览器中自动打开
    c.NotebookApp.notebook_dir = "/root/workspace"   # 设置默认文件保存目录
    c.NotebookApp.password = u'argon2:$argon2id$v=19$m=10240,t=10,p=8$NAWEQ9+WGx2QLr4iTAHgOw$ObWrgFavljTc6+wip+ONRi7qS4/O1WeH3PQU8e7sep4'

    c.NotebookApp.port =8888 #指定一个访问端口8100，默认8888
    ## 
    ## ssh
    passwd  # 修改root密码
    apt-get install openssh-server
    /etc/init.d/ssh start
    ## 编辑它的配置文件 /etc/ssh/sshd_config，注释掉配置文件中的"PermitRootLogin without-password"，再增加一句"PermitRootLogin yes"使得root用户可以远程登录
    nohup jupyter notebook --allow-root > /root/workspace/run.log 2>&1 &   # 后台启动

## root qwe123
## ssh -v -p 233 root@175.24.167.6
## passwd of jupyter : qwe123



## 配置
sudo docker pull dingsj101/jupyter:base：1.0
sudo iptables -I INPUT -p tcp --dport 233 -j ACCEPT #开放防火墙端口
sudo iptables -I INPUT -p tcp --dport 8888 -j ACCEPT #开放防火墙端口
mkdir ~/workdspace\jupyter
docker run --name jupyter --privileged=true -d -it  -p 8888:8888 -p 233:22 -v ~/workspace/jupyter:/root/workspace dingsj101/base_jupyter:1.2
docker exec -it jupyter /bin/bash  
nohup jupyter notebook --allow-root > /root/workspace/run.log 2>&1 &   # 后台启动

## 使用
http://175.24.167.6:8888
### 密码：qwe123

```



