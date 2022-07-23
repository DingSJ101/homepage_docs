---
abbrlink: 8545f871
title: Typora+VSC+PicGo+SMMS
categories: uncategorized
---
# PicGo

需要下载nodejs环境，较简单，略。

安装插件，需要能够连上github，并使用管理员权限运行PicGo.exe。
在插件设置中搜索并安装。
![](https://s2.loli.net/2022/06/19/6Ac5ea2ifvCTthm.png)

### 插件

#### compress

图片压缩

#### smms-user

连接sm.ms图床

**配置：**

- 注册并登录[SMMS](https://sm.ms)，复制API Token  ![1655605782181](https://s2.loli.net/2022/06/19/Lixvh53fkBmVbWR.png)

- 复制到图床设置里![](https://s2.loli.net/2022/06/19/p3cbjRvf6ro1txB.png)

- 设为默认图床。

#### quick-capture

通过快捷键一键完成：`截图-上传-获取图片URL链接-URL链接保存到剪贴板` 的工作流。

**注意：** 配置的截图脚本必须满足以下两点要求，否则本插件无法如期运行，Window已配置好所需脚本。

1. 可以通过命令行直接进入截图界面
2. 然后关闭截图界面之后脚本程序会退出

默认快捷键 `Ctrl+Shift+0` ， 可以打开PicGo主窗口，在 `PicGo设置` -> `修改快捷键` 处修改快捷键。

使用时，通过快捷键进行正常截图操作，之后粘贴的就是URL了。

#### github-plus

这个插件提供了比PicGo自带的GitHub图床支持更多的功能：删除操作同步、可以同步GitHub里图片的记录、支持gitee等

![](https://s2.loli.net/2022/06/19/9PvRHnJupaXqoVi.png)

# Typora

免费版，需安装1.0以前的beta版。

配合PicGo使用，需要0.9.84及以上版本

### 配置

在偏好设置中，配置如下
![](https://s2.loli.net/2022/06/19/GhDL7Bve5VasPZ8.png)

由于typora默认使用36677端口，所以需要修改PicGo服务器端口为36677
![](https://s2.loli.net/2022/06/19/leLsT8cFpkZq1ju.png)

使用时，可以将本章中的本地图片上传并切换为图床链接
![](https://s2.loli.net/2022/06/19/LsDelUXhczHCrZ9.png)
​  

# VSC

安装VSCode，略

安装VSCode插件

- Markdown All in One
  
  > markdown写作插件

- PicGo
  
  > [VS-PicGo插件](https://github.com/PicGo/vs-picgo)

  **插件配置**
  
  在Config Path 中填入PicGo的配置文件路径 ：
  "YOUR_HOME_DIR\\AppData\\Roaming\\PicGo\\data.json"

  ![](https://s2.loli.net/2022/06/19/Zy1E7ibfq85hjaA.png)
  
  由于vs-picgo插件目前还没有完成对PicGo插件的适配(作者好像咕咕了)，所以只能使用PicGo自带的图床，无法连接smms-user。

  **使用方法：**
  - 截图后，`Control+Alt+U` 上传图片并粘贴URL
  - `Control+Alt+E` 打开文件管理器，选择图片上传，并粘贴URL
  - `Control+Alt+O` 弹出输入框，输入图片路径，上传后粘贴URL

  虽然无法通过该插件使用PicGo中的插件图床，但是通过PicGo的快捷键 `Control+Alt+P` 可以实现剪贴板上传（快捷上传）
