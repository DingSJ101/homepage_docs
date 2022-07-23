---
abbrlink: 69c3279c
title: Git
categories: uncategorized
---
```bash
## 创建仓库
git init
git remote add origin git@github.com:DingSJ101/hexo_blog.git
git pull origin main
## 上传
git add .
git commit -m "note"
git pull origin master
git push -u origin master

## 分支
git branch # 展示分支
git checkout branch_name # 切换分支
git checkout -b branch_name # 创建分支
git branch -d branch_name # 删除分支
git merge another_branch_name # merge branch into current branch
### 合并冲突
git merge another_branch_name
git add .
git commit -m "info"
```

```bash
# 配置
git config --global user.name "Name"
git config --global user.email "Email"
git config --local user.name "DingSJ101"
git config --local user.email "1018966798@qq.com"
ssh-keygen -t rsa -C "Email" # 生成秘钥并上传至github
ssh git@github.com #验证秘钥
#检查信息是否写入成功
git config --list 
```


```powershell
# 用户级别配置 --global
# ~/.gitconfig
# 仓库级别配置  --local
# .git/config
[core]
        repositoryformatversion = 0
        filemode = false
        bare = false
        logallrefupdates = true
        symlinks = false
        ignorecase = true
[remote "origin"]
        url = git@gitee.com:dsj_ws/study_project.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
[http]
        sslVerify = false
        postBuffer = 1048576000
```



```bash
//当我们执行 --soft 命令后，可以看到控制台无任何输出。它只是把HEAD指向了上一个版本。
git reset --soft HEAD^
//撤销commit 并且也撤销add。(也就是删除工作空间的改动代码)
git reset --hard HEAD^

```

## 仓库嵌套

```bash
# folder文件夹下包含.git 导致父目录无法被add
git rm --cached folder
git add folder

## submodule 
# git clone https://gitee.com/dsj_ws/hexo_blog.git test
git submodule update --init --recursive
git submodule foreach git pull origin master
git submodule add https://gitee.com/dsj_ws/yilia-plus.git ./blog/theme/yilia-plus
git submodule update https://gitee.com/dsj_ws/yilia-plus.git ./blog/theme/yilia-plus
[submodule "blog/themes/yilia-plus"]
        path = blog/themes/yilia-plus
        url = https://gitee.com/dsj_ws/yilia-plus.git
        
        
        git clone --recursive https://gitee.com/dsj_ws/hexo_blog.git test
```

## .gitignore

```bash
# 文件内容为不需要跟踪的文件名，每行一个，可以正则
# 已跟踪的文件，新配置的gitignore无法生效
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```

