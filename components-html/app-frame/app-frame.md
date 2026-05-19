# AppFrame · 全局应用框架

> Figma 节点：`436:1772` ·「🤖座舱组件库-AI版 / AppFrame」· 1920×1080
> 本次交付组合：**AppFrame · SideMenu(expanded) · TopBar(title)**

---

## 框架结构（来自 Figma component description + 真实测量）

```
┌──────────────────────────────────────── 1920 ────────────────────────────────────────┐
│ ┌── SideMenu ──┐ ┌── main (FILL × 1080，padding: 24 24 24 0) ───────────────────────┐│
│ │  308 × 1080  │ │ ┌── main-card (1588×1032, radius=42, bg=card_primary) ─────────┐ ││
│ │              │ │ │ ┌── TopBar (1588×156, padding: 0 42, bg=container_primary) ─┐│ ││
│ │ nav-group    │ │ │ │  left: 标题 42px Medium       right: (style=title 时为空) ││ ││
│ │  └ nav-list  │ │ │ └──────────────────────────────────────────────────────────┘ │ ││
│ │              │ │ │ ┌── content SLOT (1588×876, padding: 0 42) ─────────────────┐│ ││
│ │ footer       │ │ │ │  ┌── content-canvas 1504×876 (bg=item_quaternary) ──────┐││ ││
│ │  └ footer-btn│ │ │ │  │   业务自由绘制                                       │││ ││
│ │              │ │ │ │  └────────────────────────────────────────────────────┘││ ││
│ │              │ │ │ └──────────────────────────────────────────────────────────┘ │ ││
│ │              │ │ └────────────────────────────────────────────────────────────┘ ││
│ └──────────────┘ └─────────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────────────────────────────┘
   bg = page_secondary (#F7F8FA)
```

注意点：
- AppFrame description 写"SideMenu 固定宽度 280"，**实测 308**（24 padding × 2 + 内部 260）。CSS 按测量值。
- main 容器 `padding-left = 0`，让 main-card 紧贴 SideMenu 右边，不留空隙。
- TopBar 在 style=title 时仅左侧标题；其他 4 个 variant 见底部 TODO。

---

## CSS 入口

| 类名 | 角色 |
|---|---|
| `.app-frame` | 顶层 1920×1080 容器，bg=page_secondary |
| `.side-menu` | 左侧导航 308×1080，VERTICAL SPACE_BETWEEN |
| `.side-menu__nav-group` | 上半 260×432 白卡 |
| `.side-menu__nav-list` | nav-item 列表（gap=24） |
| `.side-menu__footer` | 底部 254×108 白卡 |
| `.nav-item` | 导航项（hasText=true 形态，236×84，radius=30） |
| `.nav-item.is-active` / `[aria-current="page"]` | 选中态（默认 blue/0 + text/link） |
| `.app-frame__main` | 右侧 main 容器，padding 24 24 24 0 |
| `.app-frame__card` | main-card 白色圆角卡片 1588×1032 |
| `.app-frame__topbar-slot` | TopBar 槽位 |
| `.top-bar` | TopBar 默认骨架 1588×156 |
| `.top-bar__title` | style=title 标题 42px Medium |
| `.app-frame__content` | content SLOT padding 区 |
| `.app-frame__content-canvas` | 1504×876 自由绘制层 |

---

## 用法

### 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../../design-system/shadows.css" />
<link rel="stylesheet" href="../../design-system/text-styles.css" />
<link rel="stylesheet" href="./app-frame.css" />
<script src="../icon/icon-sprite-inline.js"></script>
```

### 最小骨架

```html
<div class="app-frame">
  <aside class="side-menu" aria-label="主导航">
    <div class="side-menu__nav-group">
      <ul class="side-menu__nav-list">
        <li>
          <a class="nav-item is-active" href="#" aria-current="page">
            <span class="nav-item__icon" aria-hidden="true">
              <svg viewBox="0 0 48 48"><use href="#icon/list--fill"/></svg>
            </span>
            <span class="nav-item__label">我的列表</span>
          </a>
        </li>
        <!-- 其他 nav-item ... -->
      </ul>
    </div>
    <div class="side-menu__footer">
      <a class="nav-item" href="#">
        <span class="nav-item__icon" aria-hidden="true">
          <svg viewBox="0 0 48 48"><use href="#icon/setting--line"/></svg>
        </span>
        <span class="nav-item__label">设置</span>
      </a>
    </div>
  </aside>

  <div class="app-frame__main">
    <div class="app-frame__card">
      <div class="app-frame__topbar-slot">
        <header class="top-bar">
          <div class="top-bar__left">
            <h1 class="top-bar__title">页面标题</h1>
          </div>
          <div class="top-bar__right"></div>
        </header>
      </div>

      <div class="app-frame__content">
        <div class="app-frame__content-canvas" data-slot="content">
          <!-- 业务页面 1504×876 -->
        </div>
      </div>
    </div>
  </div>
</div>
```

### 选中态主题色

Figma 描述明确："SideMenu 选中态颜色 = 当前产品主题色（brand main），不要硬编码"。

`.app-frame` 提供两个可被业务覆盖的变量（默认 = Figma 示例的 blue）：

```css
.app-frame {
  --app-frame-nav-active-bg: var(--palette-blue-0);   /* 选中底色 */
  --app-frame-nav-active-fg: var(--color-text-link);  /* 选中文字/icon */
}
```

业务方覆盖：

```html
<div class="app-frame"
     style="--app-frame-nav-active-bg: var(--palette-orange-transparent);
            --app-frame-nav-active-fg: var(--palette-orange-5);">
  ...
</div>
```

> 选中 nav-item 的 icon 形态约定：选中用 `--fill`，未选中用 `--line`。这是 Figma 真值（selected=true 走面形 / selected=false 走线形）。

---

## 与 Figma 的差异点（CSS 端补充）

- **过渡**：nav-item hover/active 时 background-color/color 160ms ease。Figma 静态稿无动效。
- **focus-visible**：键盘聚焦时 3px `--color-border-active` 描边 + 3px offset。
- **未选中 hover 反馈**：bg 给 `--color-bg-item-tertiary`，避免点击区无视觉反馈。
- **`.top-bar__title` 字色**：Figma 文本未绑 token（写死 #1F2229），CSS 用 `var(--color-text-primary)`，dark 模式自动反色。
- **content-canvas radius**：Figma description 说"圆角随父级"，本组件依赖 `.app-frame__card` 的 `overflow: hidden` 自然剪裁，content-canvas 自身不显式 radius。

---

## TODO（Figma 内已有，本仓库未实现）

| variant | Figma 节点 | 描述 |
|---|---|---|
| `SideMenu / mode=collapsed` | 436:1750 | 156×1080，nav-item 84×84 icon-only（hasText=false） |
| `TopBar / style=back` | 436:1688 | 圆形 secondary icon-only Button + "返回" 36px |
| `TopBar / style=title-actions` | 436:1700 | 标题 42px + 右侧 2 个 secondary md 按钮（gap=30） |
| `TopBar / style=button-avatar` | 436:1694 | 4 个 Tab（首个 active 带 6px indicator）+ secondary 按钮 + 72×72 圆形 avatar |
| `TopBar / style=select-actions` | 436:1709 | 48 圆形 Checkbox + 全选 36px + Danger md 按钮 + Secondary md 按钮 |

> select-actions 里 Figma 用的是**圆形** checkbox（无勾选打底，仅圆环），跟现仓库 `components-html/checkbox/` 是不同形态，待 follow-up 时统一规划。

---

## 自检清单

- [x] AppFrame 顶层 1920×1080，bg=page_secondary
- [x] SideMenu 实测 308 宽（不是 description 写的 280）
- [x] main padding 是 `24 24 24 0`（左侧紧贴 SideMenu）
- [x] main-card radius 42（`--space-2xl`）
- [x] TopBar 高度 156，左右 padding 42（`--space-2xl`）
- [x] nav-item radius 30、padding `0 30 0 24`、gap 12
- [x] 选中 nav-item icon 走 `--fill`、未选中走 `--line`
- [x] content-canvas 真实尺寸 1504×876，bg=item_quaternary
- [x] Light / Dark token 自动反色
