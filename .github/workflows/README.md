# SylphxAI Reusable Workflows

可重複使用的 GitHub Actions workflows。

## Release Workflow

自動發佈到 npm，支援 Slack 通知。

### 基本用法

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    branches: [main]

jobs:
  release:
    uses: SylphxAI/.github/.github/workflows/release.yml@main
    secrets: inherit
```

就係咁簡單！`secrets: inherit` 會自動繼承 org secrets (`NPM_TOKEN`, `SLACK_WEBHOOK`)。

### 自訂 Build

```yaml
jobs:
  release:
    uses: SylphxAI/.github/.github/workflows/release.yml@main
    with:
      build-command: 'bun run build'
    secrets: inherit
```

### Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `build-command` | Build command (empty = skip) | `''` |
| `working-directory` | Working directory | `.` |

### Required Org Secrets

在 GitHub Organization Settings → Secrets → Actions 設定：

| Secret | Description |
|--------|-------------|
| `NPM_TOKEN` | NPM authentication token |
| `SLACK_WEBHOOK` | Slack webhook URL (optional) |

## CI Workflow

持續集成：lint、typecheck、test、build。

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: SylphxAI/.github/.github/workflows/ci.yml@main
    secrets: inherit
```

## 工作流程

```
Push to main
    ↓
Release workflow
    ↓
Creates/updates Release PR
    ↓
Merge PR
    ↓
Publishes to npm + Slack notification
```

Uses [@sylphx/bump](https://github.com/SylphxAI/bump) under the hood.
