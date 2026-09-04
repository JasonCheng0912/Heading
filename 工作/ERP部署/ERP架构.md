# ERP 系统整体部署架构

## 一、架构总览

ERP 系统部署于阿里云（杭州地域），整体采用**单 VPC 私有网络 + 公网 SLB 接入**的架构。所有业务组件以 Docker 容器化方式运行在 ECS 云服务器上，通过 OpenResty 反向代理网关统一对外提供服务，由 Jenkins 自动化流水线驱动 CI/CD 全流程。

### 架构全景图

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          公网（Internet）                                 │
│                                                                         │
│  门店POS / 办公PC / 移动端 ──→ 域名(example.hdpos.com) ──→ HTTP 80端口    │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                       阿里云 公网SLB（Server Load Balancer）               │
│                     对外暴露80端口，绑定弹性公网IP(EIP)                     │
│                       SSL证书挂载点（HTTPS → HTTP转发）                    │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │ 内网转发
                                    ▼
┌═════════════════════════════════════════════════════════════════════════┐
║                      VPC 专有网络（私有网络空间）                          ║
║                                                                         ║
║  ┌──────────────────────────────────────────────────────────────────┐  ║
║  │                   安全组（防火墙规则）                               │  ║
║  │             入站：公网SLB → 80端口 / Jenkins → 22端口               │  ║
║  │             出站：NAT网关 → Harbor镜像仓库 / Git仓库 / Apollo配置中心│  ║
║  └──────────────────────────────────────────────────────────────────┘  ║
║                                                                         ║
║  ┌──────────────────────────────────────────────────────────────────┐  ║
║  │              业务服务器 ECS（如 172.16.0.61）                       │  ║
║  │                                                                    │  ║
║  │  ┌──────────────────────────────────────────────────────┐         │  ║
║  │  │        OpenResty 反向代理网关（:80）                    │         │  ║
║  │  │   hdpos.conf（URL路径匹配） + upstream.conf（端口映射）  │         │  ║
║  │  └────┬────┬────┬────┬────┬────┬────┬────┬────┬────┘         │  ║
║  │       │    │    │    │    │    │    │    │    │    │          │  ║
║  │       ▼    ▼    ▼    ▼    ▼    ▼    ▼    ▼    ▼    ▼          │  ║
║  │  ┌──────────────────────────────────────────────────────┐     │  ║
║  │  │            Docker 容器化微服务（20+个组件）              │     │  ║
║  │  │                                                      │     │  ║
║  │  │  hdpos4-dist  jposbo  pasoreport  spms-hdpos-web      │     │  ║
║  │  │  :38180        :38280  :38980      :38115             │     │  ║
║  │  │                                                      │     │  ║
║  │  │  h6-crm-service   panther-dts-server  panther-taskweb│     │  ║
║  │  │  :38169           :38480              :38380          │     │  ║
║  │  │                                                      │     │  ║
║  │  │  up-connector  gem-service  init-tool  openapi-doc    │     │  ║
║  │  │  :38110         :38105       :38093     :38210        │     │  ║
║  │  │                                                      │     │  ║
║  │  │  h6-openapi2   hdpos6-notice  sos-transfer  zl-sync  │     │  ║
║  │  │  :38176         :38136         :38135        :38298   │     │  ║
║  │  │                                                      │     │  ║
║  │  │  card-server-proxy  mkh-mas-transfer  rumba-oss      │     │  ║
║  │  │  :38119             :38201            :38112         │     │  ║
║  │  └──────────────────────────────────────────────────────┘     │  ║
║  │                                                               │  ║
║  │  ┌──────────────────────────────────────────────────────┐     │  ║
║  │  │              Docker 中间件容器                          │     │  ║
║  │  │   Redis(:6379)  OSS-MySQL(:3306)  License-server(:8088) │     │  ║
║  │  └──────────────────────────────────────────────────────┘     │  ║
║  │                                                               │  ║
║  │  ┌──────────────────────────────────────────────────────┐     │  ║
║  │  │             Oracle 数据库（:1521）                       │     │  ║
║  │  │           实例名: hdposcs / hdposzs                     │     │  ║
║  │  │           用户: hd40 / transfer                        │     │  ║
║  │  └──────────────────────────────────────────────────────┘     │  ║
║  └──────────────────────────────────────────────────────────────────┘  ║
║                                                                         ║
║  ┌──────────────────────────────────────────────────────────────────┐  ║
║  │              OPS 运维服务器 ECS（独立宿主机）                        │  ║
║  │   Docker 容器运行 Jenkins Server（:8080）                           │  ║
║  │   通过 SSH 免密（dnet用户）远程管控业务服务器                         │  ║
║  └──────────────────────────────────────────────────────────────────┘  ║
║                                                                         ║
║  ┌──────────────────────────────────────────────────────────────────┐  ║
║  │              NAT 网关 → 访问外网资源                                │  ║
║  │   Harbor镜像仓库 / Git配置仓库 / Apollo配置中心                     │  ║
║  └──────────────────────────────────────────────────────────────────┘  ║
║                                                                         ║
╚═════════════════════════════════════════════════════════════════════════╝
```

最外层是公网终端，门店POS、办公PC通过域名访问系统。
流量先到达阿里云公网 SLB（负载均衡器），它绑定弹性公网IP，通过内网转发到 VPC 专有网络中。
进入 VPC 后，第一道关卡是安全组（防火墙），仅开放公网 SLB 到 80 端口
所有业务服务都部署在单台 ECS 业务服务器（如 172.16.0.61）上，内部采用 Docker 容器化部署。
入口由 OpenResty 反向代理网关（监听 80 端口）统一承接，它根据 hdpos.conf 中的 URL 路径规则，
将不同请求转发到 upstream.conf 定义的对应后端端口，例如 /hdpos4-web 打到 38180 端口的 hdpos4-dist 容器，/jposbo 打到 38280 端口。
这台服务器上共运行 20 多个业务微服务容器，以及 Redis、OSS-MySQL、License-server 等中间件容器，
底层数据持久化由 Oracle 数据库（hdposcs 实例）提供。
此外，图中还有一台独立的 OPS 运维服务器，
上面用 Docker 运行 Jenkins，通过 SSH 免密登录远程管控业务服务器，实现自动化部署。
最后，VPC 内的 NAT 网关负责让内网服务器访问外网的 Harbor 镜像仓库、Git 配置仓库和 Apollo 配置中心，完成镜像拉取和配置同步。

---

## 二、阿里云基础设施层

### 2.1 VPC 专有网络

整个 ERP 系统部署在阿里云 **VPC（Virtual Private Cloud，专有网络）** 中，这是一个逻辑隔离的私有网络环境。

- **作用**：将 ERP 所有云资源（ECS、数据库、中间件）纳入一个封闭的私有网络空间，与公网及其他租户网络完全隔离。
- **自定义 IP 段**：如 `172.16.0.0/16`，自主划分交换机子网。
- **私网互通**：同一 VPC 内的所有 ECS 通过内网 IP 直连，延迟低、无带宽费用。

### 2.2 公网 SLB（Server Load Balancer）

公网 SLB 是 ERP 系统的**唯一公网入口**，对外暴露服务。

| 属性 | 配置说明 |
|------|----------|
| **监听端口** | 80（HTTP）/ 443（HTTPS，有证书时） |
| **公网 IP** | 绑定 EIP 弹性公网IP |
| **转发目标** | VPC 内 ECS 的 OpenResty 80 端口 |
| **SSL 证书** | 通常挂在 SLB 层，做 HTTPS → HTTP 转发 |
| **健康检查** | 定期探测后端 OpenResty 80 端口可达性 |
| **白名单/黑名单** | 可按需配置访问控制 |

**流量路径**：外部用户访问域名 → DNS解析到SLB的公网IP → SLB将请求转发至 VPC 内业务服务器 `172.16.0.61:80`。

### 2.3 安全组（Security Group）

安全组充当虚拟防火墙，控制 ECS 实例的入站/出站流量：

| 方向 | 规则 | 来源/目标 | 端口 | 用途 |
|------|------|-----------|------|------|
| 入站 | 允许 | 公网 SLB | 80 | 业务访问 |
| 入站 | 允许 | OPS/Jenkins | 22 | SSH运维 |
| 入站 | 允许 | 公司固定通道IP | 8080/8888 | Jenkins管理页面 |
| 出站 | 允许 | 0.0.0.0/0 | 443/80 | 访问Harbor、Git、Apollo |
| 入站 | 拒绝 | 0.0.0.0/0 | 全部 | 默认拒绝其他所有入站 |

### 2.4 NAT 网关

VPC 内的 ECS 默认只有私网 IP，无法直接访问外网。通过 **NAT 网关** 实现：

- **SNAT（源地址转换）**：让业务服务器能访问外网的 Harbor 镜像仓库、Git 代码仓库、Apollo 配置中心
- **安全性**：ECS 不直接暴露公网 IP，出向流量统一经 NAT 网关代理，对外隐藏真实内网地址

### 2.5 专有网络私网域名（可选）

在生产环境中，可能配置内网域名（如 `erp.inter.xxx.com`）指向业务服务器内网IP，使 VPC 内服务之间通过域名而非硬编码 IP 互访，便于切换和扩展。

---

## 三、流量接入层

### 3.1 公网流量入口：SLB → OpenResty

外部请求从用户浏览器到达后端微服务，需依次经过 DNS 解析、SLB 负载均衡、安全组、OpenResty 网关四层转发：

```
用户浏览器
  │ ① DNS 解析域名 → SLB 弹性公网 IP
  │ ② HTTPS 请求（SSL 证书在 SLB 层解密）
  ▼
公网 SLB（监听 443/80）
  │ ③ 健康检查确认后端 ECS 可用
  │ ④ 转发至业务服务器 172.16.0.61:80（HTTP）
  ▼
安全组（虚拟防火墙）
  │ ⑤ 仅允许 SLB 来源的 80 端口流量入站
  ▼
OpenResty 网关（172.16.0.61:80）
  │ ⑥ 读取 hdpos.conf 匹配 URL → proxy_pass
  │ ⑦ 读取 upstream.conf 解析后端宿主机端口
  ▼
Docker 宿主机端口（如 38180）
```

**SLB 层核心职责**：
- **SSL 卸载**：HTTPS 证书挂载在 SLB，解密后以 HTTP 转发至后端，降低 ECS 的 CPU 开销
- **健康检查**：定时探测后端 OpenResty 80 端口，异常节点自动剔除
- **访问控制**：可配置白名单，限制仅公司固定 IP 段访问

**安全组入站规则**：仅开放 80（SLB → OpenResty）、22（Jenkins SSH）两路流量，其余端口默认拒绝，形成 VPC 内部的第一道防线。

**OpenResty 处理**：请求到达后，Nginx 按 `location` 匹配 URL 路径（如 `/hdpos4-web`），通过 `proxy_pass` 转发到 `upstream` 定义的宿主机端口，最终进入 Docker 容器。

### 3.2 OpenResty 网关核心机制

OpenResty 兼具 **反向代理 + 路由分发 + 请求过滤** 功能，是 ERP 流量的"中枢神经"。

**hdpos.conf（路由规则）**：按 URL 路径匹配不同业务模块：

```nginx
# 根目录/默认首页
location / { root /usr/local/openresty/nginx/html; }

# 交易系统前端
location /hdpos4-web { proxy_pass http://hdpos4-dist; }

# 报表系统（含路径重写）
location /hdpos4-web/rest/report_query {
    proxy_pass http://pasoreport-web;
    rewrite /hdpos4-web/rest/report_query/(.*)$ /pasoreport-web/rest/report_query/$1 break;
}

# 报表系统
location /pasoreport-web { proxy_pass http://pasoreport-web; }

# CRM 会员
location /h6-crm-service { proxy_pass http://h6-crm-service; }

# 供应商平台
location /spms-web { proxy_pass http://spms-hdpos-web; }

# 数据交换平台
location /panther-web { proxy_pass http://panther-taskweb; }
location /panther-task-server { proxy_pass http://panther-taskweb; }
 

# OSS 对象存储（安全限制）
location ~* ^/rumba-oss-server/rs/oss/v1/[^/]+/o { return 403; }
```

**upstream.conf（服务端口映射表）**：

```nginx
upstream hdpos4-dist     { server 172.16.0.61:38180; }
upstream jposbo          { server 172.16.0.61:38280; }
upstream pasoreport-web  { server 172.16.0.61:38980; }
upstream spms-hdpos-web  { server 172.16.0.61:38115; }
upstream h6-crm-service  { server 172.16.0.61:38169; }
upstream card-server-proxy-service { server 172.16.0.61:38119; }
upstream sos-h6-transfer-service { server 172.16.0.61:38135; }
upstream openapi-doc-service { server 172.16.0.61:38210; }
upstream rumba-oss-server    { server 172.16.0.61:38112; }
# ... 其他 upstream 块
```

### 3.3 端口映射机制

宿主机端口与 Docker 容器内部端口的关系：

```
公网请求
  → SLB:80
    → OpenResty:80
      → upstream → 172.16.0.61:38180（宿主机端口）
        → Docker端口映射 → 容器内部:8080（业务端口）
```
1. 用户请求（公网）
用户在浏览器输入 http://xxx.com/hdpos4-web 发起请求。这个请求先到达阿里云 SLB（负载均衡）

2. SLB 转发（公网 → 内网）
SLB 监听了 80 端口，把请求转发给后端服务器（OpenResty）的 80 端口

3. OpenResty 路由（内网：80）
OpenResty 收到请求后，根据 location 规则（/hdpos4-web），匹配到 upstream hdpos4-dist。

upstream 中配置了后端的宿主机 IP + 宿主机端口：

```commandline
upstream hdpos4-dist {
    server 172.16.0.61:38180;   # 宿主机 IP + 宿主机映射端口
}

```

4. 宿主机端口映射（宿主机：38180 → 容器内部：8080）
这一步是最核心的，我来展开讲：

当请求到达宿主机的 38180 端口时，Docker 会把流量转发到容器内部的 8080 端口。

这个映射关系是在 docker run 时通过 -p 参数指定的：

```commandline

docker run -d \
  -p 38180:8080 \   # 宿主机 38180 → 容器内部 8080
  --name hdpos4-dist \
  harborka.qianfan123.com/hdpos46/hdpos4-dist:2.17.2
```
这个 38180 对应 erp.csv 中的 c_port 字段

5. 业务处理（容器内部：8080）
容器内部的 8080 端口（对应 erp.csv 中的 c_port）是业务应用实际监听的端口。应用在容器内监听 8080 端口，处理请求后返回响应。







**端口命名规则**：
- **测试环境**：3xxxx 段（如 38180）
- **生产环境**：1xxxx 段（如 18180）
- **管理端口**：宿主机端口+1000（如 38180 对应管理端口 39180）

每个微服务容器在启动时通过 Docker 的 `-p` 参数将内部业务端口映射到宿主机的唯一端口，OpenResty 的 `upstream.conf` 中配置的就是这些宿主机端口，从而实现外部请求到容器内部的转发。

---

## 四、应用服务层

### 4.1 微服务组件清单（测试环境示例）

以下表格为**测试环境（`int`）**的单机部署配置，宿主机 IP 为 `172.16.0.61`，端口段为 `38xxx`：

| 类别 | 组件名 | 版本示例 | 宿主机端口 | 管理端口 | 镜像仓库 |
|------|--------|----------|-----------|----------|----------|
| **核心交易** | hdpos4-dist | 2.17.1 | 38180 | 39180 | harborka.qianfan123.com/hdpos46/hdpos4-dist |
| **POS后台** | jposbo | 2026051.16 | 38280 | 39280 | harborka.qianfan123.com/jposbo/jposbo |
| **报表** | pasoreport-web | 1.88 | 38980 | 39980 | harborka.qianfan123.com/component/pasoreport-web |
| **供应商门户** | spms-hdpos-web | 3.36.0 | 38115 | 39115 | harborka.qianfan123.com/component/spms-hdpos-web |
| **会员/CRM** | h6-crm-service | 1.30 | 38169 | 39169 | harborka.qianfan123.com/hdpos46/h6-crm-service |
| **数据交换** | panther-dts-server | 1.92 | 38480 | 39480 | harborka.qianfan123.com/dts-store/panther-dts-server |
| **数据交换Web** | panther-taskweb | 1.92 | 38380 | 39380 | harborka.qianfan123.com/dts-store/panther-taskweb |
| **连接器** | up-connector-service | 1.65 | 38110 | 39110 | harborka.qianfan123.com/component/up-connector-service |
| **促销引擎** | gem-service | 2.9 | 38105 | 39105 | harborka.qianfan123.com/hdpos46/gem-service |
| **初始化工具** | init-tool | 2.4 | 38093 | - | harborka.qianfan123.com/component/init-tool |
| **开放接口** | h6-openapi2-service | 1.50.1 | 38176 | 39176 | harborka.qianfan123.com/hdpos46/h6-openapi2-service |
| **接口文档** | openapi-doc-service | 1.50.1 | 38210 | 39210 | harborka.qianfan123.com/hdpos46/openapi-doc-service |
| **通知服务** | hdpos6-notice-service | 2.0 | 38136 | 39136 | harborka.qianfan123.com/hdpos46/hdpos6-notice-service |
| **Transfer** | sos-h6-transfer-service | 1.69.2 | 38135 | 19135 | harborka.qianfan123.com/hdpos46/sos-h6-transfer-service |
| **门户同步** | zl-portal-sync | 1.65.0 | 38298 | 9837 | harbor.qianfan123.com/ka-sail/zl-portal-sync |
| **卡服务代理** | card-server-proxy-service | 1.1 | 38119 | 39119 | harborka.qianfan123.com/component/card-server-proxy-service |
| **MAS传输** | mkh-mas-transfer-server | 1.34.0 | 38201 | 39201 | harbor.qianfan123.com/mas/mkh-mas-transfer-server |

> **生产环境（`production`）**：端口段为 `18xxx`，组件分布在 `172.16.1.70`（app-01）和 `172.16.1.71`（app-02）两台 ECS 上，详见 [6.2 生产环境（多机部署）](#62-生产环境多机部署)。

### 4.2 中间件清单

| 中间件 | 版本 | 端口 | 说明 |
|--------|------|------|------|
| **Redis** | 2.8 | 6379 | 缓存、会话存储、分布式锁（必须部署） |
| **License-server** | 1.2.5 | 8088/9080 | 许可证服务（必须部署） |
| **OSS-MySQL** | 5.7.14 | 3306 | 对象存储元数据库（部署 SPMS 时必需） |
| **rumba-oss-server** | 2.17.1 | 38112 | 对象存储服务（部署 SPMS 时必需） |
| **MySQL（jposbo）** | 8.0.31 | 3306 | jposbo 后台管理系统数据（自建时使用） |

---

## 五、数据存储层（数据访问层）

### 5.1 数据访问架构总览

ERP 系统采用**分层存储 + 多库异构**的数据访问策略，不同业务数据按特性存入最合适的存储介质：

```
┌──────────────────────────────────────────────────────────────┐
│                     应用服务层（Docker容器）                    │
│  hdpos4-dist  jposbo  h6-crm-service  pasoreport  ...        │
│        │         │          │              │                  │
└────────┼─────────┼──────────┼──────────────┼──────────────────┘
         │         │          │              │
         ▼         ▼          ▼              ▼
┌──────────────────────────────────────────────────────────────┐
│                    数据访问层（JDBC / 连接池）                    │
│  ┌─────────────┐  ┌─────────────┐  ┌──────────────────────┐  │
│  │  Oracle JDBC │  │  Redis客户端 │  │  MySQL JDBC / Mongo  │  │
│  │  thin驱动    │  │  (Jedis)    │  │  PostgreSQL JDBC     │  │
│  └──────┬──────┘  └──────┬──────┘  └──────────┬───────────┘  │
└─────────┼────────────────┼────────────────────┼──────────────┘
          │                │                    │
          ▼                ▼                    ▼
┌─────────────────┐ ┌──────────┐ ┌────────────────────────────┐
│  Oracle 数据库   │ │  Redis   │ │  MySQL / PostgreSQL / Mongo│
│  :1521          │ │  :6379   │ │  :3306 / :5432 / :27017   │
│  交易/业务数据   │ │ 缓存/会话 │ │  元数据/后台/日志/NoSQL    │
└─────────────────┘ └──────────┘ └────────────────────────────┘
```

**数据访问原则**：
- **核心交易数据**：统一走 Oracle，通过 JDBC thin 连接，配置集中管理
- **缓存/会话/锁**：Redis 作为高速缓存层，减轻 Oracle 读压力
- **辅助业务数据**：MySQL/PostgreSQL 承载 OSS 元数据、jposbo 后台数据等
- **数据库类型可切换**：通过 `all.yaml` → `dbtype` 切换 `Oracle|Polardb|PostgreSQL|MySQL`

### 5.2 Oracle 数据库（核心交易数据）

ERP 核心交易数据（订单、库存、会员、支付、报表等）全部存储在 Oracle 中。

| 属性 | 值 |
|------|-----|
| **服务地址** | `172.16.0.61:1521`（单机）/ 独立 RDS 实例（高可用） |
| **实例名** | 测试 `hdposcs` / 生产 `hdposzs` |
| **JDBC 连接串** | `jdbc:oracle:thin:@172.16.0.61:1521:hdposcs` |
| **连接池** | 各微服务内部集成（通常 Druid / HikariCP） |
| **驱动** | Oracle JDBC thin driver（容器内自带） |
| **字符集** | AL32UTF8 |

**数据库用户隔离策略**：

| 用户类型 | 用户名 | 用途 | 隔离目的 |
|----------|--------|------|----------|
| **通用业务用户** | `hd40` | 大多数微服务（hdpos4、jposbo、crm、pasoreport 等） | 统一权限，简化运维 |
| **Transfer 专用** | `ZJYWVTVTTRANSFER` | sos-h6-transfer、zl-portal-sync、mas-transfer 等 | 数据交换类应用独立用户，避免误操作影响交易 |
| **uni 专用** | `hduni` | h6-to-uni 应用 | 与主业务隔离 |
| **adi 专用** | `hdadi` | adi 应用 | 数据独立 |
| **Card 卡系统** | `hdcardcts` / `hdcardctn` / `hdcardhqs` / `hdcardhqn` | Card 卡系统（机密/普通/总部/门店） | 卡系统内部多库隔离，机要库与普通库分离 |

### 5.3 数据库脚本管理（RDB 升级与初始化）

数据库结构变更通过 **Jenkins 自动化 Job** 管理，统一使用 `project-db.yml` 配置：

| 配置项 | 说明 | 配置位置 |
|--------|------|----------|
| `RdbScript` | 是否执行数据库脚本（`false` 默认跳过，部署时按需开启） | `all.yaml` |
| `config_gen` | 是否重新生成配置文件（首次安装时 `true`） | `all.yaml` |
| `rdb_url` | 数据库连接串（JDBC格式） | `all.yaml` |
| `rdb_user` / `rdb_pwd` | 脚本执行用户名/密码 | `all.yaml` |

**执行模式**（Jenkins Job 参数）：
- **`upgrade_db`**：增量升级，执行版本差异脚本，适用于日常迭代
- **`setup_db`**：全新初始化，执行全量建表+基础数据，适用于新环境搭建

**RDB 升级镜像示例**：
- `dpos-rdb-upgrade` / `dpos-rdb-setup`：交易核心数据库
- `dbo-rdb-upgrade` / `dbo-rdb-setup`：后台管理系统数据库
- `dpm-rdb-upgrade` / `dpm-rdb-setup`：分润管理系统数据库
- `pasoreport-rdb-upgrade` / `pasoreport-rdb-setup`：报表系统数据库

**数据库升级安全机制**：
- 升级脚本按版本号顺序执行，记录版本日志
- 升级失败自动回滚，生成错误日志
- 部署后自动校验数据库版本号一致性

### 5.4 Redis 缓存

| 属性 | 值 |
|------|-----|
| **地址** | `172.16.0.61:6379` |
| **版本** | 2.8 |
| **部署方式** | Docker 容器（单机）/ 集群（高可用扩展） |
| **客户端** | Jedis / Lettuce（各微服务应用层集成） |
| **配置来源** | `all.yaml` / `app.yaml` → `redis_host` |

**Redis 用途矩阵**：

| 用途 | 说明 | 典型场景 |
|------|------|----------|
| **会话管理** | 存储用户登录 Session | POS 收银员登录态 |
| **分布式锁** | 基于 Redis 的互斥锁 | 库存扣减、订单防重 |
| **高频缓存** | 热点数据缓存 | 商品信息、价格策略 |
| **计数器** | 原子自增/自减 | 订单号生成、流水号 |
| **队列** | 轻量级消息队列 | 异步任务通知 |


### 5.7 MongoDB（NoSQL 存储）

部分新系统使用 MongoDB 存储非结构化数据：

| 属性 | 值 |
|------|-----|
| **版本** | 3.4.16-jessie |
| **部署方式** | Docker 容器 |
| **用途** | 日志、配置快照、非结构化业务数据 |
| **配置来源** | `app.yaml` / `settings.yaml` |

### 5.8 对象存储服务（rumba-oss-server）

| 属性 | 值 |
|------|-----|
| **服务端口** | `38112` |
| **版本** | `2.17.1` |
| **用途** | 文件上传/下载/预览（商品图片、合同附件、报表导出） |
| **元数据库** | OSS-MySQL（文件索引、权限、路径） |
| **存储后端** | 阿里云 OSS / 本地磁盘 |

**OSS 访问安全**：
- OpenResty 层限制 `rumba-oss-server` 路径，非 `GET` 请求返回 `403`
- 文件上传需经过应用层鉴权，避免非法文件写入

### 5.9 数据持久化与存储卷

Docker 容器的数据持久化通过 **宿主机目录挂载** 实现：

| 挂载路径 | 容器内路径 | 用途 |
|----------|-----------|------|
| `/hdapp/{containerid}/data` | `/data/db` | 数据库数据文件（MySQL、PostgreSQL、MongoDB） |
| `/hdapp/{containerid}/logs` | `/logs` | 应用日志文件 |
| `/hdapp/{containerid}/conf` | `/conf` | 配置文件（Apollo 拉取后本地缓存） |
| `/hdapp/{containerid}/backup` | `/backup` | 配置备份目录 |

**备份策略**：
- **配置备份**：`BackupCover` 参数控制是否合并备份配置到当前版本
- **备份目录**：`backup_homepath: /hdapp/deploy_manage`
- **数据库备份**：通过 Jenkins Job `cmdb_backup` 定期执行，支持自动上传到 OSS

### 5.10 数据访问链路汇总

```
┌────────────────────────────────────────────────────────────┐
│                        应用服务层                            │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐       │
│  │ hdpos4   │ │ jposbo   │ │ h6-crm   │ │ pasoreport│      │
│  │ -dist    │ │          │ │ -service │ │          │       │
│  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘       │
└───────┼────────────┼────────────┼────────────┼─────────────┘
        │            │            │            │
        │ ┌──────────┴────────────┴────────────┴──────────┐ │
        │ │              Oracle JDBC (thin)               │ │
        │ │  jdbc:oracle:thin:@172.16.0.61:1521:hdposcs  │ │
        │ └────────────┬──────────────────┬──────────────┘ │
        │              │                  │                  │
        │    ┌─────────▼──────────┐ ┌─────▼──────────┐      │
        │    │  hd40 (通用业务)    │ │ ZJYWVTVTTRANSFER │    │
        │    │  交易/库存/会员     │ │ 数据交换/同步     │    │
        │    └──────────────────┘ └──────────────────┘      │
        │                                                   │
        │  ┌────────────────────────────────────────────┐  │
        └──┤  Redis :6379（缓存/会话/分布式锁）          │  │
           └────────────────────────────────────────────┘  │
        ┌──────────────────────────────────────────────────┐
        │  MySQL :3306（OSS元数据） / PostgreSQL :5432    │
        │  MongoDB :27017（NoSQL）                         │
        └──────────────────────────────────────────────────┘
```

---

## 六、内部服务互访链路

### 6.1 测试环境（单机部署）

测试环境（`int`）所有组件部署在同一台 ECS `172.16.0.61` 上，微服务之间、微服务与中间件/数据库之间全部通过**本机内网 IP + 端口**直接通信：

```
┌──────────────────────────────────────────────────────────┐
│                  ECS 172.16.0.61                          │
│                                                          │
│   hdpos4-dist ──→ JDBC ──→ Oracle :1521                  │
│   jposbo ──→ JDBC ──→ Oracle :1521                       │
│   h6-crm-service ──→ JDBC ──→ Oracle :1521               │
│   pasoreport ──→ JDBC ──→ Oracle :1521                   │
│   panther-* ──→ JDBC ──→ Oracle :1521                    │
│   ... 全部微服务 ──→ JDBC ──→ Oracle :1521               │
│                                                          │
│   全部微服务 ──→ Redis :6379（缓存）                       │
│   spms-hdpos-web ──→ OSS-MySQL :3306                     │
│   jposbo ──→ MySQL :3306                                 │
│   sos-h6-transfer-service ──→ transfer用户 Oracle         │
│   card-server-proxy-service ──→ Oracle :1521             │
│   mkh-mas-transfer-server ──→ Oracle :1521               │
│                                                          │
│   ★ 所有通信均为本机回环，无网关转发，延迟极低              │
└──────────────────────────────────────────────────────────┘
```

### 6.2 生产环境（多机部署）

生产环境（`production`）组件分布在 **两台 ECS** 上，通过**内网 IP 直连**通信：

| 主机 | IP | 部署组件 |
|------|-----|----------|
| **app-prd01** | `172.16.1.70` | hdpos4-dist、spms-hdpos-web、h6-crm-service、gem-service、init-tool、h6-openapi2-service、hdpos6-notice-service、card-server-proxy-service |
| **app-prd02** | `172.16.1.71` | jposbo、panther-dts-server、panther-taskweb、pasoreport-web、up-connector-service、zl-portal-sync、sos-h6-transfer-service、mkh-mas-transfer-server |

**跨机通信示例**：
- `app-prd01` 上的 `hdpos4-dist` 调用 `app-prd02` 上的 `jposbo`：`172.16.1.70:18180` → `172.16.1.71:18280`
- 所有微服务仍通过 `172.16.1.70:1521` 或 `172.16.1.71:1521` 访问 Oracle（数据库可部署在独立实例或其中一台 ECS 上）
- Redis、MySQL 等中间件部署在共享节点，两台 ECS 均可访问

**内网通信优势**：
- 同一 VPC 内网延迟 `< 1ms`，带宽无额外费用
- 无需经过公网 NAT 或 SLB，通信路径最短
- 安全组规则允许同一 VPC 内网互通，无需额外端口开放


---

## 七、运维管理层（CI/CD 流水线）

### 7.1 OPS 宿主机

Jenkins 独立部署在一台 OPS ECS 上，不混用业务服务器资源。

| 属性 | 说明 |
|------|------|
| **部署方式** | Docker 容器运行 `jenkins-server:2.492.1-lts-hd` |
| **内网端口** | `8080`（Jenkins Web UI） |
| **JNLP 端口** | `50000` |
| **公网访问** | 通过公网 SLB 或域名暴露（如 `http://118.31.170.111:8888`） |
| **白名单** | 添加公司固定通道 IP |

### 7.2 核心外部依赖

| 组件 | 地址/方式 | 作用 |
|------|----------|------|
| **Git Toolset 仓库** | `github.app.hd123.cn`（内部 GitLab） | 存储全部部署配置（3分支） |
| **Harbor 镜像仓库** | `harbor.qianfan123.com` / `harborka.qianfan123.com` | 存储所有 Docker 镜像 |
| **Apollo 配置中心** | `apollo-portal.hd123.com` | 统一动态配置管理 |
| **Wiki 文档** | `wiki.app.hd123.cn` | 部署环境信息汇总 |

### 7.3 配置仓库 Git 三分支模型

```
toolset_{客户编码}/
├── jenkins分支     ← Jenkins Job 模板 + 数据库映射配置
│   ├── config/jenkins.yaml              # Jenkins 全局配置
│   ├── config/jenkins-jcac-plugin.yaml  # JCasC 插件配置（节点管理）
│   ├── jenkins/h6/int/project-app.yml   # 应用部署Job（多应用/单应用）
│   ├── jenkins/h6/int/project-db.yml    # 数据库部署Job
│   ├── jenkins/h6/tool/project-basic.yml  # 认证凭证生成Job
│   ├── jenkins/h6/view.yml              # ERP Job分组视图
│   ├── jenkins/templates/               # 模板库（git submodule）
│   ├── jenkins_jobs.ini                 # Job构建器配置
│   └── jenkins_update.sh                # 批量更新Job脚本
│
├── erp分支         ← 环境参数 + 部署清单（CMDB）
│   ├── all.yaml                        # 全局公共参数（客户信息、DB、License）
│   ├── apollo.yaml                     # Apollo Token
│   ├── app.yaml                        # 应用认证密钥
│   ├── erpcmdb.yaml                    # CMDB主机拓扑 + 子系统定义
│   └── iwms.yaml                       # 仓储系统配置
│
└── develop分支     ← OpenResty + 部署配置 + 监控
    ├── openresty_config/erp/integration_test/
    │   ├── conf.d/hdpos.conf           # 路由规则
    │   ├── conf.d/upstream.conf        # 后端端口映射
    │   ├── conf.d/downloadfile.conf   # 大文件下载配置
    │   ├── inventory                   # 网关部署目标主机
    │   └── main.yml                    # 环境标识
    ├── docker_environment.yaml         # Docker 环境配置
    ├── settings.yaml                   # 部署工具全局配置（健康检查、系统依赖）
    ├── appinstall/                     # 应用安装参数（JVM、Docker参数）
    ├── bluegreen_deployment.yml        # 蓝绿部署配置
    ├── elk_build_quickly/              # ELK 日志收集快速构建
    ├── grafana/                        # Grafana 监控大盘
    └── jenkins/                        # Jenkins 监控配置
```

### 7.4 Jenkins 远程管控流程

```
Jenkins（OPS宿主机）
  │
  │  ① SSH免密（dnet@172.16.0.61:22）
  ▼
业务服务器 172.16.0.61
  │
  ├── 推送OpenResty配置 → GLOBLE_deploy_nginx
  │    拉取develop分支 → scp hdpos.conf/upstream.conf → 重启网关
  │
  ├── 数据库部署 → ERP_db_deploy_int
  │    远程连接Oracle → 执行建表/数据初始化脚本（upgrade_db/setup_db）
  │
  ├── 应用容器部署 → ERP_app_deploy_int
  │    读取erpcmdb.yaml清单 → docker pull from Harbor → docker run → 健康检查
  │
  └── 认证凭证生成 → ERP_basic_auth
       自动生成服务接口密钥 → 写入app.yaml → 提交Git
```

### 7.5 Jenkins Job 模板化管理

采用 **Jenkins Job Builder (JJB)** 实现 Job 的 YAML 化定义和版本控制：

- **模板集中管理**：通过 git submodule 引入 `jjb_templates`，统一维护模板
- **流水线集中管理**：通过 git submodule 引入 `jenkins_builder`，统一维护 pipeline
- **自动同步**：GitLab webhook 触发外部 Jenkins 的更新 Job，自动将 YAML 变更同步到被管理 Jenkins
- **本地调试**：支持 `jenkins-jobs test` 预览生成的 XML，确认无误后再推送

---

## 八、健康检查与监控

### 8.1 健康检查机制

每个部署的微服务均配置健康检查端点，用于部署后的自动验证和运行时监控：

| 应用类型 | 健康检查路径 | 示例 |
|---------|-------------|------|
| **传统 Tomcat** | `/{context}/healthservice/check.hd` | `/hdpos4-web/healthservice/check.hd` |
| **Spring Boot 1.x** | `/actuator/health` | `/actuator/health` |
| **Spring Boot 2.x** | `/actuator/health` | `/actuator/health` |

健康检查配置统一定义在 `erpcmdb.yaml` 中：
- `schema`: http
- `path`: 健康检查 URL
- `c_health_timeout`: 健康检查超时时间（15s~45s）
- 部署后自动执行健康检查，失败则回滚或告警

### 8.2 日志收集（ELK）

- **Filebeat**：部署在业务服务器，采集各微服务的日志文件
- **Logstash**：接收并解析日志，转发到 Elasticsearch
- **Elasticsearch**：存储和索引日志数据
- **Kibana**：可视化查询和分析日志

### 8.3 监控与告警

- **Grafana**：配置多种监控大盘（Jenkins、应用、数据库等）
- **Jenkins 监控**：`jenkins_monitor.yaml` 定义 Jenkins 实例监控
- **Elasticsearch 巡检**：每日自动检查 ES 索引状态
- **钉钉告警**：集成 DingTalk 发送告警通知

---

## 九、配置中心（Apollo）

Apollo 配置中心为所有微服务提供统一的动态配置管理：

| 属性 | 说明 |
|------|------|
| **配置格式** | `8881.{appid}.zjywvtvt`（如 `8881.hdpos4-dist.zjywvtvt`） |
| **配置内容** | 数据库连接、Redis地址、第三方接口密钥、业务开关等 |
| **动态推送** | 修改配置后实时推送到容器，无需重启 |
| **环境隔离** | 通过 `envname` 区分 int/uat/production 环境配置 |

---

## 十、完整用户请求链路（端到端）

### 10.1 一个典型请求的完整旅程

```
Step 1: 门店收银员在 POS 浏览器输入 http://erp.example.com/hdpos4-web/www/

Step 2: DNS 解析 → 阿里云公网 SLB 的弹性公网 IP

Step 3: 公网 SLB（监听80端口）接收请求，转发至后端服务器组
        └→ 目标: 172.16.0.61:80（OpenResty 网关）

Step 4: OpenResty 读取 hdpos.conf，匹配 location /hdpos4-web 规则
        └→ proxy_pass http://hdpos4-dist

Step 5: OpenResty 读取 upstream.conf，解析 hdpos4-dist 服务器组
        └→ server 172.16.0.61:38180

Step 6: 请求转发至宿主机 38180 端口
        └→ Docker 端口映射 → 容器内部 8080 端口

Step 7: hdpos4-dist 微服务处理业务逻辑
        ├→ 查询 Redis(:6379) 缓存（会话验证、高频数据）
        ├→ 读写 Oracle(:1521) 数据库（交易记录、库存变更）
        ├→ 可能调用 h6-crm-service(:38169) 查询会员信息
        └→ 可能调用 gem-service(:38105) 计算促销

Step 8: 处理结果原路返回
        容器:8080 → 宿主机:38180 → OpenResty → SLB → 用户浏览器
```

### 10.2 关键性能特性

| 特性 | 实现方式 |
|------|----------|
| **负载均衡** | 阿里云公网 SLB（单点入口，多后端可扩展） |
| **SSL 卸载** | SLB 层处理 HTTPS 加解密，后端 HTTP 通信 |
| **反向代理** | OpenResty（基于 Nginx + Lua，高性能事件驱动） |
| **内网直连** | 微服务 ↔ Oracle/Redis 全部通过本机 IP，零网络跳转 |
| **容器隔离** | Docker 提供进程/文件系统/网络命名空间隔离 |
| **动态配置** | Apollo 配置中心实时推送，无需重启容器 |
| **自动化部署** | Jenkins + Git Hook → 全自动构建/发布/回滚 |
| **健康检查** | 部署后自动 HTTP 探测，保障服务可用性 |

---

## 十一、安全架构

```
                          ┌─────────────┐
                          │   公网用户    │
                          └──────┬──────┘
                                 │ HTTPS(SSL)
                          ┌──────▼──────┐
                          │  公网 SLB    │ ← SSL证书，WAF防护
                          └──────┬──────┘
                                 │ HTTP(内网)
              ┌──────────────────┼──────────────────┐
              │       VPC 专有网络（逻辑隔离）         │
              │                  ▼                   │
              │  ┌──────────────────────────┐       │
              │  │   安全组（入站白名单）      │       │
              │  │   仅允许 SLB + Jenkins    │       │
              │  └──────────────────────────┘       │
              │                  │                   │
              │  ┌───────────────▼──────────────┐   │
              │  │      业务服务器 ECS            │   │
              │  │  OpenResty → Docker容器       │   │
              │  │  Oracle(内网) Redis(内网)      │   │
              │  └──────────────────────────────┘   │
              │                  │                   │
              │  ┌───────────────▼──────────────┐   │
              │  │   NAT 网关（SNAT出站代理）     │   │
              │  │   隐藏内网IP，访问外部仓库      │   │
              │  └──────────────────────────────┘   │
              └─────────────────────────────────────┘
```

**安全措施总结**：
- **网络隔离**：全部资源部署在 VPC 中，与公网物理隔离
- **最小暴露面**：仅 SLB 80/443 端口对外，所有后端端口仅内网可达
- **访问控制**：安全组 + SLB 白名单双重过滤
- **传输加密**：SLB 层 SSL/TLS 证书，公网传输加密
- **身份认证**：Jenkins SSH 密钥认证 + Apollo Token 鉴权 + License-server 许可证校验
- **出站安全**：通过 NAT 网关统一出站，不暴露真实内网地址
- **OSS 安全**：rumba-oss-server 路径限制，非 GET 请求返回 403

---

## 十二、高可用与扩展性

### 12.1 当前部署模式

| 环境 | 部署方式 | 说明 |
|------|----------|------|
| **测试环境（`int`）** | **单 ECS 全组件集中部署** | 所有组件在一台 `172.16.0.61` 上，适用于中小规模客户测试验证 |
| **生产环境（`production`）** | **双 ECS 分节点部署** | 组件按业务类型拆分至 `172.16.1.70`（核心交易）和 `172.16.1.71`（后台/数据交换/报表），已具备横向拆分能力 |

**生产环境双机部署拓扑**：

```
                        公网 SLB
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
        OpenResty:80               OpenResty:80
        (172.16.1.70)              (172.16.1.71)
              │                         │
    ┌─────────┴─────────┐     ┌─────────┴─────────┐
    │  app-prd01        │     │  app-prd02        │
    │  · hdpos4-dist    │     │  · jposbo         │
    │  · spms-hdpos-web │     │  · pasoreport-web │
    │  · h6-crm-service │     │  · panther-*      │
    │  · gem-service    │     │  · up-connector   │
    │  · init-tool      │     │  · zl-portal-sync │
    │  · h6-openapi2    │     │  · sos-transfer   │
    │  · hdpos6-notice  │     │  · mkh-mas-transfer│
    │  · card-server    │     │                   │
    └─────────┬─────────┘     └─────────┬─────────┘
              │                         │
              └──────────┬──────────────┘
                         │
              ┌──────────▼──────────┐
              │  Oracle + Redis     │
              │  （共享中间件）     │
              └─────────────────────┘
```

 
 
---

## 十三、关键配置项速查

| 配置项 | 配置文件 | 示例值 |
|--------|---------|--------|
| 公网域名/地址 | `all.yaml` → `extranetip` | `118.31.170.111` |
| 公网协议 | `all.yaml` → `access_protocol` | `http` |
| 客户编码 | `all.yaml` → `ClientCode` | `8881` |
| 客户简称 | `all.yaml` → `ClientenName` | `zjywvtvt` |
| OpenResty IP | `all.yaml` → `openresty_ip` | `172.16.0.61` |
| Oracle地址 | `all.yaml` → `dbserver:dbport:dbname` | `172.16.0.61:1521:hdposcs` |
| Redis地址 | `app.yaml` → `redis_host` | `172.16.0.61` |
| Apollo Token | `apollo.yaml` → `apollo_token` | Apollo开仓时获得 |
| Jenkins内网 | `jenkins.yaml` → `url` | `http://172.16.1.66:8080/` |
| Jenkins公网 | `jenkins-jcac-plugin.yaml` | `http://118.31.170.111:8888` |
| 许可证服务器 | `all.yaml` → `app_license` | `172.16.0.61` |

---

## 附录：Git 仓库与分支速查

| 仓库 | 分支 | 用途 | 关键文件 |
|------|------|------|----------|
| `toolset_zjywvtvt` | `jenkins` | Jenkins Job/View 定义 | `jenkins/h6/int/project-app.yml` |
| `toolset_zjywvtvt` | `erp` | 环境参数 + CMDB | `all.yaml`, `erpcmdb.yaml` |
| `toolset_zjywvtvt` | `develop` | OpenResty + 部署 + 监控 | `openresty_config/`, `settings.yaml` |
| `private_template` | `jenkins` | Jenkins 模板 | `templates/`, `pipeline/` |

---

