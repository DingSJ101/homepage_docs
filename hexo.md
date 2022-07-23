---
abbrlink: ab21860c
title: hexo
categories: uncategorized
---
# 启动

```bash
hexo init <folder>
cd <folder>
npm install
# <folder>
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
#</folder>
hexo clean  #清除本地站点文件夹下的缓存文件（db.json）和已有的静态文件（public）
hexo generate # 生成相应的静态网页，生成的静态网页以及相关资源都会在public目录下
hexo server # 启动服务器，您的网站会在 http://localhost:4000 下启动。-d deploy
hexo deploy #部署网站。

git@github.com:DingSJ101/hexo_blog.git
```

## webhook

创建hook

![image-20220612223800159](D:%5CAPPlications%5CTypora%5Cpicturecopy%5Cimage-20220612223800159.png)

```bash
http://175.24.167.6:4000/hooks/webhook-deploy-hexo
```

### PayloadTooLarge

```bash
cd /usr/local/lib/node_modules/webhook-cli/lib/
vi server.js
## add
#	app.use(bodyParser.json({ limit: '5000000000kb' }));
echo "app.use(bodyParser.json({ limit: '5000000000kb' }));" >> server.js
restart
```

