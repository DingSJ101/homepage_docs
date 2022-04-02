## Docker

[文档](https://docker.easydoc.net/doc/81170005/cCewZWoN/lTKfePfP)



## Windows

[安装包](https://www.docker.com/products/docker-desktop)

```powershell
bcdedit /set hypervisorlaunchtype auto # in powershell by su
```

```powershell
# 启用 VirtualMachinePlatform 组件
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux  # 开启wsl功能

```



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

