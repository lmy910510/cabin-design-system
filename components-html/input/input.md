# Input · 文本输入框

> Figma 节点：`325:12311`（ComponentSet）·路径 `Input`
> 4 个变体：`Default` / `Focused` / `Filled` / `MaxLength`

## 一句话定位
单行文本输入框；780×84，左右内边距 24，子件水平排布、垂直居中（counterAxis=CENTER）。

## 视觉真值

| 维度 | 值 | token / 备注 |
| --- | --- | --- |
| 容器尺寸 | 780 × 84 | 高度对应 token `9xl` |
| 圆角 | 24 | `--space-md` |
| 内边距 | 0 24 | 上下贴边，左右 `--space-md` |
| 子项间距 | 18 | `--space-sm` |
| 背景 | `bg/item_quaternary` | `--color-bg-item-quaternary` |
| 文本字号 | 32 / 32 Medium | placeholder + input-text + cursor |
| 计数字号 | 28 / 28 DIN Medium | number |
| 计数色（Default/Focused/Filled） | `text/tertiary` | `--color-text-tertiary` |
| 计数色（MaxLength） | `text/price` | `--color-text-price` |
| 分割线 | 1×24 | `border/secondary` |
| icon/fail | 42×42 | sprite `#icon/close--line` |
| icon/refresh | 42×42 | sprite `#icon/refresh--line` |

## State 切换

容器加 `data-state="default | focused | filled | maxlength"`，CSS 仅在 `maxlength` 下覆盖 number 颜色。`focused` 的闪烁光标由 `.input__cursor` 内置 `@keyframes input-blink` 完成（1s steps(1) 无限循环）。

## 子件结构

```html
<div class="input" data-state="...">
  <div class="input__field">
    <!-- 任选其一组合 -->
    <span class="input__placeholder">请输入</span>
    <span class="input__text">已输入文字</span>
    <span class="input__cursor">｜</span>
  </div>

  <!-- 以下三段可任意省略：直接不写或加 .is-hidden -->
  <span class="input__number">10/15</span>
  <button class="input__fail">…close icon…</button>
  <span class="input__action">
    <!-- 内置 ::before 伪元素自动出分割线 -->
    <button class="input__refresh">…refresh icon…</button>
  </span>
</div>
```

## 行为约定

- `input__field` 是 `flex:1` 主区，超长内容用 `text-overflow: ellipsis` 截尾
- `input__cursor` 默认闪烁；如要静止，移除元素或自行覆盖 `animation: none`
- `input__action` 的左竖线由 `::before` 渲染；如不需要分割线，整段 `.input__action` 不要渲染即可
- `input__number` 也可以直接通过 `data-overflow="true"` 强制改为 `text/price` 警示色（与 `data-state="maxlength"` 同效，便于上层不依赖整体 state）

## 锚点回查

- 真值出处：Figma `325:12311`（State=Default 322 → Filled 12345 → Focused 12333 → MaxLength 12497）
- 子件命名沿用 Figma：`placeholder` / `input-text` / `cursor` / `number` / `icon/fail` / `action`（line + `icon/refresh`）
- 与 `tokens.css` 对齐：bg/item_quaternary、text/primary、text/tertiary、text/price、border/secondary
- 图标依赖：sprite `#icon/close--line`、`#icon/refresh--line`
