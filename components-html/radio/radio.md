# radio

> ⚠️ **待补充：Figma 主组件清单中暂无独立的 Radio**。  
> 当前 Figma `🤖座舱组件库-AI版` / Components 页里只列了 `Checkbox`，没有 `Radio` 主组件。本组件是 HTML 端的**前置抽象**：先按"与 checkbox 对称、外形改圆 + 实心圆点"做实现，等 Figma 把 Radio 抽成独立 component set 后，再做 1:1 对齐校准（尺寸 / 状态结构已留好，不需要重做）。

单选，独立可复用控件。视觉规格与 [`checkbox`](../checkbox/checkbox.md) 节奏完全对称，仅外形（圆角 50% + 实心圆点）不同。

由原生 `<input type="radio">` 的 `name` 属性提供单选互斥语义，零 JS。

## 视觉规格

| 项 | 值 |
|---|---|
| 外圆尺寸 | 36 × 36 |
| 圆角 | 50%（圆形） |
| 描边 | 2px / `--color-border-primary` |
| 选中底色 | `--color-text-primary` |
| 选中圆点色 | `--color-text-primary-inverse`（反色） |
| 圆点尺寸 | 18 × 18 |
| 文字与外圆间距 | 18px |
| 文字尺寸 | 28 / 36 Regular `--color-text-primary` |
| 状态切换动效 | 120ms ease（bg / border / color） |

## 状态矩阵

| 状态 | 描边 | 底色 | 圆点色 |
|---|---|---|---|
| 未选中 | `--color-border-primary` | transparent | transparent (隐藏) |
| 选中 | `--color-text-primary` | `--color-text-primary` | `--color-text-primary-inverse` |
| 禁用 · 未选 | `--color-border-disabled` | `--color-bg-item-disabled` | transparent |
| 禁用 · 选中 | `--color-border-disabled` | `--color-bg-item-disabled` | `--color-text-disabled` |

所有状态由 `<input type="radio">` 的 `:checked` / `:disabled` 选择器驱动，零 JS。

## 用法

### 仅外圆

```html
<label class="radio">
  <input type="radio" name="demo" />
  <span class="radio__box" aria-hidden="true">
    <span class="radio__dot"></span>
  </span>
</label>
```

### 外圆 + 文字标签（单选组）

> 同一组的 `name` 必须一致，否则不会互斥。

```html
<label class="radio">
  <input type="radio" name="theme" />
  <span class="radio__box" aria-hidden="true"><span class="radio__dot"></span></span>
  <span class="radio__label">浅色主题</span>
</label>

<label class="radio">
  <input type="radio" name="theme" checked />
  <span class="radio__box" aria-hidden="true"><span class="radio__dot"></span></span>
  <span class="radio__label">深色主题</span>
</label>
```

### 在 list 行首使用

由 `list-leading` 提供 60×60 占位容器，由 `.radio` 提供外圆 / 圆点 / 状态：

```html
<span class="list-leading list-leading--radio">
  <label class="radio">
    <input type="radio" name="row-radio" checked />
    <span class="radio__box" aria-hidden="true">
      <span class="radio__dot"></span>
    </span>
  </label>
</span>
```

## 设计契约

- 视觉真源唯一：`radio.css`。`list-leading.css` 不写外圆 / 圆点的样式，只负责 60×60 占位。
- 主题适配：依赖 `--color-text-primary` / `--color-text-primary-inverse` 这对反色 token，自动适配 light/dark。
- 单选语义：由原生 `name` 实现互斥，**不做** JS 状态管理。
- 可访问性：原生 `<input>` 提供语义；`:focus-visible` 时外圆带 outline。

## 与 checkbox 的关系

| 维度 | checkbox | radio |
|---|---|---|
| 形状 | 方形 / 圆角 8 | 圆形 / 圆角 50% |
| 选中标识 | 对勾 svg | 实心圆点（18×18） |
| 多/单选 | 多选独立 | 同 `name` 单选互斥 |
| 间距 / 文字 / 状态色 | 完全一致 | 完全一致 |

## 与 Figma 的关系

Figma 当前 list 主组件中的 radio 尚未独立成 component set。HTML 端的 `.radio` 是**前置抽象**，等 Figma 抽出后再做 1:1 对齐校准；尺寸 / 状态结构已留好，不需要重做。
