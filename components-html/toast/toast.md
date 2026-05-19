# Toast · 轻提示

> Figma 节点 `305:11208`（COMPONENT，单态 + Boolean 属性）。用于操作结果的短暂反馈，自动消失。

## 属性

| 属性 | 类型 | 默认 | 说明 |
|---|---|---|---|
| Show icon | Boolean | true | 是否显示左侧状态图标 |

## 视觉真值（来自 Figma）

| 项 | 值 | Token |
|---|---|---|
| layoutMode | HORIZONTAL | — |
| gap | 12 | `xs` |
| padding | 18 / 24（上下 / 左右） | — |
| cornerRadius | 24 | `md` |
| 背景 | `rgba(31,34,41)` | 深色不透明（不随主题反转） |
| 图标 icon/fail | 42 × 42，白色 | — |
| 文本 label | 28 / 28，白色 | — |

## 类名

| 类名 | 作用 |
|---|---|
| `.toast` | 容器；HORIZONTAL，gap 12，padding 18/24，圆角 24 |
| `.toast__icon` | 42×42 图标槽 |
| `.toast__label` | 文本节点 |
| `.toast--no-icon` | 隐藏左侧图标（等价于 Show icon = false） |

## 用法

```html
<div class="toast" role="status" aria-live="polite">
  <span class="toast__icon" aria-hidden="true">
    <svg><use href="../icon/icon.svg#icon/fail--fill"/></svg>
  </span>
  <span class="toast__label">操作失败</span>
</div>
```

需要换状态时直接替换 `<use href>` 中的 sprite ID（如 `#icon/success--fill`）。

## 使用指引

- 用于操作成功 / 失败 / 警告的短暂反馈。
- 提示文案保持简短，建议 **≤ 10 字**。
- 居中显示在屏幕上，不遮挡关键操作。
- 默认图标为 fail；按场景替换为 success / warning 等。
- Toast 自身不带"自动消失"逻辑，由业务侧用 setTimeout 控制可见性。

## 与 Figma 的关系

Figma 单一 COMPONENT，一个 Boolean 属性 `Show icon`。本组件 1:1 还原结构与视觉，用 `.toast--no-icon` 表达 Boolean=false。

> ⚠️ **待与 Figma 对齐**：本地 `icon.svg` sprite 当前只覆盖 11 个语义 icon，**尚未补充 `fail / success / warning`** 三个状态图标。预览暂用 `icon/info--fill` 占位，待 sprite 扩展后把 `<use href>` 切到对应 ID 即可，组件结构无需改动。
