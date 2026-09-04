
# CRM问题
## 1.CRM_deploy_one构建失败MySQL 权限不足，无法创建函数
### 原因：
MySQL 开启了 binlog 日志，普通账号不能创建函数 / 存储过程。
### 报错信息
```angular2html

You do not have the SUPER privilege and binary logging is enabled
```
### 完整解决步骤：
1. 找到 MySQL 容器
```angular2html
docker ps | grep mysql
```


2. 用 root 进入 MySQL
```angular2html
docker exec -it mysql mysql -uroot -pheadingcrm
```
3.执行修复命令（立即生效)
```angular2html

SET GLOBAL log_bin_trust_function_creators = 1;
```
4. 退出 MySQL
```angular2html
exit
```


5. 验证是否生效
```angular2html
docker exec -it mysql mysql -uroot -pheadingcrm -e "SHOW VARIABLES LIKE 'log_bin_trust_function_creators';"

```
docker exec mysql mysql -uroot -pheadingcrm -e "SET GLOBAL log_bin_trust_function_creators = 1; SHOW VARIABLES LIKE 'log_bin_trust_function_creators';"
看到 ON 说明成功



方案一：单条命令完成修改
修改：docker exec mysql mysql -uroot -pheadingcrm -e "SET GLOBAL log_bin_trust_function_creators = 1;"
检验：docker exec mysql mysql -uroot -pheadingcrm -e "SHOW VARIABLES LIKE 'log_bin_trust_function_creators';"
方案二：修改 + 检验 合并为同一条命令
docker exec -it mysql mysql -uroot -pheadingcrm -e "SET GLOBAL log_bin_trust_function_creators = 1;"

 
## 2.数据库问题修复后，新增Docker容器启动超时报错
### 报错日志

```angular2html
ERROR: Timeout after 180 seconds
```

MySQL 开启了 binlog 日志，普通账号不能创建函数 / 存储过程。
 
### 原因：报错根因
- Jenkins 默认容器启动超时时间为180秒，当前服务器资源（CPU/内存/IO）压力较大
- phoenixcore: 工具容器启动、挂载目录、初始化环境耗时较长，超出默认超时时间


### ps：
改完groovy配置要手动更新

The script is already approved



## 3.CRM_deploy_one构建失败MySQL 权限不足，无法创建函数
- login docker有问题 

```bash
# 报错内容
fatal: [ops]: FAILED! => {"changed": false, "msg": "Error connecting: Error while fetching server API version: Not supported URL scheme http+docker"}
# 解决方法
先看一下linux版本：cat /etc/redhat-release 如果是高版本的linux Rocky Linux release 9.7 (Blue Onyx) ，则兼容有问题，需要执行一下命令
rpm -e --nodeps python3-requests-2.25.1-10.el9_6.noarch
pip3 install requests==2.31.0
```

- CRM_deploy_one构建失败MySQL 权限不足，无法创建函数
### 原因：
MySQL 开启了 binlog 日志，普通
账号不能创建函数 / 存储过程。
### 报错信息
```angular2htm
You do not have the SUPER privilege and binary logging is enabled
```
### 完整解决步骤：(优化版)
 

方案一：单条命令完成修改
修改：docker exec mysql mysql -uroot -pheadingcrm -e "SET GLOBAL log_bin_trust_function_creators = 1;"
检验：docker exec mysql mysql -uroot -pheadingcrm -e "SHOW VARIABLES LIKE 'log_bin_trust_function_creators';"

 