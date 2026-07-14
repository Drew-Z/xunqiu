# 验证和静态部署

本项目分为 Android 客户端验证、新后端验证和静态站部署三部分。展示站不运行后端服务，后端由独立 Render 服务承载。

## Android 客户端验证

验证分为三层：

- 编译验证：本地运行 `:app:compileDebugJavaWithJavac`，不频繁生成 APK。
- 页面验收：启动、首页、动态、短视频、社区、我的、日程、赛事、球场、球币等页面打开。
- 写入验收：文字动态、图片动态、评论、创建活动、报名、打卡等真实写入路径。

功能状态用四类标记：

- 已验证。
- 需回归。
- 兜底可用。
- 待接入。

## APK 归档

阶段 APK 命名遵循：

```text
xunqiu64-stageN-feature-name-YYYYMMDD-HHMM.apk
```

每个阶段包保留在本地归档并记录 SHA-256，避免覆盖旧包。展示站不包含阶段 APK 副本；只有正式 release 签名、版本说明、扫描与回归证据、回滚说明和维护者批准完成后，才会重新评估公开下载。

## Cloudflare Pages 部署

Cloudflare Pages 参数：

- Framework preset：`None`
- Build command：留空
- Build output directory：`/`
- Root directory：仓库根目录

部署内容：

- `index.html`
- `docs.html`
- `docs/technical/`
- `assets/`

## 新后端验证

新后端独立维护和部署，验证重点包括：

- Render Web Service 健康检查。
- PostgreSQL schema 与 seed 数据由 Flyway 初始化。
- 登录、动态、球队、球场、视频等核心接口返回兼容旧客户端的 envelope。
- 图片和视频上传通过服务端进入 Cloudflare R2。
- 支付、短信、IM、推送等第三方能力只保留安全 stub，不在展示环境触发真实外部服务。

### Health 与 smoke 边界

后端健康检查入口是 Spring Boot Actuator health，部署路径保持在
`/free_kicker/actuator/health`。它只能证明服务进程和基础依赖在该时刻可响应，
不能等同于完整业务 SLA。

部署后推荐使用后端仓库中的 `scripts/smoke-test.ps1` 做核心链路烟测。Smoke 会覆盖：

- health。
- 兼容旧 envelope 的登录接口。
- 动态列表。
- 短视频列表。
- 球队主页。
- 球场列表。

如果 Render 免费实例冷启动或 health 请求超时，应先重试并查看 Render logs，再判断是否是
数据库、Flyway、R2、环境变量或服务启动问题。展示站是纯静态站，只负责项目说明和发布状态；
它不会也不应该保存后端密钥、数据库连接或 Render/R2 私有配置。

## 后端边界

旧版寻球后端是 Java Maven WAR，需要：

- JDK。
- Tomcat。
- MySQL。
- Redis。
- 配置文件。
- 媒体上传目录。

Cloudflare Pages 不能运行这类后端服务。本展示站仅用于技术材料和发布状态说明；新版动态能力由独立后端服务提供，不在静态站中保存任何密钥、私有配置或未批准 APK。
