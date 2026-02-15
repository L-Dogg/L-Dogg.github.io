# Blog Redesign: Hugo → Jekyll Minimalist Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Replace the Hugo-compiled static blog with a Jekyll source blog featuring minimalistic serif typography, light/dark toggle, preserving all 5 existing posts and 2 static pages.

**Architecture:** Jekyll native GitHub Pages site. CSS custom properties for light/dark theming. Google Fonts (Playfair Display headings, Source Serif 4 body, Fira Code for code). Zero JS frameworks — only a small theme toggle script. Mobile-first responsive CSS with flexbox.

**Tech Stack:** Jekyll (GitHub Pages native), Liquid templates, SCSS, vanilla JS (theme toggle only), Google Fonts

---

## Task 1: Clean Hugo output and scaffold Jekyll structure

**Files:**
- Delete: all Hugo-compiled files (html, css, js, fonts, xml, taxonomy dirs)
- Keep: `CNAME`, `images/avatar-icon.png`, `images/favicon*`
- Create: `_config.yml`, `Gemfile`, `.gitignore`

**Step 1: Remove Hugo compiled output**

```bash
# Remove Hugo output directories and files
rm -rf about authors categories contact css fonts js posts series tags
rm -f 404.html index.html index.xml sitemap.xml .nojekyll
```

**Step 2: Create `_config.yml`**

```yaml
title: "orgiele's basement"
description: "Szymon's personal website"
author:
  name: "Szymon Adach"
  email: ""
  bio: "Software engineer interested in distributed systems, reliability and observability."
url: "https://orgiele.xyz"
baseurl: ""

permalink: /posts/:slug/

markdown: kramdown
kramdown:
  input: GFM
  syntax_highlighter: rouge

sass:
  sass_dir: _sass
  style: compressed

plugins:
  - jekyll-seo-tag

exclude:
  - Gemfile
  - Gemfile.lock
  - docs/
  - README.md

defaults:
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "post"
  - scope:
      path: ""
    values:
      layout: "page"

social:
  github: "L-Dogg"
  twitter: "orgiele"
  linkedin: "orgiele"

collections_dir: ""
```

**Step 3: Create `Gemfile`**

```ruby
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
```

**Step 4: Create `.gitignore`**

```
_site/
.jekyll-cache/
.sass-cache/
Gemfile.lock
.DS_Store
```

**Step 5: Create directory structure**

```bash
mkdir -p _layouts _includes _posts _sass assets/css images
```

**Step 6: Commit**

```bash
git add -A
git commit -m "chore: remove Hugo output, scaffold Jekyll structure"
```

---

## Task 2: Create base layouts and includes

**Files:**
- Create: `_layouts/default.html`
- Create: `_layouts/post.html`
- Create: `_layouts/page.html`
- Create: `_layouts/home.html`
- Create: `_includes/head.html`
- Create: `_includes/header.html`
- Create: `_includes/footer.html`
- Create: `_includes/theme-toggle.html`

**Step 1: Create `_includes/head.html`**

```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{% if page.title %}{{ page.title }} — {{ site.title }}{% else %}{{ site.title }}{% endif %}</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Source+Serif+4:ital,opsz,wght@0,8..60,300;0,8..60,400;0,8..60,600;1,8..60,400&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="{{ '/assets/css/main.css' | relative_url }}">
<link rel="icon" type="image/png" sizes="32x32" href="{{ '/images/favicon-32x32.png' | relative_url }}">
<link rel="icon" type="image/png" sizes="16x16" href="{{ '/images/favicon-16x16.png' | relative_url }}">
<link rel="alternate" type="application/rss+xml" title="{{ site.title }}" href="{{ '/feed.xml' | relative_url }}">
{% seo %}
<script>
  if (localStorage.getItem('theme') === 'dark' || (!localStorage.getItem('theme') && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    document.documentElement.setAttribute('data-theme', 'dark');
  }
</script>
```

**Step 2: Create `_includes/header.html`**

```html
<header class="site-header">
  <div class="container">
    <a class="site-title" href="{{ '/' | relative_url }}">{{ site.title }}</a>
    <nav class="site-nav">
      <a href="{{ '/' | relative_url }}">Home</a>
      <a href="{{ '/posts/' | relative_url }}">Posts</a>
      <a href="{{ '/about/' | relative_url }}">About</a>
      <a href="{{ '/contact/' | relative_url }}">Contact</a>
    </nav>
    {% include theme-toggle.html %}
  </div>
</header>
```

**Step 3: Create `_includes/footer.html`**

```html
<footer class="site-footer">
  <div class="container">
    <div class="footer-content">
      <p>&copy; 2020–{{ site.time | date: '%Y' }} {{ site.author.name }}</p>
      <div class="social-links">
        <a href="https://github.com/{{ site.social.github }}" aria-label="GitHub">GitHub</a>
        <a href="https://twitter.com/{{ site.social.twitter }}" aria-label="Twitter">Twitter</a>
        <a href="https://www.linkedin.com/in/{{ site.social.linkedin }}" aria-label="LinkedIn">LinkedIn</a>
        <a href="{{ '/feed.xml' | relative_url }}" aria-label="RSS">RSS</a>
      </div>
    </div>
  </div>
</footer>
```

**Step 4: Create `_includes/theme-toggle.html`**

```html
<button class="theme-toggle" id="theme-toggle" aria-label="Toggle dark mode" title="Toggle dark mode">
  <svg class="icon-sun" xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="5"/><line x1="12" y1="1" x2="12" y2="3"/><line x1="12" y1="21" x2="12" y2="23"/><line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/><line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/><line x1="1" y1="12" x2="3" y2="12"/><line x1="21" y1="12" x2="23" y2="12"/><line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/><line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/></svg>
  <svg class="icon-moon" xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/></svg>
</button>
```

**Step 5: Create `_layouts/default.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  {% include head.html %}
</head>
<body>
  {% include header.html %}
  <main class="site-main">
    <div class="container">
      {{ content }}
    </div>
  </main>
  {% include footer.html %}
  <script>
    document.getElementById('theme-toggle').addEventListener('click', function() {
      const html = document.documentElement;
      const current = html.getAttribute('data-theme');
      const next = current === 'dark' ? 'light' : 'dark';
      html.setAttribute('data-theme', next);
      localStorage.setItem('theme', next);
    });
  </script>
</body>
</html>
```

**Step 6: Create `_layouts/post.html`**

```html
---
layout: default
---
<article class="post">
  <header class="post-header">
    <h1 class="post-title">{{ page.title }}</h1>
    <div class="post-meta">
      <time datetime="{{ page.date | date_to_xmlschema }}">{{ page.date | date: "%B %-d, %Y" }}</time>
      {% if page.categories.size > 0 %}
        <span class="post-categories">
          {% for cat in page.categories %}
            <span class="category">{{ cat }}</span>
          {% endfor %}
        </span>
      {% endif %}
      {% if page.reading_time %}
        <span class="reading-time">{{ page.reading_time }} min read</span>
      {% endif %}
    </div>
  </header>
  <div class="post-content">
    {{ content }}
  </div>
  {% if page.tags.size > 0 %}
    <footer class="post-tags">
      {% for tag in page.tags %}
        <span class="tag">{{ tag }}</span>
      {% endfor %}
    </footer>
  {% endif %}
</article>
```

**Step 7: Create `_layouts/page.html`**

```html
---
layout: default
---
<article class="page">
  <header class="page-header">
    <h1 class="page-title">{{ page.title }}</h1>
  </header>
  <div class="page-content">
    {{ content }}
  </div>
</article>
```

**Step 8: Create `_layouts/home.html`**

```html
---
layout: default
---
<div class="home">
  <section class="hero">
    <img src="{{ '/images/avatar-icon.png' | relative_url }}" alt="{{ site.author.name }}" class="avatar">
    <h1>{{ site.author.name }}</h1>
    <p class="bio">{{ site.author.bio }}</p>
    <div class="social-links">
      <a href="https://github.com/{{ site.social.github }}">GitHub</a>
      <a href="https://twitter.com/{{ site.social.twitter }}">Twitter</a>
      <a href="https://www.linkedin.com/in/{{ site.social.linkedin }}">LinkedIn</a>
      <a href="{{ '/feed.xml' | relative_url }}">RSS</a>
    </div>
  </section>

  <section class="recent-posts">
    <h2>Recent Posts</h2>
    <ul class="post-list">
      {% for post in site.posts %}
        <li class="post-item">
          <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </li>
      {% endfor %}
    </ul>
  </section>
</div>
```

**Step 9: Commit**

```bash
git add _layouts/ _includes/
git commit -m "feat: add Jekyll layouts and includes"
```

---

## Task 3: Create SCSS stylesheets

**Files:**
- Create: `_sass/_variables.scss`
- Create: `_sass/_base.scss`
- Create: `_sass/_typography.scss`
- Create: `_sass/_layout.scss`
- Create: `_sass/_components.scss`
- Create: `_sass/_dark-mode.scss`
- Create: `assets/css/main.scss`

**Step 1: Create `_sass/_variables.scss`**

CSS custom properties for theming, SCSS variables for layout breakpoints.

**Step 2: Create `_sass/_base.scss`**

Reset/normalize, base element styles, box-sizing.

**Step 3: Create `_sass/_typography.scss`**

Font families, sizes, line heights, heading styles, link styles, code styles.

**Step 4: Create `_sass/_layout.scss`**

Container, header, footer, main, responsive grid, nav styles.

**Step 5: Create `_sass/_components.scss`**

Post list, post cards, tags, categories, avatar, hero section, theme toggle button.

**Step 6: Create `_sass/_dark-mode.scss`**

Dark theme CSS custom property overrides.

**Step 7: Create `assets/css/main.scss`**

Front matter + imports of all partials.

**Step 8: Commit**

```bash
git add _sass/ assets/
git commit -m "feat: add minimalist SCSS with serif typography and dark mode"
```

---

## Task 4: Convert and create all content pages

**Files:**
- Create: `index.html` (homepage)
- Create: `posts.html` (post listing)
- Create: `about.md`
- Create: `contact.md`
- Create: `404.html`
- Create: `feed.xml`
- Keep: `CNAME`

**Step 1: Create `index.html`**

Homepage with hero and recent posts using `home` layout.

**Step 2: Create `posts.html`**

Full post listing page.

**Step 3: Create `about.md`**

```markdown
---
layout: page
title: About
permalink: /about/
---

Software engineer interested in distributed systems, reliability and observability. Writing short posts, mostly to organize my thoughts on programming.
```

**Step 4: Create `contact.md`**

```markdown
---
layout: page
title: Contact
permalink: /contact/
---

DM me on Twitter: [@orgiele](https://twitter.com/orgiele)
```

**Step 5: Create `404.html`**

Custom 404 page with `default` layout.

**Step 6: Create `feed.xml`**

RSS feed using Liquid.

**Step 7: Commit**

```bash
git add index.html posts.html about.md contact.md 404.html feed.xml
git commit -m "feat: add content pages (home, posts, about, contact, 404, RSS)"
```

---

## Task 5: Convert all blog posts to Markdown

**Files:**
- Create: `_posts/2020-03-12-dotnet-new-template.md`
- Create: `_posts/2022-02-21-sdi-book-review.md`
- Create: `_posts/2024-01-08-tidy-first-book-review.md`
- Create: `_posts/2024-01-12-csharp-interfaces.md`
- Create: `_posts/2024-02-20-software-engineers-guidebook-book-review.md`

Each post needs YAML front matter with: layout, title, date, categories, tags, reading_time.

**Step 1-5: Create each post as markdown**

Convert the HTML content extracted from Hugo output to clean Markdown with proper front matter.

**Step 6: Commit**

```bash
git add _posts/
git commit -m "feat: convert all 5 blog posts to Jekyll markdown"
```

---

## Task 6: Verify and finalize

**Step 1: Verify all files exist and structure is correct**
**Step 2: Check all internal links work**
**Step 3: Verify CNAME is preserved**
**Step 4: Final commit if needed**

---

## Review Checkpoints

After implementation, the following reviews are required:

1. **Principal Engineer**: Code architecture review (layouts, SCSS organization, Jekyll config)
2. **Technical Writer**: Documentation review (README, post formatting, meta descriptions)
3. **QA Engineer**: Functional testing (all links, dark mode, responsive, RSS feed)
4. **Product Manager**: Interface/UX review (typography, spacing, navigation, mobile)
