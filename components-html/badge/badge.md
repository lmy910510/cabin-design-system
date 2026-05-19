# badge（徽标）

> 1:1 映射自 Figma「🤖座舱组件库-AI版 / Badge」COMPONENT_SET `299:8722`

徽标组件，用于信息提示、状态标记或数字计数展示。**叶子组件，无子组件依赖。**

---

## 来源（Figma description 原文）

> 徽标组件，用于信息提示、状态标记或数字计数展示。
>
> **属性**
> type: circle | ribbon — 徽标类型
>
> **如何使用**
> 1. 拖入 Badge 组件
> 2. 切换 type 选择徽标类型
> 3. ribbon 类型可修改 text 文本层更改显示内容
>
> **变体**
> - circle — 圆点徽标，12x12 红色小圆点，用于无数字的新消息/未读提示
> - ribbon — 文字徽标，胶囊形背景 + 数字文本，用于显示具体数量
>
> **规格**
> - circle — 尺寸 12x12，dot 圆角 6（全圆）
> - ribbon — 布局 HORIZONTAL，内边距 左右 12，圆角 18（胶囊形），文本 text 字号 24px
>
> **使用指引**
> - circle 用于图标右上角、列表项等需要简洁提示的场景
> - ribbon 用于显示未读消息数、购物车数量等需要具体数字的场景
> - ribbon 文本超过 99 时建议显示为 "99+"
> - 通常放置在宿主元素的右上角

---

## Variant 矩阵

| Variant | Figma nodeId | 尺寸 | 圆角 | 背景 (Figma 实测) | 语义 token | 文本 |
| --- | --- | --- | --- | --- | --- | --- |
| **circle** | `299:8723` | 12 × 12       | 6（全圆）  | `#FF293B` (red-5) | `--color-bg-warning` | 无 |
| **ribbon** | `299:8725` | 高 30，宽 hug | 18（胶囊） | `#FF293B` (red-5) | `--color-bg-warning` | 24px DIN Medium，white-9（`--color-text-primary-inverse`） |

> 两个 variant 颜色完全一致，只有形态/尺寸差异。

---

## 关键尺寸（Figma 真值）

| 项 | circle | ribbon |
| --- | --- | --- |
| 尺寸 | 12×12 | 高 30，min-width 30，宽 hug |
| 圆角 | 6（全圆） | 18（胶囊） |
| 内边距 | 0 | 0 / 12 |
| 字体 | – | DIN Medium 24 / 24 |
| 字色 | – | white-9（半透明 90% 白） |
| 背景色 | red-5 | red-5 |

---

## 用法

```html
<!-- 圆点徽标 -->
<span class="badge badge--circle" aria-hidden="true"></span>

<!-- 数字徽标 -->
<span class="badge badge--ribbon">3</span>
<span class="badge badge--ribbon">99+</span>

<!-- 嵌在标题旁 -->
<div style="display:inline-flex; align-items:flex-start; gap:6px;">
  <span class="title">系统更新</span>
  <span class="badge badge--circle" aria-hidden="true"></span>
</div>

<!-- 嵌在图标右上 -->
<span class="badge-host">
  <svg class="icon">…</svg>
  <span class="badge badge--ribbon">5</span>
</span>
```

> ribbon 文本 > 99 时按 Figma 指引显示 `99+`。

---

## 与上层组件的关系

| 上层使用方 | 用法 |
| --- | --- |
| `list-content` | 标题右上角圆点徽标（`Show Badge` boolean），渲染 `.badge.badge--circle` |
| `top-bar` 通知按钮 | 通知 icon 右上角 ribbon，渲染 `.badge.badge--ribbon` |
| `tab-item`        | 后续可在标签上叠加 ribbon 显示未读数 |
| 卡片 / 头像       | 头像右上角圆点（在线状态）—— 视觉同 circle，颜色后续可扩 variant |

---

## 自检清单

- [x] 2 个 variant 颜色 / 尺寸 / 圆角 与 Figma 1:1
- [x] 颜色全部使用语义 token（`--color-bg-warning` / `--color-text-primary-inverse`）
- [x] 不写死 hex（之前 list-content 内联用了 `--palette-red-5`，已在 list-content 同步迁移）
- [x] dark 模式下视觉自动跟随（背景红色不变，文字白→深色翻转）
- [x] flex 容器中不被压扁（`flex-shrink: 0`）
- [x] ribbon 字体 fallback：DIN → Inter → 系统等宽数字字体

---

## 同步改造记录

Badge 落地的同时，以下文件**已同步**从内联红点迁移到本组件，消除分叉：

| 路径 | 改动 |
| --- | --- |
| `list-content/list-content.css` | 删除 `.list-content__badge` 样式块（保留兼容 selector，统一指向 `.badge.badge--circle`） |
| `list-content/list-content.html` | `<span class="list-content__badge">` → `<span class="badge badge--circle">` |
| `list-content/list-content.preview.html` | 同上 |
| `list-content/list-content.md` | 文档说明同步 |
| `list/list.html` | 同上 |
| `list/list.preview.html` | 同上 |

---

## 变更日志

| 日期 | 变更 | 说明 |
| --- | --- | --- |
| 2026-05-19 | 首版 | 2 个 variant 实现；同步迁移 list-content 内联红点到 `.badge.badge--circle`，消除样式分叉 |
