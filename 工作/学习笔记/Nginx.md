## 1.Nignx基本概念
### 1.1反向代理
>正向代理：
客户端不能直接访问目标服务器，通过代理帮忙访问


>反向代理：把请求发送到反向代理服务器，反向代理服务器去选择目标服务器获取数据后，再返回给客户端，暴露的是代理服务器地址，隐藏真实服务器地址

### 1.2负载均衡
>把访问流量均匀分发到多台服务器，避免单台过载、提升整体服务稳定性与响应速度。

### 1.3动静分离
>网站静态资源（css,js）走缓存/CDN，动态请求(jsp、servlet）交给应用服务器，分流提速、减轻后端压力。

## 2.Nignx安装、常用命令和配置文件
### 2.1Nignx安装
### 2.2常用命令
### 2.2.1使用前提条件：进入nginx目录
/usr/local/nginx/sbin

### 2.2.2 查看nginx版本号
```shell
./nginx -v
```
### 2.2.3 启动nginx
```shell
./nginx -s stop
```
### 2.2.4 关闭nginx
```shell
./nginx
```
### 2.2.5 重加载
```shell
./nginx -s reload
```

### 3.3配置文件

### 3.3.1 配置文件位置
> /usr/local/nginx/nginx.conf
 
### 3.3.2 配置文件组成
- 全局块
- events块
- http块
- - http块
- - server块

#### 1.server 全局头部（虚拟主机基础定义）
```commandline
server {
    listen       80;
    server_name  localhost;#nginx服务器ip
    access_log   /usr/local/openresty/nginx/logs/pos_access.log main;
    error_log   /usr/local/openresty/nginx/logs/pos_error.log;
    set_by_lua_file $traceid /usr/local/openresty/nginx/sbin/traceid.lua;
```

#### 2. favicon.ico 静态图标过滤
```commandline
location = /favicon.ico 
{
    log_not_found off;
    access_log on;
}
```

#### 3. 根路径静态首页（前端入口）
```commandline
location /{
    root   /usr/local/openresty/nginx/html;
    index  index.html index.htm;
}
```
所有未匹配到其他 location 的请求先走这里
静态文件目录：OpenResty 默认 html 文件夹
访问根路径自动加载 index.html，页面内跳转至 hdpos4-web 后端系统


#### 4. 通用文件下载公共路由（抽离复用 downloadfile.conf）
```commandline
location ~(/hdpos4-web/latin/file/downloadFile.hd|/hdpos4-web/static/gembox/) {
    proxy_pass http://hdpos4-dist;
    include ../conf.d/downloadfile.conf;
}
location /pasoreport-web/latin/file/downloadFile.hd {
   proxy_pass http://pasoreport-web;
   include ../conf.d/downloadfile.conf;
}
```
正则匹配报表 / 静态文件下载接口
include 引入公共下载配置文件，统一文件下载超时、缓冲区、请求头规则，避免重复代码
分别代理到 POS 主服务、报表服务 upstream


#### 5. 路由重写转发
```commandline
proxy_pass    http://pasoreport-web;
rewrite /hdpos4-web/xxx/(.*)$ /pasoreport-web/xxx/$1 break;
```

1.路由匹配入口
```commandline
location /hdpos4-web/rest/report_query {
```
2.独立日志（报表请求单独落盘，方便排查)
```commandline
access_log   /usr/local/openresty/nginx/logs/pasoreport-web_access.log main;
error_log   /usr/local/openresty/nginx/logs/pasoreport-web_error.log;
```
3. 后端目标服务
```commandline
proxy_pass    http://pasoreport-web;
```
4. 核心路径重写
```commandline
rewrite /hdpos4-web/rest/report_query/(.*)$ /pasoreport-web/rest/report_query/$1 break;
```
5. 代理请求头透传(后端获取真实访问信息)
```commandline
proxy_pass_header Server;
proxy_set_header Host $host;
proxy_redirect off;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Scheme $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
6. 真实 IP 解析模块（real_ip 系列，配合 X-Forwarded-For）
```commandline
set_real_ip_from 0.0.0.0/0;
real_ip_header  X-Forwarded-For;
real_ip_recursive on;
```
7. 客户端上传大小限制
```commandline
client_max_body_size 100m;
client_body_buffer_size 128k;
```
8. 超时配置（报表专用长超时，核心重点）
```commandline
proxy_connect_timeout 30m;   # 连接后端最长等待30分钟
proxy_send_timeout 10m;      # Nginx向后端发包超时10分钟
proxy_read_timeout 30m;      # 等待后端返回数据最长30分钟（导出大数据报表关键）
```
9. 代理缓冲区优化（全内存缓冲，无磁盘IO）
```commandline
proxy_buffering on;
proxy_buffer_size 128k;
proxy_buffers 4 256K;
proxy_busy_buffers_size 512k;
proxy_max_temp_file_size 0;
```
10. 客户端断开不中断后端任务（业务 BUG 规避）
```commandline
proxy_ignore_client_abort on;
```



## 3.配置实例——反向代理

## 4.配置实例——负载均衡

## 5.配置实例——动静分离

## 6.配置实高可用集群

## 7.nginx原理

```commandline

```