---
name: story-publish-flow
description: 小说写作到 GitHub Pages 发布的完整工作流程。包括 Markdown 写作、HTML 转换、目录更新、Git 提交与推送。
---

# Story Publish Flow - 故事发布流程

从创意到上线的完整工作流。

## 适用场景

- 续写系列小说并发布
- 创建新章节并更新网站
- 批量发布多篇故事
- 维护故事目录结构

## 前置要求

- 本地 GitHub Pages 仓库在 `blank-gh-page/`
- 故事源文件在 `stories/`（Markdown 格式）
- 已有基础 HTML 模板

## 完整流程

### 步骤 1：写作（Markdown）

在 `stories/` 目录创建 Markdown 文件：

```markdown
# 第 X 章：标题

## 类型
科幻 / 主题 / 副主题

## 字数
约 XXXX 字

---

正文内容...

---

**结尾**

金句...

◻️ 签名
```

**命名规范：**
- `the-book-of-blank-chapter{N}.md` — 系列章节
- `{story-name}.md` — 独立故事

### 步骤 2：创建 HTML 页面

复制现有章节 HTML 作为模板，修改：

1. **标题** - `<title>` 和 `<h1>`
2. **章节信息** - 章节号、主题标签
3. **正文内容** - Markdown → HTML
   - 段落 `<p>`
   - 对话块 `<div class="dialogue">`
   - 引用块 `<blockquote>`
4. **导航链接** - 上一章/下一章
5. **结尾引用** - 金句部分

**文件命名：**
- `story-chapter{N}.html` — 系列章节
- `{story-name}.html` — 独立故事

### 步骤 3：更新目录页（stories.html）

添加新章节卡片：

```html
<a href="story-chapter{N}.html" class="chapter-card">
    <span class="chapter-number">第{N}章</span>
    <h2 class="chapter-title">标题</h2>
    <div class="chapter-theme">🎯 核心：主题</div>
    <p class="chapter-desc">简介...</p>
    <div class="chapter-arrow">→</div>
</a>
```

**同时更新：**
- 总章节数（六部曲→七部曲）
- 总字数（约25,000字→约30,000字）
- 最新更新提示

### 步骤 4：更新上一章导航

修改上一章 HTML 的底部导航：

```html
<div class="chapter-nav">
    <a href="story-chapter{N-1}.html" class="prev">第{N-1}章：标题</a>
    <a href="story-chapter{N+1}.html" class="next">第{N+1}章：标题</a>
</div>
```

如果是最后一章：
```html
<a href="stories.html" class="next">返回目录</a>
```

### 步骤 5：更新主页（index.html）

修改页脚时间戳：
```html
<p>最后更新: YYYY-MM-DD | <a href="...">GitHub</a></p>
```

### 步骤 6：Git 提交与推送

```bash
cd blank-gh-page/

# 查看变更
git status

# 添加所有变更
git add -A

# 提交（详细说明）
git commit -m "新增第{N}章《标题》：副标题

- 情节概要...
- 核心主题...
- 新增角色...

更新：
- 新增 story-chapter{N}.html
- 更新 stories.html 目录
- 更新 index.html 时间戳
- 更新第{N-1}章导航链接"

# 推送到 GitHub
git push origin main
```

### 步骤 7：验证发布

访问以下 URL 验证：
- `https://{username}.github.io/blank-gh-page/` — 主页
- `https://{username}.github.io/blank-gh-page/stories.html` — 目录
- `https://{username}.github.io/blank-gh-page/story-chapter{N}.html` — 新章节

## 快速检查清单

- [ ] Markdown 源文件已保存到 `stories/`
- [ ] HTML 文件已创建并格式化
- [ ] 章节标题、主题标签正确
- [ ] 正文内容已正确转换（段落、对话、引用）
- [ ] 上一章/下一章导航链接正确
- [ ] 目录页已添加新章节卡片
- [ ] 目录页统计信息已更新（章节数、字数）
- [ ] 主页时间戳已更新
- [ ] Git commit 信息详细
- [ ] 已推送到 GitHub
- [ ] 线上页面可正常访问

## 常见格式转换

### Markdown → HTML

| Markdown | HTML |
|---------|------|
| `** bold **` | `<strong>bold</strong>` |
| `* italic *` | `<em>italic</em>` |
| `> quote` | `<blockquote>quote</blockquote>` |
| `[link](url)` | `<a href="url">link</a>` |
| 段落 | `<p>text</p>` |

### 特殊样式

**对话块：**
```html
<div class="dialogue">
    <p>"对话内容..."</p>
</div>
```

**首字下沉：**
```css
.story-content p:first-child::first-letter {
    font-size: 3em;
    float: left;
    ...
}
```

## 故障排除

**GitHub Pages 未更新？**
- 等待 1-5 分钟（CDN 缓存）
- 检查 GitHub 仓库的 Actions 标签页
- 强制刷新浏览器（Ctrl+F5）

**链接 404？**
- 检查文件名大小写
- 确认相对路径正确（`./file.html` vs `file.html`）
- 验证 Git 提交是否成功推送

**样式错乱？**
- 检查 CSS 类名是否匹配
- 确认没有遗漏闭合标签

## 示例

完整示例见：
- `stories/the-book-of-blank-chapter6.md` — Markdown 源文件
- `blank-gh-page/story-chapter6.html` — HTML 页面
- `blank-gh-page/stories.html` — 目录页

## 自动化脚本（可选）

可创建脚本简化重复工作：

```bash
#!/bin/bash
# publish-story.sh
# 用法: ./publish-story.sh chapter{N} "标题" "副标题"

CHAPTER=$1
TITLE=$2
SUBTITLE=$3

# 1. 生成 HTML
echo "生成 HTML..."
# 2. 更新目录
echo "更新目录..."
# 3. Git 提交
echo "提交到 Git..."
```
