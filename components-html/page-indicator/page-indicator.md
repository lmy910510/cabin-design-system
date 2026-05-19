# PageIndicator · 页面指示器

> Figma 节点 `325:13051`（COMPONENT，含 SLOT 属性 `PageIndicator-list`）。  
> 用于轮播图、引导页底部的当前页码指示，由多个 [`PageIndicator-item`](../page-indicator-item/page-indicator-item.md) 组成。

## 属性

| 属性 | 类型 | 默认 | 说明 |
|---|---|---|---|
| PageIndicator-list | SLOT | 4 个 PageIndicator-item（1 Active + 3 Default） | 指示点插槽，可任意增减 |

## 视觉真值（来自 Figma）

| 项 | 值 |
|---|---|
| 外层 layout | HORIZONTAL，gap = 0 |
| SLOT layout | HORIZONTAL，gap = **12** |
| 默认子项数量 | 4 |
| 默认尺寸 | 84 × 12 |
| 容器视觉 | 无 —— layout 壳 |

## 与 PageIndicator-item 的关系

```
PageIndicator (容器，HORIZONTAL gap=0)
└─ PageIndicator-list (SLOT, HORIZONTAL gap=12)
   └─ PageIndicator-item × N （Active / Default）
```

子项视觉真源：[`PageIndicator-item`](../page-indicator-item/page-indicator-item.md)。本组件不重写子项，避免分叉。

## 类名

| 类名 | 作用 |
|---|---|
| `.page-indicator__list` | 推荐：SLOT 容器 |
| `.pi-list` | 兼容旧调用方，等同 `.page-indicator__list` |

## 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../page-indicator-item/page-indicator-item.css" />
<link rel="stylesheet" href="./page-indicator.css" />
```

## 最小代码

```html
<ul class="page-indicator__list" role="tablist" aria-label="页码">
  <li><span class="pi-item" data-state="active" aria-current="true"></span></li>
  <li><span class="pi-item" data-state="default"></span></li>
  <li><span class="pi-item" data-state="default"></span></li>
  <li><span class="pi-item" data-state="default"></span></li>
</ul>
```

## 行为约定

- **同一时刻只有一个 pi-item 为 active**。
- 指示点数量与页面数量保持一致；建议 ≤ 8 点，更多时考虑数字页码。
- 居中放置在内容区域下方。
- 容器自身不负责切换逻辑，由轮播 / Swiper 业务方控制 active 切换。

## 历史改动锚点

- 容器 css 真源已**从 `page-indicator-item.css` 迁移到本文件**（消除分叉）。原文件中 `.pi-list` 已删除；新调用方请同时引入本文件。
