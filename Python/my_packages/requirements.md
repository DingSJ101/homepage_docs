---
abbrlink: 70bea1aa
title: requirements
categories:
  - Python
  - my_packages
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

