---
abbrlink: a87bd88f
title: Win+Anaconda+Jupyter
categories: uncategorized
---
## 卸载



## 安装

[安装包](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)

### 镜像源

```bash
# conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
## ~/.condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  ##
  conda clean -i  # 清除索引缓存，保证用的是镜像站提供的索引
  conda update --strict-channel-priority --all  # 重置优先级
```

###  环境变量

```
D:\ENV\anaconda3\Library
D:\ENV\anaconda3\Library\mingw-w64
D:\ENV\anaconda3
D:\ENV\anaconda3\Scripts
```



# jupyter

```bash
(scientificCalc) C:\Users\10189>jupyter --paths
        config:
            C:\Users\10189\.jupyter
            C:\Users\10189\AppData\Roaming\Python\etc\jupyter
            D:\ENV\anaconda3\envs\scientificCalc\etc\jupyter
            C:\ProgramData\jupyter
        data:
            C:\Users\10189\AppData\Roaming\jupyter
            C:\Users\10189\AppData\Roaming\Python\share\jupyter
            D:\ENV\anaconda3\envs\scientificCalc\share\jupyter
            C:\ProgramData\jupyter
        runtime:
            C:\Users\10189\AppData\Roaming\jupyter\runtime
jupyter kernelspec list # 显示内核文件
# Available kernels:
#   python3      D:\ENV\anaconda3\share\jupyter\kernels\python3
#   py3jisuan    C:\ProgramData\jupyter\kernels\py3jisuan
pip install ipykernel
conda activate py3
python -m ipykernel install --user --name scientificCalc --display-name "jisuan"
# Installed kernelspec scientificCalc in C:\Users\10189\AppData\Roaming\jupyter\kernels\scientificcalc

pip install jupyter_contrib_nbextensions
conda install -c conda-forge jupyter_nbextensions_configurator
jupyter contrib nbextension install --user

```

