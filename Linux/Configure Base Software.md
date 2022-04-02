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

　　利用 PuTTy 通过证书认证登录服务器
　　SSH 服务中，所有的内容都是加密传输的，安全性基本有保证。但是如果能使用证书认证的话，安全性将会更上一层楼，而且经过一定的设置，还能实现证书认证自动登录的效果。
　　首先修改 sshd_config 文件，开启证书认证选项：
　　RSAAuthentication yes PubkeyAuthentication yes AuthorizedKeysFile %h/.ssh/authorized_keys修改完成后重新启动 ssh 服务。
　　下一步我们需要为 SSH 用户建立私钥和公钥。首先要登录到需要建立密钥的账户下，这里注意退出 root 用户，需要的话用 su 命令切换到其它用户下。然后运行：
　　ssh-keygen
　　这里，我们将生成的 key 存放在默认目录下即可。建立的过程中会提示输入 passphrase，这相当于给证书加个密码，也是提高安全性的措施，这样即使证书不小心被人拷走也不怕了。当然如果这个留空的话，后面即可实现 PuTTy 通过证书认证的自动登录。
　　ssh-keygen 命令会生成两个密钥，首先我们需要将公钥改名留在服务器上：
　　cd ~/.ssh mv id_rsa.pub authorized_keys然后将私钥 id_rsa 从服务器上复制出来，并删除掉服务器上的 id_rsa 文件。
　　服务器上的设置就做完了，下面的步骤需要在客户端电脑上来做。首先，我们需要将 id_rsa 文件转化为 PuTTy 支持的格式。这里我们需要利用 PuTTyGEN 这个工具：
　　点击 PuTTyGen 界面中的 Load 按钮，选择 id_rsa 文件，输入 passphrase（如果有的话），然后再点击 Save PrivateKey 按钮，这样 PuTTy 接受的私钥就做好了。
　　打开 PuTTy，在 Session 中输入服务器的 IP 地址，在 Connection->SSH->Auth 下点击 Browse 按钮，选择刚才生成好的私钥。然后回到 Connection 选项，在 Auto-login username 中输入证书所属的用户名。回到 Session 选项卡，输入个名字点 Save 保存下这个 Session。点击底部的 Open 应该就可以通过证书认证登录到服务器了。如果有 passphrase 的话，登录过程中会要求输入 passphrase，否则将会直接登录到服务器上，非常的方便。




