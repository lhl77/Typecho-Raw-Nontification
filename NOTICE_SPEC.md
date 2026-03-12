# AdminBeautify 插件通知文件规范

插件会从 `https://raw.githubusercontent.com/lhl77/Typecho-Raw-Nontification/main/AdminBeautify/notice.md` 获取公告内容。

## 文件格式

```markdown
---
id: 2026-01-01-001
title: 📢 公告标题
---

公告正文内容，支持基础 Markdown 格式：

- **加粗**、*斜体*、`行内代码`
- 无序列表项
- [超链接文字](https://example.com)
- ### 小标题

多段落内容...
```

## 字段说明

| 字段 | 必填 | 说明 |
|------|------|------|
| `id` | ✅ | 唯一标识符，建议格式 `YYYY-MM-DD-序号`。**修改此值会让已关闭的用户重新看到弹窗** |
| `title` | ✅ | 弹窗标题，显示在弹窗顶部，支持 Emoji |

## 弹窗行为

- 每次用户访问插件设置页时触发检查
- 若用户点击 **"知道了"** → 本次隐藏，下次访问仍会显示
- 若用户点击 **"不再显示"** → 写入 `localStorage['ab-notice-{id}']='1'`，永久不再显示（针对该 `id`）
- 若 `id` 变更 → 视为新公告，重新显示
- 若用户在插件设置中关闭了"插件通知" → 完全不显示

## 横幅更新通知

横幅通知（右上角滑入）从 GitHub Releases API 自动获取：

```
https://api.github.com/repos/lhl77/Typecho-Plugin-AdminBeautify/releases/latest
```

- 版本号取自 `tag_name`，内容取自 `body`
- 当 `localStorage['ab-seen-version']` 与当前插件版本不同时显示
- 点击 `×` 关闭后，将当前版本号写入 localStorage，不再重复显示
- 超过 4 行的 changelog 默认折叠，点击"展开更多"可查看完整内容
