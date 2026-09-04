# CRM 系统整体部署架构

## 一、架构总览

CRM（Phoenix CRM）系统部署于阿里云（杭州地域），整体采用**单 VPC 私有网络 + 公网 SLB 接入**的架构。所有业务组件以 Docker 容器化方式运行在 ECS 云服务器上，通过 OpenResty 反向代理网关统一对外提供服务，由 Jenkins 自动化流水线驱动 CI/CD 全流程。底层数据采用 **MySQL (RDS)** + **Redis** + **Elasticsearch** 多存储引擎组合，服务间通过 **Eureka** 注册发现 + **RabbitMQ** 消息队列实现微服务通信。

### 架构全景图

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          公网（Internet）                                 │
│                                                                         │
│  门店POS / 办公PC / 移动端 ──→ 域名(example.hdcrm.com) ──→ HTTP 80端口    │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                    阿里云 公网SLB（Server Load Balancer）                  │
│                  对外暴露81端口(HTTP)，绑定弹性公网IP(EIP)                  │
│                    SSH管理端口：2222(运维机) / 2223(应用机)                │
│                    Jenkins：8888 / License：38080 / 运维助手：38889       │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │ 内网转发 :81→应用机:80
                                    ▼
┌═════════════════════════════════════════════════════════════════════════┐
║                      VPC 专有网络（私有网络空间 10.0.0.0/24）              ║
║                                                                         ║
║  ┌──────────────────────────────────────────────────────────────────┐  ║
║  │                   安全组 crm-sg（防火墙规则）                        │  ║
║  │      入站：公网SLB → 80/81/82端口 / SSH → 22端口                   │  ║
║  │      入站：Jenkins → 8080 / License → 8080 / 运维助手 → 8889       │  ║
║  │      出站：NAT网关 → Harbor镜像仓库 / Git配置仓库 / 外部API         │  ║
║  └──────────────────────────────────────────────────────────────────┘  ║
║                                                                         ║
║  ┌──────────────────────────────────────┐ ┌────────────────────────┐  ║
║  │     OPS 运维机 ECS (cjs-crm-ops)      │ │  应用机 ECS (cjs-crm-app)│  ║
║  │     内网IP: 10.0.0.1                 │ │  内网IP: 10.0.0.2       │  ║
║  │     Rocky Linux 9.7 / 2C16G          │ │  Rocky Linux 9.7 / 4C16G│  ║
║  │                                      │ │                          │  ║
║  │  ┌─────────────────────────────┐     │ │  ┌─────────────────────┐ │  ║
║  │  │ Docker: Jenkins Server(:8080)│    │ │  │ OpenResty 网关(:80)  │ │  ║
║  │  │         运维助手(:8889)      │    │ │  │ 反向代理 + 路由分发  │ │  ║
║  │  │         License(:38080)     │    │ │  └──────┬──────┬───────┘ │  ║
║  │  └─────────────────────────────┘     │ │         │      │         │  ║
║  └──────────────────────────────────────┘ │  ┌──────▼──┐ ┌─▼──────┐ │  ║
║                                            │  │ Eureka   │ │Gateway │ │  ║
║  ┌──────────────────────────────────────┐  │  │ :8082    │ │ :82    │ │  ║
║  │        阿里云 RDS MySQL 8.0           │  │  │ 服务注册 │ │API网关 │ │  ║
║  │   rm-bp1o6cez78cpf88m3.mysql.rds     │  │  └─────────┘ └────────┘ │  ║
║  │   用户: phoenix / 库: phoenix          │  │                          │  ║
║  │   2C4G / 50GB SSD / 高可用主备         │  │  ┌─────────────────────┐ │  ║
║  └──────────────────────────────────────┘  │  │  Docker 微服务容器     │ │  ║
║                                            │  │                      │ │  ║
║  ┌──────────────────────────────────────┐  │  │  dtask        :8003  │ │  ║
║  │        NAT 网关 (cjs-nat)             │  │  │  phoenix-crm-web:8025│ │  ║
║  │  EIP: 121.40.243.175 / 100Mbps       │  │  │  phoenix-service-web │ │  ║
║  │  SNAT 覆盖 10.0.0.0/24               │  │  │               :8062  │ │  ║
║  └──────────────────────────────────────┘  │  │  phoenix-web-ui:8023 │ │  ║
║                                            │  └─────────────────────┘ │  ║
║                                            │                          │  ║
║                                            │  ┌─────────────────────┐ │  ║
║                                            │  │  Docker 中间件容器     │ │  ║
║                                            │  │  Redis(:6379)        │ │  ║
║                                            │  │  Elasticsearch       │ │  ║
║                                            │  │    (:9200/:9300)     │ │  ║
║                                            │  │  RabbitMQ(:5672)     │ │  ║
║                                            │  │  Zookeeper(:2181)    │ │  ║
║                                            │  │  OSS/MinIO(:8081)    │ │  ║
║                                            │  │  License(:38080)     │ │  ║
║                                            │  └─────────────────────┘ │  ║
║                                            └──────────────────────────┘  ║
║                                                                         ║
╚═════════════════════════════════════════════════════════════════════════╝
```

**架构说明**：最外层是公网终端，门店POS、办公PC通过域名访问系统。流量先到达阿里云公网 SLB（负载均衡器），SLB 监听 81 端口（HTTP）将业务请求通过内网转发到 VPC 内的应用服务器 `10.0.0.2:80`。进入 VPC 后，安全组 crm-sg 作为第一道防火墙，仅放行指定端口。所有业务服务部署在单台 ECS（`cjs-crm-app`，4C16G）上，采用 Docker 容器化部署。入口由 OpenResty 反向代理网关统一承接，将请求按 URL 路径规则分发到对应的后端微服务容器。CRM 采用 Spring Cloud 微服务体系，Eureka 负责服务注册发现，gateway-service 作为 API 网关统一管理内部服务间调用，RabbitMQ 处理异步消息。数据持久化由阿里云 RDS MySQL 8.0（高可用主备）提供。独立运维机 `cjs-crm-ops` 运行 Jenkins、运维助手和许可证服务，通过 SSH 免密登录远程管控应用服务器。NAT 网关绑定 EIP `121.40.243.175` 提供出网能力，用于访问 Harbor 镜像仓库和 Git 配置仓库。

---

## 二、阿里云基础设施层

### 2.1 VPC 专有网络

整个 CRM 系统部署在阿里云 **VPC（Virtual Private Cloud，专有网络）** 中，这是一个逻辑隔离的私有网络环境。

- **作用**：将 CRM 所有云资源（ECS、RDS、SLB、NAT）纳入一个封闭的私有网络空间，与公网及其他租户网络完全隔离
- **IP 段**：`10.0.0.0/24`，可用 IP 范围 10.0.0.1 ~ 10.0.0.254
- **私网互通**：同一 VPC 内的 ECS、RDS 通过内网 IP 直连，延迟低、无带宽费用

### 2.2 公网 SLB（Server Load Balancer）

公网 SLB 是 CRM 系统的**唯一业务入口**，对外暴露服务。

| 属性 | 配置说明 |
|------|----------|
| **实例名称** | `auto_named_slb` |
| **公网 IP** | `121.199.38.151`（绑定 EIP） |
| **带宽** | 5 Gbps |
| **监听端口** | 81（HTTP 业务入口） / 2222（运维机 SSH） / 2223（应用机 SSH） |
| **管理端口** | 8888（Jenkins） / 38080（License） / 38889（运维助手） |
| **转发目标** | VPC 内应用机 `10.0.0.2:80`（Nginx） |
| **SSL 证书** | 挂载在 SLB 层或 Nginx 层，做 HTTPS → HTTP 转发 |
| **健康检查** | 定期探测后端 OpenResty 80 端口可达性 |

**SLB 端口映射详情**：

| SLB 监听端口 | 协议 | 后端目标 | 用途 |
|-------------|------|----------|------|
| 81 | HTTP | 10.0.0.2:80 | CRM 业务访问入口 |
| 2222 | TCP | 10.0.0.1:22 | 运维机 SSH 远程管理 |
| 2223 | TCP | 10.0.0.2:22 | 应用机 SSH 远程管理 |
| 8888 | HTTP | 10.0.0.1:8080 | Jenkins 管理页面 |
| 38080 | TCP | License Server | 许可证服务 |
| 38889 | HTTP | 10.0.0.1:8889 | 运维助手 |

**流量路径**：外部用户访问域名 → DNS 解析到 SLB 公网 IP → SLB 将请求转发至 VPC 内应用服务器 `10.0.0.2:80`。

### 2.3 安全组（Security Group）

安全组 `crm-sg` 充当虚拟防火墙，控制 ECS 实例的入站/出站流量：

| 方向 | 规则 | 来源/目标 | 端口 | 用途 |
|------|------|-----------|------|------|
| 入站 | 允许 | 公网 SLB | 80/81/82 | 业务访问 |
| 入站 | 允许 | OPS/Jenkins | 22 | SSH 运维 |
| 入站 | 允许 | 特定 IP | 8080 | Jenkins Web UI |
| 入站 | 允许 | 特定 IP | 8888/8889 | 运维助手管理 |
| 入站 | 允许 | VPC 内网 | 9200/9300 | ES 集群通信 |
| 入站 | 允许 | VPC 内网 | 5672/15672 | RabbitMQ 通信 |
| 入站 | 允许 | VPC 内网 | 6379 | Redis 访问 |
| 入站 | 允许 | VPC 内网 | 2181 | Zookeeper |
| 出站 | 允许 | 0.0.0.0/0 | 443/80 | 访问 Harbor、Git、外部 API |
| 入站 | 拒绝 | 0.0.0.0/0 | 全部 | 默认拒绝其他所有入站 |

### 2.4 NAT 网关

VPC 内的 ECS 默认只有私网 IP，无法直接访问外网。通过 **NAT 网关** 实现：

| 属性 | 值 |
|------|-----|
| **实例名称** | `cjs-nat` |
| **EIP** | `121.40.243.175` |
| **带宽** | 100 Mbps |
| **SNAT** | 覆盖 `10.0.0.0/24` 网段 |

- **SNAT（源地址转换）**：让应用服务器能访问外网的 Harbor 镜像仓库、Git 代码仓库
- **安全性**：ECS 不直接暴露公网 IP，出向流量统一经 NAT 网关代理，对外隐藏真实内网地址

### 2.5 RDS MySQL 数据库

| 属性 | 值 |
|------|-----|
| **实例地址** | `rm-bp1o6cez78cpf88m3.mysql.rds.aliyuncs.com:3306` |
| **版本** | MySQL 8.0 |
| **规格** | 2 vCPU / 4 GB |
| **存储** | 50 GB SSD |
| **架构** | 高可用主备版 |
| **数据库名** | `phoenix` |
| **用户** | `phoenix` |

### 2.6 ECS 云服务器清单

| 主机名 | 实例名 | 内网 IP | 规格 | 系统 | 付费 | 用途 |
|--------|--------|---------|------|------|------|------|
| `middle0` | `cjs-crm-ops` | `10.0.0.1` | 2C16G | Rocky Linux 9.7 | Spot 按量 | 运维机（Jenkins + License + 运维助手） |
| `app0` | `cjs-crm-app` | `10.0.0.2` | 4C16G | Rocky Linux 9.7 | 普通按量 | 应用机（OpenResty + 所有微服务 + 中间件） |

---

## 三、流量接入层

### 3.1 公网流量入口：SLB → OpenResty

外部请求从用户浏览器到达后端微服务，需依次经过 DNS 解析、SLB 负载均衡、安全组、OpenResty 网关四层转发：

```
用户浏览器
  │ ① DNS 解析域名 → SLB 公网 IP (121.199.38.151)
  │ ② HTTPS/HTTP 请求
  ▼
公网 SLB（监听 81/443）
  │ ③ 健康检查确认后端 ECS 可用
  │ ④ 转发至应用服务器 10.0.0.2:80（Nginx）
  ▼
安全组 crm-sg（虚拟防火墙）
  │ ⑤ 仅允许 SLB 来源的 80 端口流量入站
  ▼
OpenResty/Nginx 网关（10.0.0.2:80）
  │ ⑥ URL 路径匹配 → proxy_pass
  │ ⑦ upstream 解析后端宿主机端口
  ▼
Docker 宿主机端口（如 8025）
  → Docker 端口映射 → 容器内部业务端口
```

**SLB 层核心职责**：
- **SSL 卸载**：HTTPS 证书可挂载在 SLB 层或 Nginx 层，解密后以 HTTP 转发至后端
- **健康检查**：定时探测后端 Nginx 80 端口，异常节点自动剔除
- **多端口管理**：除业务 81 端口外，还管理 SSH、Jenkins、License 等多条通道

**安全组入站规则**：仅开放指定端口，其余默认拒绝，形成 VPC 内部的第一道防线。

### 3.2 Nginx 网关核心机制

Nginx 作为 CRM 系统的反向代理网关，兼具 **路由分发 + 请求过滤** 功能，通过 `/hdapp/sslkey/` 下的证书文件提供 HTTPS 支持。

**路由分发规则**（按 URL 路径匹配不同微服务）：

```nginx
# CRM 前端界面
location /crm/ { proxy_pass http://10.0.0.2:8025; }

# 后台服务 API
location /service/ { proxy_pass http://10.0.0.2:8062; }

# API 网关
location /api/ { proxy_pass http://10.0.0.2:82; }

# Web UI
location /web-ui/ { proxy_pass http://10.0.0.2:8023; }
```

### 3.3 端口映射机制

宿主机端口与 Docker 容器内部端口的关系：

```
公网请求
  → SLB:81
    → Nginx:80
      → upstream → 10.0.0.2:8025（宿主机端口）
        → Docker端口映射 → 容器内部:8080（业务端口）
```

**端口命名规则**：
- CRM 组件端口：`8xxx` 段（如 8025、8062）
- 中间件端口：标准端口（6379、9200、5672、2181 等）
- SSL 端口（如有）：宿主机端口 + 1（如 8025 → SSL 8026）

每个微服务容器在启动时通过 Docker 的 `-p` 参数将内部业务端口映射到宿主机的唯一端口，Nginx 的 `upstream` 配置中指向这些宿主机端口，实现请求的转发。

---

## 四、应用服务层

### 4.1 微服务组件清单

以下为当前已启用的 CRM 微服务组件（集成测试环境），宿主机 IP 为 `10.0.0.2`：

| 组件名 | 用途 | 宿主机端口 | 容器内部端口 | 技术栈 |
|--------|------|-----------|-------------|--------|
| `gateway-service` | API 网关，统一入口路由与服务鉴权 | 82 | 8080 | Spring Cloud Gateway |
| `phoenix-crm-web` | CRM 核心前端界面（会员管理、积分、优惠券等） | 8025 | 8080 | Spring Boot + Web |
| `phoenix-service-web` | 后台服务管理（ERP对接、分账、推送、支付等） | 8062 | 8080 | Spring Boot + Web |
| `phoenix-web-ui` | Web UI 界面 | 8023 | 8080 | Spring Boot + Web |
| `dtask` | 分布式定时任务调度 | 8003 | 8080 | Spring Boot |

**CRM 扩展组件清单**（按需启用，用于对接第三方系统）：

| 组件名 | 用途 | 说明 |
|--------|------|------|
| `hdpos4-dist` | ERP 交易前端 | 与 ERP 系统对接 |
| `jposbo` | POS 后台管理 | 门店后台数据管理 |
| `pasoreport-web` | 报表系统 | 业务报表展示 |
| `spms-hdpos-web` | 供应商门户 | 供应商协同平台 |
| `h6-crm-service` | H6 CRM 服务 | CRM 核心业务服务（会员等级、积分计算等） |
| `panther-dts-server` | 数据交换服务 | 多系统数据同步 |
| `panther-taskweb` | 数据交换 Web | 数据交换任务管理 |
| `up-connector-service` | 通用连接器 | 第三方系统对接 |
| `gem-service` | 促销引擎 | 优惠券/促销计算 |
| `init-tool` | 初始化工具 | 系统初始化/数据迁移 |
| `h6-openapi2-service` | 开放接口服务 | 对外 API 开放平台 |
| `openapi-doc-service` | 接口文档服务 | Swagger/API 文档 |
| `hdpos6-notice-service` | 通知服务 | 短信/消息/站内信推送 |
| `sos-h6-transfer-service` | 数据桥接服务 | ERP ↔ CRM 数据同步 |
| `zl-portal-sync` | 门户同步 | 门户数据同步 |
| `card-server-proxy-service` | 卡服务代理 | 实体卡/电子卡管理 |
| `mkh-mas-transfer-server` | MAS 传输服务 | 中台数据传输 |

### 4.2 中间件清单

| 中间件 | 端口 | 内存配置 | 用途 |
|--------|------|----------|------|
| **Redis** | 6379 | 2 GB | 缓存、会话存储、分布式锁 |
| **Elasticsearch** | 9200/9300 | 2 GB | 日志存储与搜索、会员数据检索 |
| **RabbitMQ** | 5672/15672 | — | 异步消息队列、事件驱动 |
| **Zookeeper** | 2181/10001 | — | 分布式协调（任务调度、配置管理） |
| **OSS/MinIO** | 8081 | — | 对象存储（文件/图片上传） |
| **Eureka** | 8082/8600 | — | 服务注册与发现 |
| **Nginx** | 80/443 | — | 反向代理 + SSL 终止 |
| **License Server** | 38080 | — | 软件许可证验证 |

**RabbitMQ 完整端口**：

| 端口 | 用途 |
|------|------|
| 4369 | EPMD（Erlang 端口映射守护进程） |
| 5671 | AMQP over TLS |
| 5672 | AMQP（默认消息协议端口） |
| 15671 | Management over TLS |
| 15672 | Management（Web 管理控制台） |
| 15692 | Prometheus 监控指标 |
| 25672 | 集群间通信 |

### 4.3 Docker 容器运行参数

所有 Docker 容器通过 `phoenix.yaml` 定义统一的 JVM 参数和 Docker 参数：

| 参数类别 | 默认值 | 说明 |
|----------|--------|------|
| **JVM 内存** | `-Xms511m -Xmx1023m` | 默认堆内存，部分服务有独立配置 |
| **JVM 端口** | `8600` 等 | JMX 远程监控端口（宿主机端口 + 1000） |
| **日志目录** | `/hdapp/{container}/logs` | 挂载到宿主机目录持久化 |
| **配置目录** | `/hdapp/{container}/conf` | 配置文件挂载 |

---

## 五、数据存储层

### 5.1 数据访问架构总览

CRM 系统采用 **MySQL 为主 + Redis 缓存 + Elasticsearch 搜索** 的多存储引擎架构：

```
┌──────────────────────────────────────────────────────────────┐
│                    应用服务层（Docker 容器）                    │
│  phoenix-crm-web  gateway-service  dtask  phoenix-service-web│
│        │               │              │          │            │
└────────┼───────────────┼──────────────┼──────────┼────────────┘
         │               │              │          │
         ▼               ▼              ▼          ▼
┌──────────────────────────────────────────────────────────────┐
│                    数据访问层                                  │
│  ┌─────────────┐  ┌─────────────┐  ┌──────────────────────┐  │
│  │  MySQL JDBC │  │ Redis 客户端 │  │ ES REST Client       │  │
│  │  (phoenix库)│  │  (Jedis)    │  │ RabbitMQ Client      │  │
│  └──────┬──────┘  └──────┬──────┘  └──────────┬───────────┘  │
└─────────┼────────────────┼────────────────────┼──────────────┘
          │                │                    │
          ▼                ▼                    ▼
┌─────────────────┐ ┌──────────┐ ┌────────────────────────────┐
│  RDS MySQL 8.0  │ │  Redis   │ │  Elasticsearch / RabbitMQ  │
│  :3306          │ │  :6379   │ │  :9200 / :5672             │
│  核心业务数据    │ │ 缓存/会话 │ │  日志搜索 / 异步消息       │
└─────────────────┘ └──────────┘ └────────────────────────────┘
```

### 5.2 MySQL（核心业务数据，RDS）

CRM 核心业务数据（会员、积分、优惠券、储值、交易等）全部存储在阿里云 RDS MySQL 8.0 中。

| 属性 | 值 |
|------|-----|
| **服务地址** | `rm-bp1o6cez78cpf88m3.mysql.rds.aliyuncs.com:3306` |
| **版本** | MySQL 8.0 |
| **数据库名** | `phoenix` |
| **用户** | `phoenix` |
| **架构** | 高可用主备版 |
| **驱动** | MySQL JDBC Connector/J（容器内自带） |
| **字符集** | UTF8MB4 |

**核心业务表（部分）**：

| 表名 | 用途 |
|------|------|
| `phx_member` | 会员主表（手机号、姓名、等级、生日、注册时间等） |
| `phx_member_trade_org_daily_report` | 会员交易日报（金额、数量等） |
| `phx_member_trade_org_month_report` | 会员交易月报 |
| `phx_trade_member_report` | 交易会员报表 |
| `phx_trade` | 交易记录（sourceTradeIdId、命名空间等） |

**数据库初始化前置操作**：

| 操作 | 说明 |
|------|------|
| 设置 `sql_mode` | 兼容旧版 SQL 语法 |
| 执行 dtask MySQL 8.0 脚本 | 定时任务表兼容新版 MySQL |
| 储值账户初始化 | 预付费账户相关表数据初始化 |
| `log_bin_trust_function_creators=1` | 允许创建存储函数（binlog 开启时） |

### 5.3 Redis 缓存

| 属性 | 值 |
|------|-----|
| **地址** | `10.0.0.2:6379` |
| **内存** | 2 GB |
| **部署方式** | Docker 容器（单机） |
| **客户端** | Jedis / Lettuce（各微服务应用层集成） |

**Redis 用途矩阵**：

| 用途 | 说明 | 典型场景 |
|------|------|----------|
| **会话管理** | 存储用户登录 Session | 后台用户登录态 |
| **分布式锁** | 基于 Redis 的互斥锁 | 会员积分扣减防重 |
| **高频缓存** | 热点数据缓存 | 会员等级规则、优惠券模板 |
| **计数器** | 原子自增/自减 | 会员号生成、券码流水 |
| **队列缓冲** | 轻量级消息缓冲 | 即时通知排队 |

### 5.4 Elasticsearch（搜索与日志）

| 属性 | 值 |
|------|-----|
| **地址** | `10.0.0.2:9200`（HTTP）/ `9300`（Transport） |
| **内存** | 2 GB |
| **部署方式** | Docker 容器（单节点） |
| **用途** | 日志搜索与分析、会员数据全文检索 |

**ES 应用场景**：
- **ELK 日志收集**：各微服务日志 → Filebeat → Logstash → Elasticsearch → Kibana 可视化
- **日志巡检**：`elasticsearch_dailycheck.py` 每日自动检查 ES 中 ERROR 级别日志，按 traceId 分组，过滤白名单后生成报告
- **会员检索**：支持对会员信息进行模糊搜索和复杂条件查询

### 5.5 RabbitMQ（消息队列）

| 属性 | 值 |
|------|-----|
| **地址** | `10.0.0.2:5672` |
| **管理控制台** | `http://10.0.0.2:15672` |
| **用户** | `hdmq` |
| **部署方式** | Docker 容器 |

**消息队列用途**：
- **异步事件处理**：积分变动通知、优惠券发放、等级升降级
- **数据同步**：CRM ↔ ERP 双向数据同步
- **第三方对接**：微信卡包、抖音、美团等第三方平台事件推送
- **定时任务调度**：dtask 分布式任务的消息分发

### 5.6 OSS 对象存储（MinIO）

| 属性 | 值 |
|------|-----|
| **端口** | 8081 |
| **部署方式** | Docker 容器（MinIO 私有化部署） |
| **元数据库** | 共享 MySQL `phoenix` 库 |
| **用途** | 会员头像、活动图片、导出文件等 |

### 5.7 数据持久化与存储卷

Docker 容器的数据持久化通过 **宿主机目录挂载** 实现：

| 挂载路径 | 容器内路径 | 用途 |
|----------|-----------|------|
| `/hdapp/{containerid}/data` | 数据目录 | 数据库数据文件 |
| `/hdapp/{containerid}/logs` | `/logs` | 应用日志文件 |
| `/hdapp/{containerid}/conf` | `/conf` | 配置文件 |
| `/hdapp/sslkey/` | SSL 证书目录 | Nginx HTTPS 证书 |

### 5.8 数据迁移（PhoenixMigrationApplication）

CRM 系统支持从旧系统迁移数据，通过 Spring Boot 应用 `PhoenixMigrationApplication` 实现：

| 配置项 | 说明 |
|--------|------|
| **源库** | `member` 库 |
| **目标库** | `phoenix_int`（集成测试）/ `phoenix`（生产） |
| **迁移表** | `PHX_MEMBER`（会员） |
| **迁移模块** | member、points、identity、growthvalue、coupon、prepay-account-transaction |
| **线程数** | 各模块 6 线程并发 |
| **分页大小** | 5000 ~ 25000 条/页 |
| **会员号规则** | 前缀 `88`，总长度 16 位 |

---

## 六、微服务通信架构

### 6.1 服务注册与发现（Eureka）

CRM 采用 Spring Cloud Netflix Eureka 作为服务注册中心：

```
gateway-service(:82)
      │
      │ 服务发现
      ▼
Eureka Server(:8082)
      │
      ├── 注册: phoenix-crm-web(:8025)
      ├── 注册: phoenix-service-web(:8062)
      ├── 注册: dtask(:8003)
      └── 注册: phoenix-web-ui(:8023)
```

**通信模式**：

| 通信方式 | 使用场景 | 技术实现 |
|----------|----------|----------|
| **REST API** | 同步调用，如查询会员信息 | Feign + Ribbon 负载均衡 |
| **消息队列** | 异步事件，如积分变更通知 | RabbitMQ (AMQP) |
| **服务发现** | 动态定位服务实例 | Eureka Client |

### 6.2 内部服务互访链路（单机部署）

当前环境所有组件部署在同一台 ECS `10.0.0.2` 上：

```
┌──────────────────────────────────────────────────────────┐
│                  ECS 10.0.0.2 (cjs-crm-app)               │
│                                                          │
│   phoenix-crm-web ──→ JDBC ──→ RDS MySQL (内网直连)      │
│   phoenix-service-web ──→ JDBC ──→ RDS MySQL              │
│   dtask ──→ JDBC ──→ RDS MySQL                            │
│                                                          │
│   全部微服务 ──→ Redis :6379（缓存/锁）                    │
│   全部微服务 ──→ RabbitMQ :5672（异步消息）                │
│   全部微服务 ──→ Elasticsearch :9200（日志/搜索）          │
│   dtask ──→ Zookeeper :2181（任务协调）                    │
│                                                          │
│   服务间调用 ──→ gateway-service :82（API 网关）          │
│   gateway ──→ Eureka :8082（服务发现）                     │
│                                                          │
│   ★ 单机部署，内网回环通信，延迟极低                       │
└──────────────────────────────────────────────────────────┘
```

### 6.3 数据库映射

CRM 系统的所有微服务共用同一个 `phoenix` 数据库，其中两个适配器组件有专门的数据映射配置：

| 组件 | 数据库 | 用途 |
|------|--------|------|
| `phoenix-member-adapter-provider` | `phoenix` | 会员核心数据 CRUD |
| `phoenix-crm-adapter-provider` | `phoenix` | CRM 业务数据访问 |

---

## 七、运维管理层（CI/CD 流水线）

### 7.1 OPS 运维机

Jenkins 和运维工具独立部署在一台 OPS ECS 上，不混用业务服务器资源。

| 属性 | 说明 |
|------|------|
| **主机名** | `cjs-crm-ops` |
| **内网 IP** | `10.0.0.1` |
| **部署方式** | Docker 容器运行 Jenkins Server |
| **Jenkins 公网地址** | `http://121.199.38.151:8888` |
| **SSH 访问** | `ssh dnet@121.199.38.151 -p 2222`（通过 SLB） |

**运维机上运行的容器**：

| 容器 | 端口 | 用途 |
|------|------|------|
| Jenkins Server | 8080 | 持续集成/持续部署 |
| License Server (`lickit-server:1.2.6`) | 38080 | 软件许可证管理 |
| 运维助手 (`deployer-installer:1.0.0`) | 8889 | 部署辅助工具 |

### 7.2 核心外部依赖

| 组件 | 地址/方式 | 作用 |
|------|----------|------|
| **Git 配置仓库** | `github.app.hd123.cn`（内部 GitLab） | 存储全部部署配置 |
| **Harbor 镜像仓库** | `harbor.qianfan123.com` 或私有化仓库 | 存储所有 Docker 镜像 |
| **JJB 模板仓库** | `github.app.hd123.cn:10080/phoenix-config/jjb_templates.git` | Jenkins Job Builder 模板（Git 子模块） |

### 7.3 配置仓库结构

CRM 的配置管理采用 **两仓库分离** 模式：

```
┌─────────────────────────────────────────────┐
│           chengjiashuo2/ 仓库                 │
│           (Phoenix 项目部署配置)               │
│                                             │
│  ├── phoenix.yaml                           │
│  │   部署拓扑：主机(middle0/app0)、           │
│  │   SSH凭据、RDS连接、中间件端口/密码          │
│  │   容器定义(id/hostid/port/portssl/tags)     │
│  │                                          │
│  ├── docker_environments.yaml               │
│  │   Docker 环境变量：JWT密钥、ES连接、         │
│  │   微信配置、OSS配置、日志级别、功能开关等       │
│  │                                          │
│  ├── application.yml                        │
│  │   数据迁移配置（源库→目标库、模块、线程）       │
│  │                                          │
│  ├── inventory.py                           │
│  │   Ansible 动态 Inventory 脚本               │
│  │                                          │
│  ├── tests/                                 │
│  │   pytest 配置校验                          │
│  │                                          │
│  └── templates/                             │
│      └── PT-ONLINE/PT-ONLINE.j2             │
│          DDL SQL 模板                        │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│        toolset_chengjiashuo2/ 仓库            │
│        (运维工具集 + 产品线配置)               │
│                                             │
│  ├── settings-dly.yaml                      │
│  │   DLY(鼎力云-会员/CRM) 各环境组件版本清单     │
│  │                                          │
│  ├── settings.yaml / settings-*.yaml        │
│  │   各产品线（BaaS/VSS/MAS/CMS/等）版本清单    │
│  │                                          │
│  ├── bluegreen_deployment-*.yml             │
│  │   蓝绿部署 SLB 切换策略                    │
│  │                                          │
│  ├── elasticsearch_dailycheck.py            │
│  │   ELK 日志每日巡检脚本                     │
│  │                                          │
│  ├── import_export_jenkins_jobs.py          │
│  │   Jenkins Job 批量导入/导出工具             │
│  │                                          │
│  ├── create_jenkins_view.py                │
│  │   Jenkins View 按产品创建                  │
│  │                                          │
│  ├── create_patch_config.py                │
│  │   从 settings 抽取组件版本生成 patch 文件    │
│  │                                          │
│  └── utils.py                              │
│      工具函数库                               │
└─────────────────────────────────────────────┘
```

### 7.4 Jenkins 远程管控流程

```
Jenkins（OPS运维机 10.0.0.1:8080）
  │
  │  ① SSH 免密（dnet@10.0.0.2:22）
  ▼
应用服务器 10.0.0.2
  │
  ├── 服务器初始化 → GLOBLE_Centos_Init
  │    Rocky Linux 9.7 基础环境初始化
  │
  ├── 部署 License → 部署 lickit-server，上传许可证
  │
  ├── 部署数据库 → 验证 RDS MySQL 连接，执行初始化脚本
  │
  ├── 部署中间件 → Redis / ES / RabbitMQ / OSS / Eureka
  │    Docker pull from Harbor → docker run
  │
  ├── 应用前置操作 → 设置 sql_mode / dtask脚本 / 储值账户初始化
  │
  ├── 部署应用 → CRM_deploy_one
  │    读取 phoenix.yaml → docker pull → docker run → 健康检查
  │
  ├── 部署 Nginx → GLOBLE_deploy_nginx
  │    推送 OpenResty 配置 → 重启网关
  │
  └── 网关初始化 → CRM_Gateway (action=update)
       获取 appId / appSecret → 配置到 docker_environments.yaml
```

### 7.5 Jenkins Job 模板化管理

- **Git 子模块**：通过 `.gitmodules` 引入 `jjb_templates` 统一维护 Job 模板
- **View 管理**：`create_jenkins_view.py` 按产品线（CRM/BaaS/VSS 等）创建 Jenkins 视图分组
- **Job 批量操作**：`import_export_jenkins_jobs.py` 支持按产品批量导入/导出，或单个 Job 操作

### 7.6 蓝绿部署（Blue-Green Deployment）

非生产环境如需实现无中断升级，可通过 `bluegreen_deployment-*.yml` 定义组件批次和 SLB 切换策略：

- **蓝绿升级机制**：blue/green 两套容器 tag 标签，先部署新版本到空闲组 → 验证通过 → SLB 切换 → 旧版本下线
- **组件批次**：定义升级的先后顺序，确保依赖关系正确
- **SLB 切换**：按批次逐步把流量从 blue 切到 green

---

## 八、健康检查与监控

### 8.1 健康检查机制

每个部署的微服务均配置健康检查端点，通过 `phoenix.yaml` 定义：

| 应用类型 | 健康检查路径 | 说明 |
|---------|-------------|------|
| **Spring Boot** | `/actuator/health` | 标准 Spring Actuator 端点 |
| **Eureka** | `/actuator/health` | 注册中心自身健康 |
| **Nginx** | `:80` 端口可达性 | SLB 层健康检查 |

**健康检查流程**：
1. 部署后自动执行 HTTP 健康检查探测
2. 超时阈值：180s（可根据服务器性能调整）
3. 失败处理：告警通知 + 回滚

### 8.2 日志收集（ELK）

CRM 系统采用 ELK 堆栈收集和分析日志：

| 组件 | 部署位置 | 用途 |
|------|----------|------|
| **Filebeat** | 应用服务器 | 采集各微服务 `/hdapp/{container}/logs` 下的日志文件 |
| **Logstash** | 应用服务器 | 接收并解析日志，结构化处理后转发 |
| **Elasticsearch** | 应用服务器 `:9200` | 存储和索引日志数据 |
| **Kibana** | 按需部署 | 可视化查询和分析日志 |

### 8.3 日志巡检（Daily Check）

通过 `elasticsearch_dailycheck.py` 实现每日自动巡检：

- **巡检频率**：每日一次（通过 Jenkins 定时 Job）
- **查询内容**：ES 中过去 24 小时的 ERROR 级别日志
- **聚合方式**：按 `traceId` 分组，去重统计
- **白名单过滤**：过滤已知的无害错误消息
- **报告生成**：生成 HTML/Markdown 格式报告，通过邮件发送

### 8.4 监控与告警

| 监控维度 | 实现方式 |
|----------|----------|
| **Docker 容器状态** | Jenkins Pipeline 健康检查 |
| **中间件状态** | Eureka 服务状态页 / RabbitMQ Management |
| **日志异常** | ELK 每日巡检 → 邮件告警 |
| **版本一致性** | `settings-patch.yaml` 版本比对 |
| **配置合规** | pytest 自动化测试（端口唯一性、命名规范、配置校验） |

---

## 九、部署流程（11 阶段）

CRM 系统的完整部署遵循以下 11 个阶段的标准化流程：

| 阶段 | 操作 | 关键内容 | 负责工具 |
|------|------|----------|----------|
| **1. 准备** | 创建 CRM 仓库、获取进件信息、版本号、许可证 | 仓库命名: `chengjiashuo2`，版本号如 `6.108.0` | 手动 / 平台 |
| **2. 改配置** | 修改 `phoenix.yaml` + `docker_environments.yaml` | 主机 IP、RDS 连接、中间件密码、JWT 密钥、功能开关 | 手动编辑 |
| **3. 服务器初始化** | Rocky Linux 9.7 基础环境初始化 | 安装 Docker、配置 SSH、安装 Python 依赖 | Jenkins `GLOBLE_Centos_Init` |
| **4. 部署许可证** | Docker 部署 `lickit-server:1.2.6`，上传 CRM 许可证文件 | 许可证绑定 ECS 信息 | Jenkins Pipeline |
| **5. 部署数据库** | RDS MySQL 验证与初始化 | 验证 `phoenix` 库连通性，可选创建 cms-service 库 | Jenkins Pipeline |
| **6. 部署中间件** | 依次部署 Redis、ES、RabbitMQ、OSS、Eureka | 端口映射、内存限制、密码配置 | Jenkins Pipeline |
| **7. 前置操作** | sql_mode 设置、dtask MySQL 8.0 脚本、储值账户初始化 | `log_bin_trust_function_creators = 1` | Jenkins Pipeline |
| **8. 部署应用** | 部署全部微服务容器 | docker pull → docker run → 健康检查 | `CRM_deploy_one` |
| **9. 部署 Nginx** | 推送 OpenResty 配置并重启网关 | `hdpos.conf` + `upstream.conf` | `GLOBLE_deploy_nginx` |
| **10. 网关初始化** | 初始化 API 网关，获取 appId/secret | `CRM_Gateway (action=update)` | Jenkins Pipeline |
| **11. 登记信息** | 将环境信息写入 KA 环境汇总 Wiki | 公网 IP、域名、端口、版本号等 | 手动 |

---

## 十、完整用户请求链路（端到端）

### 10.1 一个典型 CRM 请求的完整旅程

```
Step 1: 运营人员浏览器输入 http://crm.example.com/crm/

Step 2: DNS 解析 → 阿里云公网 SLB 的弹性公网 IP (121.199.38.151)

Step 3: 公网 SLB（监听81端口）接收请求，转发至后端服务器组
        └→ 目标: 10.0.0.2:80（Nginx 网关）

Step 4: Nginx 读取 location 配置，匹配 /crm/ 路径规则
        └→ proxy_pass http://10.0.0.2:8025

Step 5: 请求到达宿主机 8025 端口
        └→ Docker 端口映射 → 容器内部 8080 端口

Step 6: phoenix-crm-web 微服务处理业务逻辑
        ├→ 查询 Redis(:6379) 缓存（用户 Session 验证）
        ├→ 通过 gateway-service(:82) 调用 phoenix-service-web
        │     └→ gateway 从 Eureka(:8082) 获取 phoenix-service-web 地址
        ├→ 读写 RDS MySQL(:3306) 数据库（会员信息、积分记录）
        └→ 发送 RabbitMQ 消息（积分变动通知）

Step 7: 处理结果原路返回
        容器:8080 → 宿主机:8025 → Nginx → SLB → 用户浏览器
```

### 10.2 关键性能特性

| 特性 | 实现方式 |
|------|----------|
| **负载均衡** | 阿里云公网 SLB（单点入口，多后端可扩展） |
| **SSL 卸载** | SLB 层或 Nginx 层处理 HTTPS 加解密 |
| **反向代理** | Nginx（OpenResty，高性能事件驱动） |
| **服务发现** | Eureka 注册中心，客户端侧负载均衡（Ribbon） |
| **内网直连** | 微服务 ↔ MySQL/Redis 全部通过内网 IP，零网络跳转 |
| **容器隔离** | Docker 提供进程/文件系统/网络命名空间隔离 |
| **异步解耦** | RabbitMQ 消息队列，削峰填谷 |
| **自动化部署** | Jenkins + Git → 全自动构建/发布/回滚 |
| **健康检查** | 部署后自动 HTTP 探测 + Spring Actuator |
| **日志统一** | ELK 集中收集，按 traceId 串联调用链 |
| **蓝绿升级** | blue/green 标签机制，SLB 秒级切换，零停机 |

---

## 十一、安全架构

```
                          ┌─────────────┐
                          │   公网用户    │
                          └──────┬──────┘
                                 │ HTTPS(SSL) / HTTP
                          ┌──────▼──────┐
                          │  公网 SLB    │ ← SSL证书、端口级访问控制
                          │ :81(业务)    │
                          └──────┬──────┘
                                 │ HTTP(内网)
              ┌──────────────────┼──────────────────┐
              │       VPC 专有网络（逻辑隔离）         │
              │                  ▼                   │
              │  ┌──────────────────────────┐       │
              │  │   安全组 crm-sg（入站白名单）│      │
              │  │   仅允许指定端口 + 来源     │       │
              │  └──────────────────────────┘       │
              │                  │                   │
              │  ┌───────────────▼──────────────┐   │
              │  │      应用服务器 ECS            │   │
              │  │  Nginx → Docker 微服务        │   │
              │  │  MySQL(RDS内网) Redis(内网)    │   │
              │  └──────────────────────────────┘   │
              │                  │                   │
              │  ┌───────────────▼──────────────┐   │
              │  │   NAT 网关（SNAT 出站代理）    │   │
              │  │   隐藏内网IP，访问外部仓库      │   │
              │  └──────────────────────────────┘   │
              └─────────────────────────────────────┘
```

**安全措施总结**：

| 安全层面 | 措施 |
|----------|------|
| **网络隔离** | 全部资源部署在 VPC `10.0.0.0/24` 中，与公网物理隔离 |
| **最小暴露面** | 仅 SLB 的 81/2222/2223/8888/38080/38889 端口对外，后端端口仅内网可达 |
| **访问控制** | 安全组 crm-sg + SLB 白名单双重过滤 |
| **传输加密** | SLB/Nginx 层 SSL/TLS 证书，公网传输加密 |
| **身份认证** | Jenkins SSH 密钥认证 + JWT 签名密钥 + License-server 许可证校验 |
| **出站安全** | 通过 NAT 网关（EIP `121.40.243.175`）统一出站，不暴露真实内网地址 |
| **配置安全** | JWT 密钥、第三方 API 密钥等敏感信息通过 `docker_environments.yaml` 环境变量注入，不硬编码 |
| **卡密安全** | 储值卡加密密钥 (`card-encrypt-key`) 独立配置，不以 `0` 开头纯数字形式暴露 |

---

## 十二、配置中心

CRM 系统采用**本地 YAML 配置 + 环境变量注入**的方式管理配置，区别于 ERP 的 Apollo 配置中心：

| 配置来源 | 文件 | 管理方式 |
|----------|------|----------|
| **部署拓扑** | `phoenix.yaml` | Git 版本控制 |
| **环境变量** | `docker_environments.yaml` | Git 版本控制 |
| **数据迁移** | `application.yml` | Git 版本控制 |
| **组件版本** | `settings-dly.yaml` | Git 版本控制 + patch 文件抽取 |

**docker_environments.yaml 核心配置分类**：

| 配置类别 | 示例配置项 | 说明 |
|----------|-----------|------|
| **全局配置** | `logging.level`, `ribbon.timeout` | 日志级别、超时、国际化 |
| **许可证** | `license.server.url` | License Server 地址 |
| **JWT 安全** | `jwt.signing.key` | API 网关 JWT 签名密钥 |
| **数据库** | DB 连接信息 | 在 `phoenix.yaml` 中定义 |
| **ES 连接** | ES 地址 | Elasticsearch 连接信息 |
| **微信配置** | `wechat.appid`, `wechat.secret` | 微信卡包/公众号对接 |
| **第三方平台** | 微盟、京东、抖音、美团、有赞等 | 各平台 API 密钥和回调地址 |
| **OSS 存储** | 阿里云 OSS / 腾讯云 COS | 文件存储配置 |
| **功能开关** | 会员导出、支付方式、分账等 | 按客户需求定制开关 |
| **ERP 对接** | H6/HD6 数据源 | 与 ERP 系统的数据对接配置 |

---

## 十三、高可用与扩展性

### 13.1 当前部署模式

| 环境 | 部署方式 | 说明 |
|------|----------|------|
| **集成测试（`int`）** | **单 ECS 全组件集中部署** | 所有组件 + 中间件在一台 `10.0.0.2` 上，适用于开发测试验证 |

### 13.2 可扩展架构设计

CRM 系统具备向生产级高可用架构演进的能力：

```
                        公网 SLB
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
        Nginx:80 (app-01)         Nginx:80 (app-02)
      (10.0.0.2)                 (10.0.0.3)
              │                         │
    ┌─────────┴─────────┐     ┌─────────┴─────────┐
    │  app-01           │     │  app-02           │
    │  · gateway(:82)   │     │  · gateway(:82)   │
    │  · crm-web(:8025) │     │  · crm-web(:8025) │
    │  · service-web    │     │  · service-web    │
    │  · dtask          │     │  · dtask          │
    └─────────┬─────────┘     └─────────┬─────────┘
              │                         │
              └──────────┬──────────────┘
                         │
              ┌──────────▼──────────┐
              │  RDS MySQL (主备)   │  ← 阿里云自动高可用
              │  Redis (集群)       │  ← 可扩展为 Sentinel 集群
              │  RabbitMQ (集群)    │  ← 可扩展为镜像队列集群
              │  Eureka (集群)      │  ← 可扩展为多节点互相注册
              └─────────────────────┘
```

**高可用演进路径**：

| 组件 | 当前模式 | 高可用方案 |
|------|----------|-----------|
| **ECS** | 单应用机 | 增加节点 → SLB 多后端分发 |
| **MySQL** | RDS 主备版（已高可用） | 读写分离 + 灾备实例 |
| **Redis** | 单机 2GB | Redis Sentinel / Cluster |
| **RabbitMQ** | 单节点 | 镜像队列集群（多节点） |
| **Elasticsearch** | 单节点 | ES 集群（3 节点，1 主 2 从） |
| **Eureka** | 单节点 | 多节点互相注册 |
| **Nginx** | 单机 | Keepalived VIP 双机热备 |

---

## 十四、关键配置项速查

| 配置项 | 配置文件 | 示例值 |
|--------|---------|--------|
| SLB 公网 IP | `crm清单.md` | `121.199.38.151` |
| NAT EIP | `crm清单.md` | `121.40.243.175` |
| 应用机内网 IP | `phoenix.yaml` → `hosts.app0` | `10.0.0.2` |
| 运维机内网 IP | `phoenix.yaml` → `hosts.middle0` | `10.0.0.1` |
| RDS MySQL 地址 | `phoenix.yaml` → `rds` | `rm-bp1o6cez78cpf88m3.mysql.rds.aliyuncs.com:3306` |
| 数据库用户 | `phoenix.yaml` → `rds` | `phoenix` |
| 数据库名 | `phoenix.yaml` → `rds` | `phoenix` |
| SSH 用户 | `phoenix.yaml` → `ssh` | `dnet` |
| CRM 版本 | `docker_environments.yaml` | `6.108.0` |
| RabbitMQ 用户 | `phoenix.yaml` → `middleware` | `hdmq` |
| RabbitMQ 控制台 | — | `http://10.0.0.2:15672` |
| Eureka 控制台 | — | `http://10.0.0.2:8082` |
| Jenkins 公网地址 | `crm清单.md` | `http://121.199.38.151:8888` |
| 安全组 | `crm清单.md` | `crm-sg` |
| VPC 网段 | `crm清单.md` | `10.0.0.0/24` |

---

## 十五、与 ERP 系统的关系

CRM 系统与 ERP 系统是**独立部署但数据互通**的两个系统：

```
┌─────────────────┐         ┌─────────────────────────┐
│   ERP 系统       │         │   CRM 系统               │
│   (自有 Oracle)  │◄───────►│   (自建 MySQL RDS)        │
│                 │ 数据同步 │                          │
│   · 交易数据     │ ◄────── │   · 会员数据              │
│   · 库存数据     │ ──────► │   · 积分/优惠券            │
│   · 订单数据     │         │   · 储值/预付              │
│                 │         │   · 等级体系              │
└─────────────────┘         └──────────────────────────┘
         │                            │
         │  sos-h6-transfer-service   │  h6-crm-service
         │  mkh-mas-transfer-server   │  phoenix-crm-web
         │  zl-portal-sync            │  phoenix-service-web
         │                            │
         └────────────┬───────────────┘
                      │
              共享组件（可选）：
              · gateway-service
              · dtask
              · Eureka
              · Redis / RabbitMQ / ES
```

**典型数据交互场景**：
- **交易同步**：ERP 产生交易 → Transfer 服务推送 → CRM 记录会员消费 → 积分计算
- **会员查询**：POS 调用 CRM 查询会员等级/余额 → 展示给收银员
- **促销计算**：POS 提交订单 → gem-service 计算优惠 → 返回折后价格
- **储值支付**：POS 发起储值扣款 → CRM 处理预付账户 → 返回扣款结果

---

## 附录：Git 仓库与分支速查

| 仓库 | 用途 | 关键文件 |
|------|------|----------|
| `chengjiashuo2` | Phoenix CRM 部署配置 | `phoenix.yaml`, `docker_environments.yaml`, `application.yml`, `inventory.py` |
| `toolset_chengjiashuo2` | 运维工具集 + 产品线配置 | `settings-dly.yaml`, `bluegreen_deployment.yml`, `elasticsearch_dailycheck.py` |
| `jjb_templates`（子模块） | Jenkins Job Builder 模板 | 通过 `.gitmodules` 引入 |

---

## 附录：常见部署问题速查

| 问题 | 原因 | 解决方法 |
|------|------|----------|
| MySQL 无法创建函数 | binlog 开启导致 `SUPER privilege` 缺失 | `SET GLOBAL log_bin_trust_function_creators = 1;` |
| Docker 容器启动超时（180s） | 服务器资源不足，phoenixcore 初始化慢 | 修改 Groovy 脚本超时阈值 |
| Docker login 报错（Rocky Linux 9.7） | `python3-requests` 版本不兼容 | 卸载旧版，安装 `requests==2.31.0` |
