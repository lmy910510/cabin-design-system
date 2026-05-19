# AppFrame · 全局应用框架

> Figma 节点：`436:1772` ·「🤖座舱组件库-AI版 / AppFrame」· 1920×1080
> 已实现：SideMenu 2 modes（expanded / collapsed）。
> **TopBar 已拆为独立组件** → 见 [`components-html/top-bar/`](../top-bar/top-bar.md)（5 个 style variant 全部在 top-bar 组件内）。

---

## 1. 框架结构（来自 Figma component description + 真实测量）

```
┌──────────────────────────────────────── 1920 ────────────────────────────────────────┐
│ ┌── SideMenu ──┐ ┌── main (FILL × 1080，padding: 24 24 24 0) ───────────────────────┐│
│ │  308 × 1080  │ │ ┌── main-card (FILL×FILL, radius=42, bg=card_primary) ──────────┐ ││
│ │  (collapsed  │ │ │ ┌── TopBar slot (FILL × 156) ───────────────────────────────┐│ ││
│ │   时 156)    │ │ │ │  外部组件：components-html/top-bar/                       ││ ││
│ │              │ │ │ │  5 个 style variant（title/back/title-actions/...）       ││ ││
│ │ nav-group    │ │ │ └──────────────────────────────────────────────────────────┘│ ││
│ │  └ nav-list  │ │ │ ┌── content SLOT (FILL×FILL, padding: 0 42) ────────────────┐│ ││
│ │ footer       │ │ │ │  ┌── content-canvas (bg=item_quaternary) ───────────────┐││ ││
│ │  └ footer-btn│ │ │ │  │  业务自由绘制                                        │││ ││
│ │              │ │ │ │  └────────────────────────────────────────────────────┘││ ││
│ │              │ │ │ └──────────────────────────────────────────────────────────┘│ ││
│ │              │ │ └────────────────────────────────────────────────────────────┘ ││
│ └──────────────┘ └─────────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────────────────────────────┘
   bg = page_secondary (#F7F8FA)
```

注意点：
- AppFrame description 写"SideMenu 固定宽度 280"，**实测 308**（24 padding × 2 + 内部 260）。CSS 按测量值。
- main 容器 `padding-left = 0`，让 main-card 紧贴 SideMenu 右边，不留空隙。
- TopBar 高度 156、左右 padding 42 等真值已迁到 top-bar 组件，本组件只为 TopBar 留位（`.app-frame__topbar-slot` 高度 0 0 auto）。
- SideMenu collapsed 时宽度 156，nav-item 切换为 `--icon-only`（84×84）。

---

## 2. CSS 入口（仅 AppFrame 自身的类）

| 类名 | 角色 |
|---|---|
| `.app-frame` | 顶层 1920×1080 容器，bg=page_secondary |
| `.app-frame__main` / `.app-frame__card` | 右侧主区 + 圆角白卡 |
| `.app-frame__topbar-slot` | TopBar 放置位（外部组件 top-bar 填充） |
| `.app-frame__content` / `.app-frame__content-canvas` | 卡内 content slot 与自由绘制层 |
| `.side-menu` | 左侧导航 308×1080，VERTICAL SPACE_BETWEEN（默认 expanded） |
| `.side-menu--collapsed` | 收起态 156×1080；内部白卡 width=108 |
| `.side-menu__nav-group` | 上半白卡（expanded 260 / collapsed 108） |
| `.side-menu__nav-list` | nav-item 列表（gap=24） |
| `.side-menu__footer` | 底部白卡 |
| `.nav-item` | 导航项（默认 hasText=true，236×84，radius=30） |
| `.nav-item--icon-only` | icon-only 形态（84×84，padding=0，gap=0），用于 collapsed |
| `.nav-item.is-active` / `[aria-current="page"]` | 选中态（默认 blue/0 + text/link） |

> TopBar 相关类（`.top-bar*` / `.top-bar__tab-*` / `.top-bar__avatar` / `.top-bar__select-*`）已全部迁到 `components-html/top-bar/top-bar.css`。AppFrame.css 不再含任何 TopBar 样式。

---

## 3. 用法

### 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../../design-system/shadows.css" />
<link rel="stylesheet" href="../../design-system/text-styles.css" />
<link rel="stylesheet" href="../button/button.css" />     <!-- TopBar 多 variant 复用 .btn -->
<link rel="stylesheet" href="../top-bar/top-bar.css" />   <!-- TopBar 5 个 variant 真源 -->
<link rel="stylesheet" href="./app-frame.css" />
<script src="../icon/icon-sprite-inline.js"></script>
```

> **依赖关系**：AppFrame 仅依赖 `top-bar.css` 的视觉定义；`top-bar` 组件本身又依赖 `button.css`。引入顺序按"基础 token → button → top-bar → app-frame"即可。

### 最小骨架（expanded + TopBar/title）

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

      <!-- ↓↓↓ 这里挂载 top-bar 组件实例（任选一个 variant）↓↓↓ -->
      <div class="app-frame__topbar-slot">
        <header class="top-bar top-bar--title">
          <div class="top-bar__left"><h1 class="top-bar__title">页面标题</h1></div>
          <div class="top-bar__right"></div>
        </header>
      </div>

      <div class="app-frame__content">
        <div class="app-frame__content-canvas" data-slot="content">
          <!-- 业务页面 -->
        </div>
      </div>
    </div>
  </div>
</div>
```

### 切到 collapsed

只需在 `.side-menu` 上加 `.side-menu--collapsed`，并把每个 `.nav-item` 加 `.nav-item--icon-only` 同时去掉 `.nav-item__label`。

### 切换 TopBar variant

详见 [`top-bar/top-bar.md`](../top-bar/top-bar.md)。把 `<header class="top-bar top-bar--*">` 替换为对应 variant 即可，AppFrame 这一层不需要改。

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

## 4. Variant 真值对照（AppFrame 范围内）

| variant | Figma 节点 | 关键真值 |
|---|---|---|
| `SideMenu / mode=expanded`     | 436:1725 | 308×1080，padding=24，nav-group 260×432 / footer 254×108，内部 nav-item 236×84 |
| `SideMenu / mode=collapsed`    | 436:1750 | 156×1080，padding=24，nav-group 108×432 / footer 108×108，内部 nav-item 84×84（icon-only） |

TopBar 5 个 variant 的真值见 [`top-bar/top-bar.md` → § 1](../top-bar/top-bar.md)。

---

## 5. 与 Figma 的差异点（CSS 端补充）

- **过渡**：nav-item 加了 160ms 颜色过渡。Figma 静态稿无动效。
- **focus-visible**：键盘聚焦时 3px `--color-border-active` 描边 + 3px offset。
- **未选中 hover 反馈**：nav-item 未选中态 hover 给 `--color-bg-item-tertiary`，避免点击区无视觉反馈。
- **content-canvas radius**：Figma description 说"圆角随父级"，本组件依赖 `.app-frame__card` 的 `overflow: hidden` 自然剪裁。

---

## 6. 自检清单

- [x] AppFrame 顶层 1920×1080，bg=page_secondary
- [x] SideMenu expanded 308 / collapsed 156（实测，非 description 写的 280）
- [x] main padding 是 `24 24 24 0`（左侧紧贴 SideMenu）
- [x] main-card radius 42（`--space-2xl`）
- [x] nav-item radius 30、padding `0 30 0 24`、gap 12
- [x] nav-item--icon-only 84×84，padding=0，gap=0
- [x] 选中 nav-item icon 走 `--fill`、未选中走 `--line`
- [x] **TopBar 已拆出**：app-frame.css / app-frame.html / app-frame.preview.html 不再含任何 TopBar 样式或片段；本预览仅以 `top-bar--title` 作 slot 示意，并通过 `<link>` 引用 `top-bar.css`
- [x] Light / Dark token 自动反色
- [x] index.html 导航已接入 app-frame 与 top-bar 两项
