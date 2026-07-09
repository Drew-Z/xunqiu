# Xunqiu Showcase Site | 寻球技术展示站

一个面向访客和开发者的纯静态展示站，用来说明寻球移动端重建、现代后端迁移、短视频上传链路和阶段 APK 演示边界。

简体中文文档：[README.zh-CN.md](README.zh-CN.md)

![Static Site](https://img.shields.io/badge/site-static%20html-2d5f81)
![Cloudflare Pages](https://img.shields.io/badge/deploy-Cloudflare%20Pages-f38020)
![BIAU Port](https://img.shields.io/badge/ecosystem-BIAU%20Port-4f8a46)

## Contents

- [Preview](#preview)
- [Why This Exists](#why-this-exists)
- [Features](#features)
- [Architecture](#architecture)
- [Quick Start](#quick-start)
- [Deployment](#deployment)
- [Project Structure](#project-structure)
- [Testing](#testing)
- [Release And APK Boundary](#release-and-apk-boundary)
- [Security](#security)
- [Roadmap](#roadmap)
- [License](#license)

## Preview

Public-safe assets included in this repository:

- `assets/football-hero.jpg` for the first viewport.
- `assets/video-cover-street.jpg` and `assets/video-cover-passing.jpg` for the video pipeline section.
- `downloads/latest-xunqiu64.apk` as a stage/demo Android artifact, not an official app-store release.

Open `index.html` to view the product case page, and open `docs.html` for technical documentation entry points.

## Why This Exists

Xunqiu started as a legacy football community app with old Android tooling, 32-bit native dependencies, media upload paths, and a Java web backend. This repository does not contain the old app or backend. It packages the public presentation layer so visitors can understand:

- what the old system looked like technically;
- why the 64-bit Android client was rebuilt as a lighter path;
- how the Spring Boot backend restores the core API envelope;
- how images and short videos move through upload, storage, list return, and playback;
- where the stage APK and backend verification boundaries are.

## Features

- Static product page with BIAU Port / 泊岸 branding.
- Technical docs for legacy Android analysis, Android 64-bit rebuild, video pipeline, and deployment validation.
- Public-safe visual assets and no runtime build step.
- Cloudflare Pages `_headers` for APK content type and long-lived static asset caching.
- Stage APK download entry with explicit non-official-release positioning.

## Architecture

```mermaid
flowchart LR
  visitor["Visitor"]
  pages["Cloudflare Pages\nstatic HTML/docs/assets"]
  apk["Stage APK artifact"]
  android["Xunqiu Android 64-bit client"]
  backend["xunqiu-backend-modern\nSpring Boot 3 API"]
  db["PostgreSQL + Flyway seed"]
  r2["Cloudflare R2 optional media storage"]

  visitor --> pages
  pages --> apk
  apk --> android
  android --> backend
  backend --> db
  backend --> r2
```

This repository owns only the `pages` and `apk` static hosting boundary. The Android project and backend are maintained separately.

## Quick Start

No Node.js, Python, or build tool is required.

```powershell
cd xunqiu-showcase-site
Start-Process .\index.html
Start-Process .\docs.html
```

If you prefer a local HTTP server:

```powershell
python -m http.server 4173
```

Then open `http://localhost:4173/`.

## Deployment

Cloudflare Pages settings:

| Field | Value |
| --- | --- |
| Framework preset | `None` |
| Build command | leave empty |
| Build output directory | `/` or repository root |
| Root directory | repository root |
| Environment variables | none required |

The site is intentionally static. Do not add backend secrets, Render variables, R2 credentials, signing files, or database connection strings to this repository.

## Project Structure

```text
.
├── index.html
├── docs.html
├── favicon.svg
├── _headers
├── assets/
├── docs/
│   └── technical/
└── downloads/
    └── latest-xunqiu64.apk
```

## Testing

Recommended local checks before publishing:

```powershell
Test-Path .\index.html
Test-Path .\docs.html
Test-Path .\downloads\latest-xunqiu64.apk
rg -n "sk-|DATABASE_URL|R2_SECRET|PRIVATE KEY|BEGIN RSA|BEGIN OPENSSH" .
git diff --check
```

For link and asset review, open `index.html` and `docs.html` in a browser and check the buttons for `docs/technical/*` and `downloads/latest-xunqiu64.apk`.

## Release And APK Boundary

`downloads/latest-xunqiu64.apk` is a stage/demo artifact used to show the current Android rebuild progress. Treat it as:

- suitable for controlled demonstration and manual installation testing;
- not a signed official public release;
- not a Play Store / app-store distribution package;
- gated by future release signing, SHA-256 publication, changelog, and owner approval.

When replacing the APK, record the source build, feature scope, timestamp, SHA-256, and validation result in the companion Android project before publishing the static copy.

## Security

- This repository must not contain production credentials, private backend URLs, model keys, database URLs, R2 keys, signing material, or local machine paths.
- The stage APK should not be presented as an official release until signing and release approval are complete.
- Cloudflare Pages only serves static files. Dynamic API behavior belongs to the backend service.

## Roadmap

- Add a release manifest beside the APK with version, SHA-256, build source, and validation notes.
- Add screenshots from the Android 64-bit client once they are public-safe and current.
- Add a small static link checker for docs/assets/downloads.
- Keep the showcase content aligned with the backend README and BIAU Port project detail page.

## License

No standalone license file is included yet. Add a clear open-source license before advertising this repository for unrestricted reuse.
