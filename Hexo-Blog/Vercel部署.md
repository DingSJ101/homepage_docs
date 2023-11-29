---
date: 2023-07-10 15:15:52.043268
---
## Vercel 部署

Vercel提供了无服务器的静态网络部署

在Vercel主页选择`Add New Project`

导入项目

![image-20230710151837865](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710151839.png)

设置构建配置

`build Command `: `git clone  https://github.com/next-theme/hexo-theme-next.git  ./themes/next && git clone https://gitee.com/starry101/docs.git ./source/_posts   && hexo g`

`Install Command`: `npm run talk `

![image-20230710152107586](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710152109.png)

点击`Deploy` 即可

![image-20230710152151867](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710152153.png)



## 自定义域名

在`DashBoard-Settings-Domains`中添加自己的域名

![image-20230710153506548](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710153507.png)

### CNAME转发子域名

修改CNAME记录转发到域名

![image-20230710163829214](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710163830.png)

![image-20230710163812585](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710163814.png)

![image-20230710165432217](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710165433.png)

### NS转发域名服务器

去域名服务商处设置解析，解析到vercel的dns ，`ns2.vercel-dns.com`

![image-20230710172126882](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710172128.png)

在Vercel的域名窗口，添加解析

![image-20230710171314764](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710171316.png)

最后在实例`hexo-blog-2nui`的域名中添加子域名

![image-20230710171936608](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230710171937.png)



实测 ，除了CNAME 转CNAME.dns 然后接实例域名解析，其他方法国内无法访问