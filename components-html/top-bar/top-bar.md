# TopBar · 顶部栏

> Figma：`436:1719`（COMPONENT_SET）·「🤖座舱组件库-AI版 / TopBar」
> 5 个 style variant 全部已实现。
> 共享骨架：高 156，左右 padding 42，bg=container_primary，HORIZONTAL SPACE_BETWEEN。

---

## 1. Variant 真值（来自 figma_get_component_for_development_deep）

| variant | Figma 节点 | 关键真值 |
|---|---|---|
| `top-bar--title`         | 436:1684 | 仅左侧标题 42px Medium，line-height=42，color=text/primary |
| `top-bar--back`          | 436:1688 | 圆 72 secondary icon-only Button（`btn--md btn--icon-only`）+ 36px Medium "返回" |
| `top-bar--title-actions` | 436:1700 | 标题 42px Medium + 右侧 2× secondary md（gap=30，radius=42） |
| `top-bar--button-avatar` | 436:1694 | 4 个 Tab(gap=72，label 36px Medium，active indicator 48×6) + secondary md 按钮 + 72 圆 avatar；左侧整体 padding-top=18 |
| `top-bar--select-actions`| 436:1709 | 圆形 Checkbox 48（border 36px hairline + border/primary）+ 全选 36px Medium + Danger md(disabled) + Secondary md（右侧 gap=30） |

---

## 2. CSS 类名

### 共享骨架

| 类名 | 角色 |
|---|---|
| `.top-bar` | 共享骨架（高 156，padding 0 42，HORIZONTAL SPACE_BETWEEN，gap=24） |
| `.top-bar__left` / `.top-bar__right` | 左右 cluster（默认 gap=24，多 variant 局部覆盖为 30） |

### Variant 修饰类 + 私有元素

| 类名 | 所属 variant |
|---|---|
| `.top-bar--title` + `.top-bar__title`(42px Medium) | title |
| `.top-bar--back` + `.top-bar__back-text`(36px Medium) | back |
| `.top-bar--title-actions` | title-actions（覆盖 right gap=30） |
| `.top-bar--button-avatar` + `.top-bar__tab-list/__tab-item/__tab-item-label/__tab-item-indicator` + `.top-bar__avatar` | button-avatar |
| `.top-bar--select-actions` + `.top-bar__select/__select-checkbox/__select-label` | select-actions |

> 所有 `__tab-*` `__avatar` `__select-*` 都在 TopBar 命名空间下，**不污染** button/checkbox 等基础组件。

---

## 3. 用法

### 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../button/button.css" />     <!-- back/title-actions/button-avatar/select-actions 都复用 .btn -->
<link rel="stylesheet" href="./top-bar.css" />
<script src="../icon/icon-sprite-inline.js"></script>     <!-- back 用了 icon/back--line -->
```

### 5 个 variant 模板

完整片段见 `top-bar.html`，下面给出最小示例。

```html
<!-- title -->
<header class="top-bar top-bar--title">
  <div class="top-bar__left"><h1 class="top-bar__title">页面标题</h1></div>
  <div class="top-bar__right"></div>
</header>

<!-- back -->
<header class="top-bar top-bar--back">
  <div class="top-bar__left">
    <button type="button" class="btn btn--secondary btn--md btn--icon-only" aria-label="返回">
      <span class="btn-icon" aria-hidden="true">
        <svg viewBox="0 0 48 48"><use href="#icon/back--line"/></svg>
      </span>
    </button>
    <span class="top-bar__back-text">返回</span>
  </div>
  <div class="top-bar__right"></div>
</header>

<!-- title-actions -->
<header class="top-bar top-bar--title-actions">
  <div class="top-bar__left"><h1 class="top-bar__title">标题</h1></div>
  <div class="top-bar__right">
    <button type="button" class="btn btn--secondary btn--md"><span class="btn-label">取消</span></button>
    <button type="button" class="btn btn--secondary btn--md"><span class="btn-label">确认</span></button>
  </div>
</header>

<!-- button-avatar -->
<header class="top-bar top-bar--button-avatar">
  <div class="top-bar__left">
    <ul class="top-bar__tab-list" role="tablist">
      <li><button type="button" class="top-bar__tab-item is-active" role="tab" aria-selected="true">
        <span class="top-bar__tab-item-label">标签</span>
        <span class="top-bar__tab-item-indicator" aria-hidden="true"></span>
      </button></li>
      <!-- 其余 3 个 tab-item，去掉 .is-active 与 aria-selected="true" -->
    </ul>
  </div>
  <div class="top-bar__right">
    <button type="button" class="btn btn--secondary btn--md"><span class="btn-label">设置</span></button>
    <span class="top-bar__avatar" role="img" aria-label="用户头像"
          style="background-image: url('your-avatar.jpg');"></span>
  </div>
</header>

<!-- select-actions -->
<header class="top-bar top-bar--select-actions">
  <div class="top-bar__left">
    <label class="top-bar__select">
      <input type="checkbox" class="top-bar__select-checkbox" aria-label="全选" />
      <span class="top-bar__select-label">全选</span>
    </label>
  </div>
  <div class="top-bar__right">
    <button type="button" class="btn btn--danger btn--md" disabled aria-disabled="true">
      <span class="btn-label">删除</span>
    </button>
    <button type="button" class="btn btn--secondary btn--md">
      <span class="btn-label">取消</span>
    </button>
  </div>
</header>
```

---

## 4. 弹性布局合约（width 适配）

TopBar 的 width 由**业务容器**决定（`width: 100%`），高度恒定 156。无论容器宽度是 1920 / 1560 / 1280，下面的合约都成立：

| 区域 | 行为 |
|---|---|
| `.top-bar__left`  | **始终贴左**。Tab / 标题 / Checkbox 等"内容主体"在这里，是"靠主轴起点"那一侧。 |
| `.top-bar__right` | **始终贴右**。按钮组 / 头像 等"操作 + 身份"在这里，是"靠主轴终点"那一侧。 |
| 中间空白          | 由 `.top-bar { justify-content: space-between }` 自动撑开 / 收缩。**没有显式的中间 spacer 元素**。 |

也就是说：**Tab 居左、按钮居右、中间弹性**这件事是 `.top-bar` 共享骨架的默认行为，不需要 variant 单独再配。`button-avatar` 在 1920 / 1560 / 1280 三档下：

- left（4 × Tab，gap=72，约 768px）保持原样；
- right（设置按钮 + 72 头像 + 内 gap=30，约 240px）保持原样；
- 中间留白 = 容器宽 − 左右 padding 84 − 768 − 240，宽度变化"全部" 让中间空白来吸收。

> 极窄场景（< 1100）超出 left+right 自然总宽时，需要业务层决定收缩策略（Tab 横向滚动 / 头像隐藏等），TopBar 组件自身**不主动隐藏内容**。
>
> 预览页 `top-bar.preview.html` 中所有 variant strip 默认按 **1560**（中控主屏典型尺寸）渲染，可直接对照视觉。

---

## 5. 与 Figma 的差异点（CSS 端补充）

- **`.top-bar__title` 字色**：Figma 文本未绑 token（写死 #1F2229），CSS 用 `var(--color-text-primary)`，dark 模式自动反色。
- **过渡**：select-checkbox 加了 160ms 颜色过渡。Figma 静态稿无动效。
- **focus-visible**：键盘聚焦时 3px `--color-border-active` 描边 + offset。
- **back 圆形按钮**：复用 `.btn--secondary.btn--md.btn--icon-only`（72×72 + cornerRadius=999），与 Figma 等价。
- **select-actions 圆形 Checkbox**：Figma 是**圆形**，与现仓库 `components-html/checkbox/`（方形）形态不同；TopBar 内部 `.top-bar__select-checkbox` 私有实现，未改 checkbox 组件。
- **avatar 图源**：Figma 用了一张 imageHash 实图，预览页用 CSS 渐变占位；业务方应通过 `background-image: url(...)` 注入真实头像。

---

## 6. 在 AppFrame 中的使用

TopBar 是 AppFrame 的 TopBar slot 默认填充。AppFrame 已经把 TopBar 作为外部组件依赖（`app-frame.preview.html` 同时 `<link>` 了 `top-bar.css`），不再自带 TopBar 样式。

```html
<div class="app-frame">
  <aside class="side-menu">...</aside>
  <div class="app-frame__main">
    <div class="app-frame__card">

      <!-- ↓↓↓ TopBar 在这里嵌入；按需切换 variant ↓↓↓ -->
      <div class="app-frame__topbar-slot">
        <header class="top-bar top-bar--title">
          <div class="top-bar__left"><h1 class="top-bar__title">页面标题</h1></div>
          <div class="top-bar__right"></div>
        </header>
      </div>

      <div class="app-frame__content">
        <div class="app-frame__content-canvas">…</div>
      </div>
    </div>
  </div>
</div>
```

---

## 7. 自检清单

- [x] 共享骨架高度 156、padding 0 42（`--space-2xl`）、bg=container_primary
- [x] 5 个 variant 全部按 Figma 真值落地
- [x] back 用 `.btn--secondary.btn--md.btn--icon-only`，title-actions/select-actions 用 `.btn--secondary.btn--md` / `.btn--danger.btn--md`
- [x] button-avatar 内 Tab gap=72（`--space-7xl`），indicator 48×6（`--space-3xl`×`--space-xxs`）
- [x] select-actions 内 Checkbox 圆形 48×48，border 36 hairline + border/primary
- [x] 私有元素全部以 `.top-bar__*` 命名，无侵入
- [x] Light / Dark token 自动反色
- [x] 与 AppFrame 解耦：AppFrame 不再自带 TopBar 样式，AppFrame preview 引用 `top-bar.css`
- [x] index.html 导航已接入 top-bar
- [x] 弹性布局合约成立：left 贴左 / right 贴右 / 中间留白由 `space-between` 自动吸收，预览页所有 variant 默认 1560 宽渲染
