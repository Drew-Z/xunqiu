# 寻球技术展示站

这是给 Cloudflare Pages 使用的纯静态站点，展示寻球移动端项目的遗留 App 梳理、Android 64 位客户端重建、新后端重构、R2 上传链路和验证部署信息。

## 内容

- `index.html`：项目产品与技术展示首页。
- `docs.html`：技术文档入口和 Cloudflare Pages 参数。
- `docs/technical/`：项目技术说明文档。
- `downloads/latest-xunqiu64.apk`：最新 64 位阶段 APK。
- `assets/`：页面使用的足球场景和视频封面图片。

## Cloudflare Pages 部署

在 Cloudflare Pages 中连接 GitHub 仓库后使用：

- Framework preset: `None`
- Build command: 留空
- Build output directory: `/`
- Root directory: 仓库根目录

这个站点不需要 Node、Python、构建命令或后端服务。

## 项目边界

本仓库只用于静态技术展示，不包含旧版 Java 后端、数据库、Redis、Tomcat 配置，也不包含 Render/R2 环境变量或任何敏感配置。新版后端独立维护在 `Drew-Z/xunqiu-backend-modern`。
