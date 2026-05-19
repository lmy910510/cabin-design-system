# brand-tag

业务品牌标记组件。**双段组合 tag**：左段是品牌色实底固定宽方块 + 白字（或矢量 logo），右段是同色系文字（可带 icon），整体外层是同色系半透明底。

> Figma source: 🤖座舱组件库-AI版 / BrandTag (Component Set `594:9487`)

## Figma 实测尺寸（这一版严格 1:1）

| 项 | 值 |
|---|---|
| 整体高度 | **36px** |
| 外层圆角 | **6px** |
| 左段尺寸 | 车速取-车道 **顶 90 / 底 82**（斜切 8px）<br>车速取-沿街 **顶 92 / 底 82**（斜切 10px）<br>可预约 **顶 92 / 底 82**（斜切 10px）<br>得来速 **顶 96 / 底 85**（斜切 11px） |
| 左右段间距（gap） | 红/蓝 **4px**，黄 **3px** |
| 右段右内边距 | **8px** |
| 左段字 | MFYuanHei 22 / 22，白 92% |
| 右段字 | Noto Sans SC 20 / 20 |

> **左段是斜切平行四边形**（右下角往左收 8~11px），不是矩形。这是 brand-tag 区别于普通 tag 的核心视觉特征。
> 实现：通过 `clip-path: polygon(0 0, W 0, (W-S) 100%, 0 100%)` 一比一切出，跟 Figma 里 `Rectangle 244_15` 这条 vector 的路径数据完全一致：
> `M 0 0  L W 0  L (W-S) H  L 0 H  Z`。
> 斜出来的那块三角区是**外层 6%/12% 半透明底色**，是设计意图——刻意做出"贴标"的撕裂感。

## 5 个 variant（与 Figma 一对一）

| variant | type 修饰类 | 左段 head | 右段 body | head/body 颜色 | 外层底色 |
|---|---|---|---|---|---|
| 车速取-车道 | `.brand-tag--warning .brand-tag--lane` | "车速取" | "车道" | `#D62F35` | `rgba(214,47,53,0.06)` |
| 车速取-沿街 | `.brand-tag--warning` | "车速取" | "沿街" | `#D62F35` | `rgba(214,47,53,0.06)` |
| 可预约 | `.brand-tag--link` | "可预约" | "定时出餐 美味新鲜" | `#2965FF` | `rgba(41,101,255,0.06)` |
| 得来速 | `.brand-tag--price` | 矢量 logo（红字"得来速"）| "车道取餐 无须下车" | head=`#FFDA38` / body=`#8A6000` | `rgba(255,218,56,0.12)` |
| 得来速-带 icon | `.brand-tag--price` | 同上 | 文字 + 22×22 info icon（来自通用 sprite `#icon/info--line`，48×48 viewBox 缩放至 22） | 同上 | 同上 |

## 颜色策略（v2：按 Figma 裸色还原）

之前用语义 token（`--color-bg-warning` / `--color-text-link` / `--color-bg-price`）近似的方案有两个明显偏差：

1. 得来速本应是**亮黄 `#FFDA38`**，被错误映射到 `--color-bg-price` 的橙色 `#fa6800`，色相偏差很大。
2. 红/蓝也跟 Figma 裸色（`#D62F35` / `#2965FF`）有可见的色相差。

本组件是**业务品牌色**——它本来就不该被语义 token 牵着走。所以这次直接把三套颜色硬编码进 brand-tag，与 design-system 解耦：

- 红 `#D62F35`（车速取）
- 蓝 `#2965FF`（可预约）
- 黄 `#FFDA38` + 深棕黄 `#8A6000`（得来速）

代价：**brand-tag 不再随 dark mode 自动切色**（业务品牌色本就不该跟随明暗模式切换）。

## 结构

```html
<!-- 车速取-车道：左段 90px，需要 brand-tag--lane -->
<span class="brand-tag brand-tag--warning brand-tag--lane">
  <span class="brand-tag__head">车速取</span>
  <span class="brand-tag__body">车道</span>
</span>

<!-- 车速取-沿街 / 可预约：左段 92px，默认值 -->
<span class="brand-tag brand-tag--warning">
  <span class="brand-tag__head">车速取</span>
  <span class="brand-tag__body">沿街</span>
</span>

<span class="brand-tag brand-tag--link">
  <span class="brand-tag__head">可预约</span>
  <span class="brand-tag__body">定时出餐 美味新鲜</span>
</span>
```

得来速变体的左段是矢量 logo（path data 直接来自 Figma vector `594:9506`，宽 96px）：

```html
<span class="brand-tag brand-tag--price">
  <span class="brand-tag__head brand-tag__head--logo">
    <svg class="brand-tag__logo" viewBox="0 0 74.86 22"><use href="#brand-drive-thru-logo" /></svg>
  </span>
  <span class="brand-tag__body">
    车道取餐 无须下车
    <svg class="brand-tag__icon" viewBox="0 0 48 48" aria-hidden="true">
      <use href="#icon/info--line" />
    </svg>
  </span>
</span>
```

> 在使用页面里需要：
> 1. 注入得来速 logo `<symbol id="brand-drive-thru-logo">`（brand-tag 专属资产，参考 `brand-tag.preview.html` 末尾的 `<svg><defs>` 段）
> 2. 引入通用 icon sprite：`<script src="../icon/icon-sprite-inline.js"></script>`（提供 `#icon/info--line`）
>
> info icon 走通用 sprite（`components-html/icon/`）的**线形（`--line`）** variant——圆环 + 实心 i，跟 brand-tag 的"提示性"语义匹配（线形偏中性提示，fill 偏强提醒）。48×48 viewBox 通过 `width/height: 22px` 等比缩放到 brand-tag 的 22×22 槽位。颜色由父级 body 文字色 `#8A6000` 通过 `currentColor` 继承。

## 与 Figma 的差异说明

1. **外层紫色 1px 描边**（`#8A38F5`）未保留——它是 Figma 编辑环境给变体加的外框示意，不是真实 UI。
2. **得来速 logo** 是 Figma vector 的 path data 直接内联，1:1 复刻，logo path 的红色填充用的是 Figma 实测 `#D32D20`。

## 与 `tag` 组件的关系

`brand-tag` 和现有 `tag` 是**完全独立的两个组件**，不要混用：
- `tag` 是单段、单底色、可选 icon 的通用语义标签（推荐 / 热销 / 5G ……），跟随语义 token 与 dark mode
- `brand-tag` 是双段、品牌业务专用（车速取 / 可预约 / 得来速），用裸色硬编码，不跟随主题
