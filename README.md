# OpenClaw GHCR Builder

This repository does not fork `openclaw/openclaw`.
It only hosts GitHub Actions workflows that checkout the upstream OpenClaw source at build time and publish container images to this account's GHCR namespace.

Current scope:
- `linux/amd64` only
- manual workflow dispatch
- pushes to `ghcr.io/winffychu/openclaw`

After amd64 is stable, add arm64 and manifest creation.
