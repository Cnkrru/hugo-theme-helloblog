# helloblog

English | [简体中文](README.zh-CN.md)

![helloblog screenshot](https://raw.githubusercontent.com/Cnkrru/hugo-theme-helloblog/main/images/screenshot.png)

A glassmorphism personal blog theme for Hugo, ported from Cnkrru's Vue blog. It ships with bilingual content support, light/dark themes, local search, table of contents, reading progress, a music player, Busuanzi analytics, giscus comments, and a set of Markdown writing components (admonitions, timeline, flow, code groups and more).

## Requirements

- Hugo v0.146.0 or later
- No Node.js needed — CSS and JS are compiled by Hugo Pipes

## Installation

```bash
git submodule add <repo-url> themes/helloblog
```

Site `hugo.toml`:

```toml
theme = 'helloblog'

timeZone = 'Asia/Shanghai'

[outputs]
  home = ['html', 'rss', 'json']

[pagination]
  pagerSize = 6

[services.rss]
  limit = 20

[imaging]
  quality = 85
  resampleFilter = 'Lanczos'

[sitemap]
  changefreq = 'weekly'
  priority = 0.5

# Inline highlight ==text== via Goldmark's mark extension
[markup.goldmark.extensions]
  [markup.goldmark.extensions.extras.mark]
    enable = true

enableRobotsTXT = true
disableKinds = ['term']

[taxonomies]
  tag = 'tags'
```

> `outputs.home` must include `json`, otherwise the search box cannot load `/index.json`.
> `outputs`, `pagination`, `services`, `imaging` and `sitemap` are site-level settings; placing them in the theme's `hugo.toml` has no effect.

## Preview the example site

This repository root is the theme itself. `exampleSite/hugo.toml` mounts the parent directory, so the demo can be served in place:

```bash
cd exampleSite
hugo server
```

Running `hugo server` at the repository root builds an empty site — the root has no content by design.

## Multilingual

```toml
defaultContentLanguage = 'zh'
defaultContentLanguageInSubdir = false

[languages]
  [languages.zh]
    languageCode = 'zh-cn'
    languageName = '简体中文'
    weight = 1
  [languages.en]
    languageCode = 'en'
    languageName = 'English'
    weight = 2
```

Translations are matched by filename: `content/about/index.md` pairs with `content/about/index.en.md`. UI strings live in the theme's `i18n/*.toml`. The language switcher links to the current page's translation and falls back to the target language home page when no translation exists.

Site data (`data/link.json` for the links page, `data/music.json` for the music player) is also language-aware: place a `link-<lang>.json` / `music-<lang>.json` next to the default file and the matching language picks it up automatically, e.g. `data/link-en.json` for the English site.

## SEO

The theme's `head.html` already emits the following, so a site only needs `baseURL` and a description:

- `title`, `description`, `keywords`
- `canonical`; the 404 page is marked `noindex`
- `hreflang` alternates for every translation (`.AllTranslations`)
- per-page RSS `<link rel="alternate">`
- Hugo's built-in `opengraph.html`, `twitter_cards.html` and `schema.html` partials
- `robots.txt` generated via `enableRobotsTXT = true`

The default social share image comes from `params.images`. A bundled `og-cover.png` is used as fallback and can be overridden by placing a file with the same name in your site's `static/` directory.

## Site parameters

```toml
[params]
  description = 'Site description used as SEO fallback'
  keywords = ['Hugo', 'blog']
  images = ['og-cover.png']   # default social share image, placed in static/
  color_theme = 'sakura'      # sakura / purple / ink / cyan / blue
  glass_opacity = 0.5
  siteage = '2026-10-04'
  avatar = 'avator.webp'

  [params.index]
    logo = ''
    welcome = []
    increase_speed = 60
    decrease_speed = 40
    loop_speed = 1000
    particle_enable = true
    particle_distance = 180

  [params.background]
    enable = true
    image = 'background.webp'
    overlay_dark = 0.6
    overlay_light = 0.5

  [params.giscus]
    enable = true
    repo = 'owner/repo'
    repo_id = 'R_xxx'
    category = 'General'
    category_id = 'DIC_xxx'
```

## Navigation

The header and sidebar navigation read `[menus.main]`. `identifier` is used as the i18n key first (`nav.*`), so labels follow the active language; `name` is only a fallback:

```toml
[[menus.main]]
  identifier = 'nav.posts'
  name = 'Posts'
  url = 'posts/'
  weight = 1
```

Use relative `url` values (`posts/`, `about/`, …) so the language prefix is added automatically.

## Content

Posts use page bundles at `content/posts/<slug>/index.md`:

```yaml
---
title: Post title
date: '2026-10-23'
category: Essay
tags: [hugo, markdown]
description: Post summary
keywords: [hugo, markdown]
lastmod: ''
draft: false
---
```

`category` groups posts on the archive page, `tags` feeds the tag page, `description` falls back to the site-level `params.description`, and lists are sorted by `date` descending. `lastmod` sets the displayed update date; leave it empty to hide the update row.

See the sample post "Writing Components and Typography on a Static Blog" for every writing component. Heading anchors are emitted by `layouts/_markup/render-heading.html` and appear when hovering a heading.

## Directory layout

```
.
├── archetypes/      # content templates
├── assets/          # source CSS/JS and vendor libs
├── exampleSite/     # demo site, mounts this repository
├── i18n/            # UI strings (zh/en)
├── images/          # theme gallery screenshot.png / tn.png
├── layouts/         # templates
├── static/          # favicon, OG cover, avatar, background
├── hugo.toml        # theme defaults
├── theme.toml
└── LICENSE
```

## License

MIT
