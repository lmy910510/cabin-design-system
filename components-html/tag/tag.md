# tag（标签）

> 1:1 映射自 Figma「🤖座舱组件库-AI版 / Tag」Component Set `277:116`

用于分类标记、属性标注、状态展示。叶子组件，无任何子组件依赖。

---

## Variant 矩阵

| Size × Type | Filled（强调） | Outlined（次要） | Tinted（柔和，默认） |
| ----------- | -------------- | --------------- | ------------------ |
| **Large** (h=36)  | 实底深色 + 反白文字 | 透明底 + 1px 描边 + 二级文字 | 浅灰底 + 主文字 |
| **Medium** (h=30) | 同上                | 同上                          | 同上            |

布尔属性：`Show icon`（默认 true）→ 是否渲染前置 24×24 SVG（HTML 端按需写 `.tag__icon` 子节点）。

---

## 关键尺寸 / 颜色（Figma 真值）

| 项目          | 值                                                            |
| ------------- | ------------------------------------------------------------- |
| 高度（Large） | 36                                                            |
| 高度（Medium）| 30                                                            |
| Padding       | 0 6                                                           |
| 圆角          | 6                                                             |
| 内部 gap      | 3                                                             |
| 字号 / 行高   | 20 / 20 Regular（Noto Sans SC）                               |
| icon 尺寸     | 24 × 24 容器；svg 内部 `viewBox="0 0 48 48"` + `fill=currentColor`，矢量真源由独立 [`icon`](../icon/icon.md) 组件 sprite 提供 |
| Filled bg     | `--color-bg-item-primary`（深色块）                           |
| Filled text   | `--color-text-primary-inverse`（反白）                        |
| Outlined bd   | 1px `--color-border-secondary`                                |
| Outlined text | `--color-text-secondary`                                      |
| Tinted bg     | `--color-bg-item-quaternary` = `#f7f8fa`（浅灰，dark 自动反相）|
| Tinted text   | `--color-text-primary`                                        |

> ⚠️ 旧版 `list-content.css` 内联 tag 写死了 `--palette-cold-gray-0` 作为 Tinted 底色，在 dark 模式下不会反相。本版本改为语义 token，dark 模式正确切换。`list-content` 已回填指向本组件，分叉消除。

---

## 用法

```html
<!-- 默认（= Large + Tinted） -->
<span class="tag">标签文案</span>

<!-- 显式 Large/Medium + Filled/Outlined/Tinted -->
<span class="tag tag--large tag--filled">推荐</span>
<span class="tag tag--medium tag--outlined">筛选</span>

<!-- 带前置 icon（图标视觉真源 = 独立 icon 组件 sprite） -->
<span class="tag tag--large tag--tinted">
  <svg class="tag__icon" viewBox="0 0 48 48" fill="currentColor" aria-hidden="true">
    <use href="#icon/star--fill" />
  </svg>
  <span class="tag__label">收藏</span>
</span>
```

---

## 使用指引（来自 Figma description）

- Filled — 适合高优先级或强提示标签（"推荐"、"热销"）
- Outlined — 适合次要信息或筛选条件（"全部"、"已下架"）
- Tinted — 适合分类标签或属性展示（"5G"、"自动"）
- 图标默认显示，无需前置图标时不写 `.tag__icon` 即可

---

## 自检清单

- [x] 6 个 variant 高度 / padding / gap / 圆角 与 Figma 1:1
- [x] 文字 20/20 Regular，与 `.t-cn-20-regular` 同源
- [x] 颜色全部使用语义 token（`--color-bg-*` / `--color-text-*` / `--color-border-*`），不引用 palette
- [x] 前置 icon 24×24，颜色 `currentColor`，跟随 type 自动反相
- [x] 容器自身可单独使用（叶子组件），无子组件依赖

---

## 与上层组件的关系

| 上层使用方     | 用法                                                     |
| -------------- | -------------------------------------------------------- |
| `list-content` | 标题行右侧 `Show Tag = true` 时内嵌一个 `.tag` 实例      |
| 业务页面       | 任意位置直接使用                                         |

---

## 变更日志

| 日期       | 变更       | 说明                                                                  |
| ---------- | ---------- | --------------------------------------------------------------------- |
| 2026-05-19 | 首版独立化 | 从 `list-content.css` 内联 tag 抽离为独立组件；补齐 6 个 variant；Tinted 底色由 palette 替换为语义 token，dark 模式正确切换 |
| 2026-05-19 | icon 解耦  | 前置图标视觉真源迁至独立 [`icon`](../icon/icon.md) 组件 sprite；`.tag__icon` 仅作容器尺寸（24×24），svg 内部 `viewBox` 改 `0 0 48 48`、改 `fill=currentColor` + `<use href="...#icon/star--fill" />` 引用真源 |
