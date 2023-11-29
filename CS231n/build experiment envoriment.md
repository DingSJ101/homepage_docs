---
date: 2022-10-14 16:24:38.680696
lastmod: 2022-11-04 22:55:00.927314
---
## overview



## task one : build python environment

# base_python

### jupyter + docker

Dockerfile:

```dockerfile
FROM python:3.9
RUN pip install jupyter notebook 
RUN apt update && apt install -y vim  sudo

RUN addgroup --gid 1000 dsj && \
    adduser --uid 1000 --ingroup dsj --disabled-password dsj &&\
    echo "dsj    ALL=(ALL)  NOPASSWD:ALL" >> /etc/sudoers

WORKDIR /workspace
COPY start.sh /etc/init.d/
CMD ["sh","/etc/init.d/start.sh"]

# # 添加用户：赋予sudo权限，指定密码
# RUN useradd --create-home --no-log-init --shell /bin/bash dsj \
#     && adduser dsj sudo \
#     && echo "dsj:123456" | chpasswd
```

start.sh:

```sh
#! /bin/bash
touch /workspace/run.log
nohup jupyter notebook --allow-root > /workspace/run.log 2>&1 
```

build the image and container: 

```bash
docker build -t torch .
docker run -idt --name torch -u 1000:1000 -p 8888:8888 -v ~/workspace/cs231n:/workspace -v /etc/localtime:/etc/localtime --restart=on-failure torch
docker exec -it torch bash
# jupyter contrib nbextension install --user	# 拓展
# jupyter nbextensions_configurator enable --user	# 拓展管理页面
jupyter notebook --generate-config  # 记录 config.py 文件的位置和文件名
# Writing default config to: /home/dsj/.jupyter/jupyter_notebook_config.py
python
docker run -idt --gpus all --name torch_gpu -u 1000:1000 -p 8888:8888 -v ~/workspace/cs231n:/workspace -v /etc/localtime:/etc/localtime --restart=on-failure torch
```

config jupyter , `python`


```bash
>>> from notebook.auth import passwd
>>> passwd()
Enter password: 
Verify password: 
'argon2:$argon2id$v=19$m=10240,t=10,p=8$Rh+/GUCH+U6D3Trs30e8xw$73CmvLMYOSecT3yWa3uFnX/gORdmGHAxmLmFM0gLSI8'
```

修改之前生成的config.py 文件，新增：

```bash
c.NotebookApp.allow_remote_access = True  # 允许远程
c.NotebookApp.ip = '*'   # 允许其他ip接入
c.NotebookApp.port = 8888  # 容器端口
c.NotebookApp.password = 'argon2:$argon2id$v=19$m=10240,t=10,p=8$Rh+/GUCH+U6D3Trs30e8xw$73CmvLMYOSecT3yWa3uFnX/gORdmGHAxmLmFM0gLSI8'  # 密码
c.NotebookApp.open_browser = False   # 默认不打开浏览器
c.NotebookApp.allow_root = True  # 使用root
c.NotebookApp.notebook_dir = '/workspace'  # 默认打开路径
```

重启容器

`docker restart torch`

浏览器地址`http://127.0.0.1:8888`即可进入jupyter

### nbextension

```bash
# 在 jupyter notebook == 6.5.1 版本无法显示nbextension页面 ， 拓展功能可以通过命令行单独设置
# pip install jupyter_nbextensions_configurator jupyter_contrib_nbextensions
# jupyter contrib nbextension install --user
# jupyter nbextensions_configurator enable --user  # jupyter > 4.10 后不需要这步，详情见官方文档
```

![拓展效果](https://cdn.jsdelivr.net/gh/DingSJ101/picgo_hub@master/img/20221015210736.png)