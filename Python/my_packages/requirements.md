---
date: 2022-07-23 13:51:28.160508
title: requirements
---
## 生成python的requiremets文件

```bash
# 环境中全部依赖
pip freeze > requirements.txt

# 当前项目中的依赖
# 安装
pip install pipreqs
# 在当前目录生成
pipreqs . --encoding=utf8 --force #--force 强制执行，覆盖requirements.txt
```

## 配置依赖

```bash
pip install -r requirements.txt
```

