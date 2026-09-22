# Neutron Observatory — 内容仓

这是 Neutron Observatory 个人博客（https://blog2.neutronstar.fun ）的**内容专用仓库**，只存放文章 / 书架 / Wiki 的 Markdown 源文件，不含任何代码与密钥。

推送到 `main` 分支后，GitHub Webhook 会通知 NAS 上的 FastAPI 按本次 commit 拉取内容并热发布到线上读模型，正常 1–3 秒内生效，无需登录服务器、无需重新构建前端。

## 目录结构

```
content/
├── posts/    # 文章 *.md
├── books/    # 书架 *.md
└── wiki/     # Wiki *.md
```

- 文件名为 `<slug>.md`，slug 必须与 frontmatter 中的 `slug` 一致且全网唯一。
- 每篇文件由 YAML frontmatter + Markdown 正文组成；字段规范以主代码仓 `docs/NEUTRONSTAR_DEVELOPMENT_PROCESS.md` 的内容模型为准。
- **新增 / 修改**文件并 push 即创建或更新对应内容。
- **删除**文件并 push 会把对应内容标记为 `unpublished` 下线（数据库记录保留，可恢复）。
- 一次 push 变更 ≤ 50 个内容文件时走 Contents API 增量同步；超过则自动回退为该 commit 的全量 tarball 同步。

## 发布链路

```
push main → GitHub Webhook(HMAC 验签) → Cloudflare Edge → NAS FastAPI
        → 按 commit 拉取 tarball/Contents → 仅解包 content/ → UPSERT PostgreSQL
        → 清理 Edge 缓存 → 公网可见
```

- 仅接受 `refs/heads/main` 的 push 事件，其它分支被忽略。
- 发布失败时保留上一版内容，PublishJob 标记为 failed 可重试。
- 历史版本通过 `content_version` / `publish_commit_id` 留痕，可回滚。

## 约束

- 仅提交 Markdown 内容；不要提交脚本、密钥、`.env`、令牌或大体积二进制。
- 图片在正文中以 URL 引用（图床外链），本仓不存图片二进制。
