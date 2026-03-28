# OpenClaw GHCR Builder

English | [中文](#中文说明)

This repository does not fork `openclaw/openclaw`.
It only hosts GitHub Actions workflows that check out upstream OpenClaw at build time and publish container images to `ghcr.io/winffychu/openclaw`.

## Status

Current release-candidate status is finalized for the main publish tracks.
The following images were rebuilt, smoke-checked, and scanned with Trivy for `CRITICAL/HIGH` vulnerabilities.

### Recommended Release Images

| Track | Arch | Tag | Size | Security |
|---|---|---|---:|---|
| slim | amd64 | `2026.3.24-slim-amd64-final-r1` | `796.28 MiB` | `0 HIGH/CRITICAL` |
| slim | arm64 | `2026.3.24-slim-arm64-final-r1` | `453.00 MiB` | `0 HIGH/CRITICAL` |
| mini | amd64 | `2026.3.24-mini-amd64-final-r1` | `710.84 MiB` | `0 HIGH/CRITICAL` |
| mini | arm64 | `2026.3.24-mini-arm64-final-r1` | `367.27 MiB` | `0 HIGH/CRITICAL` |

### Experimental Images

| Track | Arch | Tag | Size | Status |
|---|---|---|---:|---|
| alpine | amd64 | `2026.3.24-alpine-amd64-final-r1` | `424.01 MiB` | `0 HIGH/CRITICAL`, experimental only |
| alpine | arm64 | `2026.3.24-alpine-arm64-final-r1` | - | build failed |

## Final Recommendation

Use these as the release matrix:
- `slim-amd64-final-r1`
- `slim-arm64-final-r1`
- `mini-amd64-final-r1`
- `mini-arm64-final-r1`

Use this as an optional experimental image only:
- `alpine-amd64-final-r1`

Do not publish Alpine as a full multi-arch stable track yet.

## What Was Fixed

The final candidate workflows addressed the two shared security problems found in earlier images:
- vulnerable npm-bundled `minimatch` and `tar` versions inside the runtime image
- stale `@hono/node-server@1.19.9` vendored into `dist/extensions/discord/node_modules`

The second issue was not coming from the root workspace lockfile.
It came from the bundled Discord extension runtime dependency staging path, which performs a separate `npm install` inside the built extension output and bypasses the root `pnpm` override unless explicitly corrected.

## Workflows

Main workflows added or used for final release preparation:
- `.github/workflows/build-openclaw-slim-final.yml`
- `.github/workflows/build-openclaw-mini-final.yml`
- `.github/workflows/build-openclaw-alpine-final.yml`
- `.github/workflows/scan-openclaw-image-security.yml`

## Notes

- This repository is workflow-only. It does not vendor or fork upstream OpenClaw source.
- Builds currently target upstream tag `refs/tags/v2026.3.24` unless overridden manually.
- Security scan policy in this repo is currently based on Trivy `CRITICAL,HIGH` findings.
- Alpine remains experimental because arm64 is not yet stable here.

---

## 中文说明

这个仓库不 fork `openclaw/openclaw`。
它只维护 GitHub Actions 工作流，在构建时检出上游 OpenClaw 源码，并将容器镜像发布到 `ghcr.io/winffychu/openclaw`。

## 当前状态

这一轮发布前整理已经收口，主发布线路已经完成重建、基本功能验证和 Trivy `CRITICAL/HIGH` 安全扫描。

### 推荐发布镜像

| 线路 | 架构 | Tag | 体积 | 安全结果 |
|---|---|---|---:|---|
| slim | amd64 | `2026.3.24-slim-amd64-final-r1` | `796.28 MiB` | `0 HIGH/CRITICAL` |
| slim | arm64 | `2026.3.24-slim-arm64-final-r1` | `453.00 MiB` | `0 HIGH/CRITICAL` |
| mini | amd64 | `2026.3.24-mini-amd64-final-r1` | `710.84 MiB` | `0 HIGH/CRITICAL` |
| mini | arm64 | `2026.3.24-mini-arm64-final-r1` | `367.27 MiB` | `0 HIGH/CRITICAL` |

### 实验性镜像

| 线路 | 架构 | Tag | 体积 | 状态 |
|---|---|---|---:|---|
| alpine | amd64 | `2026.3.24-alpine-amd64-final-r1` | `424.01 MiB` | `0 HIGH/CRITICAL`，仅建议实验用途 |
| alpine | arm64 | `2026.3.24-alpine-arm64-final-r1` | - | 构建失败 |

## 最终发布建议

正式发布矩阵建议使用：
- `slim-amd64-final-r1`
- `slim-arm64-final-r1`
- `mini-amd64-final-r1`
- `mini-arm64-final-r1`

可选实验镜像：
- `alpine-amd64-final-r1`

当前不建议把 Alpine 作为完整双架构稳定发布线路。

## 这轮修复了什么

最终候选版主要解决了此前镜像里共通的两个安全问题：
- runtime 镜像中 npm 自带依赖树里的 `minimatch` / `tar` 漏洞
- `dist/extensions/discord/node_modules` 中被打包进去的旧版 `@hono/node-server@1.19.9`

第二个问题并不是根工作区 lockfile 没升级，而是 Discord 扩展的 bundled runtime deps 打包链会在构建产物目录里单独执行一次 `npm install`，如果不额外修正，就不会继承根 `pnpm override` 的约束。

## 关键工作流

本轮发布前整理主要使用或新增了这些工作流：
- `.github/workflows/build-openclaw-slim-final.yml`
- `.github/workflows/build-openclaw-mini-final.yml`
- `.github/workflows/build-openclaw-alpine-final.yml`
- `.github/workflows/scan-openclaw-image-security.yml`

## 备注

- 这个仓库只负责 workflow，不保存上游 OpenClaw fork。
- 默认构建目标仍是上游 tag `refs/tags/v2026.3.24`，除非手动覆盖。
- 这里的安全结论基于 Trivy 的 `CRITICAL,HIGH` 结果。
- Alpine 仍然只能视为实验线，因为 arm64 还不稳定。
