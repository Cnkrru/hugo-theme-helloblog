---
title: 静态博客的写作组件与排版实践
date: '2026-10-23'
lastmod: '2026-11-07'
category: 技术
tags:
  - hugo
  - markdown
  - 静态站点
description: 以本主题为例，说明为什么把 Markdown 组件放在编译期直出、把交互留给运行期，并给出各组件在真实文章里的写法。
keywords:
  - hugo
  - markdown
  - 静态站点
  - 写作组件
draft: false
---

写博客久了会发现，真正影响阅读体验的往往不是内容本身，而是排版：代码块有没有高亮、提示信息会不会被忽略、长文能不能快速跳转。这篇示例文章用来说明本主题的写作组件是怎么设计、怎么用的，也可以当作新文章的模板参考。

## 为什么把渲染放在编译期

静态站点最容易被误解的一点，是「静态等于没有交互」。实际上交互可以保留，只是执行位置不同。

```tip {title="核心原则"}
能在构建时确定的内容，就不要留到浏览器再算。
```

具体到本主题，Markdown 里的提示块、目录树、终端输出、流程图外壳都在构建时直出 HTML；只有 mermaid 绘图、代码复制、图集灯箱这类依赖浏览器能力的部分才留了少量脚本。

```quote
任何可以用编译期解决的问题，都不应该留给运行时。
设计原则 | 项目手册
```

这样做的好处很直接：首屏是完整的 HTML，搜索引擎和 RSS 读到的内容和读者看到的一致，也不会因为脚本加载失败而白屏。代价是改内容需要重新构建，对个人博客来说几乎不算成本。

## 写作流程

新增一篇文章的步骤是固定的：

```steps
建目录 | 在 content/posts 下按 slug 建文件夹
写正文 | 新建 index.md，可配 index.en.md 提供译文
本地预览 | hugo server 实时查看效果
发布构建 | hugo --minify 输出静态文件
```

本地预览只需一条命令：

```terminal
$ cd exampleSite
$ hugo server
Web Server is available at http://localhost:1313/
$ hugo --minify
```

```info
本地预览默认端口是 1313，被占用时加 -p 换一个即可；构建产物不会包含 Draft 状态的文章。
```

内容目录的结构如下，缩进即层级：

```filetree
- content/
  - posts/ | 文章，按目录组织
    - components-test/
      - index.md | 中文正文
      - index.en.md | 英文译文
      - bryce-canyon.jpg | 文章配图
  - about/ | 关于页
  - link/ | 友链页
- data/
  - link.json | 友链数据
  - music.json | 播放器歌单
- exampleSite/ | 示例站点
```

## 代码与版本对比

普通代码块交给 Chroma 在编译期着色，不需要任何前端高亮库：

```js
// 构建期完成高亮，运行时不再加载高亮脚本
const build = (source) => source.replace(/\s+$/, '')

export const slugify = (title) =>
  title
    .toLowerCase()
    .replace(/[^\w\u4e00-\u9fa5]+/g, '-')
    .replace(/^-|-$/g, '')
```

同一段内容需要在多种技术栈之间对照时，用代码组切换：

```code-group
@tab hugo
hugo --minify --gc
@tab bash
hugo server --buildDrafts --navigateToChanged
@tab toml
theme = 'helloblog'
```

改配置类的文章经常需要展示「改了什么」，diff 块比截图更清晰，复制出来也能直接应用：

```diff
diff --git a/hugo.toml b/hugo.toml
index 8d3e5a7..4b1c9f2 100644
--- a/hugo.toml
+++ b/hugo.toml
@@ -8,6 +8,7 @@ defaultContentLanguage = 'zh'
 enableRobotsTXT = true
+disableKinds = ['term']
 
 [outputs]
   home = ['html', 'rss', 'json']
```

```warn
diff 块必须符合 git 的 unified diff 格式：文件头、@@ 块头、行首的 + - 空格都要保留，否则复制出来无法应用。
```

## 结构化表达

过程类的内容适合用流程块，节点之间自动连线，不用手画图：

```flow
写 Markdown | content/posts/*.md
Goldmark 解析 | 调用匹配的 render hook
直出 HTML | 构建期产出静态文件
浏览器交互 | 少量运行期脚本
```

需要更自由的拓扑时换成 mermaid，构建期只输出容器，图形在浏览器里按需渲染：

```mermaid
graph LR
  A[Markdown] --> B[render hook]
  B --> C[静态 HTML]
  C --> D[浏览器交互]
```

带时间属性的内容用时间线，竖线由 CSS 绘制，不依赖图片：

```timeline
2026-09 | 立项 | 确定 Hugo 技术选型
2026-10 | 迁移 | 写作组件并入主题
2026-11 | 验收 | 组件回归与多语言校验
```

选型类的讨论用对比栏比表格更直观：

```compare
@tab 编译期渲染
首屏直出 HTML
SEO 友好
改内容需要重新构建
@tab 运行时渲染
首屏依赖 JS
SEO 需额外处理
改内容无需构建
```

常见问题收在问答块里，默认折叠，不打断阅读节奏：

```qa
写作组件会不会增大包体积 | 不会，它们只产出 HTML，样式集中在 pamin/post.css
为什么不用前端框架渲染 | 博客内容变化频率低，没必要为一点交互引入整个运行时
```

需要展开的长说明可以放进折叠块：

```details
这里是一段可折叠的补充说明
折叠块里的 Markdown 不会被渲染，内容整体转义、保留换行。
适合放配置片段、完整日志或次要推导过程。
```

## 排版细节

行内元素也有一套约定：快捷键写成 {{< kbd "Ctrl" >}} + {{< kbd "C" >}}，版本号用 {{< badge "v1.0" >}}，需要强调时用 ==高亮==，剩下交给标准 Markdown 的**加粗**、_斜体_和 `行内代码`。

列表和引用保持原生行为：

- 无序列表用于并列项
- 有序列表表示有先后关系的步骤
- 引用块只承载引用，不参与组件管线

> 排版的目的不是好看，而是降低读者理解内容的成本。

需要对照时用表格：

| 组件 | 渲染时机 | 依赖 |
| --- | --- | --- |
| 提示块 / 目录树 / 终端 | 编译期 | 无 |
| 流程图外壳 | 编译期 | mermaid.js |
| 代码高亮 | 编译期 | Chroma |
| 搜索 | 运行期 | minisearch |

配图使用页面资源，构建时自动补上宽高，避免布局抖动：

![Bryce Canyon 的岩柱群，用于演示正文图片的灯箱效果](./bryce-canyon.jpg)

```error
图片要和 index.md 放在同一目录，写成相对路径；路径不存在时构建会直接报错，不会静默跳过。
```

外链与站内链接都按 Markdown 语法书写，站外链接会自动追加 `target="_blank"` 与 `rel="noopener noreferrer"`，例如 [Hugo 官方文档](https://gohugo.io/documentation/) 与 [关于本站](/about/)。

## 小结

组件的价值不在数量，而在一致性：同一套排版语言贯穿全文，读者不需要为每个区块重新理解结构。本文覆盖的组件都可以直接复制到新文章里，替换文本即可使用。
