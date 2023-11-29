---
date: 2022-07-28 20:33:06.320550
---
## Please commit your changes or stash them before you merge.

git pull 之前，本地和远程都有提交，进行 fast merge 时产生 冲突

推荐解决方法 `Please commit your changes or stash them before you merge.`

```
git stash # 将本地修改存入栈中
git pull
git stash pop  # 恢复修改
git stash list
```

## error: you need to resolve your current index first