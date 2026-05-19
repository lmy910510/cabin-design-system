# Pagination-prev · 前翻页箭头

> Figma 节点 `458:41252`（COMPONENT_SET，2 variant）。  
> Pagination 主组件的子项，单独不建议使用。

## 属性 / 变体

| Variant | Disabled | 颜色 |
|---|---|---|
| 默认 | False | `#1F2229` ≈ `--color-text-primary` |
| 禁用 | True | `#D0D5DC` ≈ `--color-text-disabled` |

## 视觉真值

| 项 | 值 |
|---|---|
| 容器 | 72 × 72，居中布局 |
| 图标 | icon/back，36 × 36 |
| 背景 / 圆角 | 无 |

## 类名

| 类名 | 作用 |
|---|---|
| `.pagination-prev` | 容器（按钮） |
| `.pagination-prev__icon` | 36×36 图标槽 |
| `[disabled]` / `.is-disabled` | 禁用态（Disabled=True） |

## 用法

```html
<button type="button" class="pagination-prev" aria-label="上一页">
  <span class="pagination-prev__icon" aria-hidden="true">
    <svg><use href="../icon/icon.svg#icon/back--fill"/></svg>
  </span>
</button>
```

禁用：在 `<button>` 上加 `disabled`，颜色自动切换为 `--color-text-disabled`。

## 设计契约

- 真源唯一：颜色 / 尺寸来自本文件，Pagination 不重写。
- 不带圆角 / 背景：与 Figma 一致；Pagination 容器自身的胶囊背景由 `.pagination` 提供。
