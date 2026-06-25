# 寻球项目技术总览

本静态站用于展示寻球 Android 项目的工程整理过程。内容聚焦技术事实，不包含后端在线演示，也不包含完整源码仓库。

## 项目形态

寻球项目本地工作区包含多个部分：

- 旧版 Android 客户端：`Xun/`
- 64 位新版 Android 客户端：`xunqiu-android64/`
- 主 Java Web 后端：`xunqiu-server-20220501/`
- 部署脚本、APK 归档和历史文档：`deploy/`、`release/`、`docs/`

当前静态站只展示 Android 客户端迁移和技术治理内容。完整后端需要 JDK、Tomcat、MySQL、Redis 和媒体上传目录，不适合放入 Cloudflare Pages。

## 展示重点

- 旧版 App 的多模块结构、32 位 ABI、旧 Support 库、视频/IM/推送/支付等三方依赖。
- 新版 64 位客户端的轻量重建策略。
- 旧服务端接口复用、字段 fallback、媒体 URL 归一化。
- 短视频上传播放链路修复，包括异常 MP4 拦截和横竖屏播放比例处理。
- 编译验证、功能验收、阶段包归档和 Cloudflare Pages 静态部署。

## 部署边界

Cloudflare Pages 承载：

- 静态项目展示页。
- 技术文档。
- APK 下载文件。
- 图片素材。

Cloudflare Pages 不承载：

- Java WAR 后端。
- MySQL/Redis。
- Tomcat/Nginx 反向代理。
- 真实上传目录和动态接口。

