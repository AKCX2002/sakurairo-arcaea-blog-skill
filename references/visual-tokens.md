# Arcaea Visual Design Tokens

This reference captures the complete set of design values used across Arcaea-styled pages and articles on the blog.

## Article Wrapper Tokens (`.arcaea-article-content`)

Applied to every blog post as inline `<style>` + `<div class="arcaea-article-content">`.

| CSS Variable | Value | Purpose |
|-------------|-------|---------|
| `--arcaea-bg` | `rgba(8,21,42,0.42)` | Card/container dark blue semi-transparent background |
| `--arcaea-border` | `rgba(230,238,255,0.78)` | Bright white border, all cards |
| `--arcaea-primary` | `rgba(238,244,255,0.96)` | Heading / emphasis text |
| `--arcaea-text` | `rgba(238,244,255,0.94)` | Body text |
| `--arcaea-muted` | `rgba(238,244,255,0.65)` | Subtitle / caption text |
| `--arcaea-hash` | `rgba(255,130,130,0.55)` | `h3::before` # marker |

### Core visual pattern

```
background: rgba(8,21,42,0.42)          ← deep blue semi-transparent
border: 1px solid rgba(230,238,255,0.78)  ← bright white fine border
border-radius: 10px
backdrop-filter: blur(12px) saturate(130%)  ← light glass
box-shadow:
  0 12px 36px rgba(0,0,0,0.22),            ← drop shadow
  inset 0 1px 0 rgba(255,255,255,0.12)     ← white inner highlight edge
```

### Element-specific overrides

| Element | Differences from base |
|---------|---------------------|
| `<h2>` | `color: rgba(238,244,255,0.96)`, `border-bottom: 1px solid rgba(230,238,255,0.40)`, `text-shadow` |
| `<h3>` | Same color + text-shadow, `::before` with # marker, `::after { display:none }` |
| `<p>` | `color: rgba(238,244,255,0.94)`, `line-height: 1.8` |
| `<pre>` code blocks | Uses base card pattern + `!important` overrides for background/color/font-family |
| `<code>` inline | `background: rgba(230,238,255,0.10)`, `color: rgba(238,244,255,0.94)!important` |
| `<pre><code>` | `background:transparent`, `color:inherit!important` (inherit pre color, not Prism) |
| `<blockquote>` | Same as base but left border `3px solid rgba(230,238,255,0.90)`, `::before/::after { display:none!important }` |
| `<table>` | Same as base, `border: 1px solid rgba(230,238,255,0.78)`, `th` slightly brighter |
| `<li>` | `color: rgba(238,244,255,0.92)`, `font-weight: 600` |
| `<hr>` | Gradient from transparent → semi-white → transparent |
| `<img>` | `border-radius: 10px` |
| `.wp-block-group` | Same base card pattern, `padding: 16px` |

## Page-level Tokens (Games / Music pages)

### Overlay

```
.bg-overlay {
  background: rgba(0,0,0,0.62);
  backdrop-filter: blur(20px) brightness(0.68) saturate(60%);
  position: fixed; inset: 0;
}
```

### Ambient glow

```
.bg-glow-1: radial-gradient(circle, rgba(120,180,255,0.08), transparent 70%); blur(120px); top-right
.bg-glow-2: radial-gradient(circle, rgba(167,139,250,0.06), transparent 70%); blur(100px); bottom-left
@keyframes floatGlow: oscillate translate(0→50px,-30px) scale(1→1.06)
```

### Noise texture

```
games-arcaea-wrap::before: fractalNoise SVG data URI, opacity 0.4, z-index -2
```

### Category container (light)

```
background: rgba(10,14,24,0.42);
border: 1px solid rgba(255,255,255,0.06);
border-radius: 18px;
box-shadow: none;  /* deliberately no shadow */
```

### Entry card (heavy)

```
background: rgba(8,12,20,0.82);
border-radius: 18px;
border: 1px solid rgba(160,220,255,0.16);
box-shadow: 0 8px 32px rgba(0,0,0,0.32), inset 0 0 0 1px rgba(255,255,255,0.06);
```

## Color Palette

| Usage | Hex | Notes |
|-------|-----|-------|
| Deepest background | `#05070b` | Body/page base |
| Mid background | `#0b1020` | Section fills |
| Surface | `#111827` | Elevated surfaces |
| Dark overlay | `#09090f` | bg-overlay close match |
| Ice blue primary | `#b0dcff` | Title text |
| Violet accent | `#9db4ff` | Category text |
| Sky blue | `#8ad8ff` | Links |
| Hash marker | `rgba(255,130,130,0.55)` | h3::before |
| Card bg (article) | `rgba(8,21,42,0.42)` | Deep blue semi-trans |
| Border (article) | `rgba(230,238,255,0.78)` | Bright white |
| Body text (article) | `rgba(238,244,255,0.94)` | Near-white |
| Heading text (article) | `rgba(238,244,255,0.96)` | Higher contrast |

## Typography

| Element | Font Stack |
|---------|-----------|
| Code / Mono | `"FiraCode Nerd Font", "Fira Code", "JetBrains Mono", Consolas, monospace` |
| Body / UI | `"Noto Sans SC", "Microsoft YaHei", -apple-system, sans-serif` |
| English titles | `Orbitron, Rajdhani, Exo 2, Inter, Space Grotesk` |

## Glassmorphism Layers

Maximum 2 layers of `backdrop-filter` blur to avoid scroll jank:

```
Background image (layer 0)
  ↓
.bg-overlay: blur(20px) brightness(0.68) saturate(60%)  (layer 1, fixed)
  ↓
.game-category: blur(10px)                                (layer 2)
.game-entry: blur(8px)
```

For article wrapper, the `blur(16px)` on the container serves as the single glass layer.

## Prohibited Styles

- High-purity red, fluorescent green, rainbow gradients, oversaturated cyan/purple (`#7dd3fc`, `#c084fc`)
- Bounce/cartoon animations (use float/fade/ambient only)
- Thick borders (>2px)
- RGB gaming-style glow
- `transition: all` (use specific properties; compositor-friendly)
