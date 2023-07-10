# 安装驱动

```bash
lspci | grep NVIDIA # 查看型号
```

[下载驱动](https://www.nvidia.com/Download/index.aspx)

## 禁用 nouveau 

```bash
lsmod | grep nouveau 
sudo vim /etc/modprobe.d/blacklist.conf
# 插入
blacklist nouveau
# 重启，进入BIOS，关闭Secure boot 
```



# Cuda

**## 确定版本**

打开电脑Nvidia控制面板

![](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20221104212056.png)

**## 下载**

[Cuda 官网](https://developer.nvidia.com/cuda-toolkit-archive)  

选择版本  ,runfile会容易安装一点

![](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20221104213438.png)

## Install

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.6.2/local_installers/cuda-repo-wsl-ubuntu-11-6-local_11.6.2-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-11-6-local_11.6.2-1_amd64.deb
sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-6-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda
```

## 修改环境变量

```bash
export PATH=$PATH:/usr/local/cuda-11.6/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.6/lib64
export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/cuda-11.6/lib64
```

## 检查安装

```bash
nvcc -V
```


## ubuntu+cuda+driver

禁用nouveau

`sudo vim /etc/modprobe.d/blacklist.conf`

添加

```bash
blacklist nouveau
options nouveau modeset=0
```

`sudo update-initramfs -u #刷新内核`

重启。

安装cuda，选择附带安装driver

需要安装gcc,make等等一系列东西

如果报错 ：You do not appear to have libc header files installed on your system.
Please install your distribution’s libc development [package](https://so.csdn.net/so/search?q=package&spm=1001.2101.3001.7020).

执行`sudo apt install `

出现依赖问题，直接恢复默认源

ubuntu2204 source.list

```
#deb cdrom:[Ubuntu 22.04 LTS _Jammy Jellyfish_ - Release amd64 (20220419)]/ jammy main restricted # See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to # newer versions of the distribution. deb http://cn.archive.ubuntu.com/ubuntu/ jammy main restricted # deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy main restricted ## Major bug fix updates produced after the final release of the ## distribution. deb http://cn.archive.ubuntu.com/ubuntu/ jammy-updates main restricted # deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy-updates main restricted ## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu ## team. Also, please note that software in universe WILL NOT receive any ## review or updates from the Ubuntu security team. deb http://cn.archive.ubuntu.com/ubuntu/ jammy universe # deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy universe deb http://cn.archive.ubuntu.com/ubuntu/ jammy-updates universe # deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy-updates universe ## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu ## team, and may not be under a free licence. Please satisfy yourself as to ## your rights to use the software. Also, please note that software in ## multiverse WILL NOT receive any review or updates from the Ubuntu ## security team. deb http://cn.archive.ubuntu.com/ubuntu/ jammy multiverse # deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy multiverse deb http://cn.archive.ubuntu.com/ubuntu/ jammy-updates multiverse # deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy-updates multiverse ## N.B. software from this repository may not have been tested as ## extensively as that contained in the main release, although it includes ## newer versions of some applications which may provide useful features. ## Also, please note that software in backports WILL NOT receive any review ## or updates from the Ubuntu security team. deb http://cn.archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse # deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse deb http://security.ubuntu.com/ubuntu jammy-security main restricted # deb-src http://security.ubuntu.com/ubuntu jammy-security main restricted deb http://security.ubuntu.com/ubuntu jammy-security universe # deb-src http://security.ubuntu.com/ubuntu jammy-security universe deb http://security.ubuntu.com/ubuntu jammy-security multiverse # deb-src http://security.ubuntu.com/ubuntu jammy-security multiverse # This system was installed using small removable media # (e.g. netinst, live or single CD). The matching "deb cdrom" # entries were disabled at the end of the installation process. # For information about how to configure apt package sources, # see the sources.list(5) manual.

# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to 
# newer versions of the distribution. 
deb http://cn.archive.ubuntu.com/ubuntu/ jammy main restricted 
# deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy main restricted

## Major bug fix updates produced after the final release of the 
## distribution. 
deb http://cn.archive.ubuntu.com/ubuntu/ jammy-updates main restricted 
# deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu 
## team. Also, please note that software in universe WILL NOT receive any 
## review or updates from the Ubuntu security team. 
deb http://cn.archive.ubuntu.com/ubuntu/ jammy universe 
# deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy universe 
deb http://cn.archive.ubuntu.com/ubuntu/ jammy-updates universe 
# deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy-updates universe

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu 
## team, and may not be under a free licence. Please satisfy yourself as to 
## your rights to use the software. Also, please note that software in 
## multiverse WILL NOT receive any review or updates from the Ubuntu 
## security team. 
deb http://cn.archive.ubuntu.com/ubuntu/ jammy multiverse 
# deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy multiverse 
deb http://cn.archive.ubuntu.com/ubuntu/ jammy-updates multiverse 
# deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy-updates multiverse

## N.B. software from this repository may not have been tested as 
## extensively as that contained in the main release, although it includes 
## newer versions of some applications which may provide useful features. 
## Also, please note that software in backports WILL NOT receive any review 
## or updates from the Ubuntu security team. 
deb http://cn.archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse 
# deb-src http://cn.archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu jammy-security main restricted 
# deb-src http://security.ubuntu.com/ubuntu jammy-security main restricted 
deb http://security.ubuntu.com/ubuntu jammy-security universe 
# deb-src http://security.ubuntu.com/ubuntu jammy-security universe 
deb http://security.ubuntu.com/ubuntu jammy-security multiverse 
# deb-src http://security.ubuntu.com/ubuntu jammy-security multiverse

# This system was installed using small removable media 
# (e.g. netinst, live or single CD). The matching "deb cdrom" 
# entries were disabled at the end of the installation process. 
# For information about how to configure apt package sources, 
# see the sources.list(5) manual.

```







# Pytorch

[Pytorch 官网](https://pytorch.org/get-started/locally/)

```bash
pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu116
```

## 测试

```python 
import torch
x = torch.rand(5, 3)
print(x)
torch.cuda.is_available()
```

