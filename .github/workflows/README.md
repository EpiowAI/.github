# EpiowAI Organization Workflows

這些是可重複使用的 GitHub Actions workflows，所有 EpiowAI 組織內的項目都可以使用。

## 可用的 Workflows

### 1. CI Workflow (`ci.yml`)

持續集成流程，包括 lint、type check、test 和 build。

**使用方法：**

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:

jobs:
  ci:
    uses: EpiowAI/.github/.github/workflows/ci.yml@main
    with:
      node-version: '20'
      package-manager: 'pnpm'  # or 'npm', 'bun'
      working-directory: '.'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

**參數：**
- `node-version`: Node.js 版本（預設：`'20'`）
- `package-manager`: 套件管理器（預設：`'pnpm'`）
- `working-directory`: 工作目錄（預設：`'.'`）

**Secrets：**
- `NPM_TOKEN`: 私有套件的 NPM token（選填）

---

### 2. Deploy Workflow (`deploy.yml`)

部署流程，支援多環境部署。

**使用方法：**

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy-production:
    uses: EpiowAI/.github/.github/workflows/deploy.yml@main
    with:
      environment: 'production'
      package-manager: 'pnpm'
      build-command: 'build'
      working-directory: '.'
    secrets:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

**參數：**
- `environment`: 部署環境（必填：`'dev'`, `'staging'`, `'production'`）
- `package-manager`: 套件管理器（預設：`'pnpm'`）
- `build-command`: Build 指令（預設：`'build'`）
- `working-directory`: 工作目錄（預設：`'.'`）

**Secrets：**
- `DEPLOY_TOKEN`: 部署 token（選填）

**Outputs：**
- `deployment-url`: 部署的 URL

---

### 3. Release Workflow (`release.yml`)

自動發佈流程，支援 Changesets 和 Semantic Release。

**使用方法：**

```yaml
name: Release

on:
  push:
    branches: [main]

jobs:
  release:
    uses: EpiowAI/.github/.github/workflows/release.yml@main
    with:
      package-manager: 'pnpm'
      release-type: 'changesets'  # or 'semantic-release'
      working-directory: '.'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**參數：**
- `package-manager`: 套件管理器（預設：`'pnpm'`）
- `release-type`: 發佈類型（預設：`'changesets'`）
- `working-directory`: 工作目錄（預設：`'.'`）

**Secrets：**
- `NPM_TOKEN`: NPM 發佈 token（必填）
- `GITHUB_TOKEN`: GitHub token（必填）

---

## 組合使用範例

```yaml
name: Full Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:

jobs:
  # 先跑 CI
  ci:
    uses: EpiowAI/.github/.github/workflows/ci.yml@main
    with:
      package-manager: 'pnpm'

  # CI 通過後部署到 staging
  deploy-staging:
    needs: ci
    if: github.ref == 'refs/heads/develop'
    uses: EpiowAI/.github/.github/workflows/deploy.yml@main
    with:
      environment: 'staging'
    secrets:
      DEPLOY_TOKEN: ${{ secrets.STAGING_DEPLOY_TOKEN }}

  # CI 通過後部署到 production 並發佈
  deploy-production:
    needs: ci
    if: github.ref == 'refs/heads/main'
    uses: EpiowAI/.github/.github/workflows/deploy.yml@main
    with:
      environment: 'production'
    secrets:
      DEPLOY_TOKEN: ${{ secrets.PRODUCTION_DEPLOY_TOKEN }}

  release:
    needs: deploy-production
    if: github.ref == 'refs/heads/main'
    uses: EpiowAI/.github/.github/workflows/release.yml@main
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 注意事項

1. **Secrets 管理**:
   - Organization secrets 可以在 GitHub organization settings 設定
   - Repository secrets 會優先於 organization secrets
   - 使用 `inherit` 關鍵字可以繼承所有 secrets

2. **版本控制**:
   - 使用 `@main` 獲取最新版本
   - 使用 `@v1` 等 tag 鎖定特定版本
   - 開發時可以使用 `@develop` 分支

3. **權限**:
   - Reusable workflows 需要適當的 permissions
   - 某些 workflows 需要 `contents: write` 或 `id-token: write`

4. **繼承 Secrets**:

```yaml
jobs:
  ci:
    uses: EpiowAI/.github/.github/workflows/ci.yml@main
    secrets: inherit  # 繼承所有 secrets
```

---

## 貢獻

如需新增或修改 workflows，請：
1. Fork 此 repository
2. 建立 feature branch
3. 提交 Pull Request
4. 等待 review

---

## 資源

- [GitHub Reusable Workflows 文件](https://docs.github.com/en/actions/concepts/workflows-and-actions/reusable-workflows)
- [Organization Secrets 設定](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions)

---

**Copyright © 2025 Epiow Limited**
