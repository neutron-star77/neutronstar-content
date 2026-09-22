---
title: "Markdown 写作指南：在中子星发布文章"
slug: "markdown-writing-guide"
excerpt: "文章 frontmatter 字段说明、正文写作规范、图片引用方式，以及代码块和引用的最佳实践。 hotprobe1790101328"
published_at: "2026-09-20T14:30:00+08:00"
updated_at: "2026-09-22T09:00:00+08:00"
author: "NeutronStar"
category: "指南"
tags: ["Markdown", "写作", "指南"]
cover:
  url: "https://cdn.jsdelivr.net/gh/neutron-star77/fastimage@main/2026/08/438.webp"
  alt: "Markdown 写作指南"
  width: 1920
  height: 1080
reading_time: 6
is_pinned: false
is_draft: false
seo_description: "在 Neutron Observatory 发布文章的完整指南：frontmatter 字段、正文规范、图片引用、代码块与引用。"
related_posts: ["neutron-observatory-architecture"]
content_version: "v1"
---

## Frontmatter 字段

每篇文章开头的 YAML frontmatter 是文章的元数据。以下是必填字段：

| 字段 | 说明 |
|---|---|
| `title` | 文章标题 |
| `slug` | URL 路径，唯一 |
| `excerpt` | 摘要，用于列表和 SEO |
| `published_at` | 发布时间（ISO 8601） |
| `author` | 作者名 |
| `tags` | 标签数组 |
| `is_draft` | 是否草稿 |

## 正文规范

### 标题层级

使用 `##` 作为一级章节标题，`###` 作为子节。不要在正文中使用 `#`（它保留给页面标题）。

### 代码块

代码块必须指定语言，以获得语法高亮：

```python
def parse_markdown(content: str) -> dict:
    """解析 Markdown 为发布读模型"""
    metadata, body = content.split("---", 2)[1:]
    return {"metadata": metadata, "body": body.strip()}
```

### 引用

> 这是一段引用。引用可以用来强调关键观点，或者摘录他人的论述。
>
> 引用支持多段落。

### 图片

图片只存 URL 元数据，使用已有 GitHub 图床：

![中子星示意图](https://cdn.jsdelivr.net/gh/neutron-star77/fastimage@main/2026/08/495.webp "中子星结构")

首屏图片必须明确尺寸，正文图片默认懒加载。

## 发布流程

1. 在 `content/posts/` 下创建 Markdown 文件
2. 填写完整 frontmatter
3. 提交并推送到 GitHub
4. Webhook 自动触发热发布，1~3 秒内生效
5. 发布失败时保留上一版，可重试

## 注意事项

- 不要在文章中使用绝对本地路径
- 图片文件名避免中文、空格和特殊符号
- 不覆盖已发布的同名图片，优先使用新文件名
- 文章内容不执行任意 HTML，所有 HTML 会被清理
