---
date: 2023-06-28 19:57:58.582460
---




```bash
crontab [-u user] file //设定某个用户的cron服务，一般root用户在执行这个命令的时候需要此参数 
crontab -l //列出某个用户cron服务的详细内容 
crontab -e //编辑某个用户的cron服务
crontab -r //删除某个用户的cron服务 
```



定时任务格式：

```bash
* * * * * * command
```

其中

第一个 `*` 表示分钟，0~59
第二个 `*` 表示小时，0~23
第三个 `*` 表示每月中的第几天，1~31
第四个 `*` 表示第几个月，1~12
第五个 `*` 表示星期几，0~7

占位符可以使用特殊符号表示

![image-20230628200827679](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230628200829.png)



## 查看任务执行情况

```bash
sudo cat /var/log/syslog
```



## 实践

```bash
script_dir=$(dirname "$0")
config_file="$script_dir/config.json"
# 导入的文件使用相对路径即可
```



```bash
bash test.sh >> log.txt 
# 将脚本输出记录
```



