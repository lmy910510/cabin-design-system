# Switch · 开关

> 1:1 映射自 Figma「🤖座舱组件库-AI版」/ Switch
> Component Set 节点：`257:5522`

## 1. 真实尺寸（Figma 实测）

| 属性 | 值 | 说明 |
|---|---|---|
| 外尺寸 | 108 × 60 px | 整体胶囊 |
| 圆角 | 30 px | = 高度 / 2，等同 `--space-lg` 但含义是几何圆角，不复用 spacing token |
| thumb 尺寸 | 48 × 48 px | 圆形（Figma 内是 VECTOR） |
| thumb 内边距 | 上下 / 左右 各 6 px | 即 `--space-xxs` |
| thumb 行程 | 48 px | track 宽 - thumb 宽 - 左右各 6 边距 = 108 − 48 − 12 = 48 |
| 阴影（Off 状态） | `--shadow-light` | 白 thumb 在浅底上需要阴影区分 |

## 2. Variants（4 种）

| variant | Figma id | track 颜色 | thumb 颜色 | thumb 阴影 | thumb 位置 |
|---|---|---|---|---|---|
| `On=True,  Disabled=False` | 257:5523 | `--color-bg-item-primary`（Light=#1f2229 / Dark=white-9） | 白（`--palette-white-10`） | 无 | 右 |
| `On=True,  Disabled=True`  | 315:11852 | `--color-bg-item-tertiary`（#eaedf2） | `--color-bg-item-disabled`（#dee2e8） | 无 | 右 |
| `On=False, Disabled=False` | 257:5526 | `--color-bg-item-tertiary` | 白 | `--shadow-light` | 左 |
| `On=False, Disabled=True`  | 315:11855 | `--color-bg-item-tertiary` | `--color-bg-item-disabled` | 无 | 左 |

> 关键：disabled 状态下，**On=true 不再使用主色 track**（与 Figma 一致），track 一律降回 tertiary 灰；thumb 全部使用 `--color-bg-item-disabled`，无阴影。

## 3. HTML 用法

```html
<link rel="stylesheet" href="design-system/tokens.css" />
<link rel="stylesheet" href="design-system/shadows.css" />
<link rel="stylesheet" href="components-html/switch/switch.css" />

<label class="switch" aria-label="某项设置">
  <input type="checkbox" checked />
  <span class="switch-track"></span>
  <span class="switch-thumb"></span>
</label>
```

## 4. 状态切换矩阵（CSS 自动）

通过组合 input 的两个原生属性，CSS 自动渲染 4 种 variant：

| HTML | 渲染 variant |
|---|---|
| `<input type="checkbox" checked />` | On=True, Disabled=False |
| `<input type="checkbox" />` | On=False, Disabled=False |
| `<input type="checkbox" checked disabled />` | On=True, Disabled=True |
| `<input type="checkbox" disabled />` | On=False, Disabled=True |

## 5. 与 Figma 的差异（明示）

- **动画**：Figma 是静态的；CSS 端补了 `160ms ease` 过渡（track 颜色 / thumb 位移 / thumb 阴影），符合 HMI 一般习惯。
- **focus 样式**：Figma 不含 focus 视觉；CSS 端补了 `:focus-visible` 的 3px outline（用 `--color-border-active`），仅在键盘 Tab 操作时显示，鼠标点击不显示。
- **hover**：Figma 不含 hover 视觉；CSS 端补 `filter: brightness(0.97)` 轻反馈，对 disabled 不生效。

以上 3 点为"附加增强"，不影响 Figma 静态 1:1。

## 6. 自检清单

写完页面用 Switch 时，对照下列条件确认严格 1:1：
- [ ] 外尺寸 108 × 60，圆角 30
- [ ] thumb 48 × 48，距上下/左右各 6
- [ ] On=True 时 track 用 `--color-bg-item-primary`（深色块）
- [ ] Off 时白 thumb 必须带 `--shadow-light`
- [ ] disabled 状态 track 不变为主色，thumb 用灰色无阴影
- [ ] 在 `<html data-theme="dark">` 下，On 态 track 自动变为 white-9（亮色），与 Figma Dark mode 一致
