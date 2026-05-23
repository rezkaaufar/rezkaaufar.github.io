# SEO Audit — rezkaaufar.github.io

> Generated: 2026-05-23 | Site: Jekyll 4.3 on GitHub Pages | Theme: Featherweight-inspired minimal

---

## 1. Current Implementation Summary

### What's Working

| Feature | Status | Location |
|---|---|---|
| Clean URL slugs (`/:title/`) | ✅ | `_config.yml` |
| `<title>` tag with page fallback | ✅ | `_layouts/default.html:7` |
| Meta `description` with fallback chain | ✅ | `_layouts/default.html:8` |
| `charset` and `viewport` meta tags | ✅ | `_layouts/default.html:5-6` |
| `lang` attribute on `<html>` | ✅ | `_layouts/default.html:2` |
| Auto-generated `sitemap.xml` | ✅ | `jekyll-sitemap` plugin |
| RSS/Atom feeds for all collections | ✅ | `jekyll-feed` plugin |
| Google Analytics GA4 (async) | ✅ | `_layouts/default.html:12-18` |
| Async script loading (GA4, MathJax) | ✅ | `_layouts/default.html` |
| Inline CSS (no render-blocking stylesheet) | ✅ | `_includes/main.css` |
| Image alt text via Markdown syntax | ✅ | Post `.md` files |
| Proper H1 → H2 → H3 heading hierarchy | ✅ | `_layouts/post.html` + content |
| Per-post `description` in front matter | ✅ | `_posts/*.md` |

### What's Missing or Broken

| Issue | Severity |
|---|---|
| No Open Graph meta tags | **Critical** |
| No Twitter Card meta tags | **Critical** |
| No canonical `<link>` tag | **Critical** |
| `robots.txt` references a non-existent sitemap | **High** |
| No JSON-LD / Schema.org structured data | **High** |
| Empty `alt=""` on post hero images | **Medium** |
| No RSS autodiscovery `<link>` in `<head>` | **Medium** |
| Site description lacks topic keywords | **Medium** |
| No `jekyll-seo-tag` plugin | **Medium** |
| No `loading="lazy"` on images | **Low** |
| Responsive image `srcset` not wired up | **Low** |
| No breadcrumb structured data | **Low** |

---

## 2. Critical Issues — Fix These First

### 2.1 Fix `robots.txt` Sitemap Path

**Problem:** `robots.txt` points to `sitemap-index.xml`, which does not exist. Googlebot follows this URL and gets a 404, making it harder to discover all pages.

**Current (`robots.txt`):**
```
Sitemap: https://rezkaaufar.github.io/sitemap-index.xml
```

**Fix:**
```
User-agent: *
Disallow:

Sitemap: https://rezkaaufar.github.io/sitemap.xml
```

---

### 2.2 Add Open Graph Meta Tags

**Problem:** When you share a post link on LinkedIn, Slack, WhatsApp, or iMessage, no rich preview (title, description, image) is shown — just a plain URL. This drastically reduces click-through rates from social channels.

**Fix — add to `_layouts/default.html` inside `<head>`:**
```html
<!-- Open Graph -->
<meta property="og:title" content="{{ page.title | default: site.title | escape }}">
<meta property="og:description" content="{{ page.description | default: site.description | escape }}">
<meta property="og:type" content="{% if page.layout == 'post' %}article{% else %}website{% endif %}">
<meta property="og:url" content="{{ page.url | absolute_url }}">
<meta property="og:site_name" content="{{ site.title | escape }}">
{% if page.image %}<meta property="og:image" content="{{ page.image | absolute_url }}">{% endif %}
{% if page.layout == 'post' %}
<meta property="article:published_time" content="{{ page.date | date_to_xmlschema }}">
<meta property="article:author" content="{{ site.author }}">
{% endif %}
```

**What each tag does:**
- `og:title` — headline shown in the link preview card
- `og:description` — summary text below the headline
- `og:type` — tells platforms this is an article vs. a generic web page
- `og:url` — canonical URL used by the platform to deduplicate shares
- `og:image` — thumbnail shown in the card (highest click-through impact)
- `article:published_time` — enables "published X days ago" on some platforms

---

### 2.3 Add Twitter Card Meta Tags

**Problem:** Twitter/X uses its own card system separate from Open Graph. Without these tags, shared links appear as plain text.

**Fix — add to `_layouts/default.html` inside `<head>`:**
```html
<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{{ page.title | default: site.title | escape }}">
<meta name="twitter:description" content="{{ page.description | default: site.description | escape }}">
{% if page.image %}<meta name="twitter:image" content="{{ page.image | absolute_url }}">{% endif %}
```

**Note:** If most posts don't have a `page.image`, use `summary` instead of `summary_large_image` to avoid blank image cards:
```html
<meta name="twitter:card" content="{% if page.image %}summary_large_image{% else %}summary{% endif %}">
```

---

### 2.4 Add Canonical Link Tag

**Problem:** GitHub Pages may serve your content over multiple hostnames (e.g., `http://` vs `https://`, or via a CDN). Without a canonical tag, Google treats these as duplicate pages and may split ranking signals or penalise the site.

**Fix — add to `_layouts/default.html` inside `<head>`:**
```html
<link rel="canonical" href="{{ page.url | absolute_url }}">
```

This must come after the `<title>` tag and uses the `url: https://rezkaaufar.github.io` value already set in `_config.yml` to build the full URL.

---

## 3. High Priority Improvements

### 3.1 Add JSON-LD Structured Data

**Why:** Google uses structured data to display rich results in SERPs — publication dates, author names, breadcrumbs, and sitelinks. Without it, your posts are plain blue links. With it, they can show metadata inline.

**Fix — add to `_layouts/default.html` inside `<head>`:**

**For blog posts (BlogPosting schema):**
```html
{% if page.layout == 'post' %}
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "{{ page.title | escape }}",
  "description": "{{ page.description | default: site.description | escape }}",
  "datePublished": "{{ page.date | date_to_xmlschema }}",
  "dateModified": "{{ page.last_modified_at | default: page.date | date_to_xmlschema }}",
  "author": {
    "@type": "Person",
    "name": "{{ site.author }}",
    "url": "{{ site.url }}"
  },
  "publisher": {
    "@type": "Person",
    "name": "{{ site.author }}"
  },
  "url": "{{ page.url | absolute_url }}"{% if page.image %},
  "image": "{{ page.image | absolute_url }}"{% endif %}
}
</script>
{% endif %}
```

**For the homepage (Person schema):**
```html
{% if page.url == '/' %}
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "{{ site.author }}",
  "url": "{{ site.url }}",
  "sameAs": [
    "https://github.com/rezkaaufar",
    "https://www.linkedin.com/in/aufarleo"
  ],
  "jobTitle": "Machine Learning Engineer",
  "description": "{{ site.description | escape }}"
}
</script>
{% endif %}
```

**Verification:** Paste any post URL into [Google's Rich Results Test](https://search.google.com/test/rich-results) after deploying.

---

### 3.2 Consider Installing `jekyll-seo-tag`

**Why:** The `jekyll-seo-tag` gem (maintained by GitHub) automatically generates OG tags, Twitter Cards, canonical links, and JSON-LD from your existing front matter. It eliminates the need to hand-roll all of the above.

**To add:**

`Gemfile`:
```ruby
gem "jekyll-seo-tag", "~> 2.8"
```

`_config.yml` plugins list:
```yaml
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag
```

`_layouts/default.html` (replace manual meta tags with):
```html
{% seo %}
```

**Trade-off:** The gem's output is opinionated and hard to customise. If you want full control over the markup (e.g., custom JSON-LD fields, conditional OG image logic), the manual approach in sections 2.2–3.1 is preferable.

---

## 4. Medium Priority Improvements

### 4.1 Add RSS Autodiscovery Link in `<head>`

**Problem:** The RSS feed at `/feed.xml` exists and works, but browsers and feed readers can only auto-detect it if a `<link>` tag is present in `<head>`. Without it, users have to know the URL manually.

**Fix — add to `_layouts/default.html` inside `<head>`:**
```html
<link rel="alternate" type="application/rss+xml" title="{{ site.title }} RSS Feed" href="{{ '/feed.xml' | absolute_url }}">
```

---

### 4.2 Fix Empty `alt` on Post Hero Images

**Problem:** In `_layouts/post.html`, the hero image block renders with `alt=""`:
```html
{% if page.image %}<p><img src="{{ page.image }}" alt=""></p>{% endif %}
```

An empty `alt` attribute signals to screen readers that the image is decorative. For a meaningful hero image, this hurts accessibility and is a minor SEO signal for image search.

**Fix (`_layouts/post.html`):**
```html
{% if page.image %}<p><img src="{{ page.image }}" alt="{{ page.title }}"></p>{% endif %}
```

Or, if you add an `image_alt` front matter key in posts:
```html
{% if page.image %}<p><img src="{{ page.image }}" alt="{{ page.image_alt | default: page.title }}"></p>{% endif %}
```

---

### 4.3 Improve the Site Description

**Problem:** The current site description in `_config.yml` is:
> "Rezka Leonandya's personal site and blog."

This is used as a fallback `<meta name="description">` on pages without their own description. It lacks the keywords that define your content niche, which reduces its relevance score for topical searches.

**Fix (`_config.yml`):**
```yaml
description: "Personal site and blog by Rezka Leonandya — data science, machine learning, statistics, causal inference, and AB testing."
```

Keep it under 160 characters (the SERP snippet limit). Front-matter descriptions on individual posts already override this fallback, so this primarily affects the homepage and index pages.

---

## 5. Lower Priority / Performance

### 5.1 Add `loading="lazy"` to Images

Images load eagerly by default, which delays Time-to-Interactive on posts with many figures. Native lazy loading is a one-line HTML attribute change.

**For inline Markdown images**, you'll need to either:
- Use a custom Kramdown hook (complex), or
- Add a small JavaScript snippet after `</main>`:

```html
<script>
  document.querySelectorAll('main img').forEach(img => {
    if (!img.hasAttribute('loading')) img.setAttribute('loading', 'lazy');
  });
</script>
```

This retroactively adds `loading="lazy"` without touching every post.

---

### 5.2 Wire Up Responsive Images with `srcset`

**Problem:** The `/assets/resized/` directory already contains images at 480px, 800px, and 1400px widths — but posts reference full-resolution originals in `/assets/img/`. Bandwidth is wasted on mobile.

This is a content authoring concern: you'd need to update post front matter or post body to reference the resized variants via `srcset`. A pragmatic approach:

1. When adding new post images, author them as:
   ```markdown
   <img src="/assets/img/foo.png"
        srcset="/assets/resized/foo/foo-480x240.png 480w,
                /assets/resized/foo/foo-800x400.png 800w,
                /assets/resized/foo/foo-1400x700.png 1400w"
        sizes="(max-width: 640px) 480px, (max-width: 1024px) 800px, 1400px"
        alt="Description" loading="lazy">
   ```
2. Backfill existing posts opportunistically.

---

### 5.3 Add Breadcrumb Structured Data

**Problem:** Without breadcrumb JSON-LD, Google cannot display the path hierarchy in SERPs (e.g., `rezkaaufar.github.io > Blog > Post Title`).

**Fix — add to `_layouts/post.html` or `default.html`:**
```html
{% if page.layout == 'post' %}
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "Home", "item": "{{ site.url }}/" },
    { "@type": "ListItem", "position": 2, "name": "Blog", "item": "{{ site.url }}/blog/" },
    { "@type": "ListItem", "position": 3, "name": "{{ page.title | escape }}", "item": "{{ page.url | absolute_url }}" }
  ]
}
</script>
{% endif %}
```

---

## 6. Consolidated Checklist

| # | Action | Effort | Impact |
|---|---|---|---|
| 2.1 | Fix `robots.txt` to point to `sitemap.xml` | 1 line | High — crawler can now find all pages |
| 2.2 | Add Open Graph tags to `default.html` | ~10 lines | High — rich previews on social/chat |
| 2.3 | Add Twitter Card tags to `default.html` | ~5 lines | High — rich previews on Twitter/X |
| 2.4 | Add canonical `<link>` to `default.html` | 1 line | High — prevents duplicate content |
| 3.1 | Add JSON-LD BlogPosting schema | ~20 lines | High — rich results in Google |
| 3.1 | Add JSON-LD Person schema on homepage | ~15 lines | Medium — Knowledge Panel eligibility |
| 4.1 | Add RSS autodiscovery `<link>` | 1 line | Low-Medium — feed reader discoverability |
| 4.2 | Fix `alt=""` on hero image in `post.html` | 1 line | Medium — accessibility + image SEO |
| 4.3 | Expand site description with keywords | 1 line | Medium — homepage/index meta description |
| 5.1 | Add JS lazy-loading snippet | ~5 lines | Low — page load time on image-heavy posts |
| 5.2 | Use `srcset` for responsive images | Per post | Low — bandwidth on mobile |
| 5.3 | Add breadcrumb JSON-LD | ~12 lines | Low — SERP breadcrumb trails |

---

## 7. Quick Copy-Paste: Full `<head>` Block

Replaces lines 4–29 of `_layouts/default.html` with all fixes from sections 2–4 applied:

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{{ page.title | default: site.title }}</title>
  <meta name="description" content="{{ page.description | default: site.description | escape }}">
  <link rel="canonical" href="{{ page.url | absolute_url }}">

  <!-- Open Graph -->
  <meta property="og:title" content="{{ page.title | default: site.title | escape }}">
  <meta property="og:description" content="{{ page.description | default: site.description | escape }}">
  <meta property="og:type" content="{% if page.layout == 'post' %}article{% else %}website{% endif %}">
  <meta property="og:url" content="{{ page.url | absolute_url }}">
  <meta property="og:site_name" content="{{ site.title | escape }}">
  {% if page.image %}<meta property="og:image" content="{{ page.image | absolute_url }}">{% endif %}
  {% if page.layout == 'post' %}<meta property="article:published_time" content="{{ page.date | date_to_xmlschema }}">{% endif %}

  <!-- Twitter Card -->
  <meta name="twitter:card" content="{% if page.image %}summary_large_image{% else %}summary{% endif %}">
  <meta name="twitter:title" content="{{ page.title | default: site.title | escape }}">
  <meta name="twitter:description" content="{{ page.description | default: site.description | escape }}">
  {% if page.image %}<meta name="twitter:image" content="{{ page.image | absolute_url }}">{% endif %}

  <!-- RSS Autodiscovery -->
  <link rel="alternate" type="application/rss+xml" title="{{ site.title }} RSS Feed" href="{{ '/feed.xml' | absolute_url }}">

  <!-- JSON-LD: BlogPosting -->
  {% if page.layout == 'post' %}
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "BlogPosting",
    "headline": "{{ page.title | escape }}",
    "description": "{{ page.description | default: site.description | escape }}",
    "datePublished": "{{ page.date | date_to_xmlschema }}",
    "author": { "@type": "Person", "name": "{{ site.author }}", "url": "{{ site.url }}" },
    "url": "{{ page.url | absolute_url }}"{% if page.image %},
    "image": "{{ page.image | absolute_url }}"{% endif %}
  }
  </script>
  {% endif %}

  <!-- JSON-LD: Person (homepage only) -->
  {% if page.url == '/' %}
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Person",
    "name": "{{ site.author }}",
    "url": "{{ site.url }}",
    "sameAs": ["https://github.com/rezkaaufar", "https://www.linkedin.com/in/aufarleo"],
    "jobTitle": "Machine Learning Engineer"
  }
  </script>
  {% endif %}

  <link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'%3E%3Ctext x='50%25' y='50%25' text-anchor='middle' dominant-baseline='central' font-size='52'%3E🧠%3C/text%3E%3C/svg%3E">
  {% if site.compression.css %}<style>{% include main.css %}</style>{% endif %}

  <!-- Google tag (gtag.js) -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-1K5SB4G23C"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag() { dataLayer.push(arguments); }
    gtag('js', new Date());
    gtag('config', 'G-1K5SB4G23C');
  </script>

  <script>
    window.MathJax = {
      tex: {
        inlineMath: [['$', '$'], ['\\(', '\\)']],
        displayMath: [['$$', '$$'], ['\\[', '\\]']]
      },
      options: { skipHtmlTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code'] }
    };
  </script>
  <script async id="MathJax-script" src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>
</head>
```

---

## 8. Verification

After applying changes:

1. **Canonical + meta tags** — Run `jekyll serve` locally, open any post, view page source, and check `<head>` for all new tags.
2. **Open Graph preview** — Paste a deployed post URL into [opengraph.xyz](https://www.opengraph.xyz) or the [LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/).
3. **Twitter Card** — Use the [Twitter Card Validator](https://cards-dev.twitter.com/validator).
4. **JSON-LD** — Paste a deployed post URL into [Google Rich Results Test](https://search.google.com/test/rich-results).
5. **robots.txt** — Visit `https://rezkaaufar.github.io/robots.txt` and confirm the sitemap URL resolves to a 200.
6. **Sitemap** — Submit `https://rezkaaufar.github.io/sitemap.xml` to [Google Search Console](https://search.google.com/search-console).
