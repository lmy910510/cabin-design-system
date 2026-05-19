# SideMenu / Item · 侧栏导航项

> Figma 节点 `459:2790`（COMPONENT_SET，4 variant）。SideMenu 的子项。

> ⚠️ **待与 Figma 对齐**：Figma 当前未提供 description / Annotations，本实现按 `design-system` token + 与 TopBar / Tab-item 视觉节奏推导。待 Figma 补充详细规格后做精确回写（结构与 variant 维度已对齐）。

## 属性 / 变体（4 组合）

| selected | hasText | 视觉 |
|---|---|---|
| true | true | 背景 = `--color-bg-item-primary`，文字反色，icon + label |
| false | true | 透明底，主文字色，icon + label |
| true | false | 背景 = `--color-bg-item-primary`，仅 icon（折叠态） |
| false | false | 透明底，仅 icon（折叠态） |

## 视觉真值（暂定）

| 项 | 值 |
|---|---|
| 容器 | HORIZONTAL，gap 24，padding 24/30，圆角 24 |
| icon 槽 | 60 × 60（与 list-leading 一致） |
| icon 本体 | 36 × 36 sprite |
| label 文字 | 28 / 36 Medium |
| selected 背景 | `--color-bg-item-primary` |
| selected 文字 | `--color-text-primary-inverse` |
| 折叠态 padding | 24 四周（label 隐藏，宽度收缩） |

## 类名

| 类名 | 作用 |
|---|---|
| `.side-menu-item` | 容器（按钮） |
| `.side-menu-item__icon` | 60×60 icon 槽 |
| `.side-menu-item__label` | 文本节点 |
| `.is-selected` / `[aria-current="page"]` / `[data-selected="true"]` | selected=true |
| `.is-collapsed` / `[data-has-text="false"]` | hasText=false（折叠态，label 隐藏） |

## 用法

```html
<button type="button" class="side-menu-item is-selected" aria-current="page">
  <span class="side-menu-item__icon" aria-hidden="true">
    <svg><use href="../icon/icon.svg#icon/star--fill"/></svg>
  </span>
  <span class="side-menu-item__label">收藏</span>
</button>
```

折叠时加 `.is-collapsed`，并把 `aria-label` 写到 button 上以保留语义。

## 设计契约

- 折叠态 / 展开态由父级 [SideMenu](../side-menu/side-menu.md) 统一切换（`mode=collapsed/expanded` variant）。
- 同一 SideMenu 内 **同一时刻只有一个 side-menu-item 处于 selected**（业务保证）。
- 视觉真源唯一在本文件；SideMenu 不重写 item 样式。
