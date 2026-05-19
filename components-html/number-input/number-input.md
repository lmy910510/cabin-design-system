# NumberInput · 数字 / 字符输入容器

> Figma 节点 `325:12188`（COMPONENT，含 SLOT 属性 `NumberInput-list`）。  
> 用于验证码、车牌号、密码等**逐位输入**场景，由多个 [`NumberInput-item`](../number-input-item/number-input-item.md) 横向排列组成。

## 属性

| 属性 | 类型 | 默认 | 说明 |
|---|---|---|---|
| NumberInput-list | SLOT | 8 个 NumberInput-item | 子项插槽，可任意增减以匹配输入位数 |

## 视觉真值（来自 Figma）

| 项 | 值 |
|---|---|
| 外层 layout | HORIZONTAL，gap = 0 |
| SLOT (NumberInput-list) layout | HORIZONTAL，gap = **24**（token md） |
| 默认子项数量 | 8 个（1 Default + … + 1 Focused + 1 Special） |
| 整体尺寸 | HUG × 102（默认约 888 × 102） |
| 容器视觉 | 无背景 / 无边框 / 无 padding —— **零视觉**，layout 壳 |

## 与 NumberInput-item 的关系

NumberInput 是**容器**，子项视觉的真源是 [`NumberInput-item`](../number-input-item/number-input-item.md)。本组件不重写子项的尺寸 / 字号 / 颜色，避免分叉。

```
NumberInput (容器，HORIZONTAL gap=0)
└─ NumberInput-list (SLOT, HORIZONTAL gap=24)
   └─ NumberInput-item × N （视觉来自 .ni-item / data-state=*）
```

## 类名

| 类名 | 作用 |
|---|---|
| `.number-input__list` | 推荐：SLOT 容器，等价 NumberInput-list |
| `.ni-list` | 兼容：等同 `.number-input__list`（保留以避免破坏既有调用） |

`.number-input` 外壳类目前与 `.ni-list` 同义，因为 Figma 外层 gap=0、无视觉，单独包一层无意义；如未来需要标题 / 错误文本等附加层，再启用 `.number-input` 包裹。

## 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../number-input-item/number-input-item.css" />
<link rel="stylesheet" href="./number-input.css" />
```

## 最小代码

```html
<ul class="number-input__list" aria-label="验证码">
  <li><span class="ni-item" data-state="default">8</span></li>
  <li><span class="ni-item" data-state="default">8</span></li>
  <li><span class="ni-item" data-state="default">8</span></li>
  <li><span class="ni-item" data-state="focused"></span></li>
  <li><span class="ni-item" data-state="default"></span></li>
  <li><span class="ni-item" data-state="default"></span></li>
</ul>
```

## 行为约定

- **同一时刻只有一个 ni-item 处于 focused**，业务侧每输入一位自动跳转下一格。
- 验证码场景建议 4–6 位；车牌场景 7–8 位（最后一格用 `data-state="special"` 表达"新能源"）。
- 容器自身不负责键盘事件 / 焦点跳转逻辑，由业务侧 JS 实现。

## 已知调用方

- [NumberInput-item 自身预览](../number-input-item/number-input-item.md) 的"综合"段，展示真实排布。

## 历史改动锚点

- 容器 css 真源已**从 `number-input-item.css` 迁移到本文件**（消除分叉），原文件中 `.ni-list` 规则为兼容保留，将来如改容器规格只改本文件。
