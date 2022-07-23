---
abbrlink: c3592b96
title: Conda
categories:
  - Linux
---
```bash
## 查看
conda --version #查看conda版本，验证是否安装
conda info -e #显示所有已经创建的环境
conda list [ -n env_name] #查看所有已经安装的包
conda env list #显示所有的虚拟环境
conda config --show-sources # 查看源
conda search package_name #查找包

## 更新
conda update conda #更新至最新版本，也会更新其它相关包
conda update --all #更新所有包
conda update package_name #更新指定的包

## 包
conda install/remove [ --name env_name ] package_name
conda install package_name #在当前环境中安装包
conda install --name env_name  package_name #在指定环境中安装包
conda remove --name env_name package_name #删除指定环境中的包
conda remove package_name #删除当前环境中的包
conda install --use-local  ~/Downloads/a.tar.bz2 #安装本地包

## 环境
conda create -n env_name python=x.x [ package_name ] #创建名为env_name的新环境，并在该环境下安装名为package_name 的包
source activate env_name #切换至env_name环境
source deactivate #退出环境
conda create --name new_env_name [ --clone old_env_name ] #复制环境
conda remove --name env_name –all #删除环境

## 清理
conda clean -p      #删除没有用的包
conda clean -t      #tar打包
conda clean -y -all #删除所有的安装包及cache

## 配置
conda config --set remote_read_timeout_secs 600.0 #设置超时时限


```