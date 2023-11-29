---
date: 2023-07-10 14:42:14.065167
---


## 使用NVM 管理node环境

### install

安装目录 `~/.nvm`

```bash
# 下载文件
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
# 配置环境
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")" [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

### usage

```

nvm install <version>       // e.g. nvm install 12
nvm uninstall <version>     
nvm list                      // 显示所有安装的node.js版本
nvm use <version>           // 切换到使用指定的nodejs版本
nvm alias default v14 	//设置默认 node 版本
nvm deactivate 		//解除当前版本绑定
```

