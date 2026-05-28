# Sakurairo Arcaea Blog Skill

在 Sakurairo v3.0.10 主题的 WordPress 博客上，应用 Arcaea 风格（玻璃拟态、深色卡片层级、冰蓝配色）的综合技能。**一个文件解决全部**——从设计哲学、CSS 体系、主题集成到 WordPress 发布。

## 用途

- 在 babel36acl.xyz 上设计/美化页面，应用 Arcaea 风格
- 创建带玻璃卡片层级的新页面（Games 风格）
- 修复 Sakurairo 主题 CSS 冲突（blockquote、FontAwesome 图标等）
- 发布文章/页面到 WordPress

## 安装

```bash
# 复制 SKILL.md 到 Hermes skills 目录
mkdir -p ~/.hermes/skills/sakurairo-arcaea-blog-skill
cp SKILL.md ~/.hermes/skills/sakurairo-arcaea-blog-skill/
```

或从 releases 下载 zip：

```bash
curl -L https://github.com/AKCX2002/sakurairo-arcaea-blog-skill/releases/latest/download/sakurairo-arcaea-blog-skill.zip -o sakurairo-arcaea-blog-skill.zip
unzip sakurairo-arcaea-blog-skill.zip -d ~/.hermes/skills/sakurairo-arcaea-blog-skill/
```

## 内容

| 章节 | 说明 |
|------|------|
| 前置技能加载 | 13 个关联技能的加载顺序（Hermes 本地 + skills.sh 生态） |
| CSS 设计体系 | Arcaea 哲学总览、颜色Token、v5.2 最终 CSS、Prism 玻璃代码块、LightGallery 配置、AI Prompt、配套技术栈 |
| Sakurairo 主题集成 | CSS 变量覆盖、暗黑模式、冲突覆盖、短代码兼容、REST API |
| 文章风格 | 中文技术文档规范、技术教程/随笔/页面三种风格的写作指南、WordPress 发布约定 |
| WordPress 发布工作流 | MCP 工具、凭证安全、紧急模式、验证步骤 |
| 工作流 | 设计新页面、美化已有页面、修复 CSS 冲突、发布内容 |
| 网站实际落地状态 | 首页和 Games 页面的实际部署对比表 |
| Pitfalls | 8 条陷阱 |

## 依赖技能

### Hermes 本地
- glassmorphism-ui, ui-beautify, ui-designer, css-master
- wordpress-content-management
- upgrade-tech-tutorial-to-engineering-guide (按需)

### Skills.sh 生态
- sakurairo-theme, wordpress-pro (4.9K ⭐)
- ckm:design / ui-ux-pro-max (21.2K ⭐)
- tailwind (46.7K ⭐), animejs (634 ⭐), react (2.3K ⭐)

## 许可

MIT
