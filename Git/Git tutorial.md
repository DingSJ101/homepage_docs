---
date: 2022-07-31 16:56:45.149409
lastmod: 2023-04-20 17:50:32.029546
---
# 初始化仓库

## 创建远程仓库

在Github或Gitee上创建一个仓库，略。

## 创建本地仓库

### 创建新的本地仓库

```bash
mkdir git_demo
cd git_demo
git init # 初始化
```

执行`git init`后，会在当前文件夹下创建`.git`文件夹，存储`git`相关数据。

#### 指定远程仓库

本地仓库需要指定远程仓库信息，才能进行上传。

```bash
git remote add origin git@github.com:DingSJ101/git_tutorial.git
```

`git remote add <name> <url>`在本地配置中添加远程仓库的`url`链接，并命名为`name`，可以为一个本地仓库指定多个远程仓库地址，在进行操作时指定对应的`name`即可。该操作会在config中加入相关信息。

![](https://s2.loli.net/2022/07/31/Zwx6EgTuLh8jnC2.png)

> 注：如果本地仓库已有文件，在使用之前需要执行`git pull`拉取远程仓库，保持HEAD与远程同步

### 克隆远程仓库

通过克隆仓库方式可以直接完成以上操作，并下载远程仓库内文件。

```
git clone https://github.com/DingSJ101/git_tutorial.git git_demo
git clone git@github.com:DingSJ101/git_tutorial.git git_demo
```

使用`git clone <url> [new_repository_name]`在当前目录下新建文件夹`new_repository_name`，并克隆远程仓库文件到该文件夹

- `<url>`为仓库的连接，分为`http`和`ssh`两种格式
  - http链接在每次`git push`时需要输入账号密码
  - ssh链接需要安装ssh，在配置ssh免密后，`git push`不再需要输入密码 

- `new_repository_name`为本地仓库文件夹名，缺省时使用远程仓库的仓库名



## 配置身份信息

到这一步，仓库只能执行`git add`，在执行`git config`设置githut/gitee用户名和邮箱之后才能使用`git commit`和`git push`

```bash
# 配置账号信息
git config user.name demo_name # 作者为 demo_name
git config user.email demo@email.com # 邮箱为 demo@email.com  # 可以不设置

# 查看、修改配置
git config --local --list # 查看当前仓库所有配置信息
git config user.name # 查看user.name
git config --replace-all user.name "new_name" # 修改配置
git config --unset user.name # 删除配置

## 其他配置
git config --global core.editor vim # 修改默认编辑器为vim
git config --global https.proxy http://10.224.10.252:808 # 添加GIT全局配置(HTTPS代理)
git config --global merge.tool vimdiff # 代码比较工具

## 其他
git config --show-origin user
```

执行`git config <name> [options] [value]`默认将name字段的值设置为value，并保存在本仓库。`options`参数`--local`和`--global`表示`git config`中的配置的使用范围，默认缺省`--local`，其中 `--local`仅本仓库有效，配置记录在`.git/config`文件中；`--global`对于系统当前用户有效，配置记录在`~/.gitconfig`文件中。



# 拉取/提交代码

```bash
# git pull <远程主机名> <远程分支名>:<本地分支名> # 将远程分支合并到本地分支
git pull origin master
git push origin master
```

注意：

1. 每次进行`git pull`和`git push`时都需要指定上传到哪个分支，可以使用`git pull -u origin master`配置默认的分支，之后使用`git pull`即可。
2. github创建仓库后的默认分支为main，本地git初始化仓库默认分支为main
3. 如果想把本地已有分支提交到远程的新分支new_branch（远程和本地都没有该分支），则必须先在本地创建名为new_branch的新分支(`git checkout -b new_branch`)`，再`git push origin new_branch`

 当前所在分支为master:

![](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20220801151232.png)

执行`git pull origin main`拉取远程仓库的main分支，此时仓库状态和origin/main相同

![](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20220801151332.png)

回退到`git pull`之前，重新拉取远程仓库的master分支，此时仓库

![](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20220801151539.png)

## git commit

````bash
git add -A # 添加所有修改
git commit -m "commit info" #
````





# 切换分支

```bash
git branch # 显示所有分支
git branch -d branch_name # 删除分支
git checkout branch_name # 切换分支
git checkout -b branch_name # 创建分支
```

# 合并分支

## fast forward merge

如果需要合并的两个分支没有分叉，那么可以直接进行合并。

```bash
git merge another_branch_name # 合并另一分支到本分支
```

现在我有一个分支main，从该分支创建一个新分支newbranch，并提交一个新的commit。再main分支上也提交一个新的commit。

![](https://cdn.jsdelivr.net/gh/DingSJ101/picgo_hub@master/img/20220801195712.png)

## 3-way merge

当需要合并的两个分支存在分叉，则必须人工选择要保留那些提交。



```bash
git merge another_branch_name # 合并分支，提示需要手动处理
# 修改冲突文件
git add . 
git commit -m "info"
git push origin master 
```





# 版本控制

```bash
//执行 --soft 命令，只是把HEAD指向了上一个版本，已经add的文件不受影响。
git reset --soft HEAD^
//撤销commit 并且也撤销add。
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
        
        
git clone --recurse-submodules https://gitee.com/dsj_ws/hexo_blog.git test # clone主仓库的同时clone关联的submodule仓库
```

![](https://s2.loli.net/2022/07/23/EzkrFocDXSOg1ML.png)

## .gitignore

```bash
# 文件内容为不需要跟踪的文件名，每行一个，可以正则
# 已跟踪的文件，新配置的gitignore无法生效
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```



# 撤销

## git add 前

`git checkout <filename>`或`git restore <filename>` 撤销硬盘上的修改

## git add 后,git commit 前

`git add `之后，文件将拷贝至暂存区。

`git reset <filename>`或`git restore --staged <filename>` 撤销暂存区修改

`git checkout HEAD <filename>`同时撤销暂存区和硬盘上的修改

## git commit 后

`git commit `后，修改将同步到`local git`中

`git reset --soft HEAD~1`撤销commit 

`git reset HEAD~1`或`git reset --mixed HEAD~1`同时撤销`git commit `和`git add`

`git reset --hard HEAD~1`同时撤销`git commit `和`git add`以及硬盘上的所有修改，恢复到上一个commit状态

`git revert HEAD`增加一个commit ，效果等价于撤销某个commit

> `git revert <hash> ` 撤销hash对应的commit内容，可以单独撤销之前的任一个commit
>
> e.g. `git revert HEAD~n`
>
> 优点：撤销分支是通过增加commit链，在公共分支上便于git pull ; 其他方法是通过裁断commit链，提交时需要参数`-f`

