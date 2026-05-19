# Rating · 评分组件

> Figma 节点 `301:9051`（COMPONENT_SET，2 个 variant）  
> 来源：Figma description 原文

评分组件，用于展示评分结果，支持多星和单星两种展示模式。

## 属性

| 属性 | 类型 | 默认 | 说明 |
| --- | --- | --- | --- |
| `Type` | Variant | `Stars` | `Stars` / `Single` |
| `Show Score` | Boolean | `true` | 是否显示分数文本 |

## 变体一览

| variant | 节点 ID | layout | 内容 |
| --- | --- | --- | --- |
| Stars | `301:9021` | HORIZONTAL，gap 6 | Stars 容器（5 颗 Rating-item，gap 3）+ Score 24px |
| Single | `301:9052` | HORIZONTAL，gap 3 | 1 颗 Rating-item（Solo 默认）+ Score 24px |

## 规格

- Score 文本：DIN Medium 24px / line-height 24px，颜色 `--color-text-price` = `--palette-orange-5` = `#fa6800`
- Rating-item 单颗 24×24（视觉真源已抽至 [rating-item](../rating-item/rating-item.md)）
- Stars 内 5 颗星 gap=3，外层 gap=6（用于隔开 Stars 与 Score）
- Single 1 颗星 + Score gap=3

## 用法

```html
<!-- 4.5 分（多星） -->
<span class="rating rating--stars">
  <span class="rating__stars">
    <span class="rating-item rating-item--full"><svg class="icon"><use href="#icon/star--fill"/></svg></span>
    <span class="rating-item rating-item--full"><svg class="icon"><use href="#icon/star--fill"/></svg></span>
    <span class="rating-item rating-item--full"><svg class="icon"><use href="#icon/star--fill"/></svg></span>
    <span class="rating-item rating-item--full"><svg class="icon"><use href="#icon/star--fill"/></svg></span>
    <span class="rating-item rating-item--half"><svg class="icon"><use href="#icon/star--fill"/></svg></span>
  </span>
  <span class="rating__score">4.5</span>
</span>

<!-- 紧凑单星 -->
<span class="rating rating--single">
  <span class="rating-item rating-item--solo"><svg class="icon"><use href="#icon/star--fill"/></svg></span>
  <span class="rating__score">4.8</span>
</span>

<!-- 隐藏分数（Show Score=false） -->
<span class="rating rating--stars">
  <span class="rating__stars">…</span>
  <!-- 删除 .rating__score 节点即可 -->
</span>
```

依赖：`tokens.css` + `icon.css` + `icon-sprite-inline.js` + `rating-item.css` + `rating.css`。

## 组合规则（评分 → 星组合）

| 评分 | Full | Half | Solo |
| --- | --- | --- | --- |
| 5.0 | 5 | 0 | 0 |
| 4.5 | 4 | 1 | 0 |
| 4.0 | 4 | 0 | 1 |
| 3.5 | 3 | 1 | 1 |
| 3.0 | 3 | 0 | 2 |
| 0.0 | 0 | 0 | 5（Solo 占位） |
