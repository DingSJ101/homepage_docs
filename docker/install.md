---
abbrlink: 2fcabd79
title: install
categories:
  - docker
---
## Docker

[文档](https://docker.easydoc.net/doc/81170005/cCewZWoN/lTKfePfP)



## Windows

[安装包](https://www.docker.com/products/docker-desktop)

### win10/11安装方法

```bash
wsl --install # 在从未安装过WSL的机器上
```

### 旧版安装方法

####  启动子系统

程序和功能界面，勾选

![](https://s2.loli.net/2022/06/29/LByQMD72WdG5gCb.png)

#### 安装Linux

```bash
# 更新 wsl
wsl --update  
## 将 wsl 版本设置为 wsl2
wsl --set-default-version 2

# 列出可安装的 Linux 版本
wsl --list --online
wsl --install -d Ubuntu

```

##### 离线包安装

[下载](https://docs.microsoft.com/zh-cn/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)

安装好后是一个 `.appx` 后缀的文件，直接**重命名**修改后缀为 `.zip`，然后解压，双击里面的 `.exe` 文件安装即可。

子系统的数据将存储在此文件夹中。![](https://s2.loli.net/2022/06/29/S28btrX5MfymGBn.png)



## CentOS

```bash
yum install -y yum-utils    #为了使用yum-config-manager命令, 执行--add-repo操作.
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# docker的三个组件: docker-ce、docker-ce-cli、containerd.io
# --nobest不只使用最佳选择的软件包  --skip-broken跳过无法安装的软件包
yum install -y docker-ce docker-ce-cli  containerd.io --nobest

docker -v # 简单信息
docker version  # 查看docker的版本号，包括客户端、服务端、依赖的Go等
docker info  # 查看系统(docker)层面信息，包括管理的images, containers数等
```

```bash
# 使用
systemctl start docker  # 启动
systemctl enable docker	# 开机自启
systemctl stop docker
systemctl restart docker
systemctl status docker


```



## Linux

```bash
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common  # 依赖
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  # 添加官方密钥
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" #设置稳定版仓库
sudo apt-get update
apt-cache madison docker-ce # 查看适合的包
apt-cache madison docker-ce-cli
sudo apt-get install docker-ce=5:20.10.9~3-0~ubuntu-focal docker-ce-cli=5:20.10.9~3-0~ubuntu-focal containerd.io  #选一个较低的版本安装
sudo service docker start
sudo service docker status
sudo docker run hello-world

sudo groupadd docker
sudo gpasswd -a ${USER} docker
su dsj
```

## 提权

```bash
sudo groupadd docker
sudo gpasswd -a ${USER} docker
sudo service docker restart
newgrp - docker
```

## Docker  Compose

```bash
# way one
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose# 2.增加可执行权限
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
# way two
```

