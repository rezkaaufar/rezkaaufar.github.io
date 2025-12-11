# rezkaaufar.github.io (Featherweight Jekyll)

Jekyll-powered personal site and blog using the ultra-minimal [Featherweight](https://github.com/Cutwell/featherweight) structure. Content lives in Markdown with a few lightweight layouts for posts, news, and projects.

## Quickstart

1. Install Ruby (>= 3.0) and Bundler.
2. Install dependencies: `bundle install`.
3. Run locally: `bundle exec jekyll serve` (defaults to http://localhost:4000).
4. Build for production: `bundle exec jekyll build` (outputs to `_site/`).

## Content

- Blog posts: `_posts/YYYY-MM-DD-slug.md` (front matter: `title`, `description` optional, `date`, optional `tags`, optional `image`).
- News: `_news/<slug>.md` (front matter: `title`, `description` optional, `date`).
- Projects: `_projects/<slug>.md` (front matter: `title`, `description`, optional `heroImage`, optional `links` array of `{label, href}`, optional `tags`).
- Pages: `index.md` (about + recent lists), `blog/`, `news/`, `projects/`, `publications.md`, `404.html`.
- Assets: `/assets/` (images, PDFs, etc.). Paths in content remain `/assets/...`.

## Layouts & styling

- `_layouts/default.html` — site shell with nav/footer and inline Featherweight-style CSS from `_includes/main.css`.
- `_layouts/post.html`, `_layouts/news.html`, `_layouts/project.html` — simple content templates.
- Math rendering uses MathJax (`$...$`, `$$...$$`).

## Plugins & config

- `jekyll-feed` and `jekyll-sitemap` enabled in `_config.yml`.
- Permalinks follow `/:title/` to keep short URLs.
- RSS at `/feed.xml` and sitemap at `/sitemap.xml` after build.

Deploy with any static host (GitHub Pages works with `bundle exec jekyll build` output). EOF