---
title: "中子星观测站：这个博客的架构与理念"
slug: "neutron-observatory-architecture"
excerpt: "为什么选择 Astro + Edge BFF + FastAPI 的四层架构，以及内容热发布通道如何实现 1~3 秒生效。"
published_at: "2026-09-22T10:00:00+08:00"
updated_at: "2026-09-22T10:00:00+08:00"
author: "NeutronStar"
category: "架构"
tags: ["架构", "Astro", "FastAPI", "热发布"]
cover:
  url: "https://cdn.jsdelivr.net/gh/neutron-star77/fastimage@main/2026/08/652.webp"
  alt: "中子星观测站封面"
  width: 1920
  height: 1080
reading_time: 8
is_pinned: true
is_draft: false
seo_description: "Neutron Observatory 个人博客的四层架构设计：Astro 前端、Cloudflare Edge BFF、NAS FastAPI 与 PostgreSQL，以及内容热发布通道。"
related_posts: []
content_version: "v1"
---

## 为什么是四层架构

这个博客的核心矛盾很简单：**视觉上想要宇宙的深邃与动态，阅读时却需要绝对的安静与稳定。**

为了同时满足这两点，我把系统拆成了四层：

1. **浏览器与 CDN** — 静态资源缓存、图片加载、Cloudflare 缓存
2. **Astro 前端** — 页面壳、SEO、文章阅读布局、RSS/Sitemap
3. **Cloudflare Edge BFF** — 统一 `/api/v1` 入口、验签、限流、缓存、隐藏 NAS 地址
4. **NAS FastAPI + PostgreSQL** — 实时内容读模型、说说、留言板、发布同步

## 内容热发布：不依赖完整构建

文章发布的目标是 **1~3 秒内生效**。这不可能靠每次都跑一遍 Astro 全量构建来实现。

所以我设计了独立的热发布通道：

```
GitHub push → Edge 验签 → FastAPI 拉取指定提交
→ 解析 Markdown → 写入 PostgreSQL 发布读模型
→ Edge 精准清缓存 → 访客刷新即见新版本
```

文章页本身是 **SSG 静态生成**的，NAS 不可用时已发布文章照常打开。动态数据（说说、留言、书架状态）走 islands 请求 `/api/v1`，失败时有空态和错误态，不阻塞正文。

## React 只出现在该出现的地方

首页和普通文章页**绝不加载 React**。React 19 只用于高级阅读工作台——双栏知识阅读、章节批注、读书进度管理这类独立、复杂、可延后的交互。

轻量交互（移动端菜单、主题切换、阅读进度、书架抽书动画）全部用 Svelte 5 islands。

## 动画的克制

所有动画支持 `prefers-reduced-motion`。首页默认只用 CSS/SVG 星点，不加载大型粒子引擎。书架的抽书动画在 reduced-motion 下直接进入详情页。

> 视觉可以有记忆点，但不能让动画阻塞内容。

这就是 Neutron Observatory 的全部设计哲学。
