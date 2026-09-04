# ERP 阿里云部署配置清单

> 创建时间：2026-07-10  
> 区域：cn-hangzhou（杭州）  
> 用途：ERP 集成测试环境（integration_test）

---

## 1. 资源总览

| 资源类型 | 数量 | 计费方式 | 备注 |
|---------|------|----------|------|
| ECS 实例 | 2 台 | 按量付费（PayByTraffic） | cjs-app（应用）、cjs-ops（运维+Jenkins） |
| SLB 负载均衡 | 1 个 | 按量付费（PayOnDemand） | 经典网络（classic） |
| NAT 网关 | 1 个 | 按量付费 | 为 cjs-app 提供出网能力 |
| EIP 弹性公网IP | 1 个 | 按流量计费（PostPaid） | 绑定到 NAT 网关 |
| VPC | 1 个 | 免费 | vpc-bp1dqlii6entic6sm7f3c |
| 交换机 | 1 个 | 免费 | vsw-bp1bcru70p1s7lyv2bhuy |

---

## 2. 网络配置

### 2.1 VPC

```yaml
VPC ID: vpc-bp1dqlii6entic6sm7f3c
区域: cn-hangzhou
```

### 2.2 交换机（VSwitch）

```yaml
交换机 ID: vsw-bp1bcru70p1s7lyv2bhuy
可用区: cn-hangzhou-k
CIDR: 10.0.0.0/24（默认）
```

> **复购注意**：ECS 和 SLB 必须购买在同一个 VPC 和交换机下，否则内网不通。

---

## 3. ECS 实例

### 3.1 cjs-app（应用服务器）

```yaml
实例名称: cjs-app
实例 ID: i-bp1cqq8dab5g5e07amrf
实例规格: ecs.e-c1m4.2xlarge (8 vCPU, 32 GB 内存)
可用区: cn-hangzhou-k
VPC: vpc-bp1dqlii6entic6sm7f3c
交换机: vsw-bp1bcru70p1s7lyv2bhuy
内网 IP: 10.0.0.151
公网 IP: 无（通过 NAT 出网）
镜像: CentOS 7.9 / Alibaba Cloud Linux 3
系统盘: ESSD 云盘（建议 40GB+）
数据盘: /hdapp（建议 150GB+，ESSD）
安全组: sg-bp19rinqgjwduqkz21bx
计费方式: PayByTraffic（按量付费）
root 密码: Zn!axPqW&dLV&D7X
```

### 3.2 cjs-ops（运维+Jenkins 服务器）

```yaml
实例名称: cjs-ops
实例 ID: i-bp19rinqgjwduql2iel3
实例规格: ecs.u1-c1m8.large (2 vCPU, 16 GB 内存)
可用区: cn-hangzhou-k
VPC: vpc-bp1dqlii6entic6sm7f3c
交换机: vsw-bp1bcru70p1s7lyv2bhuy
内网 IP: 10.0.0.150
公网 IP: 47.98.107.203（自动分配）
镜像: CentOS 9.7 / Alibaba Cloud Linux 3
系统盘: ESSD 云盘（建议 40GB+）
数据盘: /hdapp（建议 100GB+，ESSD）
安全组: sg-bp19rinqgjwduqkz21bx
计费方式: PayByTraffic（按量付费）
root 密码: Zn!axPqW&dLV&D7X
```

> **复购注意**：
> - 两台机器必须同 VPC、同交换机，否则内网不通。
> - cjs-app 建议分配公网 IP 或购买 EIP 绑定，否则每次都要通过 cjs-ops 跳板访问。

---

## 4. SLB 负载均衡

```yaml
负载均衡 ID: lb-bp1u690tw6epw0k5zizob
名称: auto_named_slb
类型: internet（公网）
网络类型: classic（经典网络） ⚠️
公网 IP: 47.96.228.102
计费方式: PayOnDemand（按量付费）
规格: slb.lcu.elastic
后端服务器:
  - i-bp1cqq8dab5g5e07amrf (cjs-app, 权重 100)
  - i-bp19rinqgjwduql2iel3 (cjs-ops, 权重 100)
监听配置:
  - 端口 80 (HTTP) → 后端 80 (rr)
  - 端口 8888 (HTTP) → 后端 8080 (rr)  [Jenkins]
  - 端口 31521 (TCP) → 后端（未指定）
  - 端口 39080 (其他) → 后端（未指定）
```

> ⚠️ **重要提醒**：当前 SLB 是 **经典网络（classic）**，与 VPC 的 ECS 存在兼容性限制。虽然当前已挂载成功，但官方建议下次复购时选择 **VPC 型 SLB**，步骤：
> 1. 释放当前 classic SLB
> 2. 创建新 SLB 时选择 **VPC** 类型，指定相同 VPC 和交换机
> 3. 重新添加监听和后端服务器

---

## 5. NAT 网关 + EIP

### 5.1 NAT 网关

```yaml
NAT 网关 ID: ngw-bp1egq2z35mormqnpr6w8
名称: nat-20260710
VPC: vpc-bp1dqlii6entic6sm7f3c
计费方式: 按量付费
```

### 5.2 EIP（绑定到 NAT）

```yaml
EIP ID: eip-bp1ekyxtbzu6pxd7xj5qz
IP 地址: 118.31.37.105
状态: InUse（已绑定到 NAT）
计费方式: PostPaid（按流量计费）
```

> **作用**：让没有公网 IP 的 cjs-app（10.0.0.151）能够通过 NAT 访问外网（如下载 Docker 镜像、访问 Maven 仓库等）。

> **复购注意**：如果给 cjs-app 分配了公网 IP，则不需要购买 NAT 和 EIP。

---

## 6. 安全组规则

安全组 ID：`sg-bp19rinqgjwduqkz21bx`

### 6.1 入方向规则（Ingress）

| 协议 | 端口 | 授权对象 | 描述 | 用途 |
|------|------|----------|------|------|
| TCP | 22 | 0.0.0.0/0 | System created rule. | SSH |
| TCP | 3389 | 0.0.0.0/0 | System created rule. | RDP |
| ICMP | -1 | 0.0.0.0/0 | System created rule. | Ping |
| TCP | 8080 | 0.0.0.0/0 | jenkins | Jenkins 访问 |
| TCP | 82 | 10.0.0.0/8 | gateway_service | gateway_service |
| TCP | 38093 | 10.0.0.0/8 | init-tool | init-tool |
| TCP | 38105 | 10.0.0.0/8 | gem-service | gem-service |
| TCP | 38112 | 10.0.0.0/8 | rumba-oss-server | rumba-oss-server |
| TCP | 38115 | 10.0.0.0/8 | spms-hdpos-web | spms-hdpos-web |
| TCP | 38119 | 10.0.0.0/8 | card-server-proxy-service | card-server-proxy |
| TCP | 38169 | 10.0.0.0/8 | h6-crm-service | h6-crm-service |
| TCP | 38176 | 10.0.0.0/8 | h6-openapi2-service | h6-openapi2-service |
| TCP | 38180 | 10.0.0.0/8 | hdpos4-dist | hdpos4-dist |
| TCP | 38212 | 10.0.0.0/8 | bpfm-app-service | bpfm-app-service |
| TCP | 38280 | 10.0.0.0/8 | jposbo | jposbo |
| TCP | 38380 | 10.0.0.0/8 | panther-taskweb | panther-taskweb |
| TCP | 38480 | 10.0.0.0/8 | panther-dts-server | panther-dts-server |
| TCP | 38980 | 10.0.0.0/8 | pasoreport-web | pasoreport-web |

### 6.2 缺失端口（需手动补充）

以下端口在 Wiki 中有提到，但当前安全组未开放：

| 端口 | 描述 | 建议授权对象 |
|------|------|-------------|
| 9080 | license-server | 10.0.0.0/8 或 0.0.0.0/0 |
| 38136 | notice（hdpos6-notice-service） | 10.0.0.0/8 |
| 39105 | gem-service 健康检查 | 10.0.0.0/8 |
| 39115 | spms-hdpos-web 健康检查 | 10.0.0.0/8 |
| 39119 | card-server-proxy 健康检查 | 10.0.0.0/8 |
| 39136 | notice 健康检查 | 10.0.0.0/8 |
| 39169 | h6-crm 健康检查 | 10.0.0.0/8 |

> **复购注意**：如果后续需要外网直接访问 ERP 应用，需将 `10.0.0.0/8` 改为 `0.0.0.0/0`。但生产环境建议通过 SLB 或 Nginx 统一入口，安全组只保留内网访问。

---

## 7. Nginx / OpenResty 反向代理配置

部署位置：`cjs-ops`（`10.0.0.150`）或单独购买一台 Nginx 服务器

```nginx
upstream hdpos4-dist {
    server 10.0.0.151:38180;
}

upstream pasoreport-web {
    server 10.0.0.151:38980;
}

upstream spms-hdpos-web {
    server 10.0.0.151:38115;
}

upstream gem-service {
    server 10.0.0.151:38105;
}

upstream panther-taskweb {
    server 10.0.0.151:38380;
}

upstream panther-dts-server {
    server 10.0.0.151:38480;
}

upstream init-tool {
    server 10.0.0.151:38093;
}

upstream bpfm-app-service {
    server 10.0.0.151:38212;
}

upstream card-server-proxy-service {
    server 10.0.0.151:38119;
}

upstream h6-openapi2-service {
    server 10.0.0.151:38176;
}

upstream h6-crm-service {
    server 10.0.0.151:38169;
}

upstream rumba-oss-server {
    server 10.0.0.151:38112;
}

upstream gateway_service {
    server 10.0.0.151:82;
}

upstream jposbo {
    server 10.0.0.150:38280;
}

# 以下为保留原有配置（无需修改）
upstream dcproxy {
    server dcproxy.qianfan123.com:38081;
}

upstream hailifang {
    server portal.hd123.com;
}

server {
    listen 80;
    server_name 47.96.228.102;

    location /hdpos4-web {
        proxy_pass http://hdpos4-dist;
    }

    location /pasoreport-web {
        proxy_pass http://pasoreport-web;
    }

    location /spms-web {
        proxy_pass http://spms-hdpos-web;
    }

    location /gem-service {
        proxy_pass http://gem-service;
    }

    location /panther-web {
        proxy_pass http://panther-taskweb;
    }

    location /panther-dts-server {
        proxy_pass http://panther-dts-server;
    }

    location /init-tool-web {
        proxy_pass http://init-tool;
    }

    location /cardserver {
        proxy_pass http://card-server-proxy-service;
    }

    location /h6-crm-service {
        proxy_pass http://h6-crm-service;
    }

    location /h6-openapi2-service {
        proxy_pass http://h6-openapi2-service;
    }

    location /rumba-oss-server {
        proxy_pass http://rumba-oss-server;
    }

    location /gateway_service {
        proxy_pass http://gateway_service;
    }

    location /jposbo {
        proxy_pass http://jposbo;
    }
}
```

> **注意**：`hdpos4-web` 和 `pasoreport-web` 在 Wiki 中公网地址都是 `http://47.96.228.102/hdpos4-web`，存在路径冲突。部署前请确认路径规划。

---

## 8. 应用访问地址对照表

| 应用名称 | 公网地址（通过 SLB/Nginx） | 内网地址 | 用户名/密码 |
|---------|--------------------------|----------|------------|
| hdpos4-web | `http://47.96.228.102/hdpos4-web` | `http://10.0.0.151:38180/hdpos4-web` | hdposadmin/123456 |
| pasoreport-web | `http://47.96.228.102/hdpos4-web` ⚠️ | `http://10.0.0.151:38980/pasoreport-web` | hdposadmin/123456 |
| spms-web | `http://47.96.228.102/spms-web` | `http://10.0.0.151:38115/spms-web` | — |
| gem-service | `http://47.96.228.102/gem-service/swagger-ui/index.html` | `http://10.0.0.151:38105/gem-service/swagger-ui/index.html` | — |
| panther-web | `http://47.96.228.102/panther-web` | `http://10.0.0.151:38380/panther-web` | — |
| panther-dts-server | `http://47.96.228.102/panther-dts-server` | `http://10.0.0.151:38480/panther-dts-server` | — |
| init-tool | `http://47.96.228.102/init-tool-web/index.html` | `http://10.0.0.151:38093/init-tool-web/index.html` | — |
| card-server-proxy | `http://47.96.228.102/cardserver` | `http://10.0.0.151:38119/cardserver` | ijclzgS5/ilbpgaj*MBWCINJ0 |
| h6-crm-service | `http://47.96.228.102/h6-crm-service/doc.html` | `http://10.0.0.151:38169/h6-crm-service/doc.html` | ifieeeC3/yhjoceu*ZCIUIZK9 |
| h6-openapi2-service | `http://47.96.228.102/h6-openapi2-service` | `http://10.0.0.151:38176/h6-openapi2-service` | — |
| rumba-oss-server | `http://47.96.228.102/rumba-oss-server` | `http://10.0.0.151:38112/rumba-oss-server` | — |
| jposbo | `http://47.96.228.102/jposbo` | `http://10.0.0.150:38280/jposbo` | — |
| license-server | — | `http://10.0.0.151:9080` | admin/ysrGZ1VG |
| notice | `http://47.96.228.102/notice/swagger-ui.html` | `http://10.0.0.151:38136/notice/swagger-ui.html` | — |
| Jenkins | `http://47.96.228.102:8888/` | `http://10.0.0.150:8080` | — |

---

## 9. Jenkins 核心流水线

部署在 `cjs-ops`（`http://47.96.228.102:8888/`）上，需提前配置：

1. **ERP_db_deploy_int** — 数据库初始化部署
2. **ERP_app_deploy_int** — 业务应用容器部署
3. **GLOBLE-update-jenkins-jobs** — 生成 ERP 核心流水线
4. **GLOBLE_deploy_nginx** — OpenResty 网关配置发布
5. **ERP_basic_auth** — 生成 H6 应用接口认证信息

---

## 10. 复购步骤（CLI 命令参考）

以下命令基于阿里云 CLI（`aliyun.exe`），按顺序执行。

### 10.1 创建 VPC 和交换机（如已存在则跳过）

```bash
# 创建 VPC
aliyun vpc CreateVpc --RegionId cn-hangzhou --CidrBlock 10.0.0.0/8 --VpcName cjs-vpc

# 创建交换机（cn-hangzhou-k）
aliyun vpc CreateVSwitch --RegionId cn-hangzhou --ZoneId cn-hangzhou-k --CidrBlock 10.0.0.0/24 --VpcId <vpc-id> --VSwitchName cjs-vsw
```

### 10.2 创建安全组

```bash
aliyun ecs CreateSecurityGroup --RegionId cn-hangzhou --VpcId <vpc-id> --SecurityGroupName cjs-sg --Description "ERP security group"
```

然后按第 6 节表格添加所有规则（示例）：

```bash
aliyun ecs AuthorizeSecurityGroup --RegionId cn-hangzhou --SecurityGroupId <sg-id> --IpProtocol tcp --PortRange 22/22 --SourceCidrIp 0.0.0.0/0 --Description SSH
aliyun ecs AuthorizeSecurityGroup --RegionId cn-hangzhou --SecurityGroupId <sg-id> --IpProtocol tcp --PortRange 8080/8080 --SourceCidrIp 0.0.0.0/0 --Description jenkins
aliyun ecs AuthorizeSecurityGroup --RegionId cn-hangzhou --SecurityGroupId <sg-id> --IpProtocol tcp --PortRange 38180/38180 --SourceCidrIp 10.0.0.0/8 --Description hdpos4-dist
# ... 按表格补充所有端口
```

### 10.3 创建 ECS 实例

```bash
# cjs-app（应用服务器，建议给公网IP）
aliyun ecs RunInstances --RegionId cn-hangzhou \
  --ZoneId cn-hangzhou-k \
  --ImageId centos_7_9_x64_20G_alibase_20240628.vhd \
  --InstanceType ecs.e-c1m4.2xlarge \
  --SecurityGroupIds '["<sg-id>"]' \
  --VSwitchId <vsw-id> \
  --InstanceName cjs-app \
  --InternetChargeType PayByTraffic \
  --InternetMaxBandwidthIn 100 \
  --InternetMaxBandwidthOut 100 \
  --Password "Zn!axPqW&dLV&D7X" \
  --SystemDisk.Category cloud_essd \
  --SystemDisk.Size 40 \
  --DataDisks '[{"Size":100,"Category":"cloud_essd"}]' \
  --InstanceChargeType PostPaid \
  --Amount 1

# cjs-ops（运维+Jenkins，必须给公网IP）
aliyun ecs RunInstances --RegionId cn-hangzhou \
  --ZoneId cn-hangzhou-k \
  --ImageId centos_7_9_x64_20G_alibase_20240628.vhd \
  --InstanceType ecs.u1-c1m8.large \
  --SecurityGroupIds '["<sg-id>"]' \
  --VSwitchId <vsw-id> \
  --InstanceName cjs-ops \
  --InternetChargeType PayByTraffic \
  --InternetMaxBandwidthIn 100 \
  --InternetMaxBandwidthOut 100 \
  --Password "Zn!axPqW&dLV&D7X" \
  --SystemDisk.Category cloud_essd \
  --SystemDisk.Size 40 \
  --DataDisks '[{"Size":100,"Category":"cloud_essd"}]' \
  --InstanceChargeType PostPaid \
  --Amount 1
```

### 10.4 创建 SLB（建议 VPC 型）

```bash
# VPC 型 SLB（推荐）
aliyun slb CreateLoadBalancer --RegionId cn-hangzhou \
  --AddressType internet \
  --VpcId <vpc-id> \
  --VSwitchId <vsw-id> \
  --LoadBalancerName cjs-slb \
  --PayType PayOnDemand \
  --LoadBalancerSpec slb.lcu.elastic

# 添加 HTTP 监听（80 → 80）
aliyun slb CreateLoadBalancerHTTPListener --LoadBalancerId <slb-id> \
  --ListenerPort 80 --BackendServerPort 80 --Scheduler rr

# 添加 HTTP 监听（8888 → 8080 Jenkins）
aliyun slb CreateLoadBalancerHTTPListener --LoadBalancerId <slb-id> \
  --ListenerPort 8888 --BackendServerPort 8080 --Scheduler rr

# 挂载后端服务器
aliyun slb AddBackendServers --LoadBalancerId <slb-id> \
  --BackendServers '[{"ServerId":"<app-instance-id>","Weight":100},{"ServerId":"<ops-instance-id>","Weight":100}]'
```

> ⚠️ 如果仍选择经典网络 SLB，去掉 `--VpcId` 和 `--VSwitchId` 参数。但可能存在与 VPC ECS 的兼容问题。

### 10.5 创建 NAT 网关 + EIP（可选，如果 cjs-app 没有公网IP）

```bash
# 创建 NAT 网关
aliyun vpc CreateNatGateway --RegionId cn-hangzhou \
  --VpcId <vpc-id> \
  --Name cjs-nat \
  --Spec Small \
  --InstanceChargeType PostPaid

# 购买 EIP
aliyun vpc AllocateEipAddress --RegionId cn-hangzhou \
  --Bandwidth 100 --InternetChargeType PayByTraffic

# 绑定 EIP 到 NAT
aliyun vpc AssociateEipAddress --AllocationId <eip-id> \
  --InstanceId <nat-id> --InstanceType Nat

# 创建 SNAT 条目（让交换机下所有 ECS 共享出网）
aliyun vpc CreateSnatEntry --RegionId cn-hangzhou \
  --SnatTableId <snat-table-id> \
  --SourceVSwitchId <vsw-id> \
  --SnatIp <eip-ip>
```

---

## 11. 注意事项与踩坑记录

1. **经典网络 SLB vs VPC ECS**：阿里云经典网络 SLB 对 VPC ECS 的支持有限。如果下次购买，务必选择 **VPC 型 SLB**。
2. **安全组规则**：默认安全组只开放 22/3389/ICMP，ERP 所有端口需要手动添加。目前规则授权对象是 `10.0.0.0/8`，如需外网访问改为 `0.0.0.0/0`。
3. **路径冲突**：`hdpos4-web` 和 `pasoreport-web` 在公网地址中都使用了 `/hdpos4-web`，需要确认实际路径规划。
4. **出网问题**：cjs-app 没有公网 IP，必须依赖 NAT 网关 + EIP 才能访问外网。如果后续给 cjs-app 分配公网 IP，可节省 NAT 和 EIP 费用。
5. **按量付费**：所有收费资源均为按量付费，不使用时请及时释放，避免持续计费。
6. **密码**：两台机器 root 密码统一为 `Zn!axPqW&dLV&D7X`，建议首次登录后修改或配置密钥对。

---

*文档版本：v1.0*  
*最后更新：2026-07-10*
