# WordPress 插件 CI/CD 模式 — 本地资产 + 自动更新

## 适用场景

WordPress 插件需要本地化前端依赖（Prism、Mermaid、MathJax 等），并且希望：
- 不依赖 CDN（避开 Tracking Prevention 拦截）
- 依赖版本自动追踪上游更新
- GitHub push 自动构建 release zip
- WordPress 后台可见更新提示（PUC library）

## 仓库结构

```
plugin-name/
├── plugin-name.php          # 主插件
├── assets/                  # 本地前端资产
│   ├── prism/              # Prism.js + 组件
│   ├── mermaid/            # Mermaid ESM + chunks
│   ├── mathjax/            # MathJax
│   └── js/                 # medium-zoom 等
├── lib/                     # PUC library
│   └── plugin-update-checker.php
├── .github/workflows/
│   └── release.yml         # CI auto-bump + auto-sync
├── download-mermaid.sh     # Chunk 下载脚本
└── .gitignore
```

## CI 工作流三阶段

### 1. Auto-bump + Release（push main）

```yaml
on: push branches: [main]
  paths-ignore: ['**.md', '.gitignore']

steps:
  - Read Version: x.x.x  # grep from plugin header
  - Bump patch: x.x.(x+1)  # sed update header + constant
  - Commit: "chore: bump x.x.x → x.x.y [skip ci]"
  - Build zip: plugin-name-$VER.zip
  - gh release create "v$VER" --title "v$VER" /tmp/$ZIPNAME
```

关键点：
- 内部目录名必须匹配插件 slug（不带版本号），否则 WordPress 更新解压路径错误
- 必须包含 `lib/` 目录（PUC 本身），否则更新后 PUC 丢失
- commit 消息 `[skip ci]` 防止无限循环

### 2. Auto-sync 依赖（schedule 06:00 / workflow_dispatch）

| 依赖 | 版本来源 | 下载内容 |
|------|---------|---------|
| Mermaid | GitHub Releases api | `mermaid.esm.min.mjs` + 全部 chunk |
| Prism.js | npm registry | core + 6 plugins + 20 languages |
| MathJax | npm registry | `es5/tex-chtml.js` |
| medium-zoom | 随 Prism 同步 | `dist/medium-zoom.min.js` |

检测到新版 → 自动下载 → 更新 PHP 版本常量 → 创建 PR。

Mermaid chunk 特殊处理：
```
jsDelivr API: data.jsdelivr.com/v1/packages/npm/mermaid@${VER}
→ 过滤 dist/chunks/mermaid.esm.min/*.mjs
→ 逐一下载到 assets/mermaid/chunks/mermaid.esm.min/
```

### 3. WordPress 后台更新

集成 [YahnisElsts/plugin-update-checker](https://github.com/YahnisElsts/plugin-update-checker)：

```php
require_once 'lib/plugin-update-checker.php';
$uc = PucFactory::buildUpdateChecker(
    'https://github.com/user/plugin-name/', __FILE__, 'plugin-name'
);
$uc->getVcsApi()->enableReleaseAssets();
$uc->getVcsApi()->setAuthentication($token);  // 推荐，提速率 60→5000 req/h
```

## Pitfalls

1. **Mermaid ESM 需要完整 chunk**：`mermaid.esm.min.mjs` 是动态导入包裹，运行时 `import("./chunks/mermaid.esm.min/chunk-XXXX.mjs")`。必须保留完整目录结构。CI 通过 jsDelivr API 获取文件列表。

2. **PHP filter 必须在 Prism 之前**：`add_filter('the_content', $fn, 1)`（priority 1）确保在 Prism autoloader 扫描前替换。JS DOM 方案无法解决 Prism 异步回调的 race condition。

3. **LightGallery 主题冲突**：Sakurairo 主题自带 LightGallery v2，需抑制 license 警告：
   ```js
   console.warn = function(){/* filter 'license key' */};
   ```

4. **APlayer 空容器**：主题在无播放器页面也尝试初始化，需 patch prototype：
   ```js
   APlayer.prototype.init = function(){if(!container exists) return};
   ```
