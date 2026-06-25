# 旧版 Android App 架构分析

旧版 Android 工程位于 `Xun/`，包名为 `com.code.space.ss.freekicker`。它是一个历史较长的足球社区与球队管理客户端，包含动态、球队、日程、赛事、球场、队费、短视频、商城、IM、推送、支付和 WebView 活动页等业务。

## 工程结构

旧版是多模块 Android 工程，主要模块包括：

- `app`：主业务模块。
- `Android-PullToRefresh`、`banner`、`magicindicator`、`topsnackbar`、`BaseLib`、`mrqUtils`：UI 和工具模块。
- `PushSDK`、`easeui`、`SocialUtils`：推送、IM 和社交能力。
- `FFmpegAndroid`、`IjkLib`、`ijktest`、`Sticker`：视频录制、处理、播放和贴纸能力。

主业务目录 `com.freekicker.module` 覆盖 login、home、dynamic、team、schedule、video、pay、store、pitch、league、transfer、yueball 等业务域。

## 构建和兼容性

旧版构建特征：

- Android Gradle Plugin 处于旧版本时代。
- `minSdkVersion` 17，`targetSdkVersion` 27。
- 开启 MultiDex。
- 使用旧 `org.apache.http.legacy`。
- ABI 过滤为 `armeabi` 和 `armeabi-v7a`。
- 包含多个渠道配置。

这些因素说明旧版并不是简单改一个 `compileSdk` 就能升级到现代 Android 工程。构建链路、native ABI、旧三方 SDK 和全量业务回归都需要一起考虑。

## 网络层

旧版以 Volley 请求队列为主要入口。`VolleyUtil` 负责服务端地址、图片地址、Web 地址归一化以及 `RequestQueue` 创建，业务请求散落在多个请求类、Model 和页面中。

主要问题：

- 接口契约不集中。
- 页面层容易直接拼 URL。
- 错误处理和字段兼容逻辑分散。
- 旧域名、相对路径、WebView 地址需要统一处理。

## 视频能力

旧版视频链路包含：

- `MediaRecorder` 录制。
- FFmpeg 处理。
- IjkVideoView 播放。
- 短视频和集锦发布。

这意味着视频问题定位不能只看播放器，需要同时检查客户端文件、上传 payload、服务端保存路径、静态资源访问、MIME 类型和文件本体有效性。

## 技术债分类

- 构建债：旧 AGP、旧 Support 库、MultiDex、32 位 ABI。
- 架构债：主业务集中在 `app`，模块边界偏历史化。
- 网络债：请求分散，接口字段和错误处理不统一。
- 媒体债：录制、处理、上传、播放链路长。
- 三方债：推送、IM、支付、商城、地图等 SDK 升级风险高。
- 配置债：签名、数据库、第三方 key 等敏感配置需要隔离治理。

