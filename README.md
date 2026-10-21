# NexBoot - LazyCat App

[NexBoot / ndiskless](https://github.com/waterlemon0108/nexboot)（基于 Go 的轻量级无盘启动平台：iPXE 网络引导、ZFS 存储池、iSCSI 批量装机、高可用集群、Web 管理）的懒猫微服（LazyCat）打包。

## 镜像来源

镜像由 [dockers-x/nexboot](https://github.com/dockers-x/nexboot) 自动构建：跟随上游 release tag，多阶段构建（vite 前端 → Go 静态编译 → Ubuntu 24.04 + ZFS/iSCSI/DHCP 运行时依赖），推送 `ghcr.io/dockers-x/nexboot:{v版本,latest}`。

- **镜像模式（mirror）**：经 `ghcr.nju.edu.cn` 加速拉取，`require_digest_match: true` 校验与 GHCR 一致。
- **自动跟随上游**：每日检查（dockers-x 跟上游 tag，本包跟 dockers-x），自动构建发布。
- **喵喵商店发布**：仅私有商店（MiaoMiao private store），不发布官方商店。

## 说明

- 单容器 `privileged` + `network_mode: host`（DHCP/PXE/iSCSI 需与客户机同二层网段）
- **默认管理员**：`admin`，密码稳定随机生成（持久化在数据卷 `env` 文件）
- 数据卷：`data`（SQLite/导入镜像）、`tftp`（PXE 固件）、`dnsmasq` 配置、`keepalived` 配置
- ZFS 存储池需管理员在宿主机/容器内手工建池（如 `zpool create tank /dev/sdX`），UI 引导建池
- ⚠️ **运行环境要求**（上游硬性要求，容器内需全部满足）：宿主机有独立数据盘、静态 IP、客户机同网段无其它 DHCP、ZFS 内核模块可用。**在懒猫微服上能否提供这些条件取决于运行时**——部署后先看 `curl :8080/healthz` 与建池是否成功
- 健康检查 `/healthz`（未建池同样返回 ok）

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `APP_ID` | 可选 |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |

## 许可

AGPL-3.0（上游）
