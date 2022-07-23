---
abbrlink: 1473feae
title: Configure Base Software
categories:
  - Linux
---
# Soucre

## pip

国内镜像源列表

```
豆瓣(douban) http://pypi.douban.com/simple/ (推荐)
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
阿里云 http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```


永久置换pip镜像源

```bash
# 在.pip目录下创建pip.conf文件
mkdir ~/.pip
cd ~/.pip
touch pip.conf
# 编辑pip.conf文件
sudo vi ~/.pip/pip.conf
# 增加以下：
[global]
timeout = 6000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
```
临时使用
```bash 
pip3 install -i http://mirrors.aliyun.com/pypi/simple/ numpy scikit-image imutils --trusted-host mirrors.aliyun.com
```

windows

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```



# Conda

```bash
wget -c https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh 
chmod 777 Miniconda3-latest-Linux-x86_64.sh # 权限
sh Miniconda3-latest-Linux-x86_64.sh   ## 安装
vim ~/.bashrc 
### 文末插入/修改
	export  PATH="/home/dsj/miniconda3/bin:"$PATH
	export  PATH="/root/miniconda3/bin:"$PATH
###
source ~/.bashrc
conda #测试
# 换源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --set show_channel_urls yes 
conda config --get channels

创建虚拟环境，并安装python
	conda create -n py3 python=3.8
激活虚拟环境
	source activate py3
退出虚拟环境	
	source deactivate
	

```

```bash
conda info --envs # 查看环境
conda env list  # 查看环境
conda create -n env_name python=3.9 # 创建环境
source activate env_name # 激活环境

conda search package_name 
conda list # 

```

## apt

```python
## :set mouse -=a
## apt-get update && apt-get install gnupg  ## for docker
# /etc/apt/sources.list
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

##  apt-get update
### NO_PUBKEY 3B4FE6ACC0B21F32 NO_PUBKEY 871920D1991BC93C
##  apt-key adv --keyserver keyserver.ubuntu.com --recv-keys  871920D1991BC93C
```

## git 

```bash
git config --global user.name "DingSJ101"
git config --global user.email "1018966798@qq.com"
git init 
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin git@gitee.com:dsj_ws/course_select_system.git
git push -u origin "master"
```



# ssh

```bash
# 安装ssh服务端
sudo apt-get install openssh-server
# 确认sshserver是否启动了 sshd
ps -e | grep ssh
## 启动sshserver
sudo /etc/init.d/ssh start
## 为了方便可以设置SSH服务开机自动启动，打开/etc/rc.local文件，在语句exit 0之前加入：
#      /etc/init.d/ssh start
## 重启sshserver
sudo /etc/init.d/ssh restart
## 停止
sudo /etc/init.d/ssh stop

# SSH配置
/etc/ssh/sshd_config

# 安装客户端
sudo apt-get install ssh
# 或
sudo apt-get install openssh-client
```

```bash
# SSH登录（客户端）
ssh 175.24.167.6
ssh -l cookie 175.24.167.6
ssh cookie@175.24.167.6
```

　




