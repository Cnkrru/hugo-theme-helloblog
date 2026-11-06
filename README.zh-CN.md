# helloblog

[English](README.md) | 简体中文

一个玻璃拟态风格的 Hugo 个人博客主题，移植自 Cnkrru 的 Vue 博客。支持中英双语、明暗主题、本地搜索、文章目录、阅读进度、音乐播放器、不蒜子统计、giscus 评论，以及一套 Markdown 扩展组件（提示块、时间线、流程图、代码组等）。

## 环境要求

- Hugo v0.146.0 及以上
- Node.js 不需要，主题的 CSS/JS 由 Hugo Pipes 编译

## 安装

```bash
git submodule add <repo-url> themes/helloblog
```

站点 `hugo.toml`：

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

# 行内高亮 ==重点==：Goldmark 官方 mark 扩展
[markup.goldmark.extensions]
  [markup.goldmark.extensions.extras.mark]
    enable = true

enableRobotsTXT = true
disableKinds = ['term']

[taxonomies]
  tag = 'tags'
```

> `outputs.home` 必须包含 `json`，否则前端搜索无法加载 `/index.json`。
> `outputs`、`pagination`、`services`、`imaging`、`sitemap` 都是站点级配置，写在主题的 `hugo.toml` 里不生效。

## 预览示例站

仓库根就是主题本身，`exampleSite/hugo.toml` 通过 mounts 挂载上一级目录，所以可以直接跑：

```bash
cd exampleSite
hugo server
```

在仓库根执行 `hugo server` 会得到一个空站，因为根目录按设计不存放内容。

## 多语言

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

内容翻译按文件名：`content/about/index.md` 对应 `content/about/index.en.md`。界面文案在主题 `i18n/*.toml`，切换按钮会自动匹配当前页面的译文，没有译文时回退到目标语言首页。

站点数据（友链页的 `data/link.json`、音乐播放器的 `data/music.json`）同样支持多语言：在默认文件旁放一份 `link-<lang>.json` / `music-<lang>.json`，对应语言会自动选用，例如英文站用 `data/link-en.json`。

## SEO

主题的 `head.html` 已输出以下内容，站点只需提供 `baseURL` 和描述信息：

- `title` / `description` / `keywords`
- `canonical`，404 页输出 `noindex`
- 各语言 `hreflang`（取 `.AllTranslations`）
- 每页 RSS `<link rel="alternate">`
- Hugo 内置的 `opengraph.html`、`twitter_cards.html`、`schema.html`
- `robots.txt` 由 `enableRobotsTXT = true` 生成

默认社交分享封面取 `params.images`，未配置时使用主题自带的 `og-cover.png`，可在站点 `static/` 放同名文件覆盖。

## 站点参数

```toml
[params]
  description = '站点描述，用于 SEO 兜底'
  keywords = ['Hugo', '博客']
  images = ['og-cover.png']   # 默认社交分享封面，放在 static 目录
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

## 导航菜单

顶栏与侧栏导航读取站点的 `[menus.main]`。`identifier` 会优先作为 i18n 键（`nav.*`），文案随当前语言切换；`name` 只是回退值：

```toml
[[menus.main]]
  identifier = 'nav.posts'
  name = '文章'
  url = 'posts/'
  weight = 1
```

`url` 用相对路径（`posts/`、`about/` 等），语言前缀会自动补上。

## 内容

文章使用 page bundle，`content/posts/<slug>/index.md`：

```yaml
---
title: 文章标题
date: '2026-10-23'
category: 随笔
tags: [hugo, markdown]
description: 文章摘要
keywords: [hugo, markdown]
lastmod: ''
draft: false
---
```

`category` 用于归档页分组，`tags` 用于标签页，`description` 缺省时取站点 `params.description`；列表按 `date` 倒序排列。`lastmod` 用于展示更新时间，留空则不显示更新行。

Markdown 扩展组件的写法见示例文章《静态博客的写作组件与排版实践》。标题锚链接由 `layouts/_markup/render-heading.html` 生成，悬停标题时显示。

## 目录结构

```
.
├── archetypes/      # 内容模板
├── assets/          # 未编译的 CSS/JS 与第三方库
├── exampleSite/     # 示例站，通过 mounts 挂载本仓库
├── i18n/            # 中英文案
├── images/          # 主题市场截图 screenshot.png / tn.png
├── layouts/         # 模板
├── static/          # favicon、OG 封面、头像、背景图
├── hugo.toml        # 主题默认配置
├── theme.toml
└── LICENSE
```

## 许可

MIT
