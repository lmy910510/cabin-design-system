# brand-tag

业务品牌标记组件。**双段组合 tag**：左段是品牌色实底 + 白字（或矢量 logo），右段是同色系文字（可带 icon），整体外层是同色系半透明底。

> Figma source: 🤖座舱组件库-AI版 / BrandTag (Component Set `594:9487`)

## 5 个 variant（与 Figma 一对一）

| variant | type 修饰类 | 左段 head | 右段 body | 颜色 token（复用语义色）|
|---|---|---|---|---|
| 车速取-车道 | `.brand-tag--warning` | "车速取" | "车道" | `--color-bg-warning` / `--color-text-warning` |
| 车速取-沿街 | `.brand-tag--warning` | "车速取" | "沿街" | 同上 |
| 可预约 | `.brand-tag--link` | "可预约" | "定时出餐 美味新鲜" | `--color-text-link`（实底 = blue-5）/ `--color-bg-link`（透明 12%）|
| 得来速 | `.brand-tag--price` | 矢量 logo（红字"得来速"）| "车道取餐 无须下车" | `--color-bg-price` / `--color-text-price` |
| 得来速-带 icon | `.brand-tag--price` | 同上 | 文字 + 22×22 info icon | 同上 |

## 颜色策略

**不新增 brand 色 token**，直接复用 design-system 现有语义色：

- 红 → `warning` 系列
- 蓝 → `link` 系列（蓝色没有独立的 success/notice 语义槽，用 link 复用）
- 黄 → `price` 系列（橙色，色相比 Figma 黄略偏暖，是预期内的差异）

好处：自动跟随 dark mode、不污染 token 体系、未来真要换品牌色一处改完。

## 结构

```html
<span class="brand-tag brand-tag--{warning|link|price}">
  <span class="brand-tag__head">左段</span>
  <span class="brand-tag__body">右段</span>
</span>
```

得来速变体的左段是矢量 logo（path data 直接来自 Figma vector `594:9506`）：

```html
<span class="brand-tag brand-tag--price">
  <span class="brand-tag__head brand-tag__head--logo">
    <svg class="brand-tag__logo" viewBox="0 0 74.86 22"><use href="#brand-drive-thru-logo" /></svg>
  </span>
  <span class="brand-tag__body">
    车道取餐 无须下车
    <svg class="brand-tag__icon" viewBox="0 0 48 48" aria-hidden="true">
      <use href="#icon/info--fill" />
    </svg>
  </span>
</span>
```

> 在使用页面里需要预先注入 `#brand-drive-thru-logo` 的 `<symbol>`（参考 `brand-tag.preview.html` 末尾的 `<svg><defs>` 段）。

## 与 Figma 的差异说明

1. **外层紫色 1px 描边**（`#8A38F5`）未保留——它是 Figma 编辑环境给变体加的外框示意，不是真实 UI。
2. **色值近似**——Figma 用的是裸色值（红 `#D62F35`、蓝 `#2965FF`、黄 `#FFDA38`），本实现复用语义 token，色值与 Figma 不完全一致是预期内的取舍（换暗色模式自动可用 > 像素级精确）。
3. **得来速 logo** 是 Figma vector 的 path data 直接内联，1:1 复刻。

## 与 `tag` 组件的关系

`brand-tag` 和现有 `tag` 是**完全独立的两个组件**，不要混用：
- `tag` 是单段、单底色、可选 icon 的通用语义标签（推荐 / 热销 / 5G ……）
- `brand-tag` 是双段、品牌业务专用（车速取 / 可预约 / 得来速）
