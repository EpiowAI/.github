# Reusable workflows

The workflows EpiowAI repositories call instead of copying. A caller pins one
surface by commit; adopting one never pins another.

| Workflow | Caller adds |
| --- | --- |
| [`chat-senders.yml`](./chat-senders.yml) | the `chat-senders` job below, and its `ci-ok` result |
| [`release.yml`](./release.yml) | the `release` job below |
| [`deploy.yml`](./deploy.yml) | the `deploy` job below |
| [`ci.yml`](./ci.yml) | nothing: it is this repository's own gate, not a reusable workflow |

`ci.yml` in this repository is not a reusable workflow. It gates this
repository itself, because the runner contract in
[`scripts/check-owned-runner-profiles.sh`](../../scripts/check-owned-runner-profiles.sh)
can only be checked where the workflows live.

## Chat senders

Fails a change that adds a direct chat-API host (`api.telegram.org`,
`hooks.slack.com`, `slack.com/api/chat.postMessage`, `discord.com/api/webhooks`)
outside the allow-list the shared action owns. Chat messages go through Notify
(SylphxAI/cloud `docs/adr/ADR-01M495XFM4158579XRYVC92M6S-notify-chat-channel.md`),
which holds the bot token and applies preferences, suppression and receipts.

```yaml
jobs:
  chat-senders:
    uses: EpiowAI/.github/.github/workflows/chat-senders.yml@<40-hex commit>
```

Add `chat-senders` as a needed job of your `ci-ok` aggregate and list it in that
aggregate's `required:` input, so a skipped chat-senders lane cannot read as a
pass:

```yaml
  ci-ok:
    if: always() && !cancelled()
    needs: [lint, test, chat-senders]
    runs-on: sylphx-linux-control
    timeout-minutes: 5
    steps:
      - uses: SylphxAI/.github/.github/actions/needs-pass@<40-hex commit>
        with:
          needs: ${{ toJSON(needs) }}
          required: lint,test,chat-senders
```

The check runs the shared action pinned to
`5cfd55a41435cca8e4b7caf6506faf04ca08d200`. Only added lines are checked, so an
existing sender does not block a change while it moves to Notify.

## Release

Publishes to npm and notifies Slack through `@sylphx/bump`.

```yaml
jobs:
  release:
    uses: EpiowAI/.github/.github/workflows/release.yml@<40-hex commit>
    secrets: inherit
```

Inputs: `build` (default `bun run build`, empty skips it) and
`working-directory` (default `.`). Org secrets: `NPM_TOKEN` (required),
`SLACK_WEBHOOK` (optional).

## Deploy

Builds and deploys one package.

```yaml
jobs:
  deploy:
    uses: EpiowAI/.github/.github/workflows/deploy.yml@<40-hex commit>
    with:
      environment: production
    secrets: inherit
```

Inputs: `environment` (required), `package-manager` (default `pnpm`),
`build-command` (default `build`), `working-directory` (default `.`). Secret:
`DEPLOY_TOKEN`. Output: `deployment-url`.

## Runners

Reusable workflows here run on `sylphx-linux-standard`. `chat-senders.yml` runs
on GitHub's `ubuntu-latest`, which is free on a public repository and is not
worth a runner seat for a diff read. If any of these repositories is ever made
private, `chat-senders.yml` must move to `sylphx-linux-standard` in the same
change, which
[`scripts/check-owned-runner-profiles.sh`](../../scripts/check-owned-runner-profiles.sh)
enforces on the pull request.
