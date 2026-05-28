---
name: sakurairo-arcaea-blog-skill
description: 应用 Arcaea/音游玻璃拟态风格 Sakurairo WordPress 博客的组合技能。封装了多轮迭代设计的精确 CSS 数值、配色 Token、层级体系、Sakurairo 主题冲突覆盖方案和 WordPress MCP 发布工作流。一个文件解决全部。
version: 1.2.0
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

此技能被触发时，必须先按顺序加载以下技能：

### Hermes 本地技能

1. **`glassmorphism-ui`** — 毛玻璃 CSS 模式库（基础玻璃、轻/重玻璃、色彩变体）
2. **`ui-beautify`** — Arcaea v5 精确 CSS 数值、卡片层级系统、blockquote/# 标记设计
3. **`ui-designer`** — 整页布局生成（Hero、卡片网格、氛围背景）
4. **`css-master`** — 设计 Token 体系、Sakurairo 冲突诊断
5. **`wordpress-content-management`** — MCP 发布桥、安全发布流程
6. **`upgrade-tech-tutorial-to-engineering-guide`** — 博客写作与发布工作流（按需加载）

### Skills.sh 生态（已安装 — 功能增强）

7. **`web-design-guidelines`** (Vercel) — Web 界面规范审查
8. **`sakurairo-theme`** — Sakurairo 主题功能完整指南：短代码、颜色系统、AI 摘要等
9. **`wordpress-pro`** (4.9K ⭐) — WordPress 专业开发
10. **`ckm:design`** / **`ui-ux-pro-max`** (21.2K ⭐) — UI/UX 设计综合
11. **`tailwind`** (46.7K ⭐) — Tailwind CSS v4
12. **`animejs`** (634 ⭐) — 动画库
13. **`react`** (2.3K ⭐) — React 组件
14. **`hyva-child-theme`** — 主题开发参考

> **版本同步**：本技能的 Arcaea CSS 值（v5.2）与 `ui-beautify` v2.2+ 同步。修改其一请同步更新另一个。

加载方式：

```
skill_view(name="glassmorphism-ui")
skill_view(name="ui-beautify")
skill_view(name="ui-designer")
skill_view(name="css-master")
skill_view(name="wordpress-content-management")
skill_view(name="web-design-guidelines")        # 补充: Vercel Web界面规范审查
skill_view(name="sakurairo-theme")              # 补充: Sakurairo 主题 API/短代码/颜色系统
skill_view(name="wordpress-pro")
skill_view(name="ckm:design")
skill_view(name="tailwind")
skill_view(name="animejs")
skill_view(name="react")
```

---

## CSS 设计体系

### 核心审美

Arcaea 的视觉气质是 **"遥远、冰冷、孤独"**——白色空间 + 碎裂玻璃、终末感天空 + 数字废墟、科幻感 UI + 电子音乐、抽象叙事 + 情绪片段。

博客风格 = 数字遗迹考古感。阅读是在废墟中拾取碎片。

其本质是一种 **"未来感 × 情绪化 × 极简秩序 × 崩坏诗意"**（Cyber Minimalism + Emotional Futurism）。

它与普通赛博朋克的区别：Arcaea 不追求"霓虹城市"，而是 **空旷、孤独、神圣感、几何秩序、数据碎片、光污染极少、高透明度、低饱和、情绪压抑与希望并存**。

### 总风格标签（可直接用于 AI Prompt）

```text
Futuristic
Glassmorphism
Cyber Minimalism
Emotional UI
Ambient Light
Floating Geometry
Dark Atmospheric
Abstract Space
Music Visualization
Data Fragment
Holographic
Sci-Fi Sacredness
```

### 视觉气质核心原则

#### 1. "空"（Emptiness）

Arcaea 最大特点是 **留白极多**。不同于普通二次元站（元素堆满、高对比、高饱和），Arcaea 是：
- 大面积纯黑 ✅
- 半透明 ✅
- 漂浮元素 ✅
- 低密度布局 ✅
- 极简文本 ✅
- 呼吸感 ✅

#### 2. "宇宙中的 UI"（Space as UI Language）

所有元素都像漂浮在虚空、数据空间、记忆层、轨道、星海中。

#### 3. UI 主导 > 内容主导

- UI 主导
- 空间感主导
- 氛围主导
- 光效主导
- 几何主导
- 情绪主导

### 光效系统

核心原则：**光不是装饰，而是氛围。**

| 光效类型 | 特征 | CSS |
|---------|------|-----|
| 柔光 Soft Glow | 模糊辉光，环境光，雾状光晕 | `box-shadow: 0 0 20px rgba(120,180,255,.15)` |
| 边缘发光 Edge Glow | 元素本体暗，仅边框/描边/hover/Arc轨迹亮 | `border-color`, `box-shadow: inset` |
| 呼吸光 Breathing | 透明度缓慢变化 | `opacity: .7 → 1` |

**禁止**：彩虹渐变、RGB 电竞风、Neon Tokyo 霓虹。

### 材质系统：Glassmorphism

Arcaea 本质 = **半透明未来 UI**。

```css
/* 基础毛玻璃 */
backdrop-filter: blur(20px);
-webkit-backdrop-filter: blur(20px);

/* 半透明卡片 */
background: rgba(255, 255, 255, .05);

/* 极细边框 */
border: 1px solid rgba(255, 255, 255, .08);
```

元素必须有**浮空感**，不能像传统 Bootstrap 卡片那样厚重。

### 空间感布局

推荐：
- 星云/抽象粒子/几何空间大背景
- 悬浮布局（Floating Panel、Blur Layer、Gradient Fog、Overlay）
- 中央聚焦：中间主体，两侧留空，强纵深
- 大量 `padding-top` / `padding-bottom` 纵向空间
- 信息层级少：不堆内容、不堆按钮、不堆文字——"少，但精"

### 音乐可视化元素

Arcaea 风格网页必须"像音乐"：

| 元素 | 实现 |
|------|------|
| Arc 轨迹 | Bezier Curve + Glow Curve + Floating Arc |
| 频谱感 | 波纹、呼吸动画、微弱粒子 |
| 节奏感 | 缓慢、漂浮、呼吸、惯性、失重感 |

### 动效系统

**动效关键词**：Float, Fade, Glow, Blur, Ambient, Breathing, Parallax, Inertia

**禁止**：Bounce, Cartoon, Overshoot, 抖动

**推荐**：
```css
/* 漂浮 */
transform: translateY(-2px);

/* 呼吸 */
opacity: .7 → 1;

/* 平滑过渡 */
transition: all .6s cubic-bezier(.22, .61, .36, 1);
```

### 字体系统

| 场景 | 推荐字体 |
|------|---------|
| 英文主标题 | Orbitron |
| 音游/UI | Rajdhani, Exo 2 |
| 极简/高级 | Inter, Space Grotesk |
| 中文正文 | 思源黑体（Noto Sans SC） |
| 中文科幻 | HarmonyOS Sans |
| 中文偏二次元 | 得意黑 |
| 技术感中文 | LXGW WenKai Mono |

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

### 构图原则

1. **中央聚焦** — 中间主体，两侧留空，强纵深
2. **纵向空间感** — 大量 `padding-top/bottom`，避免页面拥挤
3. **信息层级少** — 不堆内容/按钮/文字，少但精

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

### 最终校准 CSS 值（v5.2 — 与线上 Games 页面一致）

```css
/* ===== 背景遮罩层 ===== */
.bg-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.58);
  backdrop-filter: blur(18px) brightness(0.72) saturate(65%);
  -webkit-backdrop-filter: blur(18px) brightness(0.72) saturate(65%);
  z-index: 0;
  pointer-events: none;
}

/* ===== 内容容器 ===== */
.games-arcaea-wrap {
  position: relative;
  z-index: 1;
  max-width: 100%;
  color: #f7fbff;
}

/* ===== 分类容器（轻） ===== */
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

/* ===== 条目卡片（重） ===== */
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

/* ===== 引用块 ===== */
blockquote {
  background:
    linear-gradient(
      135deg,
      rgba(40,70,120,0.22),
      rgba(20,30,50,0.14)
    );
  border-left: 3px solid rgba(126,200,255,0.55);
  border-radius: 12px;
  box-shadow: none;
}

/* ===== 重型玻璃（Steam 卡片等） ===== */
.steam-section {
  background: rgba(4, 8, 16, 0.88);
  border: 1px solid rgba(190, 225, 255, 0.30);
  border-radius: 18px;
  padding: 28px;
  backdrop-filter: blur(16px) saturate(70%);
  box-shadow:
    0 12px 40px rgba(0,0,0,0.50),
    inset 0 0 0 1px rgba(255,255,255,0.06);
}
```

### 配色 Token

```css
--bg-overlay: rgba(8,10,18,0.48);      /* 全屏遮罩 */
--bg-card: rgba(16,20,30,0.72);         /* 暗玻璃卡片底 */
--bg-entry: rgba(14,18,28,0.60);        /* 子条目底 */
--primary: #b0dcff;                      /* 冰蓝 — 标题色 */
--secondary: #b8a0ff;                    /* 淡紫 — 分类标题 */
--tag-color: rgba(255,130,130,0.50);    /* 淡红 — # 标记 */
--accent-glow: rgba(158,207,255,0.12);
--accent-border: rgba(158,207,255,0.08);
--text-body: #f0f6ff;                    /* 正文 */
--text-muted: #d8e4f8;                   /* 辅助文本 */
--text-subtitle: #a0b8d8;                /* 副标题 */
--text-desc: #c8ddf5;                     /* 分类描述 */
--text-quote: #f5faff;                    /* 引用文本 */
```

**禁止**：高纯度 cyan/purple（`#7dd3fc`, `#c084fc`）— 荧光感、廉价 RGB。

### 标题 `#` 标记

绝对禁止在 HTML 中写独立 `#` 文本节点。正确做法：

```css
.game-entry h3 {
  display: flex;
  align-items: center;
  gap: 10px;
}
.game-entry h3::before {
  content: "#";
  color: rgba(255,120,120,0.65);
  font-size: 0.9em;
  font-weight: 700;
  flex-shrink: 0;
}
```

### Blockquote 设计

- 仅保留左上引号或渐变背景 + 左边框（二选一）
- 删除右下引号（`::after { display: none }`）
- 日系/Arcaea 风 = 单引号或无引号，用左边框区分
- 必须用 `!important` 覆盖 Sakurairo 主题的 FontAwesome 引用图标

```css
/* 方案一: 渐变背景 + 左边框（推荐大量引用） */
.your-wrap blockquote {
  background:
    linear-gradient(135deg, rgba(90,120,255,0.08), rgba(120,180,255,0.03));
  border: none;
  border-left: 3px solid rgba(158,207,255,0.30);
  border-radius: 0 12px 12px 0;
  padding: 14px 20px !important;
  margin: 14px 0 !important;
  color: #f5faff !important;
  box-shadow: none !important;
}
.your-wrap blockquote::before { display: none !important; content: none !important; }
.your-wrap blockquote::after  { display: none !important; content: none !important; }
```

### 代码块 Prism 风格（玻璃卡片）

```css
pre[class*="language-"] {
    font-family: "FiraCode Nerd Font","Fira Code",Consolas,monospace !important;
    font-size: 15px;
    line-height: 1.7;

    background: rgba(15,18,22,.72) !important;

    border: 1px solid rgba(160,220,255,.18);

    border-radius: 16px;

    backdrop-filter: blur(18px);

    box-shadow:
        0 8px 24px rgba(0,0,0,.28),
        0 0 0 1px rgba(255,255,255,.02) inset;

    padding: 1.35rem 1.5rem;

    margin: 2rem 0;

    overflow: auto;
}

/* copy button */
div.code-toolbar > .toolbar button {
    background: rgba(255,255,255,.08) !important;
    border: 1px solid rgba(255,255,255,.08) !important;
    color: #d8f3ff !important;
    border-radius: 8px !important;
    backdrop-filter: blur(10px);
}

/* 去掉 token 灰块 */
.token.operator,
.token.entity,
.token.url {
    background: transparent !important;
}
```

### LightGallery 配置

```json
{
  "plugins": [
    "lgZoom",
    "lgHash"
  ],
  "speed": 500,
  "download": false,
  "counter": false,
  "hideBarsDelay": 2000,
  "backdropDuration": 300,
  "selector": ".entry-content img"
}
```

### 设计原则速查

| 原则 | 含义 |
|------|------|
| 三层分离 | 背景图 → bg-overlay(blur+brightness+saturate) → 内容容器 |
| 轻重分配 | 容器轻(~0.42, box-shadow:none) + 卡片重(~0.82) + 玻璃heavy(~0.88) |
| text-shadow 标题 | 分类标题 `0 0 10px` 紫辉，卡片标题 `0 0 12px` 蓝辉 |
| 容器无阴影 | `box-shadow: none` 是关键 |
| 卡片去独立 blur | 复用父容器 blur，避免 stutter |
| 单引号 | 仅保留左上或无引号，`::after` 禁 |
| # 绑定标题 | `h3::before`，绝对不独立文本节点 |
| ice blue | `rgba(160,220,255)` 主色，不用荧光 cyan |
| !important 覆盖 | blockquote 的 padding/伪元素/子元素 |
| 代码块玻璃化 | `pre[class*="language-"]` 毛玻璃卡片风格，15px，blur(18px) |
| token 去灰底 | `.token.operator/.entity/.url` 设 `background: transparent` |
| LightGallery | `selector: ".entry-content img"`，关 download/counter |
| 柔光非霓虹 | 模糊辉光 `box-shadow: 0 0 20px rgba(120,180,255,.15)`，非霓虹/RGB |
| 浮空感 | 元素必须有漂浮轻质感，非 Bootstrap 厚重卡片 |
| 纵向空间 | 大量 padding-top/bottom，页面不拥挤 |
| 信息层级少 | 不堆内容/按钮/文字，少但精 |

### Arcaea Web Design AI Prompt（可直接使用）

```text
Design a futuristic emotional web UI inspired by Arcaea.

Style:
- cyber minimalism
- atmospheric sci-fi
- floating holographic panels
- glassmorphism
- soft ambient glow
- dark abstract space
- music visualization aesthetics
- emotional and lonely atmosphere
- elegant geometric composition
- subtle gradients
- transparent layered interface
- low saturation
- clean typography
- sacred futuristic feeling

Avoid:
- colorful cyberpunk
- RGB gaming style
- overcrowded layout
- cartoonish UI
- neon overload
- thick borders
- flat corporate design

Use:
- blur layers
- floating cards
- soft white/blue glow
- arc-shaped decorative lines
- abstract particles
- smooth breathing animation
- cinematic spacing
- minimalist navigation
- rhythm-game inspired composition

Mood:
lonely, ethereal, futuristic, emotional, immersive
```

### 推荐配套技术栈

| 技术 | 用途 |
|------|------|
| GSAP | 漂浮动画、时间线控制 |
| Lenis | 丝滑惯性滚动 |
| Framer Motion | React 动画 |
| Three.js | 空间粒子、3D 背景 |
| Vanta.js | 星空/粒子背景 (基于 Three.js) |
| Shaders (GLSL) | Arc 光效、轨迹辉光 |
| Prism.js | 技术感代码高亮（玻璃卡片风格） |
| Mermaid | 数据流程图、状态图 |

---

## Sakurairo 主题集成

### CSS 变量覆盖

Sakurairo 主题的颜色 CSS 变量可通过自定义 CSS 覆盖：

```css
:root {
  --theme-skin: #0a0e18;              /* 覆盖主题主色为深色底 */
  --theme-skin-dark: #05070d;          /* 暗色模式更深 */
  --global-font-weight: 300;           /* 细体，符合 Arcaea 空灵感 */
}
```

### 自定义 CSS 注入路径

Sakurairo 支持两处自定义 CSS 注入：
- **主题选项 → 全局设置 → 自定义样式** — 注入 `<head>`，持久稳定
- **页面/文章中的 `<style>` 块** — 作用域仅限于该页面

Arcaea 风格的全局样式（字体、颜色变量、卡片基础）走**主题选项**。页面独有样式（game-category、game-entry 等）走**页面内 `<style>`**。

### 内容样式选择

Sakurairo 主题选项中有两种内容渲染风格：
- **Sakura 风格**（柔和圆润）— 默认，与 Arcaea 玻璃拟态最搭配
- **GitHub 风格**（简洁清晰）— 更锋利，与 Arcaea 不搭

确保选择 **Sakura 风格**。

### 暗黑模式适配

Sakurairo 暗黑模式通过 `[theme-mode="dark"]` 选择器触发（`css/dark.css`）。Arcaea 风格本身就是深色，在暗模式下可能被 `dark.css` 覆盖：

```css
[theme-mode="dark"] .games-arcaea-wrap .game-entry {
  background: rgba(8, 12, 20, 0.82) !important;
}
```

### 短代码兼容

Sakurairo 的短代码（`[bilibili]`, `[ap layer]`, `[gallery]`）在 Arcaea 风格的页面中可能被玻璃 card 包裹。在卡片添加类 `.entry-content` 以继承主题的文章内容样式。

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
- **MCP 服务器**: `~/.hermes/scripts/wp_mcp_server.py`（确保进程运行中）
- **凭证**: 不在此文件中记录。MCP 脚本从环境变量或自身源码读取

### 可用 MCP 工具

| 工具 | 用途 |
|------|------|
| `mcp_wordpress_create_post` | 创建新文章 |
| `mcp_wordpress_create_page` | 创建新页面 |
| `mcp_wordpress_list_categories` | 获取分类列表 |
| `mcp_wordpress_get_user_info` | 查看用户信息 |
| `mcp_wordpress_update_post` | 更新已有文章 |
| `mcp_wordpress_update_page` | 更新已有页面 |

### 已知页面 ID

- Page 375 = Games 页面（🎮 游戏）
- Page 430 = Music 页面（🌌 音乐）

### 已知分类

| ID | 名称 |
|----|------|
| 1  | 未分类 |
| 6  | 学习笔记 |
| 13 | 嵌入式实战 |

### 规则：MCP 优先，绝对禁止 curl 带密码

**永远不要**在命令行使用 `curl -u "user:pass"` 形式访问 WordPress REST API。凭证会被 shell history 和进程列表捕获。

正确做法按优先级：

1. **MCP 工具** — 最安全，凭证位于 MCP 脚本中
2. **Python 脚本读取凭证** — 从 `~/.hermes/scripts/wp_mcp_server.py` 中提取凭证，用 urllib 请求（不要用 requests 库避免额外依赖）

凭证提取安全模式：

```python
import os, re, json, urllib.request, base64

MCP = os.path.expanduser("~/.hermes/scripts/wp_mcp_server.py")
with open(MCP) as f:
    src = f.read()

s = re.search(r'SITE = os\\.environ\\.get\\(\\"([^"]+)\\"', src).group(1)
u = re.search(r'USER = os\\.environ\\.get\\(\\"([^"]+)\\"', src).group(1)
p = re.search(r'PASS = os\\.environ\\.get\\(\\"([^"]+)\\"', src).group(1)
token = base64.b64encode(f"{u}:{p}".encode()).decode()

HEADERS = {
    "Authorization": f"Basic {token}",
    "Content-Type": "application/json"
}

# 示例: 更新页面
data = json.dumps({"content": NEW_HTML}).encode()
req = urllib.request.Request(
    f"{s}/wp-json/wp/v2/pages/375",
    data=data, headers=HEADERS, method="POST"
)
result = json.loads(urllib.request.urlopen(req).read())
```

### 如果 MCP 工具不可用

MCP 工具在 session 启动时注册。如果重启后 MCP 工具未出现，先确认 MCP 进程在运行：

```bash
# 确认 wp_mcp_server.py 进程
ps aux | grep wp_mcp_server | grep -v grep
# 如无，手动启动（会在 ~/.hermes/logs/*.log 输出日志）
nohup python3 ~/.hermes/scripts/wp_mcp_server.py > /dev/null 2>&1 &
```

然后用上述 Python 凭证提取模式直接调用 REST API。

### 发布后的验证

发布后必须验证：
1. `mcp_wordpress_update_post` 返回 `status: publish`
2. 页面实际可访问（`curl -s -o /dev/null -w "%{http_code}" https://babel36acl.xyz/...` 返回 200）
3. CSS style 块未被 wpautop 破坏（检查页面源码中 `<style>` 标签的完整性）

---

## 工作流

### 1. 设计新页面

需求：「新建一个 xx 风格的 xx 页面」。

a. 加载前置技能
b. 使用 `ui-designer` 的三层背景结构 + Hero + 卡片网格模式作为起始骨架
c. 使用本技能的 v5 精确 CSS 数值（容器轻+卡片重）应用 Arcaea 层级
d. 使用 `glassmorphism-ui` 的色彩变体调整色调
e. 添加 Sakurairo 主题 `!important` 覆盖
f. 通过 MCP 桥发布为页面或文章

### 2. 美化已有页面 → 应用 Arcaea 风格

a. 执行 `ui-beautify` 的"UI 问题诊断框架"分析当前页面
b. 按优先级修复：
   - P1: # 独立文本 → h3::before
   - P2: 删除右下引号
   - P3: 卡片背景压暗
   - P4: 正文加亮
c. 用 `css-master` 的设计 Token 体系统一所有数值
d. 用本技能提供的 Sakurairo 冲突覆盖方案
e. 通过 `mcp_wordpress_update_page` 发布

### 3. 修复 CSS 冲突

需求：「样式乱了」、「blockquote 有图标」。

a. 用 `css-master` 的诊断命令检查主题 CSS 中的 `!important` 规则
b. 对冲突选择器用 `!important` 覆盖
c. `blockquote::before` 和 `blockquote::after` 需同时设置 `display: none !important` 和 `content: none !important`

### 4. 发布内容到博客

a. 使用 MCP 工具的 `mcp_wordpress_create_post` / `mcp_wordpress_update_post`
b. **绝对禁止** `curl` 带密码的命令行形式
c. 分类 ID 13 = 嵌入式实战

---

## 文章风格（Article & Writing Style）

此博客的内容遵循混合风格：技术教程用中文技术文档规范，游戏/音乐页面用 Arcaea 情绪化排版，随手记录用口语化笔记。以下规则覆盖所有内容类型。

### 中文技术文档规范

参考 `tech-doc-style-chinese` skill。核心规则：

| 规则 | 示例 |
|------|------|
| 中英文间加空格 | 「支持 JSON 格式」不是「支持JSON格式」 |
| 中文引号用「」 | 「这是一个示例」不是 `"这是一个示例"` |
| 禁止直接称呼 | 「通过以下步骤完成配置」不是「您可以通过以下步骤」 |
| 禁用黑话 | `赋能/抓手/闭环/沉淀/倒逼/对齐/拉齐` 全部禁用 |
| 术语大小写 | `ID HTTP URL JSON API AI LLM` 全大写 |
| 常见错别字 | 阀值→阈值, 登陆→登录, 布署→部署, 配制→配置 |

### 博客文章风格

#### 技术教程（嵌入式实战）

适用于分类 13（嵌入式实战）的文章：

- **每段必有物理含义**：不写「配置 toolchain」，写「toolchain 把 .c 编译成 MCU 能运行的 .bin，烧进去就能跑」
- **完整文件内容**：不缩写配置，展示完整 settings.json / launch.json / CMakePresets.json（>200行才截断）
- **逐行解释**：每个配置块后面跟逐行注释，说明「这个参数做什么、为什么是这个值、改了会怎样」
- **系统查询驱动**：不猜版本号。运行 `tool --version` / `cat /path/to/config` / `cmake --build --clean-first` 把真实输出写进文章
- **对比表**：3 列以上（工具名、版本、安装命令、用途）
- **小白提示**：`💡 小白提示：把 SDK 当「只读包」来用。`
- **从零开始执行清单**：`📋 清单：1.装工具 → 2.下 SDK → 3.配环境 → ...`

文章结构：
```
标题（含技术关键词）
├── 背景/概述
├── 环境搭建（版本表 + 安装命令）
├── 配置详解（逐行注释 + 完整文件）
├── 架构深层（SDK 是什么 / 层怎么分）
├── 调试链（OpenOCD / GDB 命令 + cfg 结构）
├── 常见陷阱
└── 执行清单
```

#### 随笔/技术笔记（随手记录）

适用于短篇技术笔记、bug 分析、架构反思：

- **一个故事一篇**：每个笔记只讲一个问题。800-1200 字，够短够专注
- **口语化**：像跟同事聊天——「翻了一遍 git log，发现…」
- **标题有冲突感**：`字段名叫千分比，存的是百分比` / `一个文件装了 5 种物理单元`
- **结果导向**：开头一句话点明问题，结尾一句总结教训
- **无废话**：不写「本文通过分析…」，直接说「这个 bug 的原因是…」

#### 游戏/音乐页面（Arcaea 情绪化排版）

适用于 Games 页面（Page 375）、Music 页面（Page 430）：

- **排版优先**：文字不需要很多，但必须有视觉层级
- **卡片承载内容**：每款游戏/音乐一个 glass card，标题 h3 + # 标记
- **引用用 blockquote**：渐变底 + 蓝左边框（见 CSS v5.2 定义）
- **emoji 做列表标记**：`ul { list-style: none; }` + emoji 本身
- **随动目录**：长页面加 scroll-linked side TOC

### WordPress 发布约定

| 约定 | 说明 |
|------|------|
| 主分类 | 13 = 嵌入式实战 |
| 主文章保留 | 全景概览不改，详细小节拆为独立教程 |
| 交叉引用 | 主文末尾加教程链接，教程末尾加回主文 |
| 标签 | 每篇文章自动打 2-3 个标签（HPM/STM32/OpenOCD/调试等） |
| 代码高亮 | Prism.js 玻璃卡片风格（已全局） |
| 表格 | 用 pipe table，发布前验证 <wp-block-table> 已正确渲染 |
| 图片 | LightGallery 配置已全局生效（.entry-content img） |

### 已知博客内容目录

| 类型 | 页面/文章 | ID | 风格 |
|------|----------|----|------|
| 游戏展示 | Games 页面（🎮 游戏） | 375 | Arcaea 玻璃卡片 v5.2 |
| 音乐展示 | Music 页面（🌌 音乐） | 430 | Arcaea 玻璃卡片 |
| 教程 | HPM SDK 工程化开发 | 19 | 中文技术文档 |
| 教程 | Arch Linux 嵌入式环境 | 399 | 中文技术文档 + 系统查询 |
| 教程 | HPM SDK 14 个细节 | 395 | 中文技术文档（已删除） |

以下是对 https://babel36acl.xyz/ 的实际扫描结果（2026-05-28）：

### 首页

| 元素 | 状态 | 说明 |
|------|------|------|
| Prism 玻璃代码块 | ✅ 已部署 | FiraCode Nerd Font + 玻璃背景 `rgba(15,18,22,.72)` + blur(18px) + ice blue 边框 |
| 复制按钮玻璃化 | ✅ 已部署 | `rgba(255,255,255,.08)` 背景 + blur(10px) |
| token 去灰底 | ✅ 已部署 | `.token.operator/.entity/.url` 透明 |
| LightGallery | ✅ 已配置 | 在主题选项或自定义 CSS 中 |
| background_blur | ✅ 首页有 | Sakurairo 内置 `backdrop-filter: saturate(120%) blur(8px)` |
| 字体 | ✅ JetBrainsMono | `.Ubuntu-font` 类应用了 Nerd Font |
| --theme-skin | ⚠️ `#505050` | 灰色，非 Arcaea 深色 `#0a0e18` |
| Arcaea 玻璃卡片 | ❌ 未部署 | 首页无 game-category/game-entry/arcaea-wrap |
| bg-overlay | ❌ 未部署 | 首页无 fixed 遮罩层 |

### Games 页面（Page 375）

**全线已部署 ✅** v5.2（实际值与 SKILL.md 中 v5 略有差异）：

| CSS 属性 | SKILL.md v5 值 | 线上 v5.2 实际值 |
|----------|---------------|-----------------|
| bg-overlay | `rgba(8,10,18,0.48)` | `rgba(0,0,0,0.58)` + `blur(18px) brightness(0.72) saturate(65%)` |
| game-category border | `rgba(255,255,255,0.04)` | `rgba(255,255,255,0.06)` |
| game-entry inset shadow | `rgba(255,255,255,0.03)` | `rgba(255,255,255,0.06)` |
| h3::before color | `rgba(255,120,120,0.65)` | `rgba(255,145,145,0.75)` |
| 标题 text-shadow | 无 | `0 0 12px rgba(126,200,255,0.45), 0 2px 4px rgba(0,0,0,0.75)` |
| steam-section | 无 | 额外重型玻璃卡片用 glass-heavy 模式 |
| game-category padding | 28px | 16px 20px（更紧凑） |

### 核心发现

1. **SKILL.md 的 CSS 值**需要更新为 v5.2 实际值以匹配线上
2. 首页 Arcaea 风格未落地 — 后续可考虑
3. 主题选项自定义 CSS 已包含 Prism/FiraCode/预加载等全局样式
4. LightGallery 配置已应用（`lgZoom, lgHash, selector: .entry-content img`）
5. 全局 `--theme-skin: #505050` 未覆盖为 Arcaea 深色

1. **Sakurairo 主题的 `!important` 块引用**：padding、FontAwesome 引号图标需用 `!important` 覆盖，且 `::before + ::after` 都要处理
2. **wpautop 污染 `<style>`**：WordPress 会在 `<style>` 内插 `<p>` 标签，不影响浏览器解析，但 source view 难看
3. **暗色模式覆盖**：Sakurairo 暗黑模式通过 `[theme-mode="dark"]` 选择器覆盖颜色，需加 `!important`
4. **backdrop-filter 性能**：不超过 3 层 blur，否则滚动卡顿
5. **alpha < 0.04 的边框在深色背景上不可见**：在 `#05070d` 基底上最小可用 alpha 为 0.06
6. **CSS 注释污染 meta**：CSS 注释中的文字可能被 WordPress 提取到 `<meta description>`
7. **MCP 工具在 session 重启后丢失**：需确认 wp_mcp_server.py 进程存活，否则用 Python 凭证提取模式
8. **密码安全**：绝不 `curl -u` 带密码，绝不把凭证写入文件或命令行
