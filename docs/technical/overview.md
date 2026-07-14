# 寻球项目技术总览

本静态站用于展示寻球移动端项目的工程整理过程。内容聚焦技术事实，公开展示产品页、技术文档、APK 发布门禁和新后端验证说明，不包含密钥、数据库连接或私有云配置。

## 项目形态

寻球项目本地工作区包含多个部分：

- 旧版 Android 客户端：`Xun/`
- 64 位新版 Android 客户端：`xunqiu-android64/`
- 旧 Java Web 后端：`xunqiu-server-20220501/`
- 现代新后端：`xunqiu-backend-modern/`
- 部署脚本、APK 归档和历史文档：`deploy/`、`release/`、`docs/`

当前静态站只展示产品和技术治理内容。旧后端依赖 JDK、Tomcat、MySQL、Redis 和历史上传目录，不适合放入 Cloudflare Pages；新后端使用 Spring Boot 3、PostgreSQL、Cloudflare R2 和 Render 独立部署。

## 展示重点

- 旧版 App 的多模块结构、32 位 ABI、旧 Support 库、视频/IM/推送/支付等三方依赖。
- 新版 64 位客户端的轻量重建策略。
- 新旧客户端分流：旧版客户端对应旧后端，64 位新客户端对应新后端。
- 新后端接口兼容、字段 fallback、媒体 URL 归一化。
- 图片与短视频上传链路，包括异常 MP4 拦截、R2 对象存储和横竖屏播放比例处理。
- 编译验证、后端健康检查、上传验收、阶段包归档和 Cloudflare Pages 静态部署。

## 部署边界

Cloudflare Pages 承载：

- 静态项目展示页。
- 技术文档。
- APK 发布状态和正式 release 前置条件。
- 图片素材。

Cloudflare Pages 不承载：

- 旧 Java WAR 后端。
- MySQL/Redis/Tomcat/Nginx 等旧运行环境。
- Render/R2 的环境变量和密钥。
- 动态 API 服务；动态能力由独立后端服务提供。
