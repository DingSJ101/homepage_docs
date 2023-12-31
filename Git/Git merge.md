---
date: 2022-07-28 15:27:16.029670
---


## 工作原理



## 操作步骤

### 选择分支

   ```
   git status
   git checkout branch_main
   ```

### 拉取分支

   ```
   git fetch
   git pull
   ```

  

### 合并分支

   ```
   git merge branch_to_merge
   ```



#### 快速合并

当两个分支中的提交没有分叉，只是其中一个分支在另一分支之后上多了部分提交。此时可以直接合并两个分支到最新的提交

#### 多路合并

当两个分支的提交存在分叉，即在某一个公共提交后，各自有不同的提交。

![](https://s2.loli.net/2022/07/28/mRN5fqw9lYjxLSi.png)

   此时，往往会产生冲突，需要合并冲突。

```
git status
git commit -m " merge "
git push
```

![](https://s2.loli.net/2022/07/28/LnCyXQWdYVlhzqm.png)

已成功合并的会显示待提交，接下来处理冲突的部分。在冲突的文件中，会采用<<<<<<<,=======,>>>>>>>类型的标记来区分不同的提交，其中等号前的为接受合并的分支内容，等号后为合并进来的内容。修改完成需要commit 以后才能再提交其他文件的修改。