# SegmentedControl-item · 分段控制器子项

> Figma 节点 `464:42275`（COMPONENT_SET，2 variant）。SegmentedControl 主组件的子组件，**不单独使用**。

## 属性 / 变体

| 属性 | 类型 | 默认 | 说明 |
|---|---|---|---|
| Label | TEXT | "选项卡" | 文案 |
| State | VARIANT | Active | Active \| Default |

## 视觉真值（来自 Figma）

| 项 | Active | Default |
|---|---|---|
| 背景 | `bg/container_primary`（≈ 白） | 透明 |
| 阴影 | `Shadow/Medium` | 无 |
| 文字色 | `--color-text-primary` | `--color-text-secondary` |
| 圆角 | 999（胶囊） | 999 |
| 字号 | 28 / 36 Medium | 28 / 36 Medium |

> 容器 padding（xxs=6）由父级 SegmentedControl 提供；item 自身只控制 background / 文字色 / 阴影。

## 类名

| 类名 | 作用 |
|---|---|
| `.sc-item` | 容器（按钮） |
| `.sc-item.is-active` / `[aria-selected="true"]` / `[data-state="active"]` | Active 态（任一即可触发） |

## 用法

```html
<button type="button" class="sc-item is-active" role="tab" aria-selected="true">推荐</button>
<button type="button" class="sc-item" role="tab" aria-selected="false">附近</button>
```

## 设计契约

- 同一 SegmentedControl 内 **同一时刻只有一个 sc-item 为 Active**。
- 横向通过 `flex: 1` 均分宽度（与 [SegmentedControl 容器](../segmented-control/segmented-control.md) 的 Auto Layout Fill 等价）。
- 视觉真源唯一在本文件；SegmentedControl 不重写 item 视觉。
