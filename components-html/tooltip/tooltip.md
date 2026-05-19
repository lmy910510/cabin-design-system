# Tooltip · 气泡提示

> Figma 节点 `315:11577`（COMPONENT_SET，8 个 variant）  
> 来源：Figma description 原文

气泡提示组件，用于对元素进行轻量级的信息说明。

## 属性

| 属性 | 类型 | 默认 | 说明 |
| --- | --- | --- | --- |
| `Direction` | Variant | `Right` | `Top` / `Bottom` / `Left` / `Right` —— 气泡弹出方向 |
| `Alignment` | Variant | `Center` | `Start` / `Center` / `End` —— 箭头对齐位置（仅 Top/Bottom 支持 Start/End） |
| `Show icon` | Boolean | `true` | 是否显示前置 icon/info（42×42） |
| `Show action` | Boolean | `true` | 是否显示操作文案（橙色） |

## 变体一览（8 个）

| Direction | Alignment | 节点 ID |
| --- | --- | --- |
| Top    | Start  | `315:11749` |
| Top    | Center | `315:11578` |
| Top    | End    | `315:11683` |
| Bottom | Start  | `315:11741` |
| Bottom | Center | `315:11576` |
| Bottom | End    | `315:11595` |
| Left   | Center | `315:11819` |
| Right  | Center | `315:11785` |

## 规格（Figma 真值）

- bubble：bg `--palette-cold-gray-9` `#1F2229`，圆角 24，padding 上下 18 / 左右 24
- content-row：HORIZONTAL，gap 12（icon ↔ text）
- bubble 内 content 与 action 之间 gap 36
- icon/info：42×42（白色 90% 不透明度）
- text：Noto Sans SC Medium 28/28，white-9 opacity 0.9
- action：Noto Sans SC Medium 28/28，`--color-text-brand` = `--palette-orange-5` `#FA6800`
- arrow：36×18（top/bottom 方向时；left/right 翻转为 18×36），颜色同 bubble bg，紧贴气泡（gap 0）

## 用法

```html
<!-- Top-Center：气泡在上，箭头居中 -->
<div class="tooltip" data-direction="top" data-alignment="center">
  <div class="tooltip__bubble">
    <div class="tooltip__content">
      <span class="tooltip__icon"><svg class="icon"><use href="#icon/info--line"/></svg></span>
      <span class="tooltip__text">气泡提示</span>
    </div>
    <span class="tooltip__action" role="button" tabindex="0">知道了</span>
  </div>
  <span class="tooltip__arrow" aria-hidden="true"></span>
</div>

<!-- 仅文字（Show icon=false, Show action=false） -->
<div class="tooltip" data-direction="bottom" data-alignment="center">
  <div class="tooltip__bubble">
    <div class="tooltip__content">
      <span class="tooltip__text">点击试试看</span>
    </div>
  </div>
  <span class="tooltip__arrow" aria-hidden="true"></span>
</div>
```

## 工程说明

- **arrow 实现**：用 CSS triangle（4 边 border 中三边 transparent）替代 Figma 的 vector arrow，保证零外部依赖、可走 `--palette-cold-gray-9` 跟随 token。Figma 真值中 arrow 形状为等腰三角，36×18 ↔ 18×36 翻转，CSS 用 `border-*: 18px transparent + 一边实色` 1:1 还原。
- **不随主题翻**：tooltip 是恒定深色气泡（无 light/dark 主题切换），所以直接吃 palette 层 `--palette-cold-gray-9` / `--palette-white-9`，不走 `--color-text-primary-inverse`（语义层会随主题翻转）。这是 mapping.md 中允许的"反色卡片"例外用法。
- **定位**：组件内只负责 bubble 与 arrow 的几何关系；外层"对齐到触发元素"的定位（absolute/fixed/popover）由调用方决定。

依赖：`tokens.css` + `icon.css` + `icon-sprite-inline.js`（提供 `#icon/info--line` 与 `#icon/info--fill`）+ `tooltip.css`。
