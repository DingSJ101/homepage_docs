---
date: 2023-11-28 19:00:23.874149
---
## install Hugo

### from linux package managers

`sudo apt install hugo` 安装的包版本较低，可能无法适配相关命令

### from source code

依赖项目：

1. Git
2. Go
3. Dart Sass

#### Go

https://go.dev/dl/

```bash
# rm -rf /usr/local/go
wget https://go.dev/dl/go1.21.4.linux-amd64.tar.gz
sudo tar -C /usr/local  -xzvf go1.21.4.linux-amd64.tar.gz 
echo "export PATH=$PATH:/usr/local/go/bin:/home/dsj/go/bin" >> ~/.bashrc
go version
```

#### Hugo

```bash
git clone https://github.com/gohugoio/hugo.git
cd hugo
CGO_ENABLED=1 go install -tags extended
hugo version
```



## Start Hugo

```bash
hugo new site quickstart
cd quickstart
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack

echo "theme = 'ananke'" >> hugo.toml
hugo server

hugo new content posts/my-first-post.md # run in root dir 

```



