---
title: "E2E 坏 frontmatter 探针"
category: "测试"
tags: ["e2e"]
is_draft: false
---

这是一个故意缺少必填 slug 字段的测试文件，用于验证内容同步的单文件失败隔离与旧版保留能力。正常情况下它不应进入数据库，且不应影响其它文章同步。
