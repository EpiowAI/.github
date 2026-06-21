# EpiowAI Organization Tooling

EpiowAI/.github is the organization-level entry point for EpiowAI community files, profile text, agent adapters, issue and pull request templates, and reusable GitHub Actions workflows.

## Lifecycle

- State: `production`
- Layer: `tooling`
- Machine manifest: [`.doctrine/project.json`](./.doctrine/project.json)

## Goals

- Publish the EpiowAI organization profile and contribution entry points.
- Provide EpiowAI-owned GitHub issue, pull request, CI, deploy, and release workflow surfaces.
- Keep EpiowAI runtime adapters and organization tooling discoverable to agents.

## Non-Goals

- This repository does not own EpiowAI product application code or runtime behavior.
- This repository does not own enterprise engineering doctrine; that belongs in [SylphxAI/doctrine](https://github.com/SylphxAI/doctrine).
- This repository does not own base dependency-update policy; org relay policy belongs in `EpiowAI/renovate-config`.

## Boundary

This repository owns EpiowAI organization-level GitHub surfaces. Product-specific behavior belongs in the product repository that owns it, and shared doctrine/process changes belong upstream in `SylphxAI/doctrine`.

## Public Surfaces

- Organization profile: [`profile/README.md`](./profile/README.md)
- Issue templates: [`.github/ISSUE_TEMPLATE/`](./.github/ISSUE_TEMPLATE/)
- Pull request template: [`.github/PULL_REQUEST_TEMPLATE.md`](./.github/PULL_REQUEST_TEMPLATE.md)
- Reusable CI workflow: [`.github/workflows/ci.yml`](./.github/workflows/ci.yml)
- Reusable deploy workflow: [`.github/workflows/deploy.yml`](./.github/workflows/deploy.yml)
- Reusable release workflow: [`.github/workflows/release.yml`](./.github/workflows/release.yml)
- Agent adapters: [`.claude/`](./.claude/)

## Delivery

The repository currently has no required status contexts on `main`. Changes take effect when GitHub serves the merged organization files and when consumers call the reusable workflows. Production proof is GitHub main readback plus the doctrine project-control audit; workflow behavior is additionally proven by downstream consumer workflow runs.
