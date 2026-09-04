## phoenix.yaml
### 1.hosts:(middle0、app0)
  -  innerip:
  - password:
### 2.rds
   - innerip
   - password
### 3. oss.oss_objectService_redirectionBaseUrl、commons.gateway_server_url 里带域名的部分要重新解析


## docker_environments.yaml

### 1.phoenix-common-license.server-url  许可证服务器地址

### 2.phoenix-coupon-core.es.hostAndPorts  es服务器地址

### 3.redis.host.ip

### 4. 所有指向网关/回调的域名

## Jenkins / Apollo 里的地址也会变

## 域名 / 公网 / 证书会变

