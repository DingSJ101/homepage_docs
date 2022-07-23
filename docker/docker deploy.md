---
abbrlink: 6f0cf84e
title: docker deploy
categories:
  - docker
---
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

## Course_select_system

```bash
docker run -idt  --name course_select_system -v ~/workspace/course_select_system:/course_select_system -v /etc/localtime:/etc/localtime -p 8003:8000 dingsj101/course_select_system:1.2 sh /course_select_system/run.sh


-i http://mirrors.aliyun.com/pypi/simple/ numpy scikit-image imutils --trusted-host mirrors.aliyun.com
```

