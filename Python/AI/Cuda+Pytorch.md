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

## Docker 无法使用GPU解决办法

安装`nvidia-container-toolkit`将宿主机的GPU运行时映射到容器。[参考](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#installing-on-ubuntu-and-debian)

> 注：如果使用Kubernetes，还需要安装nvidia-docker2

```bash
# https://nvidia.github.io/libnvidia-container/
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list |  sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' |  sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

更新apt

```bash
sudo apt-get update
# sudo apt-get install -y nvidia-docker2
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

测试，docker运行容器时附上参数：--gpus all 

```bash
docker run --rm --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
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

