# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the GitHub profile repo (`mahmoodhamdi/mahmoodhamdi`) — its `README.md` renders on the owner's profile page. There is no application code; the repo is a README plus three GitHub Actions workflows that regenerate visual assets.

Owner: Mahmoud Hamdy — Full-Stack Engineer (Flutter / Node.js / Python), based in Egypt, currently at ROV GROUP on the **Escore** esports platform for MENA.

## Workflows and Generated Artifacts

Three workflows run on schedule and produce assets in different locations. Knowing which workflow owns which path matters because two of these locations are auto-overwritten.

| Workflow | Schedule | Writes to | Used by README? |
|---|---|---|---|
| `.github/workflows/snake.yml` | every 12h + push to main | `output` branch (`github-snake.svg`, `github-snake-dark.svg`) | **No** — kept for future use but not embedded. The owner's contribution graph was too sparse to render a satisfying snake; section was removed on user request. Re-embed via `<picture>` referencing `raw.githubusercontent.com/.../output/github-snake-dark.svg` once the graph fills out. |
| `.github/workflows/profile-3d.yml` | daily 00:00 UTC + push to main | `profile-3d-contrib/*.svg` on `main` (auto-commits as github-actions[bot]) | Yes — `profile-night-rainbow.svg` |
| `.github/workflows/waka-readme.yml` | daily 00:00 UTC | between `<!--START_SECTION:waka-->` / `<!--END_SECTION:waka-->` markers in `README.md` | **Yes** — `WAKATIME_API_KEY` secret is set; section auto-populates with last-7-days stats. |

Implications:
- **Never hand-edit files in `profile-3d-contrib/`** — `profile-3d.yml` overwrites the directory and commits the result.
- **Never push to the `output` branch directly** — `snake.yml` owns it.

## Theme Tokens (Tokyo Night, muted)

Switched from loud cyan/coral to muted Tokyo Night on user feedback ("اللون فاقع"). All badges and SVG services now use this palette:

- Primary: `#7AA2F7` (soft sky blue) — was `#00D9FF`
- Accent: `#BB9AF7` (lavender) — was `#FF6B6B` for "accent" use
- Sage: `#9ECE6A` — for positive/success badges
- Soft coral: `#F7768E` — only for `fix` PR badge and similar warning-ish accents (much softer than `#FF6B6B`)
- Muted text: `#A9B1D6` — was `#C9D1D9` (text on dark, contact badges)
- Background: `#0D1117` (GitHub dark) — kept for seamless blend with GitHub UI; do NOT switch to `#1A1B26` (Tokyo Night native bg) because it'd visually mismatch the page background
- Stats theme name: `tokyonight` — pass this to GRS endpoints and let the theme handle title/icon/text colors. Only override `bg_color=0D1117` and `hide_border=true`.
- Badge style: `style=flat-square` for everything except the 3 final-CTA buttons in "Let's Work Together" (those use `style=for-the-badge` for prominence)

Loud colors that were explicitly removed: `#00D9FF`, `#FF6B6B`, `#9333EA` (purple was too saturated). If you ever need pure white text, prefer `#A9B1D6`.

## External SVG Services Used

Working services as of last review (2026-05-12). Multiple GRS deployments are unstable — note the warnings carefully.

| Purpose | Service | Status & Notes |
|---|---|---|
| Animated text intro | `readme-typing-svg.demolab.com` | ✅ Reliable. Pipe-separated `lines=` parameter, URL-encoded |
| Tech stack icons | `skillicons.dev` | ✅ Reliable. One `?i=...` URL per category, comma-separated |
| Stats / top-langs / pin cards | `gh-readme-stats.vercel.app` | ⚠️ Returns valid SVG in `curl` (200) and even via GitHub camo (200) but **rendered as broken images on the live profile** — intermittent 504s from camo's perspective. Don't trust for stats card / top-langs. Use only as a fallback or for testing. |
| Streak stats | `streak-stats.demolab.com` | ✅ Reliable. Use `theme=tokyonight` and override only `background=0D1117&hide_border=true`. |
| Activity graph | `github-readme-activity-graph.vercel.app` | ✅ Reliable. Use `color=7AA2F7&line=BB9AF7&point=A9B1D6&bg_color=0D1117`. |
| Contribution stats badge | `readme-contribution-stats.aman-kumar-connect.workers.dev` | ✅ Cloudflare Worker; shows top contribution repos including PRs to other users' repos |
| Profile views counter | `komarev.com/ghpvc` | ✅ Reliable |
| Generic badges | `img.shields.io` | ✅ Reliable. Use this for all per-repo `stars`, `forks`, `languages/top` badges in Featured Projects rather than pin cards |
| Snake animation | `raw.githubusercontent.com/mahmoodhamdi/mahmoodhamdi/output/...` | Not currently embedded (see Workflows table) |

### ⚠️ Services that DON'T work (avoid)

- **`github-readme-stats.vercel.app`** — paused (`DEPLOYMENT_PAUSED` 503). The official deployment.
- **`github-readme-stats.hackclub.dev`** — its `/api?username=...`, `/api/pin/`, and `/api/top-langs/` endpoints all need a `PAT_1` env token that isn't configured, so they render an SVG containing the literal text "No GitHub API tokens found - Please add an env variable called PAT_1 with your GitHub API token in vercel". Avoid until they fix it.
- **`github-readme-stats-git-masterrstaa-rickstaa.vercel.app`** — returns 401.
- **`github-profile-trophy.vercel.app`** — works technically, but the rendered trophy graphics are cartoonish/childish-looking and were removed from the README on user request. Don't re-add unless asked.

## README Structure

Sections, in order:

1. **Hero** — typing SVG (6 lines) + 3 status badges (Available · Egypt · MENA) + 5 capability badges (Mobile · Backend · Cloud · OSS · Bilingual) + combined contact-and-counters row (LinkedIn · Email · WhatsApp · GitHub · views · followers · repos). All flat-square style for visual cohesion.
2. **About** — bio with concrete numbers and explicit OSS contributor mention with stargazer counts (Zustand 58k, Payload 42k, Joi 21k, dotenv 20k, Dify 141k).
3. **Currently Building** — 3-column table: Escore (work), MWM (own SaaS), TStore (top open-source app).
4. **Tech Stack** — four `skillicons.dev` rows (Languages / Mobile & Frontend / Backend & APIs / Databases & Infra)
5. **GitHub Statistics** — **static** infographic of 6 shields.io badges (Rank · PRs · Commits · Stars · Issues · Projects) in a single row, then the dynamic `streak-stats.demolab.com` card below. The dynamic GRS stats card and top-langs were removed because every available GRS deployment is broken on the live profile (canonical paused, hackclub needs PAT, gh-readme-stats mirror gets intermittent 504s via camo). The static numbers were captured 2026-05-12 from the `gh-readme-stats.vercel.app` SVG (Rank A, 854 PRs, 3103 commits, 171 stars, 28 issues, 69 projects) — refresh quarterly via the same endpoint.
6. **Weekly Coding Activity** (subsection) — WakaTime auto-populated block between markers.
7. **Activity Graph** (subsection) — last 31 days, themed.
8. **Featured Projects** — six manual 2×3 cards. Heading link + 2-3 shields.io badges (stars/forks/lang) + description + topic hashtags.
9. **Open Source & Community Impact** — auto contribution-stats badge + **`Notable Merged Contributions` table** with REAL merged PRs to external repos (Zustand, Joi, Payload, dotenv, Dify, debug, path-to-regexp, cookie). Highest-credibility section.
10. **3D Contribution Graph** — single `<img>` from `profile-3d-contrib/profile-night-rainbow.svg`.
11. **Services I Offer** — services table only (no inline CTAs — those moved to the next section to avoid the duplicate-contact problem).
12. **Let's Work Together** — single consolidated final section with response-time note and 3 prominent `for-the-badge` CTAs (WhatsApp · Email · LinkedIn). This is the ONLY place final CTAs appear; do not duplicate.

Sections explicitly removed on user feedback (do not re-add without being asked):
- ~~Trophies / Achievements~~ — cartoonish look
- ~~Snake animation~~ — sparse contribution graph made it look weak
- ~~Duplicate Let's Connect footer~~ — was redundant with the Services-section CTAs above it

## Editing Guidance

- **Notable Merged Contributions table** is the single highest-credibility section. To add new entries, run `gh search prs --author=mahmoodhamdi --merged --limit 100 --json repository,title,url,number,createdAt | jq -r '.[] | select(.repository.nameWithOwner | startswith("mahmoodhamdi/") | not) | "\(.createdAt[:10]) | \(.repository.nameWithOwner) #\(.number) | \(.title)"'` and insert any new merged PRs to *external* repos. Then `gh api repos/<owner>/<repo>` for the stargazer count. Sort the table by stargazer count descending. The table currently lists **13 PRs across 10 projects** (~340k combined stars: Dify 141k, Zustand 58k, Payload 42k, nanoid 27k, Joi 21k, dotenv 20k, debug 11k, helmet 11k, path-to-regexp 8.5k, cookie 1.5k).
- **Featured Projects** are hard-coded as a 2×3 manual table — each cell links to a repo and pulls live stars/forks/language counts via shields.io badges (no broken pin-card service). When higher-star or more-impressive repos appear, swap the repo references in all 4 places per cell (heading link, 2-3 badge URLs, description).
- **Currently Building** lists 3 active projects. Keep the structure: one work project (Escore at ROV GROUP), one own SaaS, one open-source headliner.
- **About-section numbers and stargazer counts** ("175+ repositories", "140+ stars", "Zustand 58k", "Dify 141k") are point-in-time snapshots — refresh quarterly or when deltas exceed ~10%.
- **Typing SVG** lives in the hero — lines are semicolon-separated and URL-encoded. Spaces become `+`, `·` is `%C2%B7`, `—` is `%E2%80%94`, `'` stays literal.
- Verify every external URL renders before considering an edit done. Test in a browser, not just `curl` — some services return 200 but with error-text rendered as SVG (the HackClub PAT_1 problem).
- The 3D SVG path in the README (`./profile-3d-contrib/profile-night-rainbow.svg`) is one of ~10 variants the workflow produces; swap the filename to change the theme rather than regenerating.
- Audience focus is Arabic / MENA market — keep regional framing when rewording copy, but keep README itself in English for international visibility.
