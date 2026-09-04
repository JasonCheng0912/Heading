 # CRM 阿里云资源清单

> **更新时间**: 2026-07-16  
> **区域**: 华东1 (杭州) `cn-hangzhou`  
> **可用区**: K 区 (`cn-hangzhou-k`)

---

## 一、网络基础

| 资源 | ID | 配置 |
|------|-----|------|
| VPC | `vpc-bp1dqlii6entic6sm7f3c` | 专有网络 |
| vSwitch | `vsw-bp1bcru70p1s7lyv2bhuy` | `cjs`, K 区, 10.0.0.0/24, 可用 IP 248 |

---

## 二、ECS 实例

### cjs-crm-ops (运维机)

| 属性 | 值 |
|------|-----|
| 实例 ID | `i-bp1hl5v1njrjww44trnh` |
| 实例名 | `cjs-crm-ops` |
| 规格 | `ecs.u1-c1m8.large` |
| 规格族 | ecs.u1 |
| CPU / 内存 | 2 vCPU (1Core × 2Thread) / 16 GiB |
| 状态 | Running |
| 创建时间 | 2026-07-16 02:48 UTC |
| 计费方式 | **按量付费 (Spot)** |
| Spot 策略 | SpotWithPriceLimit (最高 0.5 元/时) |
| 中断行为 | Terminate |
| 内网 IP | **10.0.0.1** |
| 公网 IP | 无 |
| 主机名 | `iZbp1hl5v1njrjww44trnhZ` |
| 安全组 | `crm-sg` (`sg-bp19ghu0kcm7p53xpzzw`) |
| 网卡 MAC | `00:16:3e:49:54:5f` |
| ENI ID | `eni-bp1hl5v1njrjww44mug3` |
| root 密码 | `Zn!axPqW&dLV&D7X` |

#### 磁盘

| 设备 | 类型 | 大小 | 性能 | ID |
|------|------|------|------|-----|
| /dev/xvda (系统盘) | ESSD Entry | 40 GiB | 2120 IOPS / 106 MBps | `d-bp1hl5v1njrjww46apyu` |
| /dev/xvdb (数据盘) | ESSD PL0 | 60 GiB | 2520 IOPS / 115 MBps | `d-bp1hl5v1njrjww46apyv` |

#### 镜像

| 属性 | 值 |
|------|-----|
| 镜像 ID | `rockylinux_9_7_x64_20G_alibase_20260525.vhd` |
| OS | Rocky Linux 9.7 64位 |

#### 购买 CLI 命令

```bash
# cjs-crm.md-ops (Spot 实例)
aliyun ecs RunInstances \
  --RegionId cn-hangzhou \
  --ZoneId cn-hangzhou-k \
  --InstanceName cjs-crm.md-ops \
  --InstanceType ecs.u1-c1m8.large \
  --ImageId rockylinux_9_7_x64_20G_alibase_20260525.vhd \
  --VSwitchId vsw-bp1bcru70p1s7lyv2bhuy \
  --SecurityGroupId sg-bp19ghu0kcm7p53xpzzw \
  --InstanceChargeType PostPaid \
  --SpotStrategy SpotWithPriceLimit \
  --SpotPriceLimit 0.5 \
  --SystemDisk.Category cloud_essd_entry \
  --SystemDisk.Size 40 \
  --DataDisk.1.Category cloud_essd \
  --DataDisk.1.Size 60 \
  --DataDisk.1.PerformanceLevel PL0 \
  --Amount 1
```

---

### cjs-crm-app (应用机)

| 属性 | 值 |
|------|-----|
| 实例 ID | `i-bp19clvjmvlahvfh0gn8` |
| 实例名 | `cjs-crm-app` |
| 规格 | `ecs.e-c1m4.xlarge` |
| 规格族 | ecs.e |
| CPU / 内存 | 4 vCPU (2Core × 2Thread) / 16 GiB |
| 状态 | Running |
| 创建时间 | 2026-07-16 02:48 UTC |
| 计费方式 | **按量付费** (PostPaid) |
| Spot 策略 | NoSpot (普通按量, 不会被释放) |
| 内网 IP | **10.0.0.2** |
| 公网 IP | 无 |
| 主机名 | `iZbp19clvjmvlahvfh0gn8Z` |
| 安全组 | `crm-sg` (`sg-bp19ghu0kcm7p53xpzzw`) |
| 网卡 MAC | `00:16:3e:49:53:f0` |
| ENI ID | `eni-bp19clvjmvlahvfhkktf` |
| root 密码 | `Zn!axPqW&dLV&D7X` |

#### 磁盘

| 设备 | 类型 | 大小 | 性能 | ID |
|------|------|------|------|-----|
| /dev/xvda (系统盘) | ESSD Entry | 40 GiB | 2120 IOPS / 106 MBps | `d-bp19clvjmvlahvfbpfoz` |
| /dev/xvdb (数据盘) | ESSD PL0 | 100 GiB | 3000 IOPS / 125 MBps | `d-bp19clvjmvlahvfbpfp0` |

#### 镜像

| 属性 | 值 |
|------|-----|
| 镜像 ID | `rockylinux_9_7_x64_20G_alibase_20260525.vhd` |
| OS | Rocky Linux 9.7 64位 |

#### 购买 CLI 命令

```bash
# cjs-crm.md-app (普通按量, 不会被释放)
aliyun ecs RunInstances \
  --RegionId cn-hangzhou \
  --ZoneId cn-hangzhou-k \
  --InstanceName cjs-crm.md-app \
  --InstanceType ecs.e-c1m4.xlarge \
  --ImageId rockylinux_9_7_x64_20G_alibase_20260525.vhd \
  --VSwitchId vsw-bp1bcru70p1s7lyv2bhuy \
  --SecurityGroupId sg-bp19ghu0kcm7p53xpzzw \
  --InstanceChargeType PostPaid \
  --SystemDisk.Category cloud_essd_entry \
  --SystemDisk.Size 40 \
  --DataDisk.1.Category cloud_essd \
  --DataDisk.1.Size 100 \
  --DataDisk.1.PerformanceLevel PL0 \
  --Amount 1
```

---

## 三、SLB 负载均衡

| 属性 | 值 |
|------|-----|
| 名称 | `auto_named_slb` |
| ID | `lb-bp1hbpo8u1ubaa933lke8` |
| 公网 IP | **121.199.38.151** |
| 类型 | 经典网络 (classic), 弹性 LCU |
| 带宽 | 5120 Mbps |
| 计费 | PayByCLCU / PayByTraffic |
| 主备可用区 | cn-hangzhou-k / cn-hangzhou-j |

### 监听列表

| 端口 | 协议 | 描述 | 后端端口 | VServerGroup | 后端服务器 |
|------|------|------|----------|-------------|-----------|
| 81 | HTTP | crm-openresty-int | 80 | `app` (`rsp-bp1zs23nogd4n`) | 10.0.0.2:80 |
| 2222 | TCP | SSH-ops | 22 | `ops-ssh` (`rsp-bp1em3kqlwdef`) | 10.0.0.1:22 |
| 2223 | TCP | SSH-app | 22 | `ssh-app` (`rsp-bp1eddys6e1uj`) | 10.0.0.2:22 |
| 8888 | HTTP | Jenkins | 8080 | `jenkins` (`rsp-bp1k05idj68d8`) | 10.0.0.1:8080 |
| 38080 | TCP | license | 8080 | `license` (`rsp-bp1g6yghx5b5y`) | 10.0.0.2:8080 |
| 38889 | HTTP | ops-assistant | 8889 | `ops-assistant` (`rsp-bp1uvgbnvhftx`) | 10.0.0.2:8889 |

### VServerGroup 详情

| VServerGroup 名 | ID | ServerCount | 备注 |
|----------------|-----|:---:|------|
| ops-app | `rsp-bp1hm289edxcu` | 2 | 10.0.0.1:80 + 10.0.0.2:80 (未关联监听) |
| ops-ssh | `rsp-bp1em3kqlwdef` | 1 | 10.0.0.1:22 → 端口 2222 |
| ssh-app | `rsp-bp1eddys6e1uj` | 1 | 10.0.0.2:22 → 端口 2223 |
| jenkins | `rsp-bp1k05idj68d8` | 1 | 10.0.0.1:8080 → 端口 8888 |
| license | `rsp-bp1g6yghx5b5y` | 1 | 10.0.0.2:8080 → 端口 38080 |
| ops-assistant | `rsp-bp1uvgbnvhftx` | 1 | 10.0.0.2:8889 → 端口 38889 |
| app | `rsp-bp1zs23nogd4n` | 1 | 10.0.0.2:80 → 端口 81 |

---

## 四、NAT 网关 + EIP

### NAT 网关

| 属性 | 值 |
|------|-----|
| 名称 | `cjs-nat` |
| ID | `ngw-bp19i8onrpgb7eybkt6gd` |
| 说明 | `CRM出网NAT` |
| 类型 | 增强型 (Enhanced) |
| 计费 | PayByLcu (PostPaid) |
| 内网 IP | 10.0.0.150 |
| 高可用 | CrossAZ 跨可用区 |

### EIP

| 属性 | 值 |
|------|-----|
| 名称 | `cjs-eip` |
| 说明 | `CRM出网EIP` |
| 公网 IP | **121.40.243.175** |
| ID | `eip-bp1vw851xmoe35y9zcyfv` |
| 带宽 | 100 Mbps, PayByTraffic |
| ISP | BGP |
| 绑定到 | NAT 网关 |

### SNAT 条目

| 条目 ID | 源网段 | SNAT IP | 状态 |
|---------|--------|---------|------|
| `snat-bp190gvmh3v9ny2wv1xl7` | 10.0.0.0/24 | 121.40.243.175 | Available |

---

## 五、RDS 数据库

| 属性 | 值 |
|------|-----|
| 实例 ID | `rm-bp1o6cez78cpf88m3` |
| 引擎 | MySQL 8.0 |
| 规格 | `rds.mysql.s2.large` (2 vCPU / 4 GiB) |
| 存储 | 50 GB, local_ssd |
| 架构 | 高可用 (主备) |
| 主可用区 | cn-hangzhou-k |
| 备可用区 | cn-hangzhou-f |
| 连接地址 | `rm-bp1o6cez78cpf88m3.mysql.rds.aliyuncs.com:3306` |
| 网络 | VPC 内网 |
| **计费方式** | **包年包月 (Prepaid)** |
| **到期时间** | **2026-08-03 16:00 UTC** |
| 白名单 IP | 10.0.0.1, 10.0.0.2 |
| 最大连接数 | 1200 |
| 最大 IOPS | 4000 |

### 数据库 & 账号

| 数据库 | 字符集 | 账号 | 权限 | 密码 |
|--------|--------|------|------|------|
| phoenix | utf8 | phoenix | ReadWrite | `h*1PSqv8fyzoe` |

> 另有一个 DMS 自动管理账号 `dms_user_83e27c7`，勿删。

---

## 六、安全组

### crm-sg (`sg-bp19ghu0kcm7p53xpzzw`)

两台 ECS 均绑定此安全组，入方向规则：

| 端口 | 协议 | 来源 | 说明 |
|------|------|------|------|
| 22 | TCP | 0.0.0.0/0 | SSH |
| 80 | TCP | 0.0.0.0/0 | Nginx-HTTP |
| 81 | TCP | 0.0.0.0/0 | Nginx-81 |
| 82 | TCP | 0.0.0.0/0 | Nginx-82 |
| 8080 | TCP | 0.0.0.0/0 | 许可证端口 |
| 8082 | TCP | 10.0.0.0/8 | Eureka |
| 8888 | TCP | 0.0.0.0/0 | Jenkins |
| 8889 | TCP | 0.0.0.0/0 | 运维助手 |
 
---

## 七、快速访问

| 用途 | 地址 |
|------|------|
| SSH Ops | `ssh -p 2222 root@121.199.38.151` |
| SSH App | `ssh -p 2223 root@121.199.38.151` |
| OpenResty | `http://121.199.38.151:81` |
| Jenkins | `http://121.199.38.151:8888` |
| App 服务 (38080) | `http://121.199.38.151:38080` |
| 运维助手 | `http://121.199.38.151:38889` |
| MySQL (phoenix) | `rm-bp1o6cez78cpf88m3.mysql.rds.aliyuncs.com:3306` |
| NAT 出网 IP | `121.40.243.175` |

---

## 八、新机器上架检查清单

当 ECS 被释放需要重建时，按以下顺序操作：

1. **创建 ECS** → 执行第二节中的 `aliyun ecs RunInstances` 命令
2. **确认实例内网 IP** → 若 IP 变了，后续 SLB 后端地址需同步更新
3. **SLB 后端注册** → 将新实例加入对应 VServerGroup：
   - App 机 (10.0.0.2): `ssh-app`(:22), `app`(:80), `ops-app`(:80), `license`(:8080), `ops-assistant`(:8889)
   - Ops 机 (10.0.0.1): `ops-ssh`(:22), `ops-app`(:80), `jenkins`(:8080)
4. **NAT / SNAT** → 若 SNAT 条目覆盖当前子网，无需操作；否则补充 SNAT
5. **RDS 白名单** → 更新白名单加入新 ECS 内网 IP
6. **验证** → 逐个测试第七节的访问地址
