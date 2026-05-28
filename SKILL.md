---
name: sakurairo-arcaea-blog-skill
description: 应用 Arcaea/音游玻璃拟态风格 Sakurairo WordPress 博客的组合技能。封装了多轮迭代设计的精确 CSS 数值、配色 Token、层级体系、Sakurairo 主题冲突覆盖方案和 WordPress 发布工作流。一个文件解决全部。
version: 1.7.0
---

# Sakurairo Arcaea Blog Skill

## 目的

在 Sakurairo v3.0.10 主题的 WordPress 博客 babel36acl.xyz 上，应用 Arcaea 风格（玻璃拟态、深色卡片层级、冰蓝配色）的页面设计、美化和发布。一个文件包含完整工作流。

## 何时使用

- 用户要求在 babel36acl.xyz 上"美化页面"、"应用 Arcaea 风格"、"设计 xx 页面"
- 需要创建带玻璃卡片层级的新页面（Games、Music 风格）
- 需要保持跨页面一致的 Arcaea 视觉语言
- 出现 Sakurairo 主题 CSS 冲突（blockquote、FontAwesome 图标等）
- 需要将文章/页面发布到 WordPress

---

## 目录

- [前置技能加载](#前置技能加载必须)
- [CSS 设计体系](#css-设计体系)
- [Sakurairo 主题集成](#sakurairo-主题集成)
- [WordPress 发布工作流](#wordpress-发布工作流)
- [工作流](#工作流)
- [Pitfalls](#pitfalls)

---

## 前置技能加载（必须！）

此技能被触发时，必须先按顺序加载以下技能。**注意**：部分技能位于 `/root/.agents/skills/` 目录而非 Hermes 内置技能库，需通过 read_file 直接读取。

### 核心技能（可从 .agents/skills 加载）

1. **`sakurairo-theme`** — Sakurairo 主题功能完整指南：短代码、颜色系统、AI 摘要等
2. **`glassmorphism-ui`** — 毛玻璃 CSS 模式库
3. **`ui-beautify`** — Arcaea v5 精确 CSS 数值
4. **`ui-designer`** — 整页布局生成
5. **`css-master`** — 设计 Token 体系、Sakurairo 冲突诊断

### Hermes 内置技能

6. **`wordpress`** — WordPress REST API 操作

加载方式：

```
# .agents 技能用 read_file 直接读取
read_file(path="/root/.agents/skills/sakurairo-theme/SKILL.md")
read_file(path="/root/.agents/skills/sakurairo-arcaea-blog-skill/SKILL.md")

# Hermes 内置技能用 skill_view
skill_view(name="wordpress")
```

---

## CSS 设计体系

### 核心审美

Arcaea 的视觉气质是 **"遥远、冰冷、孤独"**——白色空间 + 碎裂玻璃、终末感天空 + 数字废墟、科幻感 UI + 电子音乐、抽象叙事 + 情绪片段。

博客风格 = 数字遗迹考古感。阅读是在废墟中拾取碎片。

其本质是一种 **"未来感 × 情绪化 × 极简秩序 × 崩坏诗意"**（Cyber Minimalism + Emotional Futurism）。

它与普通赛博朋克的区别：Arcaea 不追求"霓虹城市"，而是 **空旷、孤独、神圣感、几何秩序、数据碎片、光污染极少、高透明度、低饱和、情绪压抑与希望并存**。

### 配色体系

```css
/* 主色 — 冰蓝/冷白/淡紫 */
--c-primary: #dbe8ff;
--c-accent: #9db4ff;
--c-skyblue: #8ad8ff;
--c-violet: #c7b6ff;
--c-white: #ffffff;

/* 背景 — 深色太空 */
--bg-deep: #05070b;
--bg-mid: #0b1020;
--bg-surface: #111827;
--bg-dark: #09090f;
```

**禁止**：高纯度红、荧光绿、彩虹渐变、过亮蓝。

### 视觉层级体系

正确结构 = **"很多层轻重不同的小玻璃"**，而非一块巨型玻璃面板。

```
背景图（最底层）
  ↓
bg-overlay（压暗背景，fixed 定位）
  ↓
容器/分类（轻，透明度 ~0.42，仅分区）
  ↓
条目卡片（深，透明度 0.82，真正阅读区）
  ↓
强调块/引用（最实体，渐变底 + 左边框）
```

**核心规则**：每层视觉重量递增，透明度递减。

### 最终校准 CSS 值（v5.4 — Games + Music 统一）

```css
/* ===== 背景遮罩层 ===== */
.games-arcaea-wrap .bg-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.58);
  backdrop-filter: blur(18px) brightness(0.72) saturate(65%);
  -webkit-backdrop-filter: blur(18px) brightness(0.72) saturate(65%);
  z-index: 0;
  pointer-events: none;
}

/* ===== 全局覆盖 ===== */
:root {
  --theme-skin: #0a0e18;
  --theme-skin-dark: #05070d;
  --global-font-weight: 300;
}

/* ===== 动态光晕 ===== */
.bg-glow-1{position:fixed;width:1000px;height:1000px;top:-200px;right:-150px;background:radial-gradient(circle,rgba(120,180,255,0.08),transparent 70%);filter:blur(120px);pointer-events:none;z-index:0;animation:floatGlow 20s ease-in-out infinite}
.bg-glow-2{position:fixed;width:800px;height:800px;bottom:-150px;left:-100px;background:radial-gradient(circle,rgba(167,139,250,0.06),transparent 70%);filter:blur(100px);pointer-events:none;z-index:0;animation:floatGlow 24s ease-in-out infinite reverse}

/* ===== 内容容器 ===== */
.games-arcaea-wrap {
  position: relative;
  z-index: 1;
  max-width: 100%;
  color: #f7fbff;
  overflow-x: hidden;
}

/* ===== HERO（v5.4: 禁用 sticky 防止内容挤占）===== */
.games-arcaea-wrap .game-hero{padding:120px 0 80px;display:flex;align-items:flex-start;justify-content:space-between;min-height:70vh;max-width:1380px;margin:0 auto;padding-left:clamp(24px,6vw,90px);padding-right:clamp(24px,6vw,90px);gap:clamp(24px,5vw,60px)}
.hero-left{flex:1 1 auto;max-width:620px;min-width:0}
.hero-title{font-size:clamp(48px,8vw,100px);font-weight:700;letter-spacing:-0.04em;line-height:1.05;background:linear-gradient(90deg,#9ecfff,#8ba7ff,#a78bfa);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;margin:0}
.hero-right{flex:0 0 auto;min-width:180px;text-align:right;padding-top:60px}
.floating-tag{display:block;font-size:clamp(13px,1.4vw,20px);font-weight:300;color:rgba(255,255,255,0.18);letter-spacing:0.08em;line-height:2.3;transition:color 0.4s ease;cursor:default}

/* ===== 分类容器（轻）===== */
.game-category {
  background: rgba(10, 14, 24, 0.42);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 18px;
  padding: 16px 20px;
  margin: 32px 0;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  box-shadow: none;
}

/* ===== 条目卡片（重）===== */
.game-entry {
  background: rgba(8, 12, 20, 0.82);
  border-radius: 18px;
  padding: 20px 24px;
  margin: 16px 0;
  border: 1px solid rgba(160, 220, 255, 0.16);
  box-shadow:
    0 8px 32px rgba(0,0,0,0.32),
    inset 0 0 0 1px rgba(255,255,255,0.06);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}
.game-entry:hover {
  border-color: rgba(160,220,255,0.30);
  box-shadow: 0 6px 28px rgba(0,0,0,0.40);
}

/* ===== 分类标题 ===== */
.game-category > h2 {
  color: #f3f0ff;
  font-size: 1.75em;
  font-weight: 700;
  letter-spacing: 0.04em;
  margin: 0 0 8px 0;
  text-shadow:
    0 0 10px rgba(180,150,255,0.55),
    0 2px 4px rgba(0,0,0,0.75);
}

/* ===== 卡片标题 ===== */
.game-entry h3 {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #e2f4ff;
  font-size: 1.35em;
  font-weight: 700;
  margin: 0 0 6px 0;
  text-shadow:
    0 0 12px rgba(126,200,255,0.45),
    0 2px 4px rgba(0,0,0,0.75);
}
.game-entry h3::before {
  content: "#";
  color: rgba(255,145,145,0.75);
  font-size: 0.95em;
  font-weight: 700;
  flex-shrink: 0;
}
```

### Section 通用布局

```
.section-kicker — 细微分类标签 (uppercase, violet-tinted)
.section-title  — H2 标题 (text-shadow 紫辉)
.section-desc   — 段落描述 (淡蓝灰)
```

### Blockquote 设计（Sakurairo 覆盖 — 必须 !important）

```css
.games-arcaea-wrap blockquote {
  background: linear-gradient(135deg, rgba(40,70,120,0.22), rgba(20,30,50,0.14)) !important;
  border-left: 3px solid rgba(126,200,255,0.55) !important;
  border-radius: 12px !important;
  padding: 14px 20px !important;
  margin: 14px 0 !important;
  color: #f8fbff !important;
  font-size: 1.06em !important;
  line-height: 1.85 !important;
  box-shadow: none !important;
  border-right: none !important;
  border-top: none !important;
  border-bottom: none !important;
}
.games-arcaea-wrap blockquote::before,
.games-arcaea-wrap blockquote::after {
  display: none !important;
  content: none !important;
}
.games-arcaea-wrap blockquote p {
  margin: 0 !important;
  padding: 0 !important;
  padding-left: 0 !important;
  border: none !important;
  background: none !important;
}
```

### 标题 `#` 标记

绝对禁止在 HTML 中写独立 `#` 文本节点。正确做法：

```css
.game-entry h3 { display: flex; align-items: center; gap: 10px; }
.game-entry h3::before { content: "#"; color: rgba(255,145,145,0.75); font-size: 0.95em; font-weight: 700; flex-shrink: 0; }
```

### Music 页面专属组件

详见 [`references/mood-spectrum-css.md`](references/mood-spectrum-css.md) 和下方 CSS。

```css
/* Artist Grid */
.artist-grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:22px;margin:24px 0}
.artist-card{...background:rgba(8,12,20,0.82);border:1px solid rgba(160,220,255,0.16);...}
.artist-card h3::before{content:"#";color:rgba(255,145,145,0.75)}

/* Album Wall */
.album-wall{display:grid;grid-template-columns:repeat(auto-fill,minmax(132px,1fr));gap:15px}

/* Resonance */
.resonance-line{padding-left:24px;border-left:none}
.resonance-line::before{content:">";position:absolute;left:0;color:rgba(157,180,255,0.62)}
.resonance-quote{margin:26px 0;padding:16px 0 16px 24px;border-left:3px solid rgba(126,200,255,0.48);border-radius:0 12px 12px 0}
```

### Mood Spectrum（情绪标签）

关键见 [`references/mood-spectrum-css.md`](references/mood-spectrum-css.md)。核心要点：
- `flex-shrink: 0` 防止标签被压缩
- `display: inline-flex` 使内容垂直居中
- `gap: 10px 12px`（行距10px + 列距12px）
- **修补前检查是否已有前缀**，避免 `.games-arcaea-wrap .games-arcaea-wrap` 双重污染

### Arcaea Lite（Hub 页面 / 博客文章轻量包裹）

见 [`references/arcaea-lite-wrapper.md`](references/arcaea-lite-wrapper.md)。用于关于、工具箱、嵌入式专题、游记、**博客文章**等非内容密集型页面。带 `bg-glow` 光晕氛围但**无 bg-overlay 模糊层**（避免覆盖正文可读性）。

---

## Sakurairo 主题集成

### CSS 变量覆盖

```css
:root {
  --theme-skin: #0a0e18;              /* 覆盖主题主色为深色底 */
  --theme-skin-dark: #05070d;          /* 暗色模式更深 */
  --global-font-weight: 300;           /* 细体，符合 Arcaea 空灵感 */
}
```

### 自定义 CSS 注入路径

Sakurairo 支持两处自定义 CSS 注入：
- **主题选项 → 全局设置 → 自定义样式** — 注入 `<head>`，持久稳定（放 Prism/字体等全局样式）
- **页面/文章中的 `<style>` 块** — 作用域仅限于该页面（放 Arcaea 玻璃卡片等页面独有样式）

### 已知冲突（Sakurairo v3.0.10 默认）

```css
/* 主题默认（必须用 !important 覆盖） */
blockquote { padding: 20px 30px !important; }
blockquote:before { content: "\f10d" !important; }  /* FontAwesome 图标 */
blockquote:after  { content: "\f10e" !important; }
blockquote p { padding-left: 10px; }
```

### 主题 REST API 端点

| 端点 | 用途 |
|------|------|
| `GET /wp-json/sakura/v1/chatgpt?post_id={ID}` | 生成 AI 摘要 |
| `GET /wp-json/sakura/v1/archive_info` | 归档统计信息 |
| `GET /wp-json/sakura/v1/qqinfo` | QQ 用户信息 |

---

## WordPress 发布工作流

### 基础设施

- **站点**: https://babel36acl.xyz
- **主题**: Sakurairo v3.0.10
- **WP REST API**: `https://babel36acl.xyz/wp-json/wp/v2/pages/{id}` — 使用 Application Password 认证
- **MCP Adapter**: 已配置为 Hermes 原生 MCP 服务器（`hermes mcp list` 可见）。`/reload-mcp` 刷新。当前仅有 WPForms 能力。
- **凭证**: 通过 Python `urllib` + Application Password 使用，**绝不**写入文件或 `curl -u`。

### 页面更新流程

1. `GET /wp-json/wp/v2/pages/{id}?context=edit` 获取 `content.raw`（原始 HTML，未被 wpautop 污染）
2. 在 Python 中修改 `raw` 内容（精确字符串匹配）
3. `POST /wp-json/wp/v2/pages/{id}` 传回 `{"content": modified_raw}`
4. 用 `curl -sL URL | grep` 验证关键 CSS 选择器存在

### 已知页面 ID

| ID | 名称 | 风格 | 备注 |
|----|------|------|------|
| 375 | 🎮 游戏 | Arcaea v5.4 完整 | game-entry 卡片系统 |
| 430 | 🌌 音乐 | Arcaea v5.4 完整 | 已统一 wrapper 类名 |
| 190 | 关于 | Arcaea Lite | `.arcaea-wrap` 轻量包裹 |
| 188 | 工具箱 | Arcaea Lite | 同上 |
| 182 | 嵌入式专题 | Arcaea Lite | 同上 |
| 373 | 游记 | Arcaea Lite | 同上 |
| 186 | 架构与重构 | 需主题 CSS | **博客列表页**，模板覆盖内容 |
| 184 | 工程复盘 | 需主题 CSS | **博客列表页**，模板覆盖内容 |

### 已知分类

| ID | 名称 | slug |
|----|------|------|
| 1  | 未分类 | uncategorized |
| 6  | 学习笔记 | %e5%ad%a6%e4%b9%a0%e7%ac%94%e8%ae%b0 |
| 13 | 嵌入式实战 | embedded-practice |
| 14 | 工程复盘 | engineering-retrospective |
| 15 | 架构与重构 | architecture-and-refactoring |
| 16 | 方法与工具 | methods-and-tools |

### 已知标签

| ID | 名称 | slug |
|----|------|------|
| 11 | ARCH | arch |
| 8  | HPM | hpm |
| 9  | RISC-V | risc-v |
| 10 | WSL2 | wsl2 |

### 规则：Python REST API 优先，绝对禁止 curl 带密码

**永远不要**在命令行使用 `curl -u "user:pass"` 形式访问 WordPress REST API。凭证会被 shell history 和进程列表捕获。

正确做法：

1. **Python + urllib + Application Password**（当前唯一可行路径）— 凭证不落磁盘，不暴露于 shell history。使用 `execute_code` 运行。完整脚本模式见 [`references/publishing-python-pattern.md`](references/publishing-python-pattern.md)。
2. **手动粘贴** — 将生成的 HTML 提供给用户，通过 WordPress 后台 Text 模式粘贴（安全且可靠的回退方案）。
3. **MCP 工具** — 当前仅有 WPForms 能力，无法直接发布/更新文章和页面。

MCP Adapter 配置查看：`hermes mcp list`，重载：`/reload-mcp`。

### 发布 Posts（新文章）流程

1. 列出已有文章避免重复：`GET /wp-json/wp/v2/posts?per_page=100&_fields=id,title,slug`
2. 列出分类和标签，确认分类 ID：`GET /wp-json/wp/v2/categories`、`GET /wp-json/wp/v2/tags`
3. 构建文章的 HTML 内容（含 `<style>` Arcaea Lite 包裹层）
4. `POST /wp-json/wp/v2/posts` 传参 `{title, slug, content, categories[], tags[], status="publish", excerpt}`
5. 额外加 `excerpt` 字段用于文章摘要

### 发布 Pages（更新已有页面）流程

1. `GET /wp-json/wp/v2/pages/{id}?context=edit` 获取 `content.raw`（未被 wpautop 污染的原始 HTML）
2. 在 Python 中修改 `raw` 内容（精确字符串匹配或替换）
3. `POST /wp-json/wp/v2/pages/{id}` 传回 `{"content": modified_raw}`
4. 用 `curl -sL URL | grep` 验证关键 CSS 选择器存在

### 发布后的验证

发布后必须验证：
1. REST API 返回 `status: publish`
2. 页面 HTTP 200 可访问
3. CSS style 块未被 wpautop 破坏（检查页面源码中 `<style>` 标签完整性，用 Python `urllib` 或 `curl` 实现，均**不携带密码**）
4. `<pre>` 代码块在渲染后的页面中可见（通过 grep 确认）
5. `arcaea-wrap` 类名存在于渲染后的 HTML 中（确认 Arcaea 样式正确加载）

---

## 工作流

### 0. 内容审查（写新内容前必须先做）

**永远不要跳过这一步。** 列出已有文章以发现空白，避免重复。

```python
# 列出所有 posts 和 pages
req = urllib.request.Request(f"{site}/wp-json/wp/v2/posts?per_page=100&_fields=id,title,slug", headers=headers)
req2 = urllib.request.Request(f"{site}/wp-json/wp/v2/pages?per_page=50&_fields=id,title,slug", headers=headers)
```

检查输出，确认没有已存在相似内容的文章/页面后再动笔。

### 1. 写新博客文章

a. 完成步骤 0 的空缺分析
b. 加载前置技能和 Arcaea Lite 包裹层 CSS
c. 撰写文章 HTML：`<style>Arcaea Lite CSS</style>` + `<div class="arcaea-wrap"><div class="bg-glow-1">...</div><div class="bg-glow-2">...</div>`
d. 使用 Python + urllib + Application Password 发布到 `POST /wp-json/wp/v2/posts`
   - 必须传参: `title`, `slug`, `content`, `categories[]`, `status="publish"`
   - 可选: `excerpt`, `tags[]`
   - slug 用英文 kebab-case
   - excerpt 写 50-120 字的中文摘要
e. 验证: HTTP 200 + 文章可访问 + `<style>` 未被 wpautop 破坏 + `<pre>` 代码块可见 + `arcaea-wrap` 类名存在

### 2. 设计新页面

a. 加载前置技能
b. 使用三层背景结构 + Hero + 卡片网格模式作为起始骨架
c. 使用 v5.4 精确 CSS 数值应用 Arcaea 层级
d. 添加 Sakurairo 主题 `!important` 覆盖
e. 通过 Python REST API 或手动粘贴发布

### 3. 美化已有页面 → 应用 Arcaea 风格

a. 分析当前页面 CSS 结构
b. 统一 wrapper 类名为 `.games-arcaea-wrap`
c. 统一 overlay 类名为 `.bg-overlay`
d. 按优先级修复：
   - **P0**: 统一类名体系（`.page-music` → `.games-arcaea-wrap`）
   - **P1**: `#` 独立文本 → `h3::before`
   - **P2**: 删除右下引号（blockquote ::before/::after → display:none）
   - **P3**: 卡片背景压暗 + 标题 text-shadow
   - **P4**: 正文加亮
   - **P5**: hero-right 禁用 sticky（防止内容挤占）

### 4. 修复 CSS 冲突

a. 对冲突选择器用 `!important` 覆盖
b. `blockquote::before` 和 `blockquote::after` 需同时设置 `display: none !important` 和 `content: none !important`
c. wpautop 会在 CSS 中插入 `<p>` 标签——使用紧凑单行 CSS 避免空行

---

## Pitfalls

### 1. wpautop 污染 `<style>` 标签
WordPress 的 wpautop 会在 `<style>` 块内的每个换行处插入 `<p>` 标签。**解决方案**：CSS 规则写成紧凑单行，规则之间不留空行。虽然浏览器仍能解析，但会臃肿且丑陋。

### 2. hero-right sticky 造成内容挤占
旧版使用 `position: sticky; top: 25vh` 让右侧标签浮动跟随滚动，导致滚动时漂浮标签压入下方内容区。**v5.4 修复**：移除此属性，改回普通 flex 布局。

### 3. Music 页面类名不一致
历史版本中 Music 页面使用 `.page-music` / `.bg-overlay-music` / `.music-hero` 等独立类名，与 Games 页面的 `.games-arcaea-wrap` / `.bg-overlay` / `.game-hero` 完全不同。**v5.4 统一**：所有页面使用相同的 `.games-arcaea-wrap` 体系。

### 4. Python 直接 REST API 发布路径（MCP 不可用时的正确做法）
MCP Adapter 当前仅有 WPForms 能力，无法发帖。wp_mcp_server.py 不存在。使用 Python `urllib` + Application Password 直接调 REST API 发布/更新文章和页面，详见 [`references/publishing-python-pattern.md`](references/publishing-python-pattern.md)。**不要在发布路径上死磕 MCP。**

### 5. Sakurairo 暗黑模式覆盖
Sakurairo 暗黑模式通过 `[theme-mode="dark"]` 选择器覆盖颜色，Arcaea 深色方案可能被覆盖。需在页面 CSS 中添加 `!important` 防御。

### 6. backdrop-filter 性能
不超过 3 层 blur，否则滚动卡顿。本设计仅使用 2 层（bg-overlay + game-category），安全。

### 7. alpha < 0.04 的边框在深色背景上不可见
在 `#05070d` 基底上最小可用 alpha 为 0.06。

### 8. CSS 注释污染 meta
CSS 注释中的文字可能被 WordPress 提取到 `<meta description>`。保持注释简洁。

### 9. 外部技能加载路径
本技能引用的 `sakurairo-theme`、`glassmorphism-ui` 等技能位于 `/root/.agents/skills/`，**不在 Hermes 内置技能库中**。使用 `read_file` 直接读取，而非 `skill_view`。

### 10. 双重前缀陷阱（2026-05-28 发现）
对已有 `.games-arcaea-wrap .mood-*` 选择器再次追加前缀，会变成 `.games-arcaea-wrap .games-arcaea-wrap .mood-*`，匹配不到任何元素（页面只有一层 wrap），CSS 完全失效。**修补已有选择器前，先用 regex 检查是否已含目标前缀。**

### 11. 博客列表页的模板覆盖
Sakurairo 中某些页面（架构与重构 ID=186、工程复盘 ID=184）使用 post-loop 模板，**页面内容被完全忽略**，直接显示文章列表。这类页面的 Arcaea 样式无法通过 page content 注入，必须通过**主题选项 → 自定义样式**全局注入 CSS 覆盖。

### 12. `context=edit` 获取原始内容
更新页面时使用 `?context=edit` 获取 `content.raw`（wpautop 之前的原始内容），而非默认的 `content.rendered`（已被 wpautop 污染）。`raw` 中的 HTML 结构精确可匹配，`rendered` 中被插入了不可预测的 `<p>` 标签。

### 14. `<pre>` 标签在 WordPress REST API 中可正常保存

经过验证（2026-05-28），WordPress REST API 完整保留 `<pre>` 标签，不会被 kses 过滤或 wpautop 破坏。**不要用 Markdown 代码 fence 代替 `<pre>`** — WordPress 不做 Markdown 到 HTML 的转换。博文中的代码块必须始终使用 `<pre>` 标签，验证方法：发布后 fetch 页面源码并 grep `<pre>`。

### 15. `excerpt` 摘要应该始终填写

发布博文时始终提供 `excerpt` 字段（50-120 字），不要留空。摘要用于分类页和社交分享预览，留空时 WordPress 自动截取正文前几行，效果不可控。
使用 Python `urllib` + Application Password 直接调用 WordPress REST API 发布Posts和更新Pages。**不是回退方案，是标准方法。** 适用于 MCP 仅有 WPForms 能力、wp_mcp_server.py 不存在的环境。
```python
token = base64.b64encode(b"user:app-password").decode()
headers = {"Authorization": f"Basic {token}", "Content-Type": "application/json"}
data = json.dumps({"title":"T","content":html,"categories":[13],"status":"publish"}).encode()
req = urllib.request.Request("https://site/wp-json/wp/v2/posts", data=data, headers=headers, method="POST")
```
详见 [`references/publishing-python-pattern.md`](references/publishing-python-pattern.md)。**绝对禁止** `curl -u` 传密码（会泄露到 shell history）。

## 2026-05 月度变更摘要

### 博客发布
- 本月从 Packaging_machine_V1.0 项目提炼并发布 21 篇嵌入式工程博文 (523-547)
- 覆盖主题：三层架构、构建隔离、回调注册表、统一动作表、脉冲溢出保护、CommTask、抛物线起步、motion_seen、HMI双串口、DGUS→HMIS 协议演进、EEPROM参数持久化、看门狗双层策略、任务通知链、双工具链、BSP驱动分层、状态机引擎内部、S曲线启停、同定时器多轴联动、rcfg参数框架

### CSS 变更 (v5.5 → v5.6)
- Blockquote: padding 14px 20px → 16px 28px 16px 36px（防止引号与文字重叠）
- Blockquote: 添加 `text-indent:0!important`
- 段落: 添加 `text-indent:2em`（中文首行缩进）
- Blockquote ::before: `display:none` → `content:"(^_^)"` 颜文字装饰
- Blockquote p: 添加 `text-indent:0!important`
- 全部 21 篇已发布文章通过 WordPress REST API 批量更新

---

## 已知博客内容目录

| 类型 | 页面/文章 | ID | 风格版本 |
|------|----------|----|---------|
| 游戏展示 | Games 页面（🎮 游戏） | 375 | Arcaea v5.4 |
| 音乐展示 | Music 页面（🌌 音乐） | 430 | Arcaea v5.4（已统一） |
| 博客 | AI Agent + AGENTS.md 知识库 | 407 | 默认 + Arcaea Lite |
| 博客 | CMake Presets + ARM Toolchain | 406 | 默认 + Arcaea Lite |
| 博客 | VSCode + Cortex-Debug + OpenOCD | 405 | 默认 + Arcaea Lite |
| 博客 | WSL2 + Arch Linux 嵌入式环境 | 404 | 默认 + Arcaea Lite |
| 博客 | HPM + CMake 开发教程 | 396 | 默认 |
| 博客 | 一台 STM32 设备的 83 次修复 | 176 | 默认 |
| 博客 | 状态机退出不清理标志 | 175 | 默认 |
| 博客 | 加一层包装却不删旧入口 | 174 | 默认 |
| 博客 | 两个 Bug 互相抵消 | 173 | 默认 |
| 博客 | CubeMX GPIO 分组陷阱 | 172 | 默认 |
| 博客 | FreeRTOS configASSERT 边界 | 171 | 默认 |
| 博客 | UART DMA 静默死亡 | 170 | 默认 |
| 博客 | 三层重构靠构建系统 | 523 | Arcaea Lite ✅ |
| 博客 | 不要用超时保护开环步进 | 524 | Arcaea Lite ✅ |
| 博客 | 嵌入式10个隐藏问题 | 532 | Arcaea Lite ✅ |
| 博客 | 工业控制固件完整复盘 | 526 | Arcaea Lite ✅ 含表格 |
| 博客 | 回调注册表状态机模式 | 528 | Arcaea Lite ✅ |
| 博客 | 统一动作表执行器 | 529 | Arcaea Lite ✅ |
| 博客 | 抛物线起步+motion_seen | 530 | Arcaea Lite ✅ |
| 博客 | HMI 双串口隔离架构（已更新 HMIS） | 531 | Arcaea Lite ✅ |
| 博客 | 嵌入式10个隐藏问题 | 532 | Arcaea Lite ✅ |
| 博客 | EEPROM 参数持久化迁移 | 533 | Arcaea Lite ✅ |
| 博客 | DGUS→HMIS 协议演进 | 534 | Arcaea Lite ✅（已重写）|
| 博客 | 看门狗双层策略 | 535 | Arcaea Lite ✅ |
| 博客 | FreeRTOS 任务通知链 | 536 | Arcaea Lite ✅ |
| 博客 | 双工具链构建系统 | 537 | Arcaea Lite ✅ |
| 博客 | 软件 I2C + EEPROM BSP 驱动 | 538 | Arcaea Lite ✅ |
| 博客 | 状态机引擎内部协同 | 539 | Arcaea Lite ✅ |
| 博客 | 步进 S 曲线 done 置位时机 | 540 | Arcaea Lite ✅ |
| 博客 | BSP 实现总览 10 驱动模式 | 541 | Arcaea Lite ✅ |
| 博客 | HMIS 会话协议取代 DGUS | 542 | Arcaea Lite ✅ |
| 博客 | rcfg 参数框架内部 | 543 | Arcaea Lite ✅ |
| 博客 | 同定时器多轴联动 | 547 | Arcaea Lite ✅ |
| 博客 | FreeRTOS 任务通知链 | 536 | Arcaea Lite ✅ |
| 博客 | 双工具链构建 | 537 | Arcaea Lite ✅ |
| 博客 | 软件 I2C + EEPROM BSP | 538 | Arcaea Lite ✅ |
| 博客 | 状态机引擎内部 | 539 | Arcaea Lite ✅ |
| 博客 | 步进 S 曲线 done 置位 | 540 | Arcaea Lite ✅ |
| 博客 | BSP 实现总览 | 541 | Arcaea Lite ✅ |
| 博客 | HMIS 协议取代 DGUS | 542 | Arcaea Lite ✅ V1.1 |
| 博客 | rcfg 参数框架内部 | 543 | Arcaea Lite ✅ |

### 页面状态扫描（2026-05-28, v5.4 修复后）

| 元素 | Games (375) | Music (430) |
|------|------------|-------------|
| Wrapper 类名 | `.games-arcaea-wrap` | `.games-arcaea-wrap`（已统一） |
| Overlay 类名 | `.bg-overlay` | `.bg-overlay`（已统一） |
| :root 覆盖 | ✅ | ✅（新增） |
| bg-glow ×2 | ✅ | ✅（统一） |
| hero-right sticky | ❌ 已移除（v5.4 修复） | ❌ 已移除 |
| blockquote !important | ✅ | ✅（新增） |
| Mood Spectrum | ✅ | ✅（新增） |
| Artist Grid | N/A | ✅ |
| Resonance | N/A | ✅ |
| Prism 玻璃代码块 | ✅（全局） | ✅（全局） |
| LightGallery | ✅（全局） | ✅（全局） |
