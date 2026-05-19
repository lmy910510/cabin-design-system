# SideMenu · 侧栏导航

> Figma 节点 `436:1770`（COMPONENT_SET，2 variant：`mode = expanded | collapsed`）。
> 含 SLOT 属性 `nav-list`；结构 = `nav-group`（导航列表白卡） + `footer`（底部白卡）。
>
> 视觉真值通过 AppFrame (436:1772) 内嵌实例的精确测量得到（与设计稿 1:1）。
> AppFrame.css 不再声明 `.side-menu*`，全部真源在本组件 css。

## 属性 / 变体

| 属性 | 类型 | 默认 | 说明 |
|---|---|---|---|
| nav-list | SLOT | 默认 4 项 | 导航列表插槽（位于 nav-group 内） |
| mode | VARIANT | expanded | `expanded` \| `collapsed` |

## 视觉真值（来自 Figma 测量）

| 项 | expanded | collapsed |
|---|---|---|
| 容器尺寸 | 308 × 1080 | 156 × 1080 |
| 容器布局 | VERTICAL · SPACE_BETWEEN · counter=CENTER | 同左 |
| 容器 padding | 24（md） | 24 |
| nav-group 白卡 | 260 × HUG · radius 42 · padding 12 · bg=card_primary | 108 × HUG · 同形态 |
| nav-list itemSpacing | 24（md） | 24 |
| footer 白卡 | 254 × HUG · radius 42 · padding 12 · bg=card_primary | 108 × HUG |
| 子项 | `.nav-item`（hasText=true，236×84） | `.nav-item.nav-item--icon-only`（84×84） |
| 容器底色 | 透明（依赖父级 page_secondary） | 同左 |

## 与子组件的关系

```
SideMenu (容器，VERTICAL space-between，padding 24)
├─ nav-group     白卡 260（或 108）× HUG，radius 42，padding 12
│   └─ nav-list  SLOT，列向 itemSpacing 24
│       └─ nav-item × N
└─ footer        白卡 254（或 108）× HUG，radius 42，padding 12
    └─ nav-item  × M
```

子项视觉真源在 [`SideMenu / Item`](../side-menu-item/side-menu-item.md)（类名 `.nav-item`）。本组件不重写 item 视觉。

## 类名

| 类名 | 作用 |
|---|---|
| `.side-menu` | 容器（aside，默认 expanded） |
| `.side-menu--collapsed` | 收起态 156；内部白卡 width=108 |
| `.side-menu__nav-group` | 导航列表白卡（含 nav-list SLOT） |
| `.side-menu__nav-list` | nav-item 列表（gap 24） |
| `.side-menu__footer` | 底部白卡 |

## 用法

### 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../../design-system/shadows.css" />
<link rel="stylesheet" href="../side-menu-item/side-menu-item.css" />
<link rel="stylesheet" href="./side-menu.css" />
<script src="../icon/icon-sprite-inline.js"></script>
```

### 最小骨架（expanded）

```html
<aside class="side-menu" aria-label="主导航">
  <div class="side-menu__nav-group">
    <ul class="side-menu__nav-list">
      <li>
        <a class="nav-item is-active" href="#" aria-current="page">
          <span class="nav-item__icon" aria-hidden="true">
            <svg viewBox="0 0 48 48"><use href="#icon/list--fill"/></svg>
          </span>
          <span class="nav-item__label">列表</span>
        </a>
      </li>
      <!-- ... -->
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
```

### 切到 collapsed

只需在 `.side-menu` 上加 `.side-menu--collapsed`，**同时**给每个 `.nav-item` 加 `.nav-item--icon-only` 并去掉 `.nav-item__label`，把 `aria-label` 写到 `<a>` 上。

### 选中态主题色

通过 [`.nav-item` 暴露的 token](../side-menu-item/side-menu-item.md#css-变量业务可覆盖) `--nav-item-active-bg` / `--nav-item-active-fg` 覆盖。覆盖位置可放在 `.app-frame` / `.side-menu` / `.side-menu__nav-group` 任一祖先：

```html
<aside class="side-menu"
       style="--nav-item-active-bg: var(--palette-orange-transparent);
              --nav-item-active-fg: var(--palette-orange-5);">
  ...
</aside>
```

## 设计契约

- 同一 SideMenu 内 **同一时刻只有一个 `.nav-item` 处于 `.is-active`**（业务保证）。
- 折叠态由父容器统一切换：`.side-menu--collapsed` + 每个 item `.nav-item--icon-only`。
- 视觉真源唯一在「本文件（容器）+ side-menu-item.css（子项）」；AppFrame.css **不再** 声明这些类。
- 选中 nav-item 的 icon 形态约定：`selected=true` 走 `--fill`，`selected=false` 走 `--line`（Figma 真值）。
