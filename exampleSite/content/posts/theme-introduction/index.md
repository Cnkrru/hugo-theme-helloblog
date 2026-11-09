---
title: 主题介绍
date: '2026-11-09'
category: 文档
tags:
  - hugo
  - 主题
description: Helloblog 是一套玻璃拟态个人博客主题：明暗双主题、中英双语、本地搜索、写作组件齐备，安装后即可开始写作。
keywords:
  - hugo
  - 主题
  - 博客
draft: false
---

# 主题介绍

Helloblog 是一套面向个人写作的 Hugo 主题。视觉上是玻璃拟态加柔和渐变，功能上把博客常用的东西都做进了主题：明暗双主题、中英双语、本地搜索、文章目录、阅读进度、代码高亮、写作组件、评论与统计。没有前端构建依赖，一条 `hugo` 命令就能产出全站。

```tip {title="设计取向"}
内容优先：排版、目录、搜索都在构建期完成，读者拿到的第一屏就是完整 HTML，不依赖脚本加载。
```

## 主要特性

| 分类 | 能力 |
| --- | --- |
| 外观 | 玻璃拟态、五种配色、明暗切换、自定义背景与头像 |
| 阅读 | 文章目录、阅读进度、上下篇、相关文章、归档与标签 |
| 内容 | 中英双语、RSS、Open Graph、Sitemap、robots.txt |
| 写作 | 提示块、折叠块、选项卡、代码组、步骤、时间线、文件树、流程图、问答、对比、引用卡 |
| 代码 | Chroma 高亮、终端块、diff、mermaid |
| 交互 | 本地搜索、giscus 评论、分享、返回顶部、音乐播放器 |
| 统计 | 访问量统计、首页活跃热力图 |

## 写作组件

组件全部在编译期直出 HTML，只有需要浏览器能力的部分才带少量脚本。提示块按语义分四级，标题可以自定义：

```warning
tip / info / warning / danger 四级对应不同语义，不写标题时使用主题内置的多语言默认文案。
```

选项卡适合并排展示同一件事的不同侧面：

```tabs
@tab 写作
Markdown 只写内容，排版交给主题。

@tab 配置
主题参数集中在 hugo.toml 的 [params] 下。

@tab 部署
产物是纯静态文件，放到任意静态托管即可。
```

新增一篇文章的流程是固定的：

```steps
建目录 | 在 content/posts 下按 slug 建文件夹
写正文 | 新建 index.md，可配 index.en.md 提供译文
本地预览 | hugo server 实时查看效果
发布构建 | hugo --minify 输出静态文件
```

## 快速开始

```console
$ git submodule add <repo-url> themes/helloblog
$ hugo server
Web Server is available at http://localhost:1313/
```

```info
主题要求 Hugo 0.146.0 及以上，不需要 Node.js；CSS 与 JS 由 Hugo Pipes 编译。
```

完整参数、最小配置与常见问题见[配置指南](../config-guide/)。

## 目录结构

```filetree
- content/
  - posts/ | 文章，按目录组织
    - theme-introduction/
      - index.md | 中文正文
      - index.en.md | 英文译文
    - config-guide/ | 配置文档
  - about/ | 关于页
  - link/ | 友链页
- data/
  - link.json | 友链数据
  - music.json | 播放器歌单
```

## 许可与反馈

主题以 MIT 协议开源，欢迎提交 issue 与 PR。
