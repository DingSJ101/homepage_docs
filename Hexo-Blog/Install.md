---
categories:
  - Hexo-Blog
abbrlink: e07784e5
title: Install
---
```bash

docker pull taskbjorn/hexo
docker volume create my_hexo_data
docker run -it --name my_hexo_container -p 4000:4000 -v hexo_data:/home/hexo/.hexo taskbjorn/hexo

~/.docker $ cat hexo-entrypoint.sh
# Initialize blog if root folder is empty
if [ -z "$(ls)" ]; then
        hexo init ./
fi

# Clean previously generated static filed
hexo clean

# Generate new static files
hexo generate

# Serve static files through Hexo server
hexo server -i -s


cd ./themes
git submodule add https://hub.0z.gs/JoeyBling/hexo-theme-yilia-plus.git

```

## 文档结构

```markdown

---
title: [latex] 数学符号
date: 2022-06-19 19:30:29
categories: markdown
tags: 
- latex
---

```

