# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the GitHub profile repo (`mahmoodhamdi/mahmoodhamdi`) — its `README.md` renders on the owner's profile page. There is no application code; the repo is a README plus three GitHub Actions workflows that regenerate visual assets.

Owner: Mahmoud Hamdy — Full-Stack Engineer (Flutter / Node.js / Python), based in Egypt, currently at ROV GROUP on the **Escore** esports platform for MENA.

## Workflows and Generated Artifacts

Three workflows run on schedule and produce assets in different locations. Knowing which workflow owns which path matters because two of these locations are auto-overwritten.

| Workflow | Schedule | Writes to | Used by README? |
|---|---|---|---|
| `.github/workflows/snake.yml` | every 12h + push to main | `output` branch (`github-snake.svg`, `github-snake-dark.svg`) | **Yes** — embedded via `<picture>` block in the Contribution Activity section, served from `raw.githubusercontent.com/.../output/...` |
| `.github/workflows/profile-3d.yml` | daily 00:00 UTC + push to main | `profile-3d-contrib/*.svg` on `main` (auto-commits as github-actions[bot]) | Yes — `profile-night-rainbow.svg` |
| `.github/workflows/waka-readme.yml` | daily 00:00 UTC | between `<!--START_SECTION:waka-->` / `<!--END_SECTION:waka-->` in `README.md` (markers present in the "Weekly Coding Activity" subsection) | **Yes** once `WAKATIME_API_KEY` secret is added — the section title renders empty until then. To enable: get an API key from https://wakatime.com/settings/api-key, then add it as a repo Secret named `WAKATIME_API_KEY` (Settings → Secrets and variables → Actions → New repository secret). |

Implications:
- **Never hand-edit files in `profile-3d-contrib/`** — `profile-3d.yml` overwrites the directory and commits the result.
- **Never push to the `output` branch directly** — `snake.yml` owns it.
- The README's `<picture>` block already serves both light (`github-snake.svg`) and dark (`github-snake-dark.svg`) variants based on viewer preference.

## README Structure

Sections, in order:

1. **Hero** — animated typing SVG (6 lines including OSS contributor pitch) + 3 status badges + contact badges + 3 profile counters + 5 quick-stats badges (Mobile / Backend / Cloud / OSS Contributor / Languages)
2. **About** — bio with concrete numbers (175+ repos, 140+ stars) and explicit OSS contributor mention with stargazer counts (Zustand 58k, Payload 42k, Joi 21k, dotenv 20k, Dify 141k). Update numbers as they grow.
3. **Currently Building** — 3-column table: Escore (work), MWM (own SaaS), TStore (top open-source app). Update when active project mix changes.
4. **Tech Stack** — four `skillicons.dev` rows (Languages / Mobile & Frontend / Backend & APIs / Databases & Infra)
5. **GitHub Statistics** — stats card + streak + top languages, all themed `tokyonight`, then `Weekly Coding Activity` (WakaTime), then `Achievements` (trophies in 2×4 grid), then `Activity Graph` (last 31 days)
6. **Featured Projects** — six pinned-style cards in a 2×3 table
7. **Open Source & Community Impact** — auto contribution-stats badge + **`Notable Merged Contributions` table** with REAL merged PRs to external repos (Zustand, Joi, Payload, dotenv, Dify, debug, path-to-regexp, cookie). This table is the single highest-credibility section — keep it sorted by stargazer count and add new merged PRs as they land.
8. **Contribution Activity** — snake animation (`<picture>` light/dark) + 3D contribution graph
9. **Services I Offer** — services table + three big CTA buttons (WhatsApp / Email / LinkedIn)
10. **Let's Connect** — footer with full-size badges and response-time note

## Theme Tokens

Used consistently across all SVG service URLs and badges:

- Primary: `#00D9FF` (cyan) — title_color, ring_color, primary CTA
- Accent: `#FF6B6B` (coral) — icon_color, fire, line, accent CTA
- Background: `#0D1117` (GitHub dark) — bg_color, labelColor
- Text: `#C9D1D9` — text_color
- Stats theme name: `tokyonight`
- Badge style: `style=for-the-badge` for hero/CTAs, `style=flat` for inline contact badges

## External SVG Services Used

When changing or adding visual elements, prefer these proven-working services:

| Purpose | Service | Notes |
|---|---|---|
| Animated text intro | `readme-typing-svg.demolab.com` | Pipe-separated `lines=` parameter |
| Tech stack icons | `skillicons.dev` | One `?i=...` URL per category, comma-separated |
| Stats / top-langs cards | `github-readme-stats.hackclub.dev` | **Use this mirror, not `github-readme-stats.vercel.app`** — the upstream Vercel deployment was paused (`DEPLOYMENT_PAUSED`) when this README was redesigned. The HackClub mirror is API-compatible for stats and top-langs. |
| **Pin cards** (per-repo) | ⚠️ Avoid — both deployments fail | The HackClub mirror's `/api/pin/` endpoint requires a `PAT_1` GitHub token env var that's not configured (renders SVG that says "No GitHub API tokens found"). The canonical Vercel deployment is paused. **Use a custom table with `img.shields.io/github/stars/...` and `.../languages/top/...` badges instead** — see the Featured Projects section in `README.md`. |
| Streak stats | `streak-stats.demolab.com` | |
| Trophies | `github-profile-trophy.vercel.app` | |
| Activity graph | `github-readme-activity-graph.vercel.app` | |
| Contribution stats badge | `readme-contribution-stats.aman-kumar-connect.workers.dev` | Cloudflare Worker; shows top contribution repos including PRs to other users' repos |
| Profile views counter | `komarev.com/ghpvc` | |
| Generic badges | `img.shields.io` | All badges use the project's theme colors |
| Snake animation | `raw.githubusercontent.com/mahmoodhamdi/mahmoodhamdi/output/...` | Generated by `snake.yml` |

If `github-readme-stats.hackclub.dev` ever returns errors, retry the canonical `github-readme-stats.vercel.app` first — the URL paths and query params are identical, so swapping the host is sufficient.

## Editing Guidance

- **Notable Merged Contributions table** is the single highest-credibility section. To add new entries, run `gh search prs --author=mahmoodhamdi --merged --limit 100 --json repository,title,url,number` and insert any new merged PRs to *external* repos (filter out `mahmoodhamdi/*` self-owned ones). Sort by repo stargazer count descending. The table currently lists 11 PRs across 8 projects (~304k combined stars).
- **Featured Projects** are hard-coded as a 2×3 manual table — each cell links to a repo and pulls live stars/forks/language counts via shields.io badges (no broken pin-card service). When higher-star or more-impressive repos appear, swap the repo references in all 4 places per cell (heading link, 2-3 badge URLs, description). Currently featured: `Flutter-Developer-Interview-Questions` (top draw), `TStore` (production Flutter e-commerce), `flutter_google_workspace_integration`, `mwm` (bilingual CMS), `esports-flask`, `Markdown-to-PDF`.
- **Currently Building section** lists 3 active projects in a table. Update when the project mix meaningfully changes — keep one work project (Escore at ROV GROUP), one own SaaS, one open-source headliner.
- **About-section numbers and stargazer counts in the typing SVG / About list** ("175+ repositories", "140+ stars", "Zustand 58k", "Dify 141k", etc.) are point-in-time snapshots — refresh them quarterly or when the deltas exceed ~10%.
- **Typing SVG** lives in the hero `<img src="https://readme-typing-svg.demolab.com?...">` — lines are semicolon-separated and URL-encoded. Spaces become `+`, `·` is `%C2%B7`, `—` is `%E2%80%94`, `'` stays literal.
- **Trophies** are in a 2×4 grid (`row=2&column=4`) — `github-profile-trophy` auto-selects which trophies to show based on the user's stats; edit the row/column counts to change layout, not which trophies appear.
- Verify every external URL renders before considering an edit done. Many of these services have rate-limit / availability issues; test in a browser, not just `curl`.
- The 3D SVG path in the README (`./profile-3d-contrib/profile-night-rainbow.svg`) is one of ~10 variants the workflow produces; swap the filename to change the theme rather than regenerating.
- Audience focus is Arabic / MENA market — keep regional framing when rewording copy, but keep README itself in English for international visibility.
