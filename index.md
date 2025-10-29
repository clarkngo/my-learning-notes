---
title: Home
layout: home
nav_order: 1
---

Welcome — this is my learning notes site.

This site is a personal collection of short technical notes, post-mortems, quick how-tos, and project summaries I create while learning and experimenting. It’s organized so I can:

- Capture short, actionable learnings from projects and investigations.
- Store lightweight runbooks and SOPs that help with debugging and on-call work.
- Keep design notes, architecture sketches, and references for later reuse.

Where to start

- Career Journal — real-world learnings and quick retros from past projects (e.g., `docs/career-journal/ebay`).
- Docs — short documentation pages and examples (use the sidebar to navigate pages and guides).
- Posts — occasional longer write-ups and experiments (search the `docs` folder or `_posts/` if present).

Preview locally

1. Install Ruby, Bundler and the project gems:

```bash
bundle install
```

2. Serve locally (live reload):

```bash
bundle exec jekyll serve --livereload
```

This serves the site at http://localhost:4000 for quick previews.

Contributing and structure

- Notes live under `docs/` and are organized by topic folders. Files include simple front-matter (title/parent) so the theme shows them in the sidebar.
- If you keep working on these notes, add files, keep them brief, and link related pages for discoverability.

Thanks for visiting — use the sidebar to explore topics, or open an issue/PR on the repo if you want to suggest changes.

