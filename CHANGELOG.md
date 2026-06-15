# Changelog

All notable changes to this project are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [2.0.0] — 2026-06-15

### Added
- **Poisson goal model** — goals per team now sampled using Knuth's exact Poisson algorithm with a WC baseline of 1.35 goals/team/match; produces realistic 1-0, 1-1, 2-1 scoreline distributions
- **FIFA points-based team strength** — replaces rank-number inversion with actual 2025 FIFA points (1100–1900 range), preserving non-linear quality gaps between elite and mid-table nations
- **Confederation strength multipliers** — UEFA (1.00), CONMEBOL (0.98), CONCACAF (0.87), CAF (0.84), AFC (0.82), OFC (0.75); prevents over-rating of lower-confederation nations
- **Squad form factor** — team strength now incorporates average player form ratings from existing squad data, creating a live feedback loop between squad quality and match outcomes
- **Host nation boost** — USA, Canada, Mexico receive a +12% expected goals uplift, consistent with historical host-nation performance data
- **Historical penalty shootout skill** — each nation assigned a shootout conversion rating based on World Cup history (Germany 0.80, England 0.58, etc.); replaces the previous 50/50 coin flip
- **DEV/UAT/PROD CI/CD pipeline** — three GitHub Actions workflows (`deploy-dev.yml`, `deploy-uat.yml`, `deploy-prod.yml`) with automatic deploys to environment sub-paths and a manual approval gate for PROD
- **Environment banner** — orange ribbon on DEV, purple ribbon on UAT; PROD remains clean
- `CONTRIBUTING.md`, `CHANGELOG.md`, issue templates, and PR template

### Changed
- `getStrength()` now uses `FIFA_POINTS` + `CONF_MULTIPLIER` + `getSquadFormFactor()` instead of `Math.max(0.05, 1.1 - rank/100)`
- `simScore()` now calls `poissonRandom(getExpectedGoals())` instead of uniform random branches
- `simPenalties()` now accepts `(t1, t2)` team arguments and uses skill-weighted probability

### Removed
- Hardcoded 22% fixed draw probability
- Hardcoded uniform `Math.floor(Math.random()*3)` goal generation
- 50/50 penalty resolution (`Math.random() > 0.5`)

---

## [1.1.0] — 2026-06-10

### Added
- Full standings page with group-by-group breakdown
- Team roster modal — 16-player squads with club, caps, goals, age, and form bar
- Player search across all 48 teams on the Teams page
- Form pip indicators (W/D/L) in standings
- Dark mode support via `prefers-color-scheme`

### Changed
- Improved bracket visualisation with scrollable horizontal layout
- Match cards now show top-rated player per team after result

---

## [1.0.0] — 2026-06-05

### Added
- Initial release — 48 teams, 12 groups, full WC2026 draw
- Group stage simulation with standings (points, GD, GF)
- Knockout bracket: Round of 32 → Round of 16 → QF → SF → Final
- Best 8 third-place teams advance to Round of 32
- Simulate next match, simulate round, simulate to final, reset controls
- Editable group composition via team picker modal
- Dashboard with progress bar and recent match feed
- GitHub Pages deployment on `main` branch
