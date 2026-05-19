# Tab-item · 标签栏子项

> Figma 节点 `318:11881`（COMPONENT_SET，2 个 variant）  
> 来源：Figma description 原文

标签栏子项组件，表示单个标签页，用于 Tab 组件内部。

## 属性

| 属性 | 类型 | 默认 | 说明 |
| --- | --- | --- | --- |
| `State` | Variant | `Active` | `Active` / `Default` |
| `文本` | Text | `标签` | 标签文案 |

## 变体一览

| variant | 节点 ID | 尺寸 | layout | label color | indicator |
| --- | --- | --- | --- | --- | --- |
| Active | `318:11900` | 72×54 | VERTICAL，gap 12，counterAxis=CENTER | `--color-text-primary` (cold-gray-9 / `#1f2229`) | 48×6，同色 |
| Default | `318:11903` | 72×54 | HORIZONTAL，primary=CENTER | `--color-text-secondary` (cold-gray-7) | — |

## 规格

- 尺寸 72×54（width=HUG, height=FIXED）
- label：Noto Sans SC Medium 36px / line-height 36px / textAlignHorizontal=CENTER
- indicator（仅 Active）：48×6，颜色 = label color
- 工程里 indicator 用 `::after` 实现，`background: currentColor` 与 label 同步，便于换主题

## 用法

```html
<!-- 推荐：用 data-state（语义对齐 Figma 的 State 属性） -->
<span class="tab-item" data-state="active">推荐</span>
<span class="tab-item" data-state="default">附近</span>

<!-- 等价写法：modifier class -->
<span class="tab-item tab-item--active">推荐</span>
```

依赖：`tokens.css`、`text-styles.css`（间接）、`tab-item.css`。

## 与 Tab 组件的关系

Tab-item 单独可用，但常嵌在 [Tab](../tab/tab.md) 容器内。同一 Tab 中应保持 1 个 Active + N 个 Default 的组合。

## 已知调用方

- [Tab](../tab/tab.md) 容器：默认子项就是 `.tab-item`，1 Active + N Default。
- [TopBar](../top-bar/top-bar.md) `style=button-avatar` 变体的左侧 4-Tab 组：
  容器使用 `.top-bar__tab-list.tab` 复合类，子项使用 `.tab-item / .tab-item--active`，
  TopBar 自身不再保留 tab / tab-item 视觉样式。如修改 tab-item 视觉，
  Tab 与 TopBar 的 button-avatar 变体会自动同步。
