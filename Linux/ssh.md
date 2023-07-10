# SSH 

## 基本信息

### Linux 

配置文件位置`~/.ssh`，包括`known_hosts` 和`authorized_keys`

### Windows

配置文件夹位置`C:\Users\dsj\.ssh`

## 免密登录配置

### Linux 

1. 创建密钥对

   ```bash
   cd ~/.ssh 
   ssh-keygen -t rsa 
   # 在默认目录下创建 id_rsa 和 id_rsa.pub 文件
   # 可以使用 -f filename  指定生成的两个文件的文件名,注意此时需要将文件名加入到.ssh/config文件中，登陆时将使用filename和filename.pub进行验证
   ## 如下：
   ## Host 192.168.1.1
   ##  HostName 192.168.1.1
   ##  User dsj
   ##  IdentityFile .ssh/filename
   ```

2. 发送密钥

   ```bash
   ssh-copy-id 192.168.0.1 
   # 需要登陆到的服务器IP
   # 本地密钥(.pub文件)将拷贝到服务器的authorized_keys中
   ```

3. 链接测试

   ```bash
   ssh dsj@192.168.0.1
   ```
   
   ### Windows 
   
   安装 git , 使用git bash