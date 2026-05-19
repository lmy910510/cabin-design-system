# SideMenu · 侧栏导航

> Figma 节点 `436:1770`（COMPONENT_SET，2 variant：`mode = expanded | collapsed`）。  
> 含 SLOT 属性 `nav-list`；结构包含 `nav-group`（导航列表） + `footer`（底部固定区）两块。

> ⚠️ **待与 Figma 对齐**：Figma 当前未提供 description / Annotations，本实现的宽度 / padding / 间距按 `design-system` token + 与 TopBar 视觉节奏推导。结构与 variant 维度已对齐，规格待 Figma 补充后再精确回写。

## 属性 / 变体

| 属性 | 类型 | 默认 | 说明 |
|---|---|---|---|
| nav-list | SLOT | 默认 3+1 项 | 导航列表插槽（nav-group + footer） |
| mode | VARIANT | expanded | `expanded` \| `collapsed` |

## 视觉真值（暂定）

| 项 | expanded | collapsed |
|---|---|---|
| 宽度 | 360 | 108 |
| padding | 24 / 24 | 24 / 24 |
| gap（nav-group ↔ footer） | 12 | 12 |
| 背景 | `--color-bg-page-secondary` | 同 |
| 子项 | `.side-menu-item`（hasText=true） | `.side-menu-item.is-collapsed`（hasText=false） |

## 与 SideMenu/Item 的关系

```
SideMenu (容器，VERTICAL space-between)
├─ nav-group   （SLOT，列出 SideMenu/Item 实例）
│   └─ side-menu-item × N
└─ footer      （底部固定 SideMenu/Item 实例）
    └─ side-menu-item × M
```

子项视觉真源在 [`SideMenu/Item`](../side-menu-item/side-menu-item.md)。本组件不重写 item 视觉。

## 类名

| 类名 | 作用 |
|---|---|
| `.side-menu` | 容器（aside） |
| `.side-menu__nav-group` | 顶部导航 stack |
| `.side-menu__footer` | 底部固定 stack |
| `.is-collapsed` / `[data-mode="collapsed"]` | 折叠态（mode=collapsed） |

## 用法

```html
<aside class="side-menu" aria-label="主导航">
  <div class="side-menu__nav-group">
    <button class="side-menu-item is-selected" aria-current="page">…</button>
    <button class="side-menu-item">…</button>
  </div>
  <div class="side-menu__footer">
    <button class="side-menu-item">…</button>
  </div>
</aside>
```

切换折叠：在 `.side-menu` 上加 `.is-collapsed`，**同时**把所有 `.side-menu-item` 加 `.is-collapsed` —— 这是当前的简化约定，等 Figma 补描述后可改为容器单类自动收敛。

## 设计契约

- 同一 SideMenu 内 **同一时刻只有一个 side-menu-item 处于 selected**。
- 折叠态由父容器统一控制，不应该让 item 自己感知"是否折叠"。
- 视觉真源唯一在本文件（容器） + side-menu-item.css（子项）。
