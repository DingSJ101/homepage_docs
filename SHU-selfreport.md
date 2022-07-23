---
abbrlink: 95cadaf7
title: SHU-selfreport
categories: uncategorized
---
## SHU-selfreport

```bash 
ssh-keygen -t ed25519 -f ~/.ssh/id_rsa_gitee -C “1018966798@qq.com”
ssh-keygen -t ed25519 -f ~/.ssh/id_rsa_github -C “1018966798@qq.com”

vim ~/.ssh/config
## 该文件用于配置私钥对应的服务器
  # first user
  Host gitee.com
  HostName gitee.com
  User 1018966798@qq.com
  IdentityFile ~/.ssh/id_rsa_gitee
 
  # second user
  Host github.com
  HostName github.com
  User 1018966798@qq.com
  IdentityFile ~/.ssh/id_rsa_github
##
# add id_rsa_github.pub to github
# 取消git的全局配置（如果之前就有配置的情况下）
git config --global --unset user.name “XXX”
git config --global --unset user.email “xxx@xx.com”
git config --global -l  #查看是否取消成功：
# 执行ssh识别
eval $(ssh-agent -s) #Start the ‘ssh-agent.exe’ process
ssh-add ~/.ssh/id_rsa_gitee # install the SSH keys
ssh-add ~/.ssh/id_rsa_github # install the SSH keys
ssh-add -l # show all id_rsa
git config user.email '1018966798@github.com'
git config user.name 'DingSJ101'
# git config --global http.sslVerify false
# git config --global http.postBuffer 1048576000
git clone https://github.com/DingSJ101/SHU-selfreport.git --recursive


## git 调试模式  vi ~/.bashrc
export GIT_TRACE_PACKET=1
export GIT_TRACE=1
export GIT_CURL_VERBOSE=1
##
```

