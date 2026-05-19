# checkbox

复选框，独立可复用控件。视觉规格 1:1 对齐 list-leading--checkbox 内联实现，独立成组件后，list-leading 改为内嵌 `<span class="checkbox">…</span>` 形式，消除分叉。

## 视觉规格

| 项 | 值 |
|---|---|
| 方框尺寸 | 36 × 36 |
| 圆角 | 8 |
| 描边 | 2px / `--color-border-primary` |
| 选中底色 | `--color-text-primary`（深底实心） |
| 选中对勾色 | `--color-text-primary-inverse`（反色） |
| 对勾尺寸 | 30 × 30 (svg viewBox 36) |
| 文字与方框间距 | 18px (`spacing-18`) |
| 文字尺寸 | 28 / 36 Regular `--color-text-primary` |
| 状态切换动效 | 120ms ease（bg / border / color） |

## 状态矩阵

| 状态 | 描边 | 底色 | 勾色 |
|---|---|---|---|
| 未选中 | `--color-border-primary` | transparent | transparent (隐藏) |
| 选中 | `--color-text-primary` | `--color-text-primary` | `--color-text-primary-inverse` |
| 禁用 · 未选 | `--color-border-disabled` | `--color-bg-item-disabled` | transparent |
| 禁用 · 选中 | `--color-border-disabled` | `--color-bg-item-disabled` | `--color-text-disabled` |

所有状态由 `<input type="checkbox">` 的 `:checked` / `:disabled` 选择器驱动，零 JS。

## 用法

### 仅方框

```html
<label class="checkbox">
  <input type="checkbox" />
  <span class="checkbox__box" aria-hidden="true">
    <svg viewBox="0 0 36 36" fill="none">
      <path d="M9 18l6 6 12-12" stroke="currentColor" stroke-width="3.5"
            stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </span>
</label>
```

### 方框 + 文字标签

```html
<label class="checkbox">
  <input type="checkbox" />
  <span class="checkbox__box" aria-hidden="true">…svg…</span>
  <span class="checkbox__label">同意《用户协议》</span>
</label>
```

### 在 list 行首使用

由 `list-leading` 提供 60×60 占位容器，由 `.checkbox` 提供方框 / 勾 / 状态：

```html
<span class="list-leading list-leading--checkbox">
  <label class="checkbox">
    <input type="checkbox" checked />
    <span class="checkbox__box" aria-hidden="true">…svg…</span>
  </label>
</span>
```

## 设计契约

- 视觉真源唯一：`checkbox.css`。`list-leading.css` 不再写 box / 勾的样式，只负责 60×60 占位。
- 主题适配：依赖 `--color-text-primary` / `--color-text-primary-inverse` 这对反色 token，自动适配 light/dark。
- 可访问性：原生 `<input>` 提供语义；`:focus-visible` 时方框带 outline。

## 与 Figma 的关系

Figma 当前 list 主组件中的 checkbox 是直接画在 list 内的，没有独立 component set。HTML 端的 `.checkbox` 是**前置抽象**，等 Figma 抽出 Checkbox component set 后再做对齐校准；尺寸/状态结构已留好，不需要重做。
