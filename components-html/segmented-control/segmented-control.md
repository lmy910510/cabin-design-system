# SegmentedControl · 分段控制器

> Figma 节点 `464:43476`（COMPONENT，含 SLOT 属性 `list`）。  
> 由多个 [`SegmentedControl-item`](../segmented-control-item/segmented-control-item.md) 横向均分组成。

## 属性

| 属性 | 类型 | 默认 | 说明 |
|---|---|---|---|
| list | SLOT | 默认 3 个 sc-item | 子项插槽，可增减（推荐 2~5 个） |

## 视觉真值（来自 Figma）

| 项 | 值 |
|---|---|
| layout | HORIZONTAL，gap 0 |
| padding | 6（xxs） 四周 |
| 背景 | `bg/container_primary` |
| 描边 | `border/on_inverse` 1px |
| 圆角 | 999（胶囊） |
| 子项均分 | 每个 item 水平 Fill |

## 与 SegmentedControl-item 的关系

```
SegmentedControl (胶囊壳 + padding 6 + 描边 1)
└─ list (SLOT)
   └─ sc-item × N （flex:1 均分；视觉真源在 segmented-control-item.css）
```

## 类名

| 类名 | 作用 |
|---|---|
| `.segmented-control` | 容器 |
| `.sc-item` / `.sc-item.is-active` | 子项（来自子组件文件） |

## 用法

```html
<div class="segmented-control" role="tablist" aria-label="视图切换">
  <button type="button" class="sc-item is-active" role="tab" aria-selected="true">推荐</button>
  <button type="button" class="sc-item" role="tab" aria-selected="false">附近</button>
  <button type="button" class="sc-item" role="tab" aria-selected="false">收藏</button>
</div>
```

切换 Active：业务侧给目标 button 加 `is-active` / `aria-selected="true"`，给其它去掉。

## 设计契约

- **同一时刻只有一个 sc-item 为 Active**。
- 推荐 **2~5 个** item；多于 5 时考虑 Tab。
- 子项均分：依赖 `.sc-item { flex: 1 }`（已在子组件 css 写好）。
- 不在本文件重写 sc-item 视觉，避免分叉。
