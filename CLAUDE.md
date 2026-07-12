# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 这是什么

个人 Jekyll 博客（GitHub: [futingx/futingx.github.io](https://futingx.github.io)），基于 [Gaohaoyang 的 Jekyll 主题](https://github.com/Gaohaoyang/gaohaoyang.github.io) 二开并扩展。内容为中文，覆盖编程、AI、网络、项目日志等方向。MIT 许可。

## 构建与运行

```bash
# 安装依赖（使用 Ruby China gem 源，Windows 平台需要 wdm）
bundle install

# 启动本地服务，默认地址 http://127.0.0.1:4000/（自带热重载）
bundle exec jekyll s

# 构建，输出到 _site/
bundle exec jekyll build
```

**注意：修改 `_config.yml` 后需要重启服务器**，热重载不会感知配置文件变更。依赖版本：Jekyll `~> 4.3.3`，Gemfile 除默认依赖外安装了 `jekyll-feed`、`jekyll-sitemap`、`jekyll-paginate`。

## 项目结构

内容流转路径：

```
_posts/ 或 _drafts/  →  _layouts/  →  _includes/  →  _sass/ + css/ + js/  →  _site/
  (已发布文章)   (草稿)         (页面壳)       (可复用 UI)     (样式与脚本)            (编译产物)

_config.yml              ← 站点全局配置
index.html              ← 首页（独立模板，不使用 post/page 布局）
page/                   ← 静态页面集合
images/                 ← 文章配图
feed.xml                ← jekyll-feed 自动生成
README.md               ← 原版主题说明（未二开）
```

### 内容 — `_posts/` 和 `_drafts/`

文章文件遵循 `YYYY-MM-DD-title.md` 命名规范，头部需要 YAML front matter：

```yaml
---
layout: post
title: "文章标题"
date: YYYY-MM-DD HH:MM:SS +0800
categories: category-name
tags: tag1 tag2
author: optional-name
mathjax: true              # 单篇文章启用 LaTeX 渲染
---
```

**摘要**（首页展示）通过 4 个连续换行符切割（`excerpt_separator: "\n\n\n\n"`，见 `_config.yml`），分隔符之前的内容在首页展示，之后的内容仅在文章页可见。

草稿存放在 `_drafts/`，不需要日期前缀，默认不发布。预览草稿使用 `bundle exec jekyll s --drafts`。

**目录生成：** 在 Markdown 内容中插入 `* content`（首行）`{:toc}`（末行），kramdown 会自动生成 `<div id="markdown-toc">`，由 `pageContent.js` 移动到右侧目录面板。

### 布局 — `_layouts/`

| 文件 | 用途 |
|---|---|
| `default.html` | 基础底层布局；加载 head、header、footer、backToTop、main.js、smooth-scroll，并初始化平滑滚动参数。其他布局嵌套在此之内。 |
| `post.html` | 文章详情页，含右侧目录导航（pageContent.js）、相关文章（最多 6 篇）、上下篇导航、评论。底部内联脚本将所有文章内链接设为 `target="_blank"`（排除带 id 的锚点）。 |
| `page.html` | 静态页面（归档、分类等），含右侧目录，比 post 布局更轻量。 |
| `demo.html` | 瀑布流展示页（使用 Masonry），布局为 `default` + `post` + `demo` CSS 类组合。 |

首页 `index.html` 使用 `layout: default`，不走 post/page 布局，自行实现了文章列表 + 右侧边栏（近期文章、分类、标签云）。

布局调用链示例：`layout: post` → `post.html` → `default.html`。

### 嵌入组件 — `_includes/`

可复用片段：

- **`head.html`** — `<head>` 部分，含 meta、favicon、Font Awesome 4.7（CDN）、主样式、`main.css`。按条件注入：百度统计、Google Analytics、MathJax（按文章级别）。
- **`header.html`** — 响应式导航栏，移动端汉堡菜单（JS 驱动，断点 770px）。导航项自动遍历 `site.pages` 并筛选 `type: page` 生成，图标通过页面 front matter 的 `icon` 字段匹配 Font Awesome 类名。
- **`footer.html`** — 包含站点描述、社交链接、不蒜子访问量统计、Jekyll / 主题作者信息。
- **`backToTop.html`** — 滚动超过 200px 显示的回到顶部按钮。
- **`previousAndNext.html`** — 上下篇文章链接（Jekyll 内置 `page.previous` / `page.next`）。
- **`category.html`**、**`tag.html`** — 分类和标签展示。
- **`comments.html`** — Disqus / 多说评论嵌入（当前两套均已关闭）。

### 样式 — `css/main.scss` → `_sass/`

`css/main.scss` 是唯一入口，导入以下 partial：

`reset`、`header`、`post`、`page`、`syntax-highlighting`、`index`、`demo`、`footer`、`scrollbar`、`backToTop`

> `_sass/_layout.scss` 和 `_sass/_post-old.scss` 存在于目录中但未被导入（死代码）。

修改样式时编辑对应的 `_sass/_xxx.scss` partial，Jekyll 会在保存时自动重新生成 `css/main.css`。

### 脚本 — `js/`

| 文件 | 使用页面 | 用途 |
|---|---|---|
| `main.js` | 全部页面 | 移动端导航切换（≤770px）、回到顶部按钮显隐控制 |
| `smooth-scroll.min.js` | 全部页面 | 锚点平滑滚动（speed=500ms, easing=easeInOutCubic, offset=20px） |
| `pageContent.js` | `post.html`、`page.html` | 将 kramdown 生成的 `#markdown-toc` 移至右侧 `#content-side`，PC 吸顶 + 移动端锚点弹窗 |
| `waterfall.js` | demo 页 | 基于 Masonry 的瀑布流布局 |
| `masonry.pkgd.min.js` | demo 页 | Masonry 依赖 |
| `imagesLoaded.min.js` | demo 页 | 图片加载完成后触发 Masonry 重排 |

### 静态页面 — `page/`

命名约定：`0archives.html`、`1category.html`、`2tags.html`、`3collections.md`、`3demo.md`、`4about.md`。数字前缀控制导航栏显示顺序（数字越小越靠前）。

每个页面必须在 front matter 中声明 `type: page` 才会出现在导航栏中。

### 配置 — `_config.yml`

影响构建行为的关键配置：
- **`permalink: /:year/:month/:day/:title/`** — URL 结构，含日级维度
- **`paginate: 10`** — 首页每页文章数
- **`excerpt_separator: "\n\n\n\n"`** — 摘要切割点
- **`kramdown: { input: GFM, syntax_highlighter: rouge }`** — GitHub 风格 Markdown + Rouge 高亮
- **`category_dir: category/`**、**`tag_dir: tag/`** — 分类和标签的 URL 前缀
- **评论配置**：`duoshuo_shortname` 和 `disqus_shortname` 均已留空/占位，评论功能当前关闭
- **统计**：`baidu_tongji_id` 和 `google_analytics_id` 均已注释，实际使用不蒜子（busuanzi）统计（见 footer）
- **插件**：`jekyll-paginate`、`jekyll-feed`、`jekyll-sitemap`

## 常见任务

**写新文章：** 在 `_posts/` 下创建 `YYYY-MM-DD-title.md`，写好 front matter，摘要部分之后加 4 个空白行即可在首页触发摘要。文章末尾加 `{:toc}` 可启用右侧目录。

**草稿预览：** `bundle exec jekyll s --drafts`

**添加导航页面：** 在 `page/` 下用数字前缀创建文件，front matter 设置 `layout: page` 和 `type: page`，通过 `icon` 字段选择图标（任意 [Font Awesome 免费图标名](https://fontawesome.com/icons)）。

**修改样式：** 编辑 `_sass/_xxx.scss` partial（各 partial 职责见上方表格）。

**修改站点身份：** 改 `_config.yml` 中的 `title`、`brief-intro`、`description_footer`、社交链接等，并更换根目录的 `favicon.ico`。

## 第三方服务

- **不蒜子 (busuanzi.ibruce.info)** — 底部实时显示站点 PV/UV 和文章阅读量
- **Font Awesome 4.7** — 导航、图标（CDN：cdn.bootcss.com）
- **MathJax 2.7.1** — 按文章级 `mathjax: true` 启用（CDN：cdnjs.cloudflare.com），支持 `$...$` 行内公式和 `\(...\)` 行间公式

## 来源与致谢

主题原设计者：[HyG](https://github.com/gaohaoyang)。[README](https://github.com/Gaohaoyang/gaohaoyang.github.io) 要求保留 footer 中的主题作者信息，本项目已保留。本项目使用 MIT 许可。
