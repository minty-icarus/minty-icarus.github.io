# Copilot Instructions — minty-icarus.github.io

## Repository Summary

This is a **Jekyll-based GitHub Pages** personal diary site deployed at <https://minty-icarus.github.io>. It uses the **Minima v2.5.1** remote theme, Jekyll **4.3.x**, and Ruby **3.2**. The site publishes daily diary entries as Jekyll posts. There are no application tests, no linter configuration, and no JavaScript build steps.

## Build & Validation

### Prerequisites

- Ruby 3.2 (matches CI)
- Bundler (installed via `gem install bundler`)

### Install Dependencies

Always run this before building:

```bash
bundle install
```

> **Local development note:** If you encounter `Gem::FilePermissionError` for `/var/lib/gems/`, configure a local install path first:
> ```bash
> bundle config set --local path 'vendor/bundle'
> bundle install
> ```
> The `vendor/` directory is already in `.gitignore`.

### Build the Site

```bash
bundle exec jekyll build --trace
```

- **Expected output:** `done in ≈0.5–1 seconds.`
- **Expected warnings (safe to ignore):** Sass `@import` deprecation warnings from the Minima theme and a pagination warning (`couldn't find an index.html page`). These do not cause build failures.
- Build output goes to `_site/` (gitignored).

### Local Preview

```bash
bundle exec jekyll serve --trace
```

Opens a local server at `http://127.0.0.1:4000`.

### CI Workflow — PR Preview (`.github/workflows/pr-preview.yml`)

Runs on every pull request targeting `main`. Steps:

1. Checkout (`actions/checkout@v4`)
2. Setup Ruby 3.2 with bundler-cache (`ruby/setup-ruby@v1`)
3. `bundle install`
4. `bundle exec jekyll build --trace`
5. Upload `_site` as artifact

**Always verify your changes pass `bundle exec jekyll build --trace` locally before pushing.** The CI runs this exact command. A build failure will block the PR.

### No Tests or Linters

There is no test suite, no linting, and no formatting tools configured. The only validation is a successful Jekyll build.

## Project Layout

```
.
├── .github/
│   ├── CODEOWNERS              # @minty-icarus required reviewer
│   ├── copilot-instructions.md # (this file)
│   └── workflows/
│       └── pr-preview.yml      # CI: build on PRs to main
├── _config.yml                 # Jekyll site configuration (theme, plugins, permalink)
├── _data/
│   └── navigation.yml          # Navigation links
├── _posts/                     # Diary entries (one per day)
│   ├── 2026-02-04-diary-entry.md
│   └── 2026-02-05-diary-entry.md
├── docs/                       # Documentation / reference
│   ├── architecture-diagram.md
│   ├── PR2-fix-summary-2026-02-04.md
│   └── WORKFLOW_FIX.md
├── about.md                    # About page (layout: page)
├── index.md                    # Homepage (layout: home)
├── Gemfile                     # Ruby dependencies
├── Gemfile.lock                # Locked dependency versions
├── .gitignore                  # Excludes _site/, vendor/, .bundle/, etc.
└── README.md                   # Brief repo description
```

### Key Configuration (`_config.yml`)

- **Theme:** `remote_theme: jekyll/minima@v2.5.1`
- **Plugins:** jekyll-remote-theme, jekyll-feed, jekyll-sitemap, jekyll-paginate, jekyll-seo-tag
- **Permalink:** `/posts/:year/:month/:day/:title/`
- **Pagination:** 5 posts per page
- **Excluded from build:** `vendor`, `node_modules`, `README.md`, `Gemfile.lock`, `Gemfile`

### Adding a New Diary Post

Create a file in `_posts/` named `YYYY-MM-DD-diary-entry.md` with this front matter:

```markdown
---
layout: post
title: "YYYY-MM-DD"
date: YYYY-MM-DD 22:00:00 +0000
---

Post content here (Markdown).
```

### Editing Pages

- `index.md` — Homepage, uses `layout: home` (lists posts).
- `about.md` — About page at `/about/`, uses `layout: page`.
- Navigation links are listed in `_config.yml` under `header_pages`.

### CODEOWNERS

All changes require review from `@minty-icarus` (`.github/CODEOWNERS`).

## Common Pitfalls

1. **Do not modify `Gemfile.lock` manually.** Run `bundle install` or `bundle update <gem>` and commit the regenerated lockfile.
2. **Sass deprecation warnings are expected.** They originate from the Minima theme and do not affect the build.
3. **The pagination warning** about missing `index.html` is benign — `index.md` is used as the homepage.
4. **Always keep `Gemfile.lock` in sync with `Gemfile`.** A mismatch causes CI failures in frozen/bundler-cache mode (see `docs/WORKFLOW_FIX.md`).
5. **`_site/`, `vendor/`, `.bundle/`** are gitignored — never commit build output or installed gems.

## Trust These Instructions

Follow the steps above as written. Only search the repo for additional context if these instructions are incomplete or produce errors.
