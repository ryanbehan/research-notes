# ryanbehan.github.io

Research notes — experiments, benchmarks, and demos on AI infrastructure and
agent tooling.

Live at https://ryanbehan.pages.dev/ (Cloudflare Pages, primary) and
https://ryanbehan.github.io/ (GitHub Pages, mirrors `main`).

Workflow for adding posts: see `.devin/skills/blog-publishing/SKILL.md`.

## Structure

- `index.html` — post index (edit the `POSTS` array inline to add an entry)
- `posts/<date>-<slug>.html` — standalone reports; each is a single file with
  data, charts, and media embedded — no build step, no external assets
- `.nojekyll` — serves files as-is, no Jekyll processing

## Adding a post

1. Drop the HTML file into `posts/YYYY-MM-DD-slug.html`
2. Add an entry to `POSTS` in `index.html`
3. Commit and push — Pages redeploys from `main` root automatically
