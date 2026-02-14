# Contributing

Thanks for contributing! Here's how we work.

## Branch Strategy

We use two long-lived branches:

- **`main`** — Production. Always deployable. Never push directly.
- **`dev`** — Development. All work lands here first.

### Workflow

1. Create a feature branch from `dev`
2. Open a PR targeting **`dev`** (never `main`)
3. Get 1 approval + CI green → squash merge into `dev`
4. Periodically, `dev` is promoted to `main` (merge, not squash)

### Branch Protection

Both `main` and `dev` are protected:

- Pull request required (no direct pushes)
- 1 approval required
- `build` CI check must pass
- No force pushes
- No branch deletion

## Development

### Stack

- **Framework:** Next.js (App Router)
- **API:** Hono
- **Database:** Neon (Postgres) + Drizzle ORM
- **Migrations:** Atlas (not drizzle-kit)
- **Hosting:** Vercel
- **Styling:** Tailwind CSS

### Tooling

- **Package manager:** Bun (not npm)
- **Linting/Formatting:** Biome (not eslint + prettier)
- **Testing:** Bun test

### Commands

```bash
bun install        # Install dependencies
bun run dev        # Start dev server
bun run build      # Production build
bun run lint       # Lint + format check
bun test           # Run tests
```

## Pull Requests

- Keep PRs focused — one feature or fix per PR
- Write a clear summary of what changed and why
- Ensure CI is green before requesting review
- Squash merge is preferred
