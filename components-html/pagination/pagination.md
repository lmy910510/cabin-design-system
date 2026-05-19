# Pagination · 分页器

> Figma 节点 `458:42138`（COMPONENT，单态）。  
> 由 [`Pagination-prev`](../pagination-prev/pagination-prev.md) + [`Pagination-next`](../pagination-next/pagination-next.md) 横向组合而成的胶囊形分页器。

## 结构

```
Pagination (胶囊壳 + 阴影)
├─ Pagination-prev   （子组件实例，Disabled toggle）
└─ Pagination-next   （子组件实例，Disabled toggle）
```

## 视觉真值（来自 Figma）

| 项 | 值 |
|---|---|
| layout | HORIZONTAL，gap 6 |
| padding | 0 / 6（上下 / 左右） |
| 背景 | `#FFFFFF` ≈ `--color-bg-card-primary` |
| 圆角 | 999（胶囊形） |
| 阴影 | `Shadow/Medium` |
| 箭头容器 | 72 × 72 |
| 箭头图标 | 36 × 36 |

## 类名

| 类名 | 作用 |
|---|---|
| `.pagination` | 胶囊容器，承载 prev / next |
| `.pagination-prev` / `.pagination-next` | 子组件类（视觉真源在各自 css） |

## 用法

```html
<nav class="pagination" aria-label="分页">
  <button type="button" class="pagination-prev" aria-label="上一页">
    <span class="pagination-prev__icon" aria-hidden="true">
      <svg><use href="../icon/icon.svg#icon/back--fill"/></svg>
    </span>
  </button>
  <button type="button" class="pagination-next" aria-label="下一页">
    <span class="pagination-next__icon" aria-hidden="true">
      <svg><use href="../icon/icon.svg#icon/enter--fill"/></svg>
    </span>
  </button>
</nav>
```

- 首页：在 `.pagination-prev` 上加 `disabled`
- 末页：在 `.pagination-next` 上加 `disabled`

## 设计契约

- 真源唯一：胶囊壳样式来自本文件，箭头视觉来自子组件文件。
- 不在 `.pagination` 内重写 `.pagination-prev` / `.pagination-next`。
- 当前为基础样式；如未来扩展（数字页码 / 页大小切换），升级为 COMPONENT_SET 并在 md 增 variant 段。
