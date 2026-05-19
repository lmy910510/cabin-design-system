# line（分割线）

> 1:1 映射自 Figma「🤖座舱组件库-AI版 / Line」COMPONENT_SET `10:1333`

分割线组件，用于分隔内容区域，支持水平、垂直和虚线样式。**叶子组件，无任何子组件依赖。**

---

## 来源（Figma description 原文）

> 分割线组件，用于分隔内容区域，支持水平、垂直和虚线样式。
>
> **属性**
> Type: Primary | Secondary | Vertical | Dashed — 分割线类型
>
> **如何使用**
> 1. 拖入 Line 组件
> 2. 切换 Type 选择分割线类型
> 3. 水平分割线（Primary / Secondary / Dashed）宽度可自适应拉伸
> 4. 垂直分割线（Vertical）高度可自适应拉伸
>
> **变体**
> - Primary — 一级分割线，用于主要内容区域之间的分隔
> - Secondary — 二级分割线，用于次要内容之间的分隔
> - Vertical — 垂直分割线，用于水平排列元素之间的分隔
> - Dashed — 虚线，用于视觉较弱的分隔或边界提示
>
> **规格**
> - Primary / Secondary / Dashed 默认宽度 270，高度 0（纯线条）
> - Vertical 默认宽度 0，高度 60（纯线条）
> - 每个变体内部包含一个 Line VECTOR 子层

---

## Variant 矩阵

| Variant | Figma nodeId | 方向 | 描边样式 | 颜色（Figma 实测） | 对应语义 token |
| --- | --- | --- | --- | --- | --- |
| **Primary**   | `10:1342`     | 水平 | solid 1px | `#DEE2E8` = `cold-gray-2` | `--color-border-secondary` |
| **Secondary** | `393:17791`   | 水平 | solid 1px | `#EAEDF2` = `cold-gray-1` | `--color-border-tertiary`  |
| **Vertical**  | `10:1340`     | 垂直 | solid 1px | `#DEE2E8` = `cold-gray-2` | `--color-border-secondary` |
| **Dashed**    | `286:7774`    | 水平 | dashed 1px | `#DEE2E8` = `cold-gray-2` | `--color-border-secondary` |

### ⚠️ token 对照说明（关键，避免后续误用）

Figma 里 Line 的 **Primary** variant 实测绑定到 `VariableID:20:477`，该变量值为 `#DEE2E8`，等于 palette `cold-gray-2`。但我们语义层 `tokens.css` 的 `--color-border-primary` 实际指向 `cold-gray-3`（`#D0D5DC`，更深一档）——也就是说 **Figma Line/Primary 的视觉颜色 ≠ 语义 token `--color-border-primary` 的值**，反而等于 `--color-border-secondary` 的值。

我们这里 **以 Figma 实测颜色为准**，而不是按语义命名"对号入座"，以避免视觉漂移。具体取舍：

- `.line--primary` → `var(--color-border-secondary)` （颜色对齐 Figma 实测 #DEE2E8）
- `.line--secondary` → `var(--color-border-tertiary)` （颜色对齐 Figma 实测 #EAEDF2）

如果未来要把命名"扳正"，需要改的是 `tokens.css` 里 `--color-border-primary/secondary` 的指向，然后 line.css 跟随更新——这是 token 治理的活，不是组件层的活。

---

## 关键尺寸 / 颜色（Figma 真值）

| 项目 | 值 |
| --- | --- |
| 描边粗细 | 1px |
| 水平 Line 默认宽度 | 270（Figma 画板尺寸；实际 CSS 走 `width:100%`，由父容器决定）|
| 垂直 Line 默认高度 | 60（Figma 画板尺寸；实际 CSS 走 `height:100%`，由父容器决定）|
| Dashed 节奏 | 浏览器默认 dashed 节奏（约 4-4），Figma 上视觉一致 |

---

## 用法

```html
<!-- 水平 · Primary（最常用） -->
<hr class="line line--primary" />

<!-- 水平 · Secondary -->
<hr class="line line--secondary" />

<!-- 水平 · Dashed -->
<hr class="line line--dashed" />

<!-- 垂直 · 用在 toolbar / 并排数据之间 -->
<div style="display:flex; align-items:center; height:60px; gap:18px;">
  <span>32 公里</span>
  <span class="line line--vertical"></span>
  <span>预计 25 分钟</span>
</div>
```

> 推荐用 `<hr>` 标签作水平线，语义最准；垂直线没有原生标签，用 `<span>` 或 `<div>` 都可以。

---

## 使用指引（来自 Figma description）

- **Primary** 用于列表项之间、卡片内主要分区
- **Secondary** 用于次级分区，视觉层级低于 Primary
- **Vertical** 用于工具栏按钮分隔、并排数据分隔
- **Dashed** 用于占位提示、可选区域边界

---

## 自检清单

- [x] 4 个 variant 颜色 / 粗细 / 方向 与 Figma 1:1
- [x] 颜色全部使用语义 token，不引用 palette
- [x] token 对照说明已记录（避免命名歧义被后续修改时误判）
- [x] 水平线 `width:100%` 自适应父容器宽度
- [x] 垂直线 `height:100%` 自适应父容器高度
- [x] dark 模式自动切换（语义 token 已在 `:root[data-theme="dark"]` 下重定义）

---

## 与上层组件的关系

| 上层使用方 | 用法 |
| --- | --- |
| `list`         | 列表项 `<li>` 之间用 `.line.line--primary` 隔开 |
| 卡片内分区     | 卡片标题与内容之间用 `.line.line--secondary` |
| `top-bar` 等工具栏 | 并排操作之间用 `.line.line--vertical` |
| 占位 / 拖拽提示 | 用 `.line.line--dashed` 标记可放置区 |

---

## 变更日志

| 日期 | 变更 | 说明 |
| --- | --- | --- |
| 2026-05-19 | 首版 | 4 个 variant 全部实现；token 对照说明记录 Figma 实测颜色与语义 token 的偏移 |
