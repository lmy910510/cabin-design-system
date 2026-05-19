# Pagination-next · 后翻页箭头

> Figma 节点 `458:41275`（COMPONENT_SET，2 variant）。Pagination 主组件子项。

## 属性 / 变体

| Variant | Disabled | 颜色 |
|---|---|---|
| 默认 | False | `--color-text-primary` |
| 禁用 | True | `--color-text-disabled` |

## 视觉真值

| 项 | 值 |
|---|---|
| 容器 | 72 × 72 |
| 图标 | icon/enter，36 × 36 |

## 类名

| 类名 | 作用 |
|---|---|
| `.pagination-next` | 容器（按钮） |
| `.pagination-next__icon` | 36×36 图标槽 |

## 用法

```html
<button type="button" class="pagination-next" aria-label="下一页">
  <span class="pagination-next__icon" aria-hidden="true">
    <svg><use href="../icon/icon.svg#icon/enter--fill"/></svg>
  </span>
</button>
```

与 [`Pagination-prev`](../pagination-prev/pagination-prev.md) 视觉规则完全对称。
