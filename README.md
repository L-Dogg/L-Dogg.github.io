# orgiele's basement

Personal blog built with Jekyll 4, deployed to GitHub Pages via GitHub Actions.

**Live site:** [orgiele.xyz](https://orgiele.xyz)

## Local Development

**Requirements:** Ruby 3.3+

```bash
bundle install
bundle exec jekyll serve
```

Site will be available at `http://localhost:4000/`.

## Deployment

Push to `master` triggers the GitHub Actions workflow (`.github/workflows/jekyll.yml`) which builds and deploys to GitHub Pages automatically.

## Structure

```
_posts/       Blog posts (Markdown with YAML front matter)
_layouts/     HTML layouts (default, post, page, home)
_includes/    Reusable HTML fragments (head, header, footer, theme-toggle, post-list)
_sass/        SCSS partials (variables, base, typography, layout, components)
assets/css/   Main SCSS entry point
images/       Avatar, favicons
```

## Adding a New Post

Create a file in `_posts/` named `YYYY-MM-DD-slug.md`:

```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD
categories: [Category]
tags: [tag1, tag2]
reading_time: 3
---

Post content in Markdown.
```

## Theme

Minimalist design with serif typography (Playfair Display headings, Source Serif 4 body, Fira Code for code). Light/dark mode with toggle, CSS custom properties for theming.
