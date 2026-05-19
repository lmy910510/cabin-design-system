# Tab · 标签栏容器

> Figma 节点 `319:11934`（COMPONENT），布局型组件，**容器自身零视觉**。

## 视觉真值（来自 Figma）

| 项 | 值 | Token |
|---|---|---|
| layoutMode | HORIZONTAL | — |
| primaryAxisAlignItems | MIN（左对齐） | — |
| counterAxisAlignItems | CENTER | — |
| itemSpacing | **72** | `7xl` |
| width × height | HUG × HUG（4 项时 504 × 54） | — |
| 背景 / 边框 / padding | 全部无 | — |

## 与 Tab-item 的关系

Tab 是容器，**子项视觉的真源是 [Tab-item](../tab-item/tab-item.md)**。
本组件不重写 tab-item 的尺寸、字号、颜色，避免分叉。

```
Tab (容器，layout 壳)
└─ Tab-item × N （视觉来自 .tab-item / .tab-item--active）
```

## 类名

| 类名 | 作用 |
|---|---|
| `.tab` | 容器，`display: inline-flex; gap: 72px; align-items: center;` |
| `.tab--gap-lg` | 可选 modifier，gap=36（4xl），用于更紧凑的栏 |
| `.tab--gap-7xl` | 默认 gap 的显式 alias，用于阅读 |

## 引入

```html
<link rel="stylesheet" href="../../design-system/tokens.css" />
<link rel="stylesheet" href="../tab-item/tab-item.css" />  <!-- 子项视觉真源 -->
<link rel="stylesheet" href="./tab.css" />
```

## 最小代码

```html
<ul class="tab" role="tablist" aria-label="标签栏">
  <li><button class="tab-item tab-item--active" role="tab" aria-selected="true">推荐</button></li>
  <li><button class="tab-item" role="tab" aria-selected="false">附近</button></li>
  <li><button class="tab-item" role="tab" aria-selected="false">收藏</button></li>
  <li><button class="tab-item" role="tab" aria-selected="false">历史</button></li>
</ul>
```

## 行为约定

- **同一时刻只有一个 Tab-item 处于 active**。
- 标签数量建议 **2–5 个**；更多时考虑滚动栏（不在本组件范围）。
- 标签文案保持简短，建议 2-4 字。
- 容器自身不负责"激活态切换"逻辑，由业务/页面级 JS 决定。

## 已知调用方

- [TopBar `style=button-avatar`](../top-bar/top-bar.md)：左侧 4-Tab 区
  使用 `.top-bar__tab-list.tab` 复合类，复用本组件的 layout，避免在 TopBar
  内部重写 flex+gap72。修改本文件 gap / 对齐方式，TopBar 自动同步。
- [Tab-item 自身预览](../tab-item/tab-item.md) 的"综合"段落，展示 Tab 容器
  下的真实排布。

## 历史改动锚点

- 容器零视觉：Figma 没有 fills / strokes / padding；如未来加入背景或圆角，
  需要先在 Figma 确认真值再回写本文件，不要在调用方私改。
- 已合并 `top-bar.css` 中重复的 `.top-bar__tab-list` 布局规则到本文件。
