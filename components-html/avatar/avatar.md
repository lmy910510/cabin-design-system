# avatar（头像）

> 1:1 映射自 Figma「🤖座舱组件库-AI版 / avatar」COMPONENT_SET `360:16621`

头像组件，用于展示用户或实体的头像图片，支持四种尺寸。**叶子组件，无子组件依赖。**

---

## 来源（Figma description 原文）

> 头像组件，用于展示用户或实体的头像图片，支持多种尺寸。
>
> **属性**
> Size: XS | SM | MD | LG — 头像尺寸
>
> **如何使用**
> 1. 拖入 avatar 组件
> 2. 切换 Size 选择合适的尺寸
> 3. 选中组件后在右侧 Fill 区域替换头像图片
>
> **变体**
> - XS — 48×48，适用于列表项、紧凑布局中的小头像
> - SM — 72×72，适用于评论区、消息列表等场景
> - MD — 84×84，适用于卡片、联系人列表等中等展示场景
> - LG — 150×150，适用于个人主页、资料详情等大头像展示
>
> **规格**
> - 圆形裁切（cornerRadius 999）
> - 头像通过 IMAGE Fill 实现，支持替换任意图片
> - 无子图层，组件本身即为头像容器
>
> **使用指引**
> - 根据场景选择合适尺寸，保持同一场景内头像尺寸一致
> - 建议搭配用户名文本使用，增强可识别性
> - 在列表中使用 XS 或 SM，在详情页使用 MD 或 LG
>
> **结构**
> - avatar — 圆形容器，IMAGE Fill
> - 无子图层，头像图片通过 Fill 填充

---

## Variant 矩阵

| Variant | Figma nodeId | 尺寸 | cornerRadius | fill | 适用场景 |
| --- | --- | --- | --- | --- | --- |
| **XS** | `360:16617` | 48×48   | 999（圆） | IMAGE FILL | 列表项、紧凑布局 |
| **SM** | `360:16618` | 72×72   | 999       | IMAGE FILL | 评论、消息列表 |
| **MD** | `360:16619` | 84×84   | 999       | IMAGE FILL | 卡片、联系人 |
| **LG** | `360:16620` | 150×150 | 999       | IMAGE FILL | 个人主页、详情页 |

> 4 个 variant **只有尺寸差异**，没有任何颜色 / 描边 / 阴影差异。

---

## 关键尺寸 / 规格（Figma 真值）

| 项目 | 值 |
| --- | --- |
| 形状 | 圆形（cornerRadius 999 ≡ `border-radius:50%`）|
| 描边 | 无 |
| 阴影 | 无 |
| 填充模式 | IMAGE FILL，scaleMode = FILL（等价 CSS `object-fit: cover`）|
| 裁切 | clipsContent = true（CSS 上由 `border-radius` + `overflow:hidden` 共同保证）|

---

## 用法

```html
<!-- 推荐：<img> 直接承载，alt 必填 -->
<img class="avatar avatar--xs" src="..." alt="张三" />
<img class="avatar avatar--sm" src="..." alt="张三" />
<img class="avatar avatar--md" src="..." alt="张三" />
<img class="avatar avatar--lg" src="..." alt="张三" />

<!-- 占位（工程兜底，Figma 无此 variant）-->
<span class="avatar avatar--md avatar--placeholder">张</span>
```

---

## 使用指引（来自 Figma description）

- 同一场景下统一尺寸，不要混用 SM 与 MD。
- 列表里用 **XS / SM**；卡片或联系人用 **MD**；详情页用 **LG**。
- 头像旁通常跟用户名文本，避免单独头像无上下文。

---

## 占位（工程补充）

Figma 没有"无图"状态，但实际工程一定会遇到。`.avatar--placeholder` 作为兜底：

- 圆形 + 灰底 + 居中字符（如取用户名首字）
- 尺寸完全跟随 `.avatar--xs/sm/md/lg`，**不破坏布局**
- 字号已按 4 个尺寸调好（18 / 28 / 32 / 56），保持视觉重量一致
- 颜色用语义 token：背景 `--color-bg-item-secondary`，字色 `--color-text-tertiary`

---

## 自检清单

- [x] 4 个尺寸 1:1 对齐 Figma（48 / 72 / 84 / 150）
- [x] 圆形裁切（border-radius:50%）
- [x] 图像填充模式 = cover，对齐 Figma scaleMode FILL
- [x] 无图占位状态（工程补充）已实现，尺寸字号匹配
- [x] 在 flex 容器中不会被压扁（`flex-shrink:0`）
- [x] dark 模式下背景兜底使用语义 token，自动跟随主题

---

## 与上层组件的关系

| 上层使用方 | 用法 |
| --- | --- |
| `list-leading` | 槽内嵌 `<img class="avatar avatar--xs">`，60×60 槽位中居中（XS 留 6px 内边） |
| 卡片头部       | `<img class="avatar avatar--md">` + 用户名 + 副标题，标准联系人布局 |
| 详情页 / 个人主页 | `<img class="avatar avatar--lg">` 居中放置 |
| 评论 / 消息    | `<img class="avatar avatar--sm">` + 文本气泡 |

---

## 变更日志

| 日期 | 变更 | 说明 |
| --- | --- | --- |
| 2026-05-19 | 首版 | 4 个尺寸 variant 实现；新增工程兜底 `--placeholder` 状态 |
