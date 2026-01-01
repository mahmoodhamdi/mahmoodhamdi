# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a GitHub profile repository (`mahmoodhamdi/mahmoodhamdi`). It contains a special `README.md` that displays on the owner's GitHub profile page.

## Repository Structure

```
mahmoodhamdi/
├── README.md                    # Main profile README
├── CLAUDE.md                    # Claude Code guidance
├── .github/
│   └── workflows/
│       ├── snake.yml            # Snake animation generator
│       ├── waka-readme.yml      # WakaTime stats updater
│       └── profile-3d.yml       # 3D contribution graph generator
└── profile-3d-contrib/          # Generated 3D contribution SVGs (auto-generated)
```

## GitHub Actions Workflows

### Snake Animation (`snake.yml`)
- Generates snake animation from contribution graph
- Runs every 12 hours and on push to main
- Outputs to `output` branch as `github-snake.svg` and `github-snake-dark.svg`

### WakaTime Stats (`waka-readme.yml`)
- Updates coding stats in README
- Runs daily at midnight UTC
- **Requires:** `WAKATIME_API_KEY` secret (get from wakatime.com/settings/api-key)

### 3D Profile (`profile-3d.yml`)
- Generates 3D contribution graph
- Runs daily and on push to main
- Outputs to `profile-3d-contrib/` directory

## Key Details

- Owner: Mahmoud Hamdy (Full-Stack Software Engineer)
- Tech focus: Flutter, Node.js, Python, Web Scraping
- Location: Egypt
- Contact: hmdy7486@gmail.com, +201019793768

## When Modifying

- Preserve Tokyo Night theme consistency (color: `#00D9FF`, accent: `#FF6B6B`)
- Test image/badge URLs before committing
- Run workflows manually after changes to verify they work
- Keep the Arabic/MENA market focus in content
