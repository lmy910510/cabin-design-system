# Rating-item · 评分子项

> Figma 节点 `301:8952`（COMPONENT_SET，3 个 variant）  
> 来源：Figma description 原文

评分子项组件，表示单颗星星，用于 Rating 组件内部。

## 属性

| 属性 | 类型 | 默认 | 说明 |
| --- | --- | --- | --- |
| `Style` | Variant | `Full` | `Full` / `Half` / `Solo` |

## 变体一览

| variant | 节点 ID | 外层 | 背景 | 内嵌 star |
| --- | --- | --- | --- | --- |
| Full | `301:8917` | 24×24，圆角 6 | `--palette-orange-5` `#fa6800` | 18×18，white-9 |
| Half | `301:8918` | 24×24，圆角 6 | `--palette-cold-gray-2` `#dee2e8` | 18×18，white-9 |
| Solo | `301:8951` | 24×24，无背景 | — | 24×24，orange-5 |

## 规格

- 统一外层尺寸 24×24
- Full / Half 圆角 6，作为"已评分/半评分"色块
- Solo 无底板，仅一颗实心橙星，作为"未评分"占位
- icon 来源：`icon.svg` 中 `#icon/star--fill`，颜色由 `currentColor` 继承

## 用法

```html
<span class="rating-item rating-item--full">
  <svg class="icon"><use href="#icon/star--fill"/></svg>
</span>
<span class="rating-item rating-item--half">
  <svg class="icon"><use href="#icon/star--fill"/></svg>
</span>
<span class="rating-item rating-item--solo">
  <svg class="icon"><use href="#icon/star--fill"/></svg>
</span>
```

依赖：`tokens.css` + `icon.css` + `icon-sprite-inline.js`（提供 `#icon/star--fill`）+ `rating-item.css`。

## 与 Rating 组件的关系

Rating-item 单独可用，但通常作为 [Rating](../rating/rating.md) 容器内部的星星单元。  
组合策略：4.5 分 = 4 Full + 1 Half；3 分 = 3 Full + 2 Solo（按设计意图选）。
