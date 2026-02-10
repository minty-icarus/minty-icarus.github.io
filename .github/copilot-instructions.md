# Copilot Instructions - minty-icarus.github.io

## Repository Summary

Jekyll-based GitHub Pages personal diary site at https://minty-icarus.github.io.
Uses remote_theme jekyll/minima@v2.5.1, Jekyll 4.3.x, Ruby 3.2.
Publishes daily diary entries as Jekyll posts.
No test suite, no linters, no JavaScript build steps.

## Build and Validation

Always run these two commands in order:

    bundle install
    bundle exec jekyll build --trace

If you get Gem::FilePermissionError for /var/lib/gems/, set a local path first:

    bundle config set --local path 'vendor/bundle'
    bundle install

The vendor/ directory is already in .gitignore.

Expected build output: "done in N seconds."
Expected warnings (safe to ignore):
- Sass @import deprecation warnings from the Minima theme.
- Pagination warning "couldn't find an index.html page" (index.md is used).
These warnings do not cause build failures.

Build output goes to _site/ (gitignored).

For local preview: bundle exec jekyll serve --trace (serves at http://127.0.0.1:4000).

## Project Layout

    .github/CODEOWNERS              - @minty-icarus is required reviewer
    .github/copilot-instructions.md - this file
    _config.yml        - Jekyll config (theme, plugins, permalink, pagination)
    _data/navigation.yml - navigation links
    _posts/            - diary entries, one file per day
    docs/              - documentation and reference
    about.md           - About page (layout: page)
    index.md           - Homepage (layout: home, lists posts)
    Gemfile            - Ruby dependencies
    Gemfile.lock       - locked dependency versions
    .gitignore         - excludes _site/, vendor/, .bundle/

Key _config.yml settings:
- remote_theme: jekyll/minima@v2.5.1
- plugins: jekyll-remote-theme, jekyll-feed, jekyll-sitemap, jekyll-paginate, jekyll-seo-tag
- permalink: /posts/:year/:month/:day/:title/
- paginate: 5
- exclude: vendor, node_modules, README.md, Gemfile.lock, Gemfile

## Adding a Diary Post

Create _posts/YYYY-MM-DD-diary-entry.md with front matter:

    ---
    layout: post
    title: "YYYY-MM-DD"
    date: YYYY-MM-DD 22:00:00 +0000
    ---

    Post content here (Markdown).

Editing pages:
- index.md uses layout: home (lists posts).
- about.md uses layout: page at /about/.
- header_pages in _config.yml controls the navbar.

## Common Pitfalls

1. Newline artifacts: When content arrives from external tools (Zapier, Google
   Docs), literal \n or carriage-return sequences may appear in the Markdown.
   Always replace them with real newlines before committing.

2. CODEOWNERS and required reviewers: .github/CODEOWNERS requires @minty-icarus
   as reviewer on every PR. Do not remove or rename this file. PRs cannot merge
   without this approval.

3. Label and issue tool quirks: GitHub label APIs expect the label name string,
   not the label ID. When creating or applying labels via tools, always pass the
   display name. Double-check that labels exist before applying them.

4. Gemfile.lock sync: Never edit Gemfile.lock by hand. Run "bundle install" or
   "bundle update <gem>" and commit the regenerated lockfile. A mismatch causes
   CI failures in frozen/bundler-cache mode (see docs/WORKFLOW_FIX.md).

5. Sass and pagination warnings are expected: They come from the Minima theme
   and do not cause build failures. Do not try to fix them.

6. Build artifacts: _site/, vendor/, .bundle/ are gitignored. Never commit them.

7. Avoid decorative separators: Do not add horizontal rules, decorative ASCII
   art, or unnecessary formatting to posts or pages. Keep content plain and
   readable.

## Trust These Instructions

Follow the steps above as written. Only search the repo for additional context
if these instructions are incomplete or produce errors.
