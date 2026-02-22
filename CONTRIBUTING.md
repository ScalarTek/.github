# Contributing to ScalarTek Projects

Internal development guide for the ScalarTek team.

## Repositories

Each site is its own repo under the [ScalarTek](https://github.com/ScalarTek) org. There is no monorepo — work on each site independently.

| Repo | Vertical |
|---|---|
| `wheeler-legal` | Legal |
| `haven-stone-realty` | Real estate |
| `powerful-hands-cleaning` | Cleaning services |
| `ironwood-table` | Restaurant |
| `scalartek-website` | Agency (ScalarTek itself) |
| `forge-athletics` | CrossFit gym |
| `coastal-threads` | Surf shop |
| `pristine-detail` | Auto detailing |
| `serenity-wellness` | Med spa |
| `DamGoodLandscaping` | Landscaping |
| `controlresults-website` | Client site |

## Branch Strategy

```
main        → production (protected, deploy on merge)
develop     → integration branch (protected, CI required)
feature/*   → short-lived, branch from develop
fix/*       → bug fixes, branch from develop
chore/*     → maintenance, branch from develop
```

- Branch from `develop`, not `main`
- Keep branches short-lived — one feature or fix per branch
- Delete branches after merging

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add mortgage calculator to hero
fix: correct mobile nav z-index on scroll
chore: update dependencies
ci: add build workflow
```

- Lowercase, present tense, no period at the end
- Keep the subject line under 72 characters
- No co-author lines

## Development Workflow

### Starting a new feature

```bash
git checkout develop
git pull origin develop
git checkout -b feature/my-feature
```

### Local dev

```bash
npm install
npm run dev
```

### Submitting work

```bash
git push origin feature/my-feature
# Open a PR targeting develop on GitHub
```

PRs to `develop` require the CI build to pass.
PRs to `main` require CI to pass + 1 approval.

## CI

Every repo runs a GitHub Actions build on push and pull request to `main` and `develop`. The workflow runs `npm ci && npm run build`. PRs cannot be merged if the build fails.

## Deployment

All sites deploy via **Cloudflare Pages** with GitHub integration. Merging to `main` triggers an automatic production deploy. No manual deploy steps required.

## Stack

Every ScalarTek site is built on the same foundation:

- **Framework:** Next.js (App Router, static export)
- **Styling:** Tailwind CSS v4 with `@theme` design tokens
- **Animations:** Framer Motion
- **Forms:** React Hook Form + Zod
- **Fonts:** Google Fonts via `next/font`
- **Language:** TypeScript (strict)

Using a shared stack across all sites is intentional — it means any team member can context-switch between sites without relearning tooling. Patterns, component structure, and conventions carry over. Only the visual identity and domain-specific features differ per site.

See the `CLAUDE.md` in each repo for site-specific conventions and design tokens.

## Adding a New Site to Cloudflare Pages

When a new site repo is ready, connect it to Cloudflare Pages via the dashboard:

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
2. Authorize GitHub if prompted, then select the repo from the ScalarTek org
3. Configure the build settings:
   - **Framework preset:** None
   - **Build command:** `npm run build`
   - **Build output directory:** `out`
4. Click **Save and Deploy**

Cloudflare Pages will deploy automatically on every push to `main` from that point on. No further configuration is required for production deploys.
