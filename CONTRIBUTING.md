# Contributing to WC2026 Simulator

Thank you for taking the time to contribute! This document explains the branch strategy, workflow, and standards to follow so changes flow safely from development to production.

---

## Table of Contents

- [Branch Strategy](#branch-strategy)
- [Development Workflow](#development-workflow)
- [Commit Message Convention](#commit-message-convention)
- [Pull Request Process](#pull-request-process)
- [Environment Pipeline](#environment-pipeline)
- [Code Style](#code-style)
- [Reporting Issues](#reporting-issues)

---

## Branch Strategy

| Branch | Purpose | Auto-deploys to |
|---|---|---|
| `dev` | Active development, feature work | DEV environment |
| `uat` | Staging — stakeholder testing before production | UAT environment |
| `main` | Production-ready code only | PROD (with manual approval) |

**Never commit directly to `uat` or `main`.** All changes start on `dev` and move up via pull requests.

---

## Development Workflow

### 1. Start from `dev`

```bash
git checkout dev
git pull origin dev
```

### 2. Make your changes

Since the entire app is a single `index.html`, edit that file directly. Keep changes focused — one logical change per commit.

### 3. Commit with a conventional commit message

```bash
git add index.html
git commit -m "feat: add group edit validation"
```

See [Commit Message Convention](#commit-message-convention) below.

### 4. Push to `dev`

```bash
git push origin dev
```

This **automatically triggers the DEV deploy**. Your changes will be live at the DEV URL within ~60 seconds. Verify them there before raising a PR.

### 5. Open a PR: `dev` → `uat`

Once DEV looks good, open a pull request from `dev` into `uat` on GitHub. Merging it automatically deploys to UAT.

### 6. Open a PR: `uat` → `main`

After stakeholder sign-off on UAT, open a pull request from `uat` into `main`. This triggers the PROD deploy workflow, which **pauses for manual approval** from the repo owner before going live.

---

## Commit Message Convention

This project uses [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>: <short description>
```

| Type | When to use |
|---|---|
| `feat` | A new feature or capability |
| `fix` | A bug fix |
| `refactor` | Code restructuring with no behaviour change |
| `perf` | Performance improvement |
| `style` | CSS / visual changes only |
| `docs` | Documentation changes |
| `chore` | Build, config, or tooling changes |
| `ci` | Changes to GitHub Actions workflows |
| `test` | Adding or updating tests |

**Examples:**

```
feat: add host nation boost to expected goals calculation
fix: penalty shootout not resolving in extra time
refactor: extract getSquadFormFactor into standalone function
ci: add workflow_dispatch trigger to deploy-uat.yml
docs: update README with new environment URLs
```

---

## Pull Request Process

1. **Title** — use a conventional commit style title summarising the change
2. **Description** — fill in the PR template (What, Why, Changes)
3. **Self-review** — check the DEV environment looks right before requesting review
4. **One approval** required before merging to `uat`
5. **Stakeholder sign-off on UAT** required before merging to `main`
6. **PROD approval** — the repo owner must approve the GitHub Actions deployment gate before PROD goes live

---

## Environment Pipeline

```
You push to dev
      │
      ▼
DEV auto-deploys  ──►  https://bobiknaj1229.github.io/wc2026-simulator/dev/
      │
      │  (PR: dev → uat, merged after review)
      ▼
UAT auto-deploys  ──►  https://bobiknaj1229.github.io/wc2026-simulator/uat/
      │
      │  (PR: uat → main, merged after stakeholder sign-off)
      ▼
PROD awaits approval ──► https://bobiknaj1229.github.io/wc2026-simulator/
```

Each environment injects its own visual banner (orange = DEV, purple = UAT) so it is always clear which environment you are looking at. PROD has no banner.

---

## Code Style

- The app is intentionally a **single self-contained HTML file** — keep it that way
- JavaScript is written in **vanilla ES6+** — no frameworks, no build step
- CSS uses **CSS custom properties** for theming — add new colours to `:root` not inline
- Minified one-liners are acceptable for data constants (e.g. `FIFA_POINTS`, `FLAGS`) — favour readability for functions
- Only add a comment when the **why** is non-obvious — avoid restating what the code does

---

## Reporting Issues

- **Bug?** Use the [bug report template](.github/ISSUE_TEMPLATE/bug_report.md)
- **Feature idea?** Use the [feature request template](.github/ISSUE_TEMPLATE/feature_request.md)
- **Question?** Open a [blank issue](https://github.com/bobiknaj1229/wc2026-simulator/issues/new)
