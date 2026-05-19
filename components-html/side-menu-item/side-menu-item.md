# SideMenu / Item · 侧栏导航项

> Figma 节点 `459:2790`（COMPONENT_SET，4 variant）· SideMenu 的子项 · 类名 **`.nav-item`**
>
> 视觉真值通过 AppFrame (436:1772) 内嵌实例的精确测量得到（与设计稿 1:1）。
> AppFrame.css 不再声明 `.nav-item*`，全部真源在本组件 css。

## 属性 / 变体（4 组合）

| selected | hasText | 视觉 |
|---|---|---|
| true  | true  | bg=palette-blue-0（业务可覆盖到 brand），icon+label=text/link，icon 走 `--fill` |
| false | true  | 透明底，icon+label=text/tertiary，icon 走 `--line` |
| true  | false | bg=palette-blue-0，仅 icon（icon-only，84×84） |
| false | false | 透明底，仅 icon（icon-only，84×84） |

## 视觉真值（来自 Figma 测量）

| 维度 | hasText=true | hasText=false |
|---|---|---|
| 尺寸 | 236 × 84（width=父容器拉满） | 84 × 84 |
| padding | 0 / 30 / 0 / 24 | 0 |
| gap | 12 (xs) | 0 |
| 圆角 | 30 (lg) | 30 (lg) |
| icon 槽 | 42 × 42（48 viewBox 缩放） | 同左 |
| label | 32 / 38 Medium | — |
| 选中底 | `--nav-item-active-bg`（默认 `--palette-blue-0`） | 同左 |
| 选中文 | `--nav-item-active-fg`（默认 `--color-text-link`） | 同左 |
| 未选中文 | `--color-text-tertiary` | 同左 |

## 类名

| 类名 | 作用 |
|---|---|
| `.nav-item` | 容器（默认 hasText=true，236×84） |
| `.nav-item__icon` | 42×42 icon 槽 |
| `.nav-item__label` | 32/38 Medium 文本节点 |
| `.is-active` / `[aria-current="page"]` | selected=true |
| `.nav-item--icon-only` | hasText=false（84×84，padding=0，gap=0） |

## CSS 变量（业务可覆盖）

```css
.nav-item {
  --nav-item-active-bg: var(--palette-blue-0);   /* 选中底色 */
  --nav-item-active-fg: var(--color-text-link);  /* 选中文字/icon */
}
```

可在祖先节点（`.app-frame` / `.side-menu` / `.side-menu__nav-group` 任一）覆盖到产品 brand：

```html
<aside class="side-menu"
       style="--nav-item-active-bg: var(--palette-orange-transparent);
              --nav-item-active-fg: var(--palette-orange-5);">
  ...
</aside>
```

## 用法

```html
<a class="nav-item is-active" href="#" aria-current="page">
  <span class="nav-item__icon" aria-hidden="true">
    <svg viewBox="0 0 48 48"><use href="#icon/list--fill"/></svg>
  </span>
  <span class="nav-item__label">列表</span>
</a>
```

折叠时加 `.nav-item--icon-only` 并去掉 `.nav-item__label`，把 `aria-label` 写到 `<a>` 上保留语义。

## 设计契约

- 折叠态 / 展开态由父级 [SideMenu](../side-menu/side-menu.md) 统一切换（`mode=collapsed/expanded`）。
- 同一 SideMenu 内 **同一时刻只有一个 nav-item 处于 selected**（业务保证）。
- 选中 nav-item 的 icon 形态约定：`selected=true` 走 `--fill`，`selected=false` 走 `--line`（Figma 真值）。
- 视觉真源唯一在本文件；SideMenu / AppFrame 不重写 item 样式。
