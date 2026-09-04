| # | App Image Name | Image URL | Setup Image URL | Upgrade Image URL | DB Type | Middleware | Public |
|------|-----------|----------|-------------|----------------|-----------|-------------| ------- |
| 1 | adi | `harborka.qianfan123.com/adi/adi` | `-` | `-` | MySQL, Oracle, Polardb | - | Yes |
| 2 | anda-web | `harborka.qianfan123.com/component/anda-web` | harborka.qianfan123.com/component/anda-rdb-setup | harborka.qianfan123.com/component/anda-rdb-upgrade | MySQL, Oracle | - | Yes |
| 3 | ays-dm-web | `harborka.qianfan123.com/component/ays-dm-web` | harborka.qianfan123.com/component/ays-dm-rdb-setup | harborka.qianfan123.com/component/ays-dm-rdb-upgrade | Oracle | Kafka | Yes |
| 4 | ays-service | `harborka.qianfan123.com/hdpos46/ays-service` | harborka.qianfan123.com/hdpos46/ays-rdb-setup | harborka.qianfan123.com/hdpos46/ays-rdb-upgrade | Oracle | Redis | Yes |
| 5 | ays-transfer2-server | `harbor.qianfan123.com/mas/ays-transfer2-server` | `-` | `-` | Oracle | - | No |
| 6 | gateway-service | `harbor.qianfan123.com/baas/gateway-service` | harbor.qianfan123.com/baas/gateway-rdb-setup | harbor.qianfan123.com/baas/gateway-rdb-upgrade | Oracle | Redis | No |
| 7 | babycare-service | `harborka.qianfan123.com/hdpos46/babycare-service` | harborka.qianfan123.com/hdpos46/babycare-rdb-setup | harborka.qianfan123.com/hdpos46/babycare-rdb-upgrade | Oracle | Redis | Yes |
| 8 | bazhuang-service | `harborka.qianfan123.com/hdpos46/bazhuang-service` | harborka.qianfan123.com/hdpos46/bazhuang-rdb-setup | harborka.qianfan123.com/hdpos46/bazhuang-rdb-upgrade | Oracle | Redis | Yes |
| 9 | bbw-connector-service | `harborka.qianfan123.com/hdpos46/bbw-connector-service` | harborka.qianfan123.com/hdpos46/bbw-connector-rdb-setup | harborka.qianfan123.com/hdpos46/bbw-connector-rdb-upgrade | Oracle | Redis, Sentinel | Yes |
| 10 | bbw-srm-service | `harborka.qianfan123.com/hdpos46/bbw-srm-service` | harborka.qianfan123.com/hdpos46/bbw-srm-rdb-setup | harborka.qianfan123.com/hdpos46/bbw-srm-rdb-upgrade | Oracle | - | Yes |
| 11 | bbw-web | `harborka.qianfan123.com/hdpos46/bbw-web` | harborka.qianfan123.com/hdpos46/bbw-rdb-setup | harborka.qianfan123.com/hdpos46/bbw-rdb-upgrade | Oracle | - | Yes |
| 12 | bpfm-app-service | `harborka.qianfan123.com/component/bpfm-app-service` | `-` | `-` | Oracle, Polardb, Polardb2 | Redis | Yes |
| 13 | c3-dist | `harborka.qianfan123.com/c3/c3-dist` | harborka.qianfan123.com/c3/c3-rdb-setup | harborka.qianfan123.com/c3/c3-rdb-upgrade | Oracle | OSS, Redis | Yes |
| 14 | cadi-console-web-service | `harbor.qianfan123.com/adi/cadi-console-web-service` | `-` | `-` | Oracle | Redis | Yes |
| 15 | cadi-service | `harbor.qianfan123.com/adi/cadi-service` | harbor.qianfan123.com/adi/cadi-rdb-setup | harbor.qianfan123.com/adi/cadi-rdb-upgrade | Oracle | Redis | Yes |
| 16 | cadi-web-service | `harbor.qianfan123.com/adi/cadi-web-service` | `-` | `-` | Oracle | Redis | Yes |
| 17 | card | `harborka.qianfan123.com/card/card` | `-` | `-` | Oracle | - | Yes |
| 18 | card-server-proxy-service | `harborka.qianfan123.com/component/card-server-proxy-service` | harborka.qianfan123.com/component/card-server-proxy-rdb-setup | harborka.qianfan123.com/component/card-server-proxy-rdb-upgrade | MySQL, Oracle, Polardb, Polardb2 | - | Yes |
| 19 | cardserver | `harborka.qianfan123.com/card/cardserver` | `-` | `-` | Oracle | - | Yes |
| 20 | category-manage-service | `harborka.qianfan123.com/component/category-manage-service` | harborka.qianfan123.com/component/category-manage-rdb-setup | harborka.qianfan123.com/component/category-manage-rdb-upgrade | Oracle | - | Yes |
| 21 | chanjetconnector-server | `harborka.qianfan123.com/adi/chanjetconnector-server` | `-` | `-` | Oracle | - | No |
| 22 | chaoyang-service | `harborka.qianfan123.com/component/chaoyang-service` | harborka.qianfan123.com/component/chaoyang-rdb-setup | harborka.qianfan123.com/component/chaoyang-rdb-upgrade | Oracle | Redis | Yes |
| 23 | cxzy-service | `harborka.qianfan123.com/component/cxzy-service` | harborka.qianfan123.com/component/cxzy-rdb-setup | harborka.qianfan123.com/component/cxzy-rdb-upgrade | Oracle | Redis | Yes |
| 24 | datadocking-web | `harborka.qianfan123.com/hdpos46/datadocking-web` | harborka.qianfan123.com/hdpos46/datadocking-rdb-setup | harborka.qianfan123.com/hdpos46/datadocking-rdb-upgrade | PostgreSQL | - | Yes |
| 25 | datamanager-web | `harborka.qianfan123.com/component/datamanager-web` | harborka.qianfan123.com/component/datamanager-rdb-setup | harborka.qianfan123.com/component/datamanager-rdb-upgrade | MySQL, Oracle, Polardb, Polardb2 | MongoDB | Yes |
| 26 | dbwc-k3cloudconnector-server | `harborka.qianfan123.com/adi/dbwc-k3cloudconnector-server` | `-` | `-` | Oracle | - | Yes |
| 27 | dongyi-service | `harborka.qianfan123.com/component/dongyi-service` | harborka.qianfan123.com/component/dongyi-rdb-setup | harborka.qianfan123.com/component/dongyi-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 28 | door-service | `harborka.qianfan123.com/hdpos46/door-service` | harborka.qianfan123.com/hdpos46/door-rdb-setup | harborka.qianfan123.com/hdpos46/door-rdb-upgrade | Oracle | - | Yes |
| 29 | dqsh-service | `harborka.qianfan123.com/hdpos46/dqsh-service` | harborka.qianfan123.com/hdpos46/dqsh-rdb-setup | harborka.qianfan123.com/hdpos46/dqsh-rdb-upgrade | Oracle | Redis | Yes |
| 30 | dts-store | `harborka.qianfan123.com/dts-store/dts-store` | harborka.qianfan123.com/dts-store/dts-store-rdb-setup | harborka.qianfan123.com/dts-store/dts-store-rdb-upgrade | Oracle | License | Yes |
| 31 | dts-store-wanda | `harborka.qianfan123.com/dts-store/dts-store-wanda` | harborka.qianfan123.com/dts-store/dts-store-wanda-rdb-setup | harborka.qianfan123.com/dts-store/dts-store-wanda-rdb-upgrade | Oracle | License | Yes |
| 32 | eaccount-server | `harborka.qianfan123.com/component/eaccount-server` | `-` | `-` | Oracle, Polardb, Polardb2 | - | No |
| 33 | easconnector2-server | `harborka.qianfan123.com/adi/easconnector2-server` | `-` | `-` | - | - | Yes |
| 34 | erp-mas2-transfer-server | `harbor.qianfan123.com/mas/erp-mas2-transfer-server` | `-` | `-` | Oracle | - | No |
| 35 | fas-h6-transfer-service | `harborka.qianfan123.com/hdpos46/fas-h6-transfer-service` | harborka.qianfan123.com/hdpos46/fas-h6-transfer-rdb-setup | harborka.qianfan123.com/hdpos46/fas-h6-transfer-rdb-upgrade | Oracle, Polardb | - | Yes |
| 36 | fas-spider-service | `harborka.qianfan123.com/hdpos46/fas-spider-service` | harborka.qianfan123.com/hdpos46/fas-spider-rdb-setup | harborka.qianfan123.com/hdpos46/fas-spider-rdb-upgrade | Oracle | - | Yes |
| 37 | fwd-store-server | `harborka.qianfan123.com/hdpos46/fwd-store-server` | `-` | `-` | Oracle | - | Yes |
| 38 | gem-service | `harborka.qianfan123.com/hdpos46/gem-service` | `-` | `-` | MySQL, Oracle, Polardb, Polardb2, PostgreSQL | OSS, Redis | Yes |
| 39 | gssm-service | `harborka.qianfan123.com/component/gssm-service` | harborka.qianfan123.com/component/gssm-rdb-setup | harborka.qianfan123.com/component/gssm-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 40 | guoda-service | `harborka.qianfan123.com/component/guoda-service` | harborka.qianfan123.com/component/guoda-rdb-setup | harborka.qianfan123.com/component/guoda-rdb-upgrade | Oracle | Redis | Yes |
| 41 | gykh-u8connector-server | `harborka.qianfan123.com/adi/gykh-u8connector-server` | `-` | `-` | Oracle | - | Yes |
| 42 | h4cs-dist | `harborka.qianfan123.com/hdpos46/h4cs-dist` | harborka.qianfan123.com/hdpos46/h4cs-rdb-setup | harborka.qianfan123.com/hdpos46/h4cs-rdb-upgrade | Oracle, Polardb, Polardb2, PostgreSQL | Redis, ZooKeeper | Yes |
| 43 | h4cs-syncdata-service | `harborka.qianfan123.com/hdpos46/h4cs-syncdata-service` | harborka.qianfan123.com/hdpos46/h4cs-syncdata-rdb-setup | harborka.qianfan123.com/hdpos46/h4cs-syncdata-rdb-upgrade | Oracle, Polardb, Polardb2, PostgreSQL | Redis | No |
| 44 | h6-bankbus-service | `harborka.qianfan123.com/hdpos46/h6-bankbus-service` | harborka.qianfan123.com/hdpos46/h6-bankbus-rdb-setup | harborka.qianfan123.com/hdpos46/h6-bankbus-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 45 | h6-baozun-service | `harborka.qianfan123.com/hdpos46/h6-baozun-service` | harborka.qianfan123.com/hdpos46/h6-baozun-rdb-setup | harborka.qianfan123.com/hdpos46/h6-baozun-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 46 | h6-best-service | `harborka.qianfan123.com/hdpos46/h6-best-service` | harborka.qianfan123.com/hdpos46/h6-best-rdb-setup | harborka.qianfan123.com/hdpos46/h6-best-rdb-upgrade | Oracle | Redis | Yes |
| 47 | h6-boke-service | `harborka.qianfan123.com/hdpos46/h6-boke-service` | harborka.qianfan123.com/hdpos46/h6-boke-rdb-setup | harborka.qianfan123.com/hdpos46/h6-boke-rdb-upgrade | Oracle, PostgreSQL | Redis | Yes |
| 48 | h6-chanjet-service | `harborka.qianfan123.com/hdpos46/h6-chanjet-service` | harborka.qianfan123.com/hdpos46/h6-chanjet-rdb-setup | harborka.qianfan123.com/hdpos46/h6-chanjet-rdb-upgrade | Oracle | Redis | Yes |
| 49 | h6-cloudfund-service | `harborka.qianfan123.com/hdpos46/h6-cloudfund-service` | harborka.qianfan123.com/hdpos46/h6-cloudfund-rdb-setup | harborka.qianfan123.com/hdpos46/h6-cloudfund-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 50 | h6-crm-service | `harborka.qianfan123.com/hdpos46/h6-crm-service` | harborka.qianfan123.com/hdpos46/h6-crm-rdb-setup | harborka.qianfan123.com/hdpos46/h6-crm-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 51 | h6-flux-service | `harborka.qianfan123.com/hdpos46/h6-flux-service` | harborka.qianfan123.com/hdpos46/h6-flux-rdb-setup | harborka.qianfan123.com/hdpos46/h6-flux-rdb-upgrade | Oracle | Redis | Yes |
| 52 | h6-flux2-service | `harborka.qianfan123.com/hdpos46/h6-flux2-service` | harborka.qianfan123.com/hdpos46/h6-flux2-rdb-setup | harborka.qianfan123.com/hdpos46/h6-flux2-rdb-upgrade | Oracle | Redis | Yes |
| 53 | h6-greeneryfruit-service | `harborka.qianfan123.com/hdpos46/h6-greeneryfruit-service` | harborka.qianfan123.com/hdpos46/h6-greeneryfruit-rdb-setup | harborka.qianfan123.com/hdpos46/h6-greeneryfruit-rdb-upgrade | Oracle | Redis | Yes |
| 54 | h6-haikang-service | `harborka.qianfan123.com/hdpos46/h6-haikang-service` | harborka.qianfan123.com/hdpos46/h6-haikang-rdb-setup | harborka.qianfan123.com/hdpos46/h6-haikang-rdb-upgrade | Oracle | Redis | Yes |
| 55 | h6-ias-transfer | `harbor.qianfan123.com/baas/h6-ias-transfer` | `-` | `-` | Oracle | - | No |
| 56 | h6-inv-service | `harborka.qianfan123.com/hdpos46/h6-inv-service` | harborka.qianfan123.com/hdpos46/h6-inv-rdb-setup | harborka.qianfan123.com/hdpos46/h6-inv-rdb-upgrade | Oracle, Polardb, Polardb2 | - | Yes |
| 57 | h6-invoice-service | `harborka.qianfan123.com/hdpos46/h6-invoice-service` | harborka.qianfan123.com/hdpos46/h6-invoice-rdb-setup | harborka.qianfan123.com/hdpos46/h6-invoice-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | No |
| 58 | h6-jd-eclp-service | `harborka.qianfan123.com/hdpos46/h6-jd-eclp-service` | harborka.qianfan123.com/hdpos46/h6-jd-eclp-rdb-setup | harborka.qianfan123.com/hdpos46/h6-jd-eclp-rdb-upgrade | Oracle | Redis | Yes |
| 59 | h6-jdl-service | `harborka.qianfan123.com/hdpos46/h6-jdl-service` | harborka.qianfan123.com/hdpos46/h6-jdl-rdb-setup | harborka.qianfan123.com/hdpos46/h6-jdl-rdb-upgrade | Oracle | Redis | Yes |
| 60 | h6-k3cloud-service | `harborka.qianfan123.com/hdpos46/h6-k3cloud-service` | harborka.qianfan123.com/hdpos46/h6-k3cloud-rdb-setup | harborka.qianfan123.com/hdpos46/h6-k3cloud-rdb-upgrade | Oracle | Redis | Yes |
| 61 | h6-kingdee-xinghan-service | `harborka.qianfan123.com/hdpos46/h6-kingdee-xinghan-service` | harborka.qianfan123.com/hdpos46/h6-kingdee-xinghan-rdb-setup | harborka.qianfan123.com/hdpos46/h6-kingdee-xinghan-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 62 | h6-mas-transfer-server | `harbor.qianfan123.com/mas/h6-mas-transfer-server` | `-` | `-` | Oracle | - | No |
| 63 | h6-netsuite-service | `harborka.qianfan123.com/hdpos46/h6-netsuite-service` | harborka.qianfan123.com/hdpos46/h6-netsuite-rdb-setup | harborka.qianfan123.com/hdpos46/h6-netsuite-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 64 | h6-openapi-service | `harborka.qianfan123.com/hdpos46/h6-openapi-service` | harborka.qianfan123.com/hdpos46/openapi-rdb-setup | harborka.qianfan123.com/hdpos46/openapi-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 65 | h6-openapi2-service | `harborka.qianfan123.com/hdpos46/h6-openapi2-service` | harborka.qianfan123.com/hdpos46/h6-openapi2-rdb-setup | harborka.qianfan123.com/hdpos46/h6-openapi2-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 66 | h6-ovopark-service | `harborka.qianfan123.com/hdpos46/h6-ovopark-service` | harborka.qianfan123.com/hdpos46/h6-ovopark-rdb-setup | harborka.qianfan123.com/hdpos46/h6-ovopark-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 67 | h6-pms-service | `harborka.qianfan123.com/hdpos46/h6-pms-service` | harborka.qianfan123.com/hdpos46/h6-pms-rdb-setup | harborka.qianfan123.com/hdpos46/h6-pms-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 68 | h6-qimen-service | `harborka.qianfan123.com/hdpos46/h6-qimen-service` | harborka.qianfan123.com/hdpos46/h6-qimen-rdb-setup | harborka.qianfan123.com/hdpos46/h6-qimen-rdb-upgrade | Oracle | Redis | Yes |
| 69 | h6-qnh-service | `harborka.qianfan123.com/hdpos46/h6-qnh-service` | harborka.qianfan123.com/hdpos46/h6-qnh-rdb-setup | harborka.qianfan123.com/hdpos46/h6-qnh-rdb-upgrade | Oracle | Redis | Yes |
| 70 | h6-sop-service | `harborka.qianfan123.com/hdpos46/h6-sop-service` | harborka.qianfan123.com/hdpos46/h6-sop-rdb-setup | harborka.qianfan123.com/hdpos46/h6-sop-rdb-upgrade | Oracle, Polardb | - | Yes |
| 71 | h6-ssd-service | `harborka.qianfan123.com/hdpos46/h6-ssd-service` | harborka.qianfan123.com/hdpos46/h6-ssd-rdb-setup | harborka.qianfan123.com/hdpos46/h6-ssd-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 72 | h6-to-uni | `harbor.qianfan123.com/uni/h6-to-uni` | `-` | `-` | Oracle, Polardb | - | No |
| 73 | h6-tobacco-cis-service | `harborka.qianfan123.com/hdpos46/h6-tobacco-cis-service` | harborka.qianfan123.com/hdpos46/h6-tobacco-cis-rdb-setup | harborka.qianfan123.com/hdpos46/h6-tobacco-cis-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 74 | h6-tobacco-cpos-service | `harborka.qianfan123.com/hdpos46/h6-tobacco-cpos-service` | harborka.qianfan123.com/hdpos46/tobaccocpos-rdb-setup | harborka.qianfan123.com/hdpos46/tobaccocpos-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 75 | h6-tobacco-ncm-service | `harborka.qianfan123.com/hdpos46/h6-tobacco-ncm-service` | harborka.qianfan123.com/hdpos46/h6-tobacco-ncm-rdb-setup | harborka.qianfan123.com/hdpos46/h6-tobacco-ncm-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 76 | h6-vendor-service | `harborka.qianfan123.com/hdpos46/h6-vendor-service` | harborka.qianfan123.com/hdpos46/h6-vendor-rdb-setup | harborka.qianfan123.com/hdpos46/h6-vendor-rdb-upgrade | Oracle | Redis | Yes |
| 77 | h6-wanwei-service | `harborka.qianfan123.com/hdpos46/h6-wanwei-service` | harborka.qianfan123.com/hdpos46/h6-wanwei-rdb-setup | harborka.qianfan123.com/hdpos46/h6-wanwei-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 78 | h6-wms-service | `harborka.qianfan123.com/hdpos46/h6-wms-service` | harborka.qianfan123.com/hdpos46/h6-wms-rdb-setup | harborka.qianfan123.com/hdpos46/h6-wms-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 79 | h6-wos-service | `harborka.qianfan123.com/hdpos46/h6-wos-service` | harborka.qianfan123.com/hdpos46/h6-wos-rdb-setup | harborka.qianfan123.com/hdpos46/h6-wos-rdb-upgrade | Oracle, Polardb, Polardb2 | OSS | Yes |
| 80 | h6-ylz-service | `harborka.qianfan123.com/hdpos46/h6-ylz-service` | harborka.qianfan123.com/hdpos46/h6-ylz-rdb-setup | harborka.qianfan123.com/hdpos46/h6-ylz-rdb-upgrade | Oracle | Redis | Yes |
| 81 | h6-yonbip-service | `harborka.qianfan123.com/hdpos46/h6-yonbip-service` | harborka.qianfan123.com/hdpos46/h6-yonbip-rdb-setup | harborka.qianfan123.com/hdpos46/h6-yonbip-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 82 | h6-youzan-service | `harborka.qianfan123.com/hdpos46/h6-youzan-service` | harborka.qianfan123.com/hdpos46/h6-youzan-rdb-setup | harborka.qianfan123.com/hdpos46/h6-youzan-rdb-upgrade | Oracle | Redis | Yes |
| 83 | h6-yzvcm-service | `harborka.qianfan123.com/component/h6-yzvcm-service` | harborka.qianfan123.com/component/h6-yzvcm-rdb-setup | harborka.qianfan123.com/component/h6-yzvcm-rdb-upgrade | Oracle | Redis | Yes |
| 84 | h6cadi-service | `harbor.qianfan123.com/adi/h6cadi-service` | harbor.qianfan123.com/adi/h6cadi-rdb-setup | harbor.qianfan123.com/adi/h6cadi-rdb-upgrade | Oracle | Redis | Yes |
| 85 | h6cadi-web-service | `harbor.qianfan123.com/adi/h6cadi-web-service` | `-` | `-` | Oracle | Redis | Yes |
| 86 | hai-web-ui | `harbor.qianfan123.com/ka-sail/hai-web-ui` | `-` | `-` | Oracle | Nginx | No |
| 87 | haichat-web-ui | `harbor.qianfan123.com/ka-sail/haichat-web-ui` | `-` | `-` | Oracle | Nginx | No |
| 88 | haigang-service | `harborka.qianfan123.com/component/haigang-service` | harborka.qianfan123.com/component/haigang-rdb-setup | harborka.qianfan123.com/component/haigang-rdb-upgrade | Oracle | Redis | Yes |
| 89 | hca | `harbor.qianfan123.com/toolset/hca` | `-` | `-` | Oracle | - | No |
| 90 | hdportal | `harbor.qianfan123.com/ka-sail/hdportal` | harbor.qianfan123.com/ka-sail/zl-portal-rdb-setup | harbor.qianfan123.com/ka-sail/zl-portal-rdb-upgrade | MySQL, Oracle, Polardb, Polardb2, PostgreSQL | - | No |
| 91 | hd-portal-web-ui | `harbor.qianfan123.com/ka-sail/hd-portal-web-ui` | `-` | `-` | Oracle | Nginx | No |
| 92 | hdpos-ali-o2o-service | `harborka.qianfan123.com/hdpos46/hdpos-ali-o2o-service` | harborka.qianfan123.com/hdpos46/hdpos-ali-o2o-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos-ali-o2o-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 93 | hdpos-oas-service | `harborka.qianfan123.com/hdpos46/hdpos-oas-service` | harborka.qianfan123.com/hdpos46/hdpos-oas-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos-oas-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 94 | hdpos-stdcomponent-service | `harborka.qianfan123.com/component/hdpos-stdcomponent-service` | `-` | `-` | Oracle, Polardb, Polardb2 | Redis | Yes |
| 95 | hdpos-wms-service | `harborka.qianfan123.com/hdpos46/hdpos-wms-service` | harborka.qianfan123.com/hdpos46/hdpos-wms-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos-wms-rdb-upgrade | Oracle, Polardb, Polardb2 | MongoDB, Redis | Yes |
| 96 | hdpos4-dist | `harborka.qianfan123.com/hdpos46/hdpos4-dist` | harborka.qianfan123.com/hdpos46/hdpos4-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos4-rdb-upgrade | Oracle, Polardb, Polardb2 | OSS, Redis | Yes |
| 97 | hdpos4-mci-dist | `harborka.qianfan123.com/hdpos46/hdpos4-mci-dist` | harborka.qianfan123.com/hdpos46/hdpos4-mci-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos4-mci-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 98 | hdpos4-mincha-dist | `harborka.qianfan123.com/hdpos46/hdpos4-mincha-dist` | harborka.qianfan123.com/hdpos46/hdpos4-mincha-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos4-mincha-rdb-upgrade | Oracle | Redis | Yes |
| 99 | hdpos4-nome-dist | `harborka.qianfan123.com/hdpos46/hdpos4-nome-dist` | harborka.qianfan123.com/hdpos46/hdpos4-nome-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos4-nome-rdb-upgrade | Oracle, Polardb | MongoDB, OSS, Redis | Yes |
| 100 | hdpos4-wanda-dist | `harborka.qianfan123.com/hdpos46/hdpos4-wanda-dist` | harborka.qianfan123.com/hdpos46/hdpos4-wanda-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos4-wanda-rdb-upgrade | Oracle | Redis | Yes |
| 101 | hdpos4-wanda-invoice-server | `harborka.qianfan123.com/hdpos46/hdpos4-wanda-invoice-server` | harborka.qianfan123.com/hdpos46/hdpos4-wanda-invoice-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos4-wanda-invoice-rdb-upgrade | Oracle | Elasticsearch, Redis | Yes |
| 102 | hdpos4-wanda-mpos-server | `harborka.qianfan123.com/hdpos46/hdpos4-wanda-mpos-server` | harborka.qianfan123.com/hdpos46/hdpos4-wanda-mpos-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos4-wanda-mpos-rdb-upgrade | Oracle | Redis, Sentinel | Yes |
| 103 | hdpos6-etl-service | `harborka.qianfan123.com/hdpos46/hdpos6-etl-service` | harborka.qianfan123.com/hdpos46/hdpos6-etl-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos6-etl-rdb-upgrade | Oracle | - | Yes |
| 104 | hdpos6-mdata-service | `harborka.qianfan123.com/hdpos46/hdpos6-mdata-service` | harborka.qianfan123.com/hdpos46/hdpos6-mdata-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos6-mdata-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 105 | hdpos6-notice-service | `harborka.qianfan123.com/hdpos46/hdpos6-notice-service` | harborka.qianfan123.com/hdpos46/hdpos6-notice-rdb-setup | harborka.qianfan123.com/hdpos46/hdpos6-notice-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 106 | hedwig-server | `harborka.qianfan123.com/hdpos46/hedwig-server` | harborka.qianfan123.com/hdpos46/hedwig-rdb-setup | harborka.qianfan123.com/hdpos46/hedwig-rdb-upgrade | Oracle, Polardb, Polardb2 | - | Yes |
| 107 | hlcoming-service | `harborka.qianfan123.com/component/hlcoming-service` | harborka.qianfan123.com/component/hlcoming-rdb-setup | harborka.qianfan123.com/component/hlcoming-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 108 | hsjd-service | `harborka.qianfan123.com/component/hsjd-service` | harborka.qianfan123.com/component/hsjd-rdb-setup | harborka.qianfan123.com/component/hsjd-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 109 | hsyp-service | `harborka.qianfan123.com/hdpos46/hsyp-service` | harborka.qianfan123.com/hdpos46/hsyp-rdb-setup | harborka.qianfan123.com/hdpos46/hsyp-rdb-upgrade | Oracle | Redis | Yes |
| 110 | init-tool | `harborka.qianfan123.com/component/init-tool` | `-` | `-` | Oracle, Polardb, Polardb2 | - | Yes |
| 111 | jcrm-server-card | `harborka.qianfan123.com/jcrm/jcrm-server-card` | `-` | `-` | Oracle | ZooKeeper | No |
| 112 | jieqiang-service | `harborka.qianfan123.com/component/jieqiang-service` | harborka.qianfan123.com/component/jieqiang-rdb-setup | harborka.qianfan123.com/component/jieqiang-rdb-upgrade | Oracle | Redis | Yes |
| 113 | jindie-yxcconnector-server | `harborka.qianfan123.com/adi/jindie-yxcconnector-server` | `-` | `-` | Oracle | - | Yes |
| 114 | jiuduorouduo-service | `harborka.qianfan123.com/component/jiuduorouduo-service` | harborka.qianfan123.com/component/jiuduorouduo-rdb-setup | harborka.qianfan123.com/component/jiuduorouduo-rdb-upgrade | Oracle | Redis | Yes |
| 115 | jjl-service | `harborka.qianfan123.com/component/jjl-service` | harborka.qianfan123.com/component/jjl-rdb-setup | harborka.qianfan123.com/component/jjl-rdb-upgrade | Oracle | Redis | Yes |
| 116 | jjleg-service | `harborka.qianfan123.com/hdpos46/jjleg-service` | harborka.qianfan123.com/hdpos46/jjleg-rdb-setup | harborka.qianfan123.com/hdpos46/jjleg-rdb-upgrade | Oracle | Redis | Yes |
| 117 | jlgougo-service | `harborka.qianfan123.com/component/jlgougo-service` | harborka.qianfan123.com/component/jlgougo-rdb-setup | harborka.qianfan123.com/component/jlgougo-rdb-upgrade | Oracle | Redis | Yes |
| 118 | jposbo | `harborka.qianfan123.com/jposbo/jposbo` | `-` | `-` | MySQL, Oracle, Polardb, Polardb2 | License | Yes |
| 119 | k3cloudconnector-server | `harborka.qianfan123.com/adi/k3cloudconnector-server` | `-` | `-` | Oracle | - | No |
| 120 | kaihui-service | `harborka.qianfan123.com/component/kaihui-service` | harborka.qianfan123.com/component/kaihui-rdb-setup | harborka.qianfan123.com/component/kaihui-rdb-upgrade | Oracle | Redis | Yes |
| 121 | keduo-web | `harborka.qianfan123.com/component/keduo-web` | harborka.qianfan123.com/component/keduo-rdb-setup | harborka.qianfan123.com/component/keduo-rdb-upgrade | Oracle | - | Yes |
| 122 | kingdee-eas-service | `harborka.qianfan123.com/hdpos46/kingdee-eas-service` | harborka.qianfan123.com/hdpos46/kingdee-eas-rdb-setup | harborka.qianfan123.com/hdpos46/kingdee-eas-rdb-upgrade | Oracle | Redis | Yes |
| 123 | kinglomo-service | `harborka.qianfan123.com/component/kinglomo-service` | harborka.qianfan123.com/component/kinglomo-rdb-setup | harborka.qianfan123.com/component/kinglomo-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 124 | ksx-service | `harborka.qianfan123.com/component/ksx-service` | harborka.qianfan123.com/component/ksx-rdb-setup | harborka.qianfan123.com/component/ksx-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 125 | ldap-to-uni | `harbor.qianfan123.com/uni/ldap-to-uni` | `-` | `-` | MySQL, Oracle, Polardb, Polardb2, PostgreSQL | - | No |
| 126 | ledoujia-service | `harborka.qianfan123.com/component/ledoujia-service` | harborka.qianfan123.com/component/ledoujia-rdb-setup | harborka.qianfan123.com/component/ledoujia-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 127 | lianhua-web | `harborka.qianfan123.com/component/lianhua-web` | harborka.qianfan123.com/component/lianhua-rdb-setup | harborka.qianfan123.com/component/lianhua-rdb-upgrade | Oracle, Polardb | - | Yes |
| 128 | linji-web | `harborka.qianfan123.com/component/linji-web` | harborka.qianfan123.com/component/linji-rdb-setup | harborka.qianfan123.com/component/linji-rdb-upgrade | Oracle | - | Yes |
| 129 | lmt-service | `harborka.qianfan123.com/component/lmt-service` | harborka.qianfan123.com/component/lmt-rdb-setup | harborka.qianfan123.com/component/lmt-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 130 | lsym-service | `harborka.qianfan123.com/component/lsym-service` | harborka.qianfan123.com/component/lsym-rdb-setup | harborka.qianfan123.com/component/lsym-rdb-upgrade | Oracle | Redis | Yes |
| 131 | lvhang-service | `harborka.qianfan123.com/hdpos46/lvhang-service` | harborka.qianfan123.com/hdpos46/lvhang-rdb-setup | harborka.qianfan123.com/hdpos46/lvhang-rdb-upgrade | Oracle | Redis | Yes |
| 132 | mas2c-transfer | `harbor.qianfan123.com/mas/mas2c-transfer` | `-` | `-` | MySQL, Oracle, Polardb | - | Yes |
| 133 | mas2c-transfer-nec-etl | `harbor.qianfan123.com/oas/mas2c-transfer-nec-etl` | `-` | `-` | MySQL, Oracle | - | No |
| 134 | mc-home-service | `harborka.qianfan123.com/hdpos46/mc-home-service` | harborka.qianfan123.com/hdpos46/mc-home-rdb-setup | harborka.qianfan123.com/hdpos46/mc-home-rdb-upgrade | Oracle | Redis | Yes |
| 135 | mc-scm-wowcolour-service | `harborka.qianfan123.com/hdpos46/mc-scm-wowcolour-service` | harborka.qianfan123.com/hdpos46/mc-scm-wowcolour-rdb-setup | harborka.qianfan123.com/hdpos46/mc-scm-wowcolour-rdb-upgrade | Oracle | - | Yes |
| 136 | mincha-service | `harborka.qianfan123.com/component/mincha-service` | harborka.qianfan123.com/component/mincha-rdb-setup | harborka.qianfan123.com/component/mincha-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 137 | mkh-account-service | `harborka.qianfan123.com/hdpos46/mkh-account-service` | harborka.qianfan123.com/hdpos46/mkh-account-rdb-setup | harborka.qianfan123.com/hdpos46/mkh-account-rdb-upgrade | Oracle | Redis | Yes |
| 138 | mkh-connector-service | `harborka.qianfan123.com/hdpos46/mkh-connector-service` | harborka.qianfan123.com/hdpos46/mkh-connector-rdb-setup | harborka.qianfan123.com/hdpos46/mkh-connector-rdb-upgrade | Oracle | - | Yes |
| 139 | mkh-dms-service | `harborka.qianfan123.com/hdpos46/mkh-dms-service` | harborka.qianfan123.com/hdpos46/mkh-dms-rdb-setup | harborka.qianfan123.com/hdpos46/mkh-dms-rdb-upgrade | Oracle | Redis | Yes |
| 140 | mkh-ec-service | `harborka.qianfan123.com/hdpos46/mkh-ec-service` | harborka.qianfan123.com/hdpos46/mkh-ec-rdb-setup | harborka.qianfan123.com/hdpos46/mkh-ec-rdb-upgrade | Oracle | - | Yes |
| 141 | mkh-mas-transfer-server | `harbor.qianfan123.com/mas/mkh-mas-transfer-server` | `-` | `-` | Oracle, Polardb, Polardb2 | - | No |
| 142 | mkh-pur-service | `harborka.qianfan123.com/hdpos46/mkh-pur-service` | harborka.qianfan123.com/hdpos46/mkh-pur-rdb-setup | harborka.qianfan123.com/hdpos46/mkh-pur-rdb-upgrade | Oracle | Redis | Yes |
| 143 | mkh-screen-service | `harborka.qianfan123.com/hdpos46/mkh-screen-service` | harborka.qianfan123.com/hdpos46/mkh-screen-rdb-setup | harborka.qianfan123.com/hdpos46/mkh-screen-rdb-upgrade | Oracle | Redis | Yes |
| 144 | mkh-web | `harborka.qianfan123.com/component/mkh-web` | harborka.qianfan123.com/component/mkh-rdb-setup | harborka.qianfan123.com/component/mkh-rdb-upgrade | MySQL, Oracle | MongoDB, Redis | Yes |
| 145 | mpa-service | `harborka.qianfan123.com/hdpos46/mpa-service` | harborka.qianfan123.com/hdpos46/mpa-rdb-setup | harborka.qianfan123.com/hdpos46/mpa-rdb-upgrade | Oracle | OSS, Redis | Yes |
| 146 | mtj-service | `harborka.qianfan123.com/component/mtj-service` | harborka.qianfan123.com/component/mtj-rdb-setup | harborka.qianfan123.com/component/mtj-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 147 | myt-dm-service | `harborka.qianfan123.com/hdpos46/myt-dm-service` | harborka.qianfan123.com/hdpos46/myt-dm-rdb-setup | harborka.qianfan123.com/hdpos46/myt-dm-rdb-upgrade | Oracle | Redis | Yes |
| 148 | ncconnector-server | `harborka.qianfan123.com/adi/ncconnector-server` | `-` | `-` | Oracle | - | Yes |
| 149 | openapi-doc-service | `harborka.qianfan123.com/hdpos46/openapi-doc-service` | `-` | `-` | Oracle, Polardb, Polardb2 | - | Yes |
| 150 | otter-r3-std | `harborka.qianfan123.com/otter/otter-r3-std` | `-` | `-` | Oracle, Polardb | - | Yes |
| 151 | otter-mcyp-gn-sap | `harborka.qianfan123.com/otter/otter-mcyp-gn-sap` | `-` | `-` | Polardb | - | Yes |
| 152 | otter-r3-sap | `harborka.qianfan123.com/otter/otter-r3-sap` | `-` | `-` | Oracle | - | Yes |
| 153 | panther | `harborka.qianfan123.com/dts-store/panther` | harborka.qianfan123.com/dts-store/panther-rdb-setup | harborka.qianfan123.com/dts-store/panther-rdb-upgrade | Oracle | - | No |
| 154 | panther-dts-server | `harborka.qianfan123.com/dts-store/panther-dts-server` | `-` | `-` | Oracle, Polardb, Polardb2, PostgreSQL | - | Yes |
| 155 | panther-taskweb | `harborka.qianfan123.com/dts-store/panther-taskweb` | `-` | `-` | Oracle, Polardb, Polardb2, PostgreSQL | - | Yes |
| 156 | pasodata_consumer | `harborka.qianfan123.com/dc/pasodata_consumer` | `-` | `-` | Oracle, PostgreSQL | Kafka, Redis | No |
| 157 | pasodata_producer | `harborka.qianfan123.com/dc/pasodata_producer` | `-` | `-` | Oracle | Kafka | No |
| 158 | pasoreport-web | `harborka.qianfan123.com/component/pasoreport-web` | harborka.qianfan123.com/component/pasoreport-rdb-setup | harborka.qianfan123.com/component/pasoreport-rdb-upgrade | MySQL, Oracle, Polardb, Polardb2 | Redis | Yes |
| 159 | pfs | `harborka.qianfan123.com/pfs/pfs` | `-` | `-` | Oracle | - | Yes |
| 160 | pos-pms-server-service | `harborka.qianfan123.com/component/pos-pms-server-service` | harborka.qianfan123.com/component/pos-pms-server-rdb-setup | harborka.qianfan123.com/component/pos-pms-server-rdb-upgrade | MySQL | - | Yes |
| 161 | ppmt-intl-service | `harborka.qianfan123.com/component/ppmt-intl-service` | harborka.qianfan123.com/component/ppmt-intl-rdb-setup | harborka.qianfan123.com/component/ppmt-intl-rdb-upgrade | Oracle | Redis | Yes |
| 162 | ppmt-product-service | `harborka.qianfan123.com/component/ppmt-product-service` | `-` | `-` | Oracle | - | No |
| 163 | ppmt-product-web-service | `harborka.qianfan123.com/component/ppmt-product-web-service` | harborka.qianfan123.com/component/ppmt-product-rdb-setup | harborka.qianfan123.com/component/ppmt-product-rdb-upgrade | Oracle | - | No |
| 164 | ppmt-web | `harborka.qianfan123.com/component/ppmt-web` | harborka.qianfan123.com/component/ppmt-rdb-setup | harborka.qianfan123.com/component/ppmt-rdb-upgrade | Oracle | Redis | Yes |
| 165 | ptxs-service | `harborka.qianfan123.com/component/ptxs-service` | harborka.qianfan123.com/component/ptxs-rdb-setup | harborka.qianfan123.com/component/ptxs-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 166 | quanfuyuan-service | `harborka.qianfan123.com/component/quanfuyuan-service` | harborka.qianfan123.com/component/quanfuyuan-rdb-setup | harborka.qianfan123.com/component/quanfuyuan-rdb-upgrade | Oracle | Redis | Yes |
| 167 | ras-h4-transfer | `harbor.qianfan123.com/baas/ras-h4-transfer` | `-` | `-` | Oracle, Polardb, Polardb2 | - | No |
| 168 | router | `harborka.qianfan123.com/smd/router` | `-` | `-` | PostgreSQL | - | No |
| 169 | rtmark-dms-service | `harborka.qianfan123.com/component/rtmark-dms-service` | harborka.qianfan123.com/component/rtmark-dms-rdb-setup | harborka.qianfan123.com/component/rtmark-dms-rdb-upgrade | Oracle | Redis | Yes |
| 170 | sanfu-service | `harborka.qianfan123.com/component/sanfu-service` | harborka.qianfan123.com/component/sanfu-rdb-setup | harborka.qianfan123.com/component/sanfu-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 171 | sanrio-service | `harborka.qianfan123.com/component/sanrio-service` | harborka.qianfan123.com/component/sanrio-rdb-setup | harborka.qianfan123.com/component/sanrio-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 172 | sanxin-service | `harborka.qianfan123.com/hdpos46/sanxin-service` | harborka.qianfan123.com/hdpos46/sanxin-rdb-setup | harborka.qianfan123.com/hdpos46/sanxin-rdb-upgrade | Oracle | Redis | Yes |
| 173 | sapconnector-server | `harborka.qianfan123.com/adi/sapconnector-server` | `-` | `-` | Oracle | - | Yes |
| 174 | sas-h4-transfer | `harbor.qianfan123.com/baas/sas-h4-transfer` | `-` | `-` | Oracle, Polardb, Polardb2, PostgreSQL | - | No |
| 175 | sec-wms-service | `harborka.qianfan123.com/hdpos46/sec-wms-service` | harborka.qianfan123.com/hdpos46/sec-wms-rdb-setup | harborka.qianfan123.com/hdpos46/sec-wms-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 176 | sofomall-service | `harborka.qianfan123.com/hdpos46/sofomall-service` | harborka.qianfan123.com/hdpos46/sofomall-rdb-setup | harborka.qianfan123.com/hdpos46/sofomall-rdb-upgrade | Oracle | Redis | Yes |
| 177 | sos-service | `harborka.qianfan123.com/hdpos46/sos-service` | harborka.qianfan123.com/hdpos46/sos-rdb-setup | harborka.qianfan123.com/hdpos46/sos-rdb-upgrade | Oracle | Redis | Yes |
| 178 | sos-config-service | `harborka.qianfan123.com/hdpos46/sos-config-service` | harborka.qianfan123.com/hdpos46/sos-config-rdb-setup | harborka.qianfan123.com/hdpos46/sos-config-rdb-upgrade | Oracle, PostgreSQL | - | Yes |
| 179 | sos-h6-transfer-service | `harborka.qianfan123.com/hdpos46/sos-h6-transfer-service` | harborka.qianfan123.com/hdpos46/sos-h6-transfer-rdb-setup | harborka.qianfan123.com/hdpos46/sos-h6-transfer-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 180 | spms-h4-transfer | `harbor.qianfan123.com/pms/spms-h4-transfer` | `-` | `-` | Oracle | - | No |
| 181 | spms-hdpos-web | `harborka.qianfan123.com/component/spms-hdpos-web` | `-` | `-` | Oracle, Polardb, Polardb2 | Redis | Yes |
| 182 | spms-server | `harbor.qianfan123.com/pms/spms-server` | `-` | `-` | Oracle, Polardb | Redis | No |
| 183 | spms-stdpms-transfer | `harborka.qianfan123.com/pms/spms-stdpms-transfer` | `-` | `-` | Oracle, Polardb | - | No |
| 184 | spms-web | `harbor.qianfan123.com/pms/spms-web` | `-` | `-` | Oracle | Dubbo, Redis, ZooKeeper | No |
| 185 | spms-web-ui | `harbor.qianfan123.com/pms/spms-web-ui` | `-` | `-` | Oracle | Nginx | No |
| 186 | sungivenfoods-service | `harborka.qianfan123.com/component/sungivenfoods-service` | harborka.qianfan123.com/component/sungivenfoods-rdb-setup | harborka.qianfan123.com/component/sungivenfoods-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 187 | sydaojia-mas-transfer-server | `harbor.qianfan123.com/mas/sydaojia-mas-transfer-server` | `-` | `-` | Oracle | - | No |
| 188 | taobaowdk-service | `harborka.qianfan123.com/component/taobaowdk-service` | harborka.qianfan123.com/component/taobaowdk-rdb-setup | harborka.qianfan123.com/component/taobaowdk-rdb-upgrade | Oracle | - | Yes |
| 189 | tiaoma-service | `harborka.qianfan123.com/component/tiaoma-service` | harborka.qianfan123.com/component/tiaoma-rdb-setup | harborka.qianfan123.com/component/tiaoma-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 190 | tmallwdk-server | `harborka.qianfan123.com/component/tmallwdk-server` | harborka.qianfan123.com/component/tmallwdk-rdb-setup | harborka.qianfan123.com/component/tmallwdk-rdb-upgrade | Oracle | - | Yes |
| 191 | tmsh-u8connector-server | `harborka.qianfan123.com/adi/tmsh-u8connector-server` | `-` | `-` | Oracle | - | Yes |
| 192 | tobaccocpos-service | `harborka.qianfan123.com/hdpos46/tobaccocpos-service` | harborka.qianfan123.com/hdpos46/tobaccocpos-rdb-setup | harborka.qianfan123.com/hdpos46/tobaccocpos-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 193 | toys52-service | `harborka.qianfan123.com/component/toys52-service` | harborka.qianfan123.com/component/toys52-rdb-setup | harborka.qianfan123.com/component/toys52-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 194 | u8connector-server | `harborka.qianfan123.com/adi/u8connector-server` | `-` | `-` | Oracle | - | Yes |
| 195 | up-connector-service | `harborka.qianfan123.com/component/up-connector-service` | harborka.qianfan123.com/component/up-connector-rdb-setup | harborka.qianfan123.com/component/up-connector-rdb-upgrade | Oracle, Polardb, Polardb2 | - | Yes |
| 196 | vbs-service | `harborka.qianfan123.com/hdpos46/vbs-service` | harborka.qianfan123.com/hdpos46/vbs-rdb-setup | harborka.qianfan123.com/hdpos46/vbs-rdb-upgrade | Oracle, Polardb | OSS, Redis | Yes |
| 197 | vss-spider-server | `harborka.qianfan123.com/vss/vss-spider-server` | harborka.qianfan123.com/vss/vss-spider-rdb-setup | harborka.qianfan123.com/vss/vss-spider-rdb-upgrade | Oracle, Polardb, Polardb2 | - | Yes |
| 198 | vss-spider-46 | `harborka.qianfan123.com/vss/vss-spider-46` | harborka.qianfan123.com/vss/vss-spider-46-rdb-setup | harborka.qianfan123.com/vss/vss-spider-46-rdb-upgrade | Oracle | - | No |
| 199 | wds-service | `harborka.qianfan123.com/hdpos46/wds-service` | harborka.qianfan123.com/hdpos46/wds-rdb-setup | harborka.qianfan123.com/hdpos46/wds-rdb-upgrade | Oracle | Redis | Yes |
| 200 | wln-service | `harborka.qianfan123.com/component/wln-service` | harborka.qianfan123.com/component/wln-rdb-setup | harborka.qianfan123.com/component/wln-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 201 | wowcolour-service | `harborka.qianfan123.com/hdpos46/wowcolour-service` | harborka.qianfan123.com/hdpos46/wowcolour-rdb-setup | harborka.qianfan123.com/hdpos46/wowcolour-rdb-upgrade | Oracle | Redis | Yes |
| 202 | wrjh-service | `harborka.qianfan123.com/component/wrjh-service` | harborka.qianfan123.com/component/wrjh-rdb-setup | harborka.qianfan123.com/component/wrjh-rdb-upgrade | Oracle | Redis | Yes |
| 203 | wuchanrexuan-service | `harborka.qianfan123.com/component/wuchanrexuan-service` | harborka.qianfan123.com/component/wuchanrexuan-rdb-setup | harborka.qianfan123.com/component/wuchanrexuan-rdb-upgrade | Oracle, Polardb, Polardb2 | Redis | Yes |
| 204 | xmsf-service | `harborka.qianfan123.com/component/xmsf-service` | harborka.qianfan123.com/component/xmsf-rdb-setup | harborka.qianfan123.com/component/xmsf-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 205 | yc-mas-transfer-server | `harbor.qianfan123.com/mas/yc-mas-transfer-server` | `-` | `-` | Oracle | - | No |
| 206 | yiran-service | `harborka.qianfan123.com/component/yiran-service` | harborka.qianfan123.com/component/yiran-rdb-setup | harborka.qianfan123.com/component/yiran-rdb-upgrade | Oracle | Redis | Yes |
| 207 | yongyou-bipconnector-server | `harborka.qianfan123.com/adi/yongyou-bipconnector-server` | `-` | `-` | Oracle | - | Yes |
| 208 | yueyun-service | `harborka.qianfan123.com/component/yueyun-service` | harborka.qianfan123.com/component/yueyun-rdb-setup | harborka.qianfan123.com/component/yueyun-rdb-upgrade | Polardb | Redis | Yes |
| 209 | yz-service | `harborka.qianfan123.com/component/yz-service` | harborka.qianfan123.com/component/yz-rdb-setup | harborka.qianfan123.com/component/yz-rdb-upgrade | Oracle | Redis | Yes |
| 210 | zh-transfer-server | `harbor.qianfan123.com/mas/zh-transfer-server` | harbor.qianfan123.com/mas/zh-transfer-rdb-setup | harbor.qianfan123.com/mas/zh-transfer-rdb-upgrade | Oracle | - | No |
| 211 | zhenhua-service | `harborka.qianfan123.com/component/zhenhua-service` | harborka.qianfan123.com/component/zhenhua-rdb-setup | harborka.qianfan123.com/component/zhenhua-rdb-upgrade | Oracle | Redis | Yes |
| 212 | zhijing-service | `harborka.qianfan123.com/component/zhijing-service` | harborka.qianfan123.com/component/zhijing-rdb-setup | harborka.qianfan123.com/component/zhijing-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 213 | zhy-service | `harborka.qianfan123.com/component/zhy-service` | harborka.qianfan123.com/component/zhy-rdb-setup | harborka.qianfan123.com/component/zhy-rdb-upgrade | Oracle, Polardb | Redis | Yes |
| 214 | zjgssm-service | `harborka.qianfan123.com/component/zjgssm-service` | harborka.qianfan123.com/component/zjgssm-rdb-setup | harborka.qianfan123.com/component/zjgssm-rdb-upgrade | - | - | Yes |
| 215 | zjlhkk-server-service | `harborka.qianfan123.com/component/zjlhkk-server-service` | harborka.qianfan123.com/component/zjlhkk-server-rdb-setup | harborka.qianfan123.com/component/zjlhkk-server-rdb-upgrade | Oracle, Polardb, Polardb2 | - | Yes |
| 216 | zjzsh-service | `harborka.qianfan123.com/component/zjzsh-service` | harborka.qianfan123.com/component/zjzsh-rdb-setup | harborka.qianfan123.com/component/zjzsh-rdb-upgrade | Oracle | Redis | Yes |
| 217 | zl-portal-service | `harbor.qianfan123.com/ka-sail/zl-portal-service` | `-` | `-` | MySQL | OSS, Redis | No |
| 218 | zl-portal-sync | `harbor.qianfan123.com/ka-sail/zl-portal-sync` | `-` | `-` | Oracle, Polardb, Polardb2 | - | No |
| 219 | zl-portal-web-service | `harbor.qianfan123.com/ka-sail/zl-portal-web-service` | `-` | `-` | MySQL | OSS, Redis | No |
| 220 | zsl-service | `harborka.qianfan123.com/hdpos46/zsl-service` | harborka.qianfan123.com/hdpos46/zsl-rdb-setup | harborka.qianfan123.com/hdpos46/zsl-rdb-upgrade | Oracle | Redis | Yes |
| 221 | zzbz-u8connector-server | `harborka.qianfan123.com/adi/zzbz-u8connector-server` | `-` | `-` | Oracle | - | Yes |
customer_code,project,host_id,c_version,c_tags,image_name,docker_repository,app_type,c_port,c_managerport,c_jmxport,c_health_timeout
<input>,<input>,app-01,<input>,blue,hdpos4-dist,harborka.qianfan123.com/hdpos46/hdpos4-dist,backend,38180,39180,9744,45
<input>,<input>,app-01,<input>,blue,panther-dts-server,harborka.qianfan123.com/dts-store/panther-dts-server,backend,38480,39480,9755,30
<input>,<input>,app-01,<input>,blue,panther-taskweb,harborka.qianfan123.com/dts-store/panther-taskweb,backend,38380,39380,9754,30
<input>,<input>,app-01,<input>,blue,pasoreport-web,harborka.qianfan123.com/component/pasoreport-web,backend,38980,39980,9756,45
<input>,<input>,app-01,<input>,blue,gem-service,harborka.qianfan123.com/hdpos46/gem-service,backend,38105,39105,9787,15
<input>,<input>,app-01,<input>,blue,card-server-proxy-service,harborka.qianfan123.com/component/card-server-proxy-service,backend,38119,39119,37119,15
<input>,<input>,app-01,<input>,blue,h6-crm-service,harborka.qianfan123.com/hdpos46/h6-crm-service,backend,38169,39169,0,15
<input>,<input>,app-01,<input>,blue,spms-hdpos-web,harborka.qianfan123.com/component/spms-hdpos-web,backend,38115,39115,9752,15
<input>,<input>,app-01,<input>,blue,h6-openapi2-service,harborka.qianfan123.com/hdpos46/h6-openapi2-service,backend,38176,39176,9768,15
<input>,<input>,app-01,<input>,blue,openapi-doc-service,harborka.qianfan123.com/hdpos46/openapi-doc-service,backend,38210,39210,0,15
<input>,<input>,app-01,<input>,blue,jposbo,harborka.qianfan123.com/jposbo/<input>,backend,38280,39280,10001,30
<input>,<input>,app-01,<input>,blue,up-connector-service,harborka.qianfan123.com/component/up-connector-service,backend,38110,39110,9758,15
<input>,<input>,app-01,<input>,blue,init-tool,harborka.qianfan123.com/component/init-tool,backend,38093,0,0,15
<input>,<input>,app-01,<input>,blue,vss-spider-server,harborka.qianfan123.com/vss/vss-spider-server,backend,38081,39081,0,15
<input>,<input>,app-01,<input>,blue,sos-h6-transfer-service,harborka.qianfan123.com/hdpos46/sos-h6-transfer-service,backend,38135,39135,9773,15
<input>,<input>,app-01,<input>,blue,mkh-mas-transfer-server,harbor.qianfan123.com/mas/mkh-mas-transfer-server,backend,38201,39201,37201,15
<input>,<input>,app-01,<input>,blue,bpfm-app-service,harborka.qianfan123.com/component/bpfm-app-service,backend,38212,39212,9757,15
<input>,<input>,app-01,<input>,blue,h6-sop-service,harborka.qianfan123.com/hdpos46/h6-sop-service,backend,38172,39172,0,15
<input>,<input>,app-01,<input>,blue,hdpos6-notice-service,harborka.qianfan123.com/hdpos46/hdpos6-notice-service,backend,38136,39136,0,15
<input>,<input>,app-01,<input>,blue,hdpos-wms-service,harborka.qianfan123.com/hdpos46/hdpos-wms-service,backend,38118,39118,0,15
<input>,<input>,app-01,<input>,blue,h6-cloudfund-service,harborka.qianfan123.com/hdpos46/h6-cloudfund-service,backend,38197,39197,9807,15
<input>,<input>,app-01,<input>,blue,cadi-service,harbor.qianfan123.com/adi/cadi-service,backend,38259,39259,9799,15
<input>,<input>,app-01,<input>,blue,cadi-web-service,harbor.qianfan123.com/adi/cadi-web-service,backend,38260,39260,9800,15
<input>,<input>,app-01,<input>,blue,cadi-console-web-service,harbor.qianfan123.com/adi/cadi-console-web-service,backend,38261,39261,9801,15

<input>,<input>,app-01,<input>,blue,hdpos4-disttask,harborka.qianfan123.com/hdpos46/hdpos4-dist,backend,38180,39180,9744,45
<input>,<input>,app-01,<input>,blue,adi,harborka.qianfan123.com/adi/adi,backend,38144,0,0,15
<input>,<input>,app-01,<input>,blue,cardserver,harborka.qianfan123.com/card/cardserver,backend,38880,0,0,0
<input>,<input>,app-01,<input>,blue,hdpos4-nome-dist,harborka.qianfan123.com/hdpos46/hdpos4-nome-dist,backend,38180,39180,9744,45
<input>,<input>,app-01,<input>,blue,c3-dist,harborka.qianfan123.com/c3/c3-dist,backend,38180,39180,9744,45
<input>,<input>,app-01,<input>,blue,h4cs-dist,harborka.qianfan123.com/hdpos46/h4cs-dist,backend,38082,39082,9745,45
<input>,<input>,app-01,<input>,blue,h4cs-distb1,harborka.qianfan123.com/hdpos46/h4cs-dist,backend,38072,39072,9645,45
<input>,<input>,app-01,<input>,blue,h4cs-distb2,harborka.qianfan123.com/hdpos46/h4cs-dist,backend,38052,39052,9545,45
<input>,<input>,app-01,<input>,blue,h4cs-disttask,harborka.qianfan123.com/hdpos46/h4cs-dist,backend,38092,39092,9845,45
<input>,<input>,app-01,<input>,blue,h4cs-distlssc,harborka.qianfan123.com/hdpos46/h4cs-dist,backend,38062,39062,9945,45
<input>,<input>,app-01,<input>,blue,router,harborka.qianfan123.com/smd/router,backend,38095,0,0,0
<input>,<input>,app-01,<input>,blue,hdpos-oas-service,harborka.qianfan123.com/hdpos46/hdpos-oas-service,backend,38156,39156,0,15
<input>,<input>,app-01,<input>,blue,hdpos6-mdata-service,harborka.qianfan123.com/hdpos46/hdpos6-mdata-service,backend,38126,39126,0,15
<input>,<input>,app-01,<input>,blue,h6-wos-service,harborka.qianfan123.com/hdpos46/h6-wos-service,backend,38149,39149,0,15
<input>,<input>,app-01,<input>,blue,h6-best-service,harborka.qianfan123.com/hdpos46/h6-best-service,backend,38229,39229,0,15
<input>,<input>,app-01,<input>,blue,h6-openapi-service,harborka.qianfan123.com/hdpos46/h6-openapi-service,backend,8176,9176,0,15
<input>,<input>,app-01,<input>,blue,mpa-service,harborka.qianfan123.com/hdpos46/mpa-service,backend,38186,39186,0,15
<input>,<input>,app-01,<input>,blue,h6-wms-service,harborka.qianfan123.com/hdpos46/h6-wms-service,backend,38164,39164,0,15
<input>,<input>,app-01,<input>,blue,fas-h6-transfer-service,harborka.qianfan123.com/hdpos46/fas-h6-transfer-service,backend,38177,39177,9774,15
<input>,<input>,app-01,<input>,blue,fas-spider-service,harborka.qianfan123.com/hdpos46/fas-spider-service,backend,38178,39178,0,15
<input>,<input>,app-01,<input>,blue,zhenhua-service,harborka.qianfan123.com/component/zhenhua-service,backend,38204,39204,0,15
<input>,<input>,app-01,<input>,blue,category-manage-service,harborka.qianfan123.com/component/category-manage-service,backend,38194,0,9771,15
<input>,<input>,app-01,<input>,blue,card,harborka.qianfan123.com/card/card,backend,0,0,0,0
<input>,<input>,app-01,<input>,blue,zh-transfer-server,harborka.qianfan123.com/mas/zh-transfer-server,backend,38202,39202,37202,15
<input>,<input>,app-01,<input>,blue,erp-mas2-transfer-server,harbor.qianfan123.com/mas/erp-mas2-transfer-server,backend,38203,39203,37203,15
<input>,<input>,app-01,<input>,blue,yc-mas-transfer-server,harbor.qianfan123.com/mas/yc-mas-transfer-server,backend,38204,39204,37204,15
<input>,<input>,app-01,<input>,blue,h6-mas-transfer-server,harbor.qianfan123.com/mas/h6-mas-transfer-server,backend,38205,39205,37205,15
<input>,<input>,app-01,<input>,blue,sydaojia-mas-transfer-server,harbor.qianfan123.com/mas/sydaojia-mas-transfer-server,backend,38206,39206,37206,15
<input>,<input>,app-01,<input>,blue,h6-ias-transfer,harbor.qianfan123.com/baas/h6-ias-transfer,backend,38207,39207,37207,15
<input>,<input>,app-01,<input>,blue,mas2c-transfer,harbor.qianfan123.com/mas/mas2c-transfer,backend,38208,39208,37208,15
<input>,<input>,app-01,<input>,blue,mas2c-transfer-nec-etl,harbor.qianfan123.com/oas/mas2c-transfer-nec-etl,backend,38209,39209,37209,15
<input>,<input>,app-01,<input>,blue,spms-stdpms-transfer,harbor.qianfan123.com/pms/spms-stdpms-transfer,backend,0,0,0,0
<input>,<input>,app-01,<input>,blue,sas-h4-transfer,harbor.qianfan123.com/baas/sas-h4-transfer,backend,18289,19302,9797,0
<input>,<input>,app-01,<input>,blue,ras-h4-transfer,harbor.qianfan123.com/baas/ras-h4-transfer,backend,18290,19303,9790,0
<input>,<input>,app-01,<input>,blue,dbwc-k3cloudconnector-server,harborka.qianfan123.com/adi/dbwc-k3cloudconnector-server,backend,38196,0,0,0
<input>,<input>,app-01,<input>,blue,h6-k3cloud-service,harborka.qianfan123.com/hdpos46/h6-k3cloud-service,backend,38204,39204,0,15
<input>,<input>,app-01,<input>,blue,h6-ssd-service,harborka.qianfan123.com/hdpos46/h6-ssd-service,backend,38160,39160,0,15
<input>,<input>,app-01,<input>,blue,zjlhkk-server-service,harborka.qianfan123.com/component/zjlhkk-server-service,backend,38161,39161,0,15
<input>,<input>,app-01,<input>,blue,h6-tobacco-cis-service,harborka.qianfan123.com/hdpos46/h6-tobacco-cis-service,backend,38163,39163,0,15
<input>,<input>,app-01,<input>,blue,h6-tobacco-ncm-service,harborka.qianfan123.com/hdpos46/h6-tobacco-ncm-service,backend,38200,39200,0,15
<input>,<input>,app-01,<input>,blue,tobaccocpos-service,harborka.qianfan123.com/hdpos46/tobaccocpos-service,backend,38158,39158,0,15
<input>,<input>,app-01,<input>,blue,h6-invoice-service,harborka.qianfan123.com/hdpos46/h6-invoice-service,backend,38219,39219,0,15
<input>,<input>,app-01,<input>,blue,taobaowdk-service,harborka.qianfan123.com/component/taobaowdk-service,backend,38097,39097,0,15
<input>,<input>,app-01,<input>,blue,jcrm-server-card,harborka.qianfan123.com/jcrm/jcrm-server-card,backend,38881,0,0,0
<input>,<input>,app-01,<input>,blue,keduo-web,harborka.qianfan123.com/component/keduo-web,backend,38108,0,0,0
<input>,<input>,app-01,<input>,blue,lianhua-web,harborka.qianfan123.com/component/lianhua-web,backend,38089,0,0,0
<input>,<input>,app-01,<input>,blue,anda-web,harborka.qianfan123.com/component/anda-web,backend,38222,0,0,0
<input>,<input>,app-01,<input>,blue,datamanager-web,harborka.qianfan123.com/component/datamanager-web,backend,38096,0,0,0
<input>,<input>,app-01,<input>,blue,door-service,harborka.qianfan123.com/hdpos46/door-service,backend,38211,39211,0,15
<input>,<input>,app-01,<input>,blue,ppmt-web,harborka.qianfan123.com/component/ppmt-web,backend,38092,39092,0,15
<input>,<input>,app-01,<input>,blue,ppmt-product-web-service,harborka.qianfan123.com/component/ppmt-product-web-service,backend,38215,39215,9766,15
<input>,<input>,app-01,<input>,blue,jieqiang-service,harborka.qianfan123.com/component/jieqiang-service,backend,38213,39213,0,15
<input>,<input>,app-01,<input>,blue,mkh-web,harborka.qianfan123.com/component/mkh-web,backend,38106,39106,9759,15
<input>,<input>,app-01,<input>,blue,mkh-account-service,harborka.qianfan123.com/hdpos46/mkh-account-service,backend,38153,39153,9760,15
<input>,<input>,app-01,<input>,blue,mkh-connector-service,harborka.qianfan123.com/hdpos46/mkh-connector-service,backend,38171,39171,9761,15
<input>,<input>,app-01,<input>,blue,mkh-ec-service,harborka.qianfan123.com/hdpos46/mkh-ec-service,backend,38214,39214,9762,15
<input>,<input>,app-01,<input>,blue,mkh-dms-service,harborka.qianfan123.com/hdpos46/mkh-dms-service,backend,38181,39181,9763,15
<input>,<input>,app-01,<input>,blue,mkh-pur-service,harborka.qianfan123.com/hdpos46/mkh-pur-service,backend,38120,39120,9764,15
<input>,<input>,app-01,<input>,blue,mkh-screen-service,harborka.qianfan123.com/hdpos46/mkh-screen-service,backend,38129,39129,9765,15
<input>,<input>,app-01,<input>,blue,dqsh-service,harborka.qianfan123.com/hdpos46/dqsh-service,backend,38147,39147,0,15
<input>,<input>,app-01,<input>,blue,h6-youzan-service,harborka.qianfan123.com/hdpos46/h6-youzan-service,backend,38216,39216,0,15
<input>,<input>,app-01,<input>,blue,yz-service,harborka.qianfan123.com/component/yz-service,backend,38220,39220,0,15
<input>,<input>,app-01,<input>,blue,yiran-service,harborka.qianfan123.com/component/yiran-service,backend,38221,39221,0,15
<input>,<input>,app-01,<input>,blue,h4cs-syncdata-service,harborka.qianfan123.com/hdpos46/h4cs-syncdata-service,backend,38223,39223,0,15
<input>,<input>,app-01,<input>,blue,h4cs-syncdata-serviceb1,harborka.qianfan123.com/hdpos46/h4cs-syncdata-service,backend,38223,39223,0,15
<input>,<input>,app-01,<input>,blue,bazhuang-service,harborka.qianfan123.com/hdpos46/bazhuang-service,backend,38224,39224,0,15
<input>,<input>,app-01,<input>,blue,zzbz-u8connector-server,harborka.qianfan123.com/adi/zzbz-u8connector-server,backend,38225,0,0,15
<input>,<input>,app-01,<input>,blue,gykh-u8connector-server,harborka.qianfan123.com/adi/gykh-u8connector-server,backend,38226,0,0,15
<input>,<input>,app-01,<input>,blue,ncconnector-server,harborka.qianfan123.com/adi/ncconnector-server,backend,38227,0,0,15
<input>,<input>,app-01,<input>,blue,kingdee-eas-service,harborka.qianfan123.com/hdpos46/kingdee-eas-service,backend,39228,39228,0,15
<input>,<input>,app-01,<input>,blue,rtmark-dms-service,harborka.qianfan123.com/component/rtmark-dms-service,backend,38230,39230,0,15
<input>,<input>,app-01,<input>,blue,hdpos6-etl-service,harborka.qianfan123.com/hdpos46/hdpos6-etl-service,backend,38165,39165,0,15
<input>,<input>,app-01,<input>,blue,sos-service,harborka.qianfan123.com/hdpos46/sos-service,backend,38137,39137,0,15
<input>,<input>,app-01,<input>,blue,sos-config-service,harborka.qianfan123.com/hdpos46/sos-config-service,backend,38133,39133,0,15
<input>,<input>,app-01,<input>,blue,eaccount-server,harborka.qianfan123.com/component/eaccount-server,backend,38173,0,0,15
<input>,<input>,app-01,<input>,blue,otter-r3-std,harborka.qianfan123.com/otter/otter-r3-std,backend,38088,0,9048,15
<input>,<input>,app-01,<input>,blue,h6-kingdee-xinghan-service,harborka.qianfan123.com/hdpos46/h6-kingdee-xinghan-service,backend,38233,39233,0,15
<input>,<input>,app-01,<input>,blue,zjgssm-service,harborka.qianfan123.com/component/zjgssm-service,backend,38094,39094,0,15
<input>,<input>,app-01,<input>,blue,gssm-service,harborka.qianfan123.com/component/gssm-service,backend,38234,39234,0,15
<input>,<input>,app-01,<input>,blue,h6-flux2-service,harborka.qianfan123.com/hdpos46/h6-flux2-service,backend,38235,39235,0,15
<input>,<input>,app-01,<input>,blue,sanxin-service,harborka.qianfan123.com/hdpos46/sanxin-service,backend,38236,39236,0,15
<input>,<input>,app-01,<input>,blue,myt-dm-service,harborka.qianfan123.com/hdpos46/myt-dm-service,backend,38237,39237,0,15
<input>,<input>,app-01,<input>,blue,h6-pms-service,harborka.qianfan123.com/hdpos46/h6-pms-service,backend,38238,39238,0,15
<input>,<input>,app-01,<input>,blue,hdpos-stdcomponent-service,harborka.qianfan123.com/component/hdpos-stdcomponent-service,backend,38239,39239,0,15
<input>,<input>,app-01,<input>,blue,ays-service,harborka.qianfan123.com/hdpos46/ays-service,backend,38240,39240,0,15
<input>,<input>,app-01,<input>,blue,ays-dm-web,harborka.qianfan123.com/component/ays-dm-web,backend,38241,39241,0,15
<input>,<input>,app-01,<input>,blue,wds-service,harborka.qianfan123.com/hdpos46/wds-service,backend,38242,39242,0,15
<input>,<input>,app-01,<input>,blue,sungivenfoods-service,harborka.qianfan123.com/component/sungivenfoods-service,backend,38243,39243,0,15
<input>,<input>,app-01,<input>,blue,jlgougo-service,harborka.qianfan123.com/component/jlgougo-service,backend,38244,39244,0,15
<input>,<input>,app-01,<input>,blue,h6-flux-service,harborka.qianfan123.com/hdpos46/h6-flux-service,backend,38245,39245,0,15
<input>,<input>,app-01,<input>,blue,h6-greeneryfruit-service,harborka.qianfan123.com/hdpos46/h6-greeneryfruit-service,backend,38246,39246,0,15
<input>,<input>,app-01,<input>,blue,quanfuyuan-service,harborka.qianfan123.com/component/quanfuyuan-service,backend,38247,39247,0,15
<input>,<input>,app-01,<input>,blue,h6-qnh-service,harborka.qianfan123.com/hdpos46/h6-qnh-service,backend,38248,39248,0,15
<input>,<input>,app-01,<input>,blue,guoda-service,harborka.qianfan123.com/component/guoda-service,backend,38249,39249,0,15
<input>,<input>,app-01,<input>,blue,jiuduorouduo-service,harborka.qianfan123.com/component/jiuduorouduo-service,backend,38250,39250,0,15
<input>,<input>,app-01,<input>,blue,h6-haikang-service,harborka.qianfan123.com/hdpos46/h6-haikang-service,backend,38251,39251,0,15
<input>,<input>,app-01,<input>,blue,k3cloudconnector-server,harborka.qianfan123.com/adi/k3cloudconnector-server,backend,38252,0,0,15
<input>,<input>,app-01,<input>,blue,wowcolour-service,harborka.qianfan123.com/hdpos46/wowcolour-service,backend,38253,39253,9793,15
<input>,<input>,app-01,<input>,blue,sofomall-service,harborka.qianfan123.com/hdpos46/sofomall-service,backend,38254,39254,9794,15
<input>,<input>,app-01,<input>,blue,h6-vendor-service,harborka.qianfan123.com/hdpos46/h6-vendor-service,backend,38255,39255,9795,15
<input>,<input>,app-01,<input>,blue,mc-scm-wowcolour-service,harborka.qianfan123.com/hdpos46/mc-scm-wowcolour-service,backend,38256,39256,9796,15
<input>,<input>,app-01,<input>,blue,h6-boke-service,harborka.qianfan123.com/hdpos46/h6-boke-service,backend,38257,39257,9797,15
<input>,<input>,app-01,<input>,blue,haigang-service,harborka.qianfan123.com/component/haigang-service,backend,38258,39258,9798,15
<input>,<input>,app-01,<input>,blue,lsym-service,harborka.qianfan123.com/component/lsym-service,backend,38265,39265,9805,15
<input>,<input>,app-01,<input>,blue,dts-store-wanda,harborka.qianfan123.com/dts-store/dts-store-wanda,backend,38086,0,9742,15
<input>,<input>,app-01,<input>,blue,hdpos4-wanda-dist,harborka.qianfan123.com/hdpos46/hdpos4-wanda-dist,backend,38180,39180,9744,15
<input>,<input>,app-01,<input>,blue,pfs,harborka.qianfan123.com/pfs/pfs,backend,38087,0,9743,15
<input>,<input>,app-01,<input>,blue,bbw-web,harborka.qianfan123.com/hdpos46/bbw-web,backend,38266,39266,9806,15
<input>,<input>,app-01,<input>,blue,bbw-srm-service,harborka.qianfan123.com/hdpos46/bbw-srm-service,backend,38267,39267,9807,15
<input>,<input>,app-01,<input>,blue,bbw-connector-service,harborka.qianfan123.com/hdpos46/bbw-connector-service,backend,38268,39268,9808,15
<input>,<input>,app-01,<input>,blue,hdpos4-wanda-invoice-server,harborka.qianfan123.com/hdpos46/hdpos4-wanda-invoice-server,backend,38269,39269,9809,15
<input>,<input>,app-01,<input>,blue,hdpos4-wanda-mpos-server,harborka.qianfan123.com/hdpos46/hdpos4-wanda-mpos-server,backend,38270,39270,9810,15
<input>,<input>,app-01,<input>,blue,yueyun-service,harborka.qianfan123.com/component/yueyun-service,backend,38271,39271,9811,15
<input>,<input>,app-01,<input>,blue,h6-jd-eclp-service,harborka.qianfan123.com/hdpos46/h6-jd-eclp-service,backend,38272,39272,9812,15
<input>,<input>,app-01,<input>,blue,wrjh-service,harborka.qianfan123.com/component/wrjh-service,backend,38273,39273,9813,15
<input>,<input>,app-01,<input>,blue,h6-ylz-service,harborka.qianfan123.com/hdpos46/h6-ylz-service,backend,38274,39274,9814,15
<input>,<input>,app-01,<input>,blue,pos-pms-server-service,harborka.qianfan123.com/component/pos-pms-server-service,backend,38275,39275,9815,15
<input>,<input>,app-01,<input>,blue,cxzy-service,harborka.qianfan123.com/component/cxzy-service,backend,38276,39276,9816,15
<input>,<input>,app-01,<input>,blue,h6-yzvcm-service,harborka.qianfan123.com/component/h6-yzvcm-service,backend,38277,39277,9817,15
<input>,<input>,app-01,<input>,blue,ppmt-intl-service,harborka.qianfan123.com/component/ppmt-intl-service,backend,38278,39278,9818,15
<input>,<input>,app-01,<input>,blue,h6-jdl-service,harborka.qianfan123.com/hdpos46/h6-jdl-service,backend,38279,39279,9819,15
<input>,<input>,app-01,<input>,blue,h6-qimen-service,harborka.qianfan123.com/hdpos46/h6-qimen-service,backend,38280,39280,9820,15
<input>,<input>,app-01,<input>,blue,babycare-service,harborka.qianfan123.com/hdpos46/babycare-service,backend,38281,39281,9821,15
<input>,<input>,app-01,<input>,blue,config-service,harborka.qianfan123.com/baas/config-service,backend,38114,39114,9751,15
<input>,<input>,app-01,<input>,blue,spms-server,harborka.qianfan123.com/pms/spms-server,backend,38115,39115,9752,15
<input>,<input>,app-01,<input>,blue,spms-web,harborka.qianfan123.com/pms/spms-web,backend,38116,39116,9753,15
<input>,<input>,app-01,<input>,blue,spms-web-ui,harborka.qianfan123.com/pms/spms-web-ui,frontend,38117,0,0,0
<input>,<input>,app-01,<input>,blue,rumba-oss-server,harborka.qianfan123.com/rumba/rumba-oss-server,backend,38112,0,0,0
<input>,<input>,app-01,<input>,blue,zhy-service,harborka.qianfan123.com/component/zhy-service,backend,38283,39283,9823,15
<input>,<input>,app-01,<input>,blue,zjzsh-service,harborka.qianfan123.com/component/zjzsh-service,backend,38282,39282,9822,15
<input>,<input>,app-01,<input>,blue,datadocking-web,harborka.qianfan123.com/hdpos46/datadocking-web,backend,38284,39284,9824,15
<input>,<input>,app-01,<input>,blue,chaoyang-service,harborka.qianfan123.com/component/chaoyang-service,backend,38285,39285,9825,15
<input>,<input>,app-01,<input>,blue,hlcoming-service,harborka.qianfan123.com/component/hlcoming-service,backend,38286,39286,9826,15
<input>,<input>,app-01,<input>,blue,hdpos-ali-o2o-service,harborka.qianfan123.com/hdpos46/hdpos-ali-o2o-service,backend,38287,39287,9827,15
<input>,<input>,app-01,<input>,blue,hdpos4-mci-dist,harborka.qianfan123.com/hdpos46/hdpos4-mci-dist,backend,38180,39180,9744,45
<input>,<input>,app-01,<input>,blue,hdpos4-mincha-dist,harborka.qianfan123.com/hdpos46/hdpos4-mincha-dist,backend,38180,39180,9744,45
<input>,<input>,app-01,<input>,blue,otter-mcyp-gn-sap,harborka.qianfan123.com/otter/otter-mcyp-gn-sap,backend,38181,0,9048,15
<input>,<input>,app-01,<input>,blue,hedwig-server,harborka.qianfan123.com/hdpos46/hedwig-server,backend,38288,39288,9828,15
<input>,<input>,app-01,<input>,blue,zsl-service,harborka.qianfan123.com/hdpos46/zsl-service,backend,38198,39198,9860,15
<input>,<input>,app-01,<input>,blue,xmsf-service,harborka.qianfan123.com/component/xmsf-service,backend,38289,39289,9829,15
<input>,<input>,app-01,<input>,blue,easconnector2-server,harborka.qianfan123.com/adi/easconnector2-server,backend,38290,0,0,15
<input>,<input>,app-01,<input>,blue,otter-r3-sap,harborka.qianfan123.com/otter/otter-r3-sap,backend,38181,0,9048,15
<input>,<input>,app-01,<input>,blue,ledoujia-service,harborka.qianfan123.com/component/ledoujia-service,backend,38291,39291,9830,15
<input>,<input>,app-01,<input>,blue,h6-chanjet-service,harborka.qianfan123.com/hdpos46/h6-chanjet-service,backend,38292,39292,9831,15
<input>,<input>,app-01,<input>,blue,mincha-service,harborka.qianfan123.com/component/mincha-service,backend,38293,39293,9832,15
<input>,<input>,app-01,<input>,blue,jjl-service,harborka.qianfan123.com/component/jjl-service,backend,38294,39294,9833,15
<input>,<input>,app-01,<input>,blue,jjleg-service,harborka.qianfan123.com/hdpos46/jjleg-service,backend,38295,39295,9834,15
<input>,<input>,app-01,<input>,blue,lmt-service,harborka.qianfan123.com/component/lmt-service,backend,38296,39296,9835,15
<input>,<input>,app-01,<input>,blue,tmallwdk-server,harborka.qianfan123.com/component/tmallwdk-server,backend,38297,39297,9836,15
<input>,<input>,app-01,<input>,blue,zl-portal-sync,harbor.qianfan123.com/ka-sail/zl-portal-sync,backend,38298,39298,9837,15
<input>,<input>,app-01,<input>,blue,pasodata-consumer,harborka.qianfan123.com/dc/pasodata_consumer,backend,38218,0,0,15
<input>,<input>,app-01,<input>,blue,pasodata-producer,harborka.qianfan123.com/dc/pasodata_producer,backend,38206,0,0,15
<input>,<input>,app-01,<input>,blue,dts-store,harborka.qianfan123.com/dts-store/dts-store,backend,38086,0,9742,15
<input>,<input>,app-01,<input>,blue,chanjetconnector-server,harborka.qianfan123.com/adi/chanjetconnector-server,backend,38299,0,0,15
<input>,<input>,app-01,<input>,blue,ays-transfer2-server,harbor.qianfan123.com/mas/ays-transfer2-server,backend,38300,39300,37300,15
<input>,<input>,app-01,<input>,blue,gateway-service,harbor.qianfan123.com/baas/gateway-service,backend,38301,39301,37301,15
<input>,<input>,app-01,<input>,blue,zhijing-service,harborka.qianfan123.com/component/zhijing-service,backend,38303,39303,37303,15
<input>,<input>,app-01,<input>,blue,tmsh-u8connector-server,harborka.qianfan123.com/adi/tmsh-u8connector-server,backend,38304,0,0,15
<input>,<input>,app-01,<input>,blue,mc-home-service,harborka.qianfan123.com/hdpos46/mc-home-service,backend,38305,39305,37305,15
<input>,<input>,app-01,<input>,blue,fwd-store-server,harborka.qianfan123.com/hdpos46/fwd-store-server,backend,38306,39306,37306,15
<input>,<input>,app-01,<input>,blue,fwd-store-serverwow,harborka.qianfan123.com/hdpos46/fwd-store-server,backend,38316,39316,37316,15
<input>,<input>,app-01,<input>,blue,fwd-store-serverminiso,harborka.qianfan123.com/hdpos46/fwd-store-server,backend,38326,39326,37326,15
<input>,<input>,app-01,<input>,blue,fwd-store-servertoptoy,harborka.qianfan123.com/hdpos46/fwd-store-server,backend,38336,39336,37336,15
<input>,<input>,app-01,<input>,blue,dongyi-service,harborka.qianfan123.com/component/dongyi-service,backend,38308,39308,37308,15
<input>,<input>,app-01,<input>,blue,ptxs-service,harborka.qianfan123.com/component/ptxs-service,backend,38309,39309,37309,15
<input>,<input>,app-01,<input>,blue,vbs-service,harborka.qianfan123.com/hdpos46/vbs-service,backend,38310,39310,37310,15
<input>,<input>,app-01,<input>,blue,yongyou-bipconnector-server,harborka.qianfan123.com/adi/yongyou-bipconnector-server,backend,38311,0,0,15
<input>,<input>,app-01,<input>,blue,wln-service,harborka.qianfan123.com/component/wln-service,backend,38311,39311,37311,15
<input>,<input>,app-01,<input>,blue,sanrio-service,harborka.qianfan123.com/component/sanrio-service,backend,38312,39312,37312,15
<input>,<input>,app-01,<input>,blue,h6-ovopark-service,harborka.qianfan123.com/hdpos46/h6-ovopark-service,backend,38313,39313,37313,15
<input>,<input>,app-01,<input>,blue,sapconnector-server,harborka.qianfan123.com/adi/sapconnector-server,backend,38314,0,0,15
<input>,<input>,app-01,<input>,blue,sanfu-service,harborka.qianfan123.com/component/sanfu-service,backend,38315,39315,37315,15
<input>,<input>,app-01,<input>,blue,hsyp-service,harborka.qianfan123.com/hdpos46/hsyp-service,backend,38316,39316,37316,15
<input>,<input>,app-01,<input>,blue,toys52-service,harborka.qianfan123.com/component/toys52-service,backend,38317,39317,37317,15
<input>,<input>,app-01,<input>,blue,h6-inv-service,harborka.qianfan123.com/hdpos46/h6-inv-service,backend,38318,39318,37318,15
<input>,<input>,app-01,<input>,blue,u8connector-server,harborka.qianfan123.com/adi/u8connector-server,backend,38319,0,0,15
<input>,<input>,app-01,<input>,blue,hdportal,harbor.qianfan123.com/ka-sail/hdportal,backend,38320,39320,0,15
<input>,<input>,app-01,<input>,blue,hd-portal-web-ui,harbor.qianfan123.com/ka-sail/hd-portal-web-ui,frontend,38321,0,0,15
<input>,<input>,app-01,<input>,blue,linji-web,harborka.qianfan123.com/component/linji-web,backend,38322,39317,37317,15
<input>,<input>,app-01,<input>,blue,wuchanrexuan-service,harborka.qianfan123.com/component/wuchanrexuan-service,backend,38323,39323,37323,15
<input>,<input>,app-01,<input>,blue,h6-bankbus-service,harborka.qianfan123.com/hdpos46/h6-bankbus-service,backend,38325,39325,37325,15
<input>,<input>,app-01,<input>,blue,litepay-server,harborka.qianfan123.com/pay/litepay-server,backend,38326,39326,37326,15
<input>,<input>,app-01,<input>,blue,hsjd-service,harborka.qianfan123.com/component/hsjd-service,backend,38327,39327,37327,15
<input>,<input>,app-01,<input>,blue,mtj-service,harborka.qianfan123.com/component/mtj-service,backend,38327,39327,37327,15
<input>,<input>,app-01,<input>,blue,jindie-yxcconnector-server,harborka.qianfan123.com/adi/jindie-yxcconnector-server,backend,38332,0,0,15
<input>,<input>,app-01,<input>,blue,baas-config,harbor.qianfan123.com/baas/config-service,backend,38329,39329,37329,15
<input>,<input>,app-01,<input>,blue,sop-service,harbor.qianfan123.com/sop/sop-service,backend,38330,39330,37330,15
<input>,<input>,app-01,<input>,blue,sop-bff,harbor.qianfan123.com/sop/sop-bff,backend,38331,39331,37331,15
<input>,<input>,app-01,<input>,blue,gateway-service,harbor.qianfan123.com/baas/gateway-service,backend,38301,39301,37301,15
<input>,<input>,app-01,<input>,blue,h6-netsuite-service,harborka.qianfan123.com/hdpos46/h6-netsuite-service,backend,38333,39333,37333,15
<input>,<input>,app-01,<input>,blue,kinglomo-service,harborka.qianfan123.com/component/kinglomo-service,backend,38334,39334,37334,15
<input>,<input>,app-01,<input>,blue,h6-baozun-service,harborka.qianfan123.com/hdpos46/h6-baozun-service,backend,38335,39335,37335,15
<input>,<input>,app-01,<input>,blue,lvhang-service,harborka.qianfan123.com/component/lvhang-service,backend,38336,39336,37336,15
<input>,<input>,app-01,<input>,blue,yxy-service,harbor.qianfan123.com/ares/yxy-service,backend,38338,39338,37338,15
<input>,<input>,app-01,<input>,blue,soms-sec-server,8852-harbor.hd123.com/sec_soms/soms-sec-server,backend,38339,39339,37339,15
<input>,<input>,app-01,<input>,blue,sec-wms-service,8852-harbor.hd123.com/hdpos46/sec-wms-service,backend,18340,19340,17340,15
<input>,<input>,app-01,<input>,blue,h6-yonbip-service,harborka.qianfan123.com/hdpos46/h6-yonbip-service,backend,18341,19341,17341,15
<input>,<input>,app-01,<input>,blue,tiaoma-service,harborka.qianfan123.com/component/tiaoma-service,backend,18342,19342,17342,15
<input>,<input>,app-01,<input>,blue,h6-wanwei-service,harborka.qianfan123.com/hdpos46/h6-wanwei-service,backend,18343,19343,17343,15
<input>,<input>,app-01,<input>,blue,ksx-service,harborka.qianfan123.com/component/ksx-service,backend,18346,19346,17346,15
<input>,<input>,app-01,<input>,blue,h6-duodian-service,harborka.qianfan123.com/hdpos46/h6-duodian-service,backend,38347,39347,37347,15