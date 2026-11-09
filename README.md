# helloblog

English | [简体中文](README.zh-CN.md)

![helloblog theme preview: light and dark modes](images/screenshot.png)

A glassmorphism personal blog theme for Hugo — bilingual content, light/dark modes, local search, and a rich set of Markdown writing components.

**Demo:** https://helloblog.pages.dev/

Articles and the configuration guide live on the demo site: start with **Theme Introduction**, then follow **Configuration Guide** to set up your own site.

## Features

- Glassmorphism UI, five color presets, light/dark mode, custom background and avatar
- Bilingual (Chinese / English) with a translation-aware language switcher
- Local search without a backend, plus archive and tag pages
- Article table of contents, reading progress, related posts, previous/next links
- Markdown writing components: tip / warning / danger / info, tabs, steps, code groups, details, timeline, flow, file tree, diff, console, mermaid, Q&A, compare, quote cards
- giscus comments, Busuanzi analytics, music player
- SEO: canonical, hreflang, Open Graph / Twitter cards, JSON-LD, RSS, sitemap, robots.txt
- No Node.js needed — CSS and JS are built by Hugo Pipes

## Requirements

Hugo v0.146.0 or later.

## Install

```bash
git submodule add <repo-url> themes/helloblog
```

```toml
theme = 'helloblog'

[outputs]
  home = ['html', 'rss', 'json']   # json is required for local search
```

See the **Configuration Guide** on the demo site for the rest.

## License

MIT
