---
name: blog-publishing
description: Add or update posts on the ryanbehan.github.io research blog. Use when copying reports from the decisions-agent repo into posts/, adding a POSTS index entry, keeping the shared theme working, or publishing new standalone article pages.
---

# Blog publishing

The site is plain static HTML on GitHub Pages (`main` branch, repo root, `.nojekyll`). There is no build step on the blog side. Live at `https://ryanbehan.github.io/`.

## Layout

- `index.html` — landing page. Posts are listed by the inline `POSTS` array (newest first).
- `posts/YYYY-MM-DD-slug.html` — one post per file. Every post must be fully standalone: inlined CSS/JS/media, no external assets, no trackers.

## Hard rules

- **No personal information or secrets in post files.** No name, employer, real email, file paths, hostnames, API keys, or `.env` contents. Demo credentials (e.g. `agent@tixly.test`) are fine. Scan before committing: `rg -in 'behan|/users/|sk-|api[_-]?key|bearer|@gmail' posts/`.
- **Post files must keep the shared theme working.** Reports are generated with the theme inlined from `decisions-agent/docs/theme.{css,js}` (see "Source of truth" below). If you hand-author a post, inline `docs/theme.css` into `<style>` and `docs/theme.js` into a `<script>` in `<head>`, and use theme CSS variables (`--bg`, `--ink`, `--accent`, …) instead of hardcoded colors.
- **JS-drawn charts** must read `getComputedStyle` palette values and the page's `<html>` needs `data-theme-reload` so a theme switch redraws correctly.

## Theme contract

- Three themes: `neon` (default), `paper`, `term`. Stored in `localStorage["rb-theme"]` under `data-theme` on `<html>` — shared with `index.html`.
- `theme.js` prepends the rb://notes brand + `../` home link and appends the switcher, only inside `nav.top .inner`. It only shows the home link when the URL contains `/posts/`.
- Cross-report links use source filenames (`report.html`, `qa-report.html`); `theme.js` maps them to post slugs via `SLUGS`. **Update `SLUGS` in `decisions-agent/docs/theme.js` when adding a new post that other posts link to.**

## Source of truth

Posts are generated in `/Users/ryan/build/decisions-agent`:

1. `bun run report` builds `docs/report.html` and `docs/qa-report.html` (media embedded).
2. Copy each into `posts/<date>-<slug>.html` in this repo.
3. Add/update the entry in `index.html`'s `POSTS` array.
4. `git add posts index.html && git commit && git push` — Pages redeploys automatically.
5. Verify the Pages build: `gh api repos/ryanbehan/ryanbehan.github.io/pages/builds/latest --jq '.status'`, then curl the post URL for `rb-theme`.

Template edits belong in `decisions-agent/docs/*.template.html` + `*-data.json`, never by editing the built HTML directly — changes would be lost on the next `bun run report`.
