# NumberInput-item · 数字输入子项（单格）

> Figma 节点 `323:12150`（COMPONENT_SET）/ 5 个 variant
> 用于验证码、车牌号、密码等"逐格输入"场景。**作为 NumberInput 容器子项使用**，
> 不建议单独出现在页面里。

## 视觉真值（来自 Figma）

| 项 | 值 | Token |
|---|---|---|
| 尺寸 | 90 × 102 | 宽 = `--space-10xl` |
| 圆角 | 18 | `--space-sm` |
| layout | VERTICAL，primary CENTER，counter CENTER | — |
| text 字符 | DIN Medium 48 / 48 | — |
| placeholder（Special） | Noto Sans SC Medium 24 / 24，竖排 | — |

## 5 个 State

| state | bg | border | 内容 | 业务用途 |
|---|---|---|---|---|
| `default` | `--color-bg-item-quaternary` | `--color-border-secondary`（1px） | 已输入字符 | 已确定/历史输入 |
| `focused` | 同上 | `--color-border-active`（**3px**） | **空** | 当前正在输入的格子 |
| `special` | 透明 | `--color-border-secondary`（**2px**） | 竖排 3 字（如 "新能源"） | 车牌占位/提示槽 |
| `error` | `--color-bg-warning-transparent` | `--color-border-warning`（1px） | 已输入字符（warning 色） | 校验失败 |
| `picked` | `--color-bg-positive`（12%） | `--color-border-positive`（**3px**） | **空** | 特殊态-已选中（如新能源） |

> **关键**：Focused / 特殊态-选中两态都用 **3px** 描边强调当前焦点；
> 其它态都是 1-2px 的常规描边，保证视觉静默。

## NumberInput 容器（layout 壳）

NumberInput 在 Figma 是 `325:12188`，结构非常薄：HORIZONTAL + gap 24 +
N 个 NumberInput-item。我们把这层 layout 直接放在本文件的 `.ni-list` 类里，
不另开组件目录，避免"两个文件描述同一件事"。

```html
<ul class="ni-list" aria-label="验证码输入">
  <li><span class="ni-item" data-state="default">9</span></li>
  <li><span class="ni-item" data-state="default">2</span></li>
  <li><span class="ni-item" data-state="default">3</span></li>
  <li><span class="ni-item" data-state="focused"></span></li>
  <li><span class="ni-item" data-state="default"></span></li>
  <li><span class="ni-item" data-state="default"></span></li>
</ul>
```

## 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../number-input-item/number-input-item.css" />
```

## 单格用法

```html
<!-- 已输入 -->
<span class="ni-item" data-state="default">A</span>

<!-- 当前输入位（不要塞 text） -->
<span class="ni-item" data-state="focused"></span>

<!-- 车牌"新能源"提示槽（竖排占位） -->
<span class="ni-item" data-state="special">
  <span class="ni-item__placeholder">新<br>能<br>源</span>
</span>

<!-- 校验失败 -->
<span class="ni-item" data-state="error">F</span>

<!-- 已选中（如新能源已确认） -->
<span class="ni-item" data-state="picked"></span>
```

## 行为约定

- **同一时刻最多一个 Focused**（业务方约束，组件不强制）
- 切换 Default → Focused → Default 时不要做尺寸过渡（仅 border-color/border-width 变化），
  否则文字位置会抖动
- Special 态的内容**必须用 `.ni-item__placeholder` 内层包裹 + `<br>` 显式换行**：
  Figma 真值是文本节点 `width=24, height=72`，content `"新\n能\n源"`，
  即 24×72 的窄文本块在 90×102 容器内居中。**不要给 `.ni-item` 自身加 `max-width`**，
  那会把整个格子挤窄。

## 已知调用方

- `number-input-item.preview.html` 的"综合"段
- 当业务页面落地"输入验证码 / 输入车牌"时，直接复用 `.ni-list + .ni-item`

## 历史改动锚点

- 2026-05：从 Figma 节点 323:12150 + 325:12188 1:1 落地
- 2026-05 修复：Special 态曾错误地在容器自身加 `max-width: 24px`，导致整个格子被挤窄。
  改为新增内层 `.ni-item__placeholder` 节点（24×72），容器保持 90×102，与 Figma 真值一致。
