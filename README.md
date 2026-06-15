# 🏆 FIFA World Cup 2026 Simulator

[![DEV](https://img.shields.io/badge/env-DEV-orange?style=flat-square)](https://bobiknaj1229.github.io/wc2026-simulator/dev/)
[![UAT](https://img.shields.io/badge/env-UAT-purple?style=flat-square)](https://bobiknaj1229.github.io/wc2026-simulator/uat/)
[![PROD](https://img.shields.io/badge/env-PROD-brightgreen?style=flat-square)](https://bobiknaj1229.github.io/wc2026-simulator/)
[![GitHub Pages](https://img.shields.io/badge/hosted_on-GitHub_Pages-blue?style=flat-square&logo=github)](https://bobiknaj1229.github.io/wc2026-simulator/)
[![Deploy PROD](https://github.com/bobiknaj1229/wc2026-simulator/actions/workflows/deploy-prod.yml/badge.svg)](https://github.com/bobiknaj1229/wc2026-simulator/actions/workflows/deploy-prod.yml)

A realistic, data-driven simulator for the **2026 FIFA World Cup** — hosted jointly by USA, Canada & Mexico. Simulate all 104 matches across the group stage and knockout rounds using a statistically grounded Poisson-based match engine.

---

## Live Environments

| Environment | URL | Purpose |
|---|---|---|
| **PROD** | [wc2026-simulator](https://bobiknaj1229.github.io/wc2026-simulator/) | Live public app |
| **UAT** | [wc2026-simulator/uat](https://bobiknaj1229.github.io/wc2026-simulator/uat/) | Stakeholder testing & sign-off |
| **DEV** | [wc2026-simulator/dev](https://bobiknaj1229.github.io/wc2026-simulator/dev/) | Developer preview |

---

## Features

- **48 teams** across 12 groups — full WC2026 draw
- **Realistic match simulation** using a Poisson goal model (industry standard for football analytics)
- **FIFA points-based team strength** — non-linear, preserving real gaps between elite and mid-table nations
- **Confederation quality multipliers** — UEFA, CONMEBOL, CONCACAF, CAF, AFC, OFC
- **Squad form ratings** — derived from real player form data across 9 fully populated national teams
- **Host nation boost** — USA, Canada & Mexico receive a statistically accurate home advantage
- **Historical penalty shootout skill** — based on each nation's real WC penalty record
- **Full tournament bracket** — Round of 32 through to the Final
- **Group standings, goal difference, form tracking**
- **Team roster viewer** — full 16-player squads with club, caps, goals, age, form
- **Dark mode** support
- **Search** across teams and players

---

## How the Simulation Works

The simulator uses a **Poisson distribution** to model goals, which is the standard approach in football analytics and sports betting markets.

### Team Strength

```
Strength = FIFA_Points_Score × Confederation_Multiplier × Squad_Form_Factor
```

| Factor | What it does |
|---|---|
| FIFA Points Score | Normalises 2025 FIFA points (1100–1900 range) to 0.28–1.00; preserves non-linear elite gaps |
| Confederation Multiplier | UEFA 1.00 → CONMEBOL 0.98 → CONCACAF 0.87 → CAF 0.84 → AFC 0.82 → OFC 0.75 |
| Squad Form Factor | Aggregates individual player form ratings (6.5–9.4 scale) into a team multiplier (0.88–1.12) |

### Expected Goals (xG)

```
xG = 1.35 (WC baseline) × (Attacker_Strength / Defender_Strength) × 0.9
```

Host nations (USA, Canada, Mexico) receive a **+12% xG boost** backed by historical host-nation data.

### Goal Generation

Goals are sampled using the **Knuth Poisson algorithm** — mathematically exact sampling that naturally produces the correct frequency of 1-0, 1-1, 2-1 results without needing hardcoded probabilities.

### Penalties

Each nation has a historical shootout skill rating (Germany 0.80 → England 0.58) derived from World Cup history. The winner is drawn probabilistically rather than with a coin flip.

---

## Tech Stack

| Layer | Technology |
|---|---|
| App | Vanilla HTML, CSS, JavaScript (single file, zero dependencies) |
| Icons | [Tabler Icons](https://tabler.io/icons) CDN |
| Hosting | GitHub Pages |
| CI/CD | GitHub Actions |

No build step, no framework, no package manager — the entire app is `index.html`.

---

## Project Structure

```
wc2026-simulator/
├── index.html                          # Entire app — HTML + CSS + JS
└── .github/
    └── workflows/
        ├── deploy-dev.yml              # Auto-deploys dev branch → /dev/
        ├── deploy-uat.yml              # Auto-deploys uat branch → /uat/
        └── deploy-prod.yml             # Deploys main → / (requires manual approval)
```

---

## CI/CD Pipeline

```
dev branch ──► DEV deploy (automatic)
    │
    │  Pull Request: dev → uat
    ▼
uat branch ──► UAT deploy (automatic)
    │
    │  Pull Request: uat → main
    ▼
main branch ──► PROD deploy (requires manual approval in GitHub Environments)
```

Every push to `dev` or `uat` triggers an automatic deploy to its respective environment. Deploying to **PROD requires a manual approval** from the repo owner — configured via GitHub → Settings → Environments → Production.

---

## Getting Started (Local Development)

No build tools required. Open the file directly in a browser:

```bash
git clone https://github.com/bobiknaj1229/wc2026-simulator.git
cd wc2026-simulator
open index.html        # macOS
# or
start index.html       # Windows
```

To work on a feature:

```bash
git checkout dev
# make your changes to index.html
git add index.html
git commit -m "feat: your change description"
git push origin dev    # triggers DEV deploy automatically
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full branch and PR workflow.

---

## Contributing

All contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

- [Report a bug](.github/ISSUE_TEMPLATE/bug_report.md)
- [Request a feature](.github/ISSUE_TEMPLATE/feature_request.md)

---

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for a full history of changes.

---

## License

MIT License — see [LICENSE](LICENSE) for details.
