# Button · 按钮

> Figma 节点：`6:38` ·「🤖座舱组件库-AI版 / Button」· Component Set 含 130 variants
> 严格 1:1 复刻 Figma Variables 真实测量值

---

## 维度系统（4 个 Size）

| Size | 高度 | 圆角（普通/Icon-Only） | 字号/行高 | icon | None padding | Leading padding（左/右） | Trailing padding（左/右） | gap |
|---|---|---|---|---|---|---|---|---|
| Compact-60 (`btn--xs`) | 60 | 42 / 999 | 24/24 | 30×30 | 30/30 | 24/30 | 30/24 | 6 |
| Small-48 (`btn--sm`) | 48 | 42 / 999 | 24/24 | 24×24 | 24/24 | 18/24 | 24/18 | 6 |
| Medium-72 (`btn--md`) | 72 | 42 / 999 | 28/28 | 36×36 | 36/36 | 30/36 | 36/30 | 6 |
| Large-84 (`btn--lg`) | 84 | 42 / 999 | 32/32 | 42×42 | 36/36 | 30/36 | 36/30 | 6 |

> Icon-Only 模式下宽=高 = Size 高度，圆角变为 999（正圆）。

---

## 5 个 Variant

| Variant | Default 背景 | Default 文字/图标 | Disabled 背景 | Disabled 文字/图标 |
|---|---|---|---|---|
| `btn--primary` | `--color-bg-item-primary`（深色） | `--color-text-primary-inverse` | `--color-bg-item-disabled` | `--color-text-disabled` |
| `btn--secondary` | `--color-bg-item-tertiary` | `--color-text-primary` | `--color-bg-item-disabled` | `--color-text-disabled` |
| `btn--ghost` | 透明 + 2px `--color-border-disabled` | `--color-text-primary` | 同左 | `--color-text-disabled` |
| `btn--danger` | `--palette-red-transparent-0` (red5 12%) | `--color-text-warning` | `--color-bg-item-disabled` | `--color-text-disabled` |
| `btn--text` | 透明 | `--color-text-primary` | 透明 | `--color-text-disabled` |

### ⚠️ Figma 独有：Primary + Icon-Only + Default

这是个反直觉但必须遵守的规则：

| 状态 | 背景 | 图标 | 阴影 |
|---|---|---|---|
| Default | `--color-bg-item-elevated`（白） | `--color-text-primary`（深） | `--shadow-medium` |
| Disabled | `--color-bg-item-disabled`（灰） | `--color-text-disabled` | 无 |

**也就是说，Primary Icon-Only 的"默认状态"是个白底浮起的圆形按钮，不是深色实心**。这个差异在视觉上很明显，AI 生成时必须严格遵循 Figma。

---

## 用法

### 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../../design-system/shadows.css" />
<link rel="stylesheet" href="../../design-system/text-styles.css" />
<link rel="stylesheet" href="./button.css" />
```

### 基础

```html
<button class="btn btn--primary btn--md">按钮</button>
```

### 修饰类组合规则

```text
.btn  +  size 修饰  +  variant 修饰  +  icon-position 修饰
       (xs/sm/md/lg) (primary/secondary/ghost/danger/text) (leading/trailing/icon-only)
```

未加 icon-position 修饰 = `None`（纯文字）。

### 带图标

> 图标统一从项目 sprite 取，不要在 button 里手画 path。
> 页面需先在 `<body>` 顶部加载：`<script src="../icon/icon-sprite-inline.js"></script>`，
> 再用同文档锚点 `<use href="#icon/<name>--<line|fill>"/>` 引用。

```html
<!-- Leading icon：收藏（实心五角星） -->
<button class="btn btn--primary btn--md btn--leading">
  <span class="btn-icon" aria-hidden="true">
    <svg viewBox="0 0 48 48"><use href="#icon/star--fill"/></svg>
  </span>
  <span class="btn-label">收藏</span>
</button>

<!-- Trailing icon：下一步（chevron-right） -->
<button class="btn btn--secondary btn--md btn--trailing">
  <span class="btn-label">下一步</span>
  <span class="btn-icon" aria-hidden="true">
    <svg viewBox="0 0 48 48"><use href="#icon/enter--line"/></svg>
  </span>
</button>

<!-- Icon-Only（必须给 aria-label） -->
<button class="btn btn--primary btn--md btn--icon-only" aria-label="收藏">
  <span class="btn-icon" aria-hidden="true">
    <svg viewBox="0 0 48 48"><use href="#icon/star--fill"/></svg>
  </span>
</button>
```

> sprite 内 path 不写 `fill`，由 `.btn .btn-icon svg { fill: currentColor }`
> 接管，按钮的 variant 颜色会自动驱动图标颜色。
> 外层 `<svg>` 不需要加 `.icon` class —— `.btn-icon` 容器已按 size 给定 30/24/36/42。

### Disabled

直接加 `disabled` 属性：

```html
<button class="btn btn--primary btn--md" disabled>按钮</button>
```

`btn-icon svg` 在 `button.css` 里默认 `fill: currentColor`，所以 sprite 内的 path 无需固定颜色，会自动跟随 `.btn` 的 `color`。

---

## 与 Figma 的差异点（CSS 端补充）

以下是 Figma 静态稿没有但 CSS 必须补的：

- **过渡**：所有 variant 在 hover/active 时 `filter: brightness()` ±8% 反馈，160ms ease。
- **focus-visible**：键盘聚焦时 3px 蓝色描边（`--color-border-active`）+ 3px offset。
- **Text variant padding**：Figma 没明确 padding 数值，CSS 端给 12px 左右，避免点击区过小。

这些都不影响 1:1 视觉，只在交互态出现。

---

## 自检清单

- [ ] Primary Default 是深色 (#1f2229 light / 白 90% dark)
- [ ] Primary Icon-Only Default 是白底 + shadow-medium，不是深色！
- [ ] Ghost 边框是 **2px**（不是 1px）
- [ ] Danger 背景是 red5 的 **12% 透明度**，不是实心红
- [ ] 所有尺寸圆角都是 42（除 Icon-Only=999）
- [ ] Leading 和 Trailing padding 是镜像关系
- [ ] Disabled 优先级高于 variant 默认色，所有 variant 在 disabled 下背景都降为 `--color-bg-item-disabled`（除 Ghost/Text 保持透明）
