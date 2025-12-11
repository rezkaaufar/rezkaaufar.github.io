# Contributing

Thanks for contributing! The site now runs on Jekyll with a Featherweight-style layout.

## Workflow

- Create a topic branch from `main` for each change.
- Install dependencies: `bundle install`.
- Run locally with `bundle exec jekyll serve` while editing.
- Before opening a PR, run `bundle exec jekyll build` to ensure the site builds cleanly.

## Content changes

- Blog posts: add files under `_posts/YYYY-MM-DD-slug.md` with front matter `title`, optional `description`, `date`, optional `tags`, and optional `image`.
- News: add `_news/<slug>.md` with `title`, `date`, and optional `description`.
- Projects: add `_projects/<slug>.md` with `title`, `description`, optional `heroImage`, optional `links` array (`{label, href}`), and optional `tags`.
- Assets: place images/PDFs under `assets/` and reference them with `/assets/...` paths.
