# PageIndicator-item · 分页指示点

> Figma 节点：`325:13044`（PageIndicator-item ComponentSet）+ `325:13051`（PageIndicator 容器）

## 一句话定位
轮播 / 分页用的小圆点；本组件**同时承载子项 `.pi-item` 与容器 `.pi-list`**——容器只是 layout 壳，没单独建目录。

## 视觉真值

| 维度 | Active | Default |
| --- | --- | --- |
| 形状 | ELLIPSE 12×12 | ELLIPSE 12×12 |
| 填充 | `text/primary` → `--color-text-primary` | `bg/item_tertiary` → `--color-bg-item-tertiary` |

容器 `.pi-list`（PageIndicator）：
- 排列：HORIZONTAL，水平排列
- 间距：12（与子项尺寸相同；不映射 token，按 Figma 显式值）
- 对齐：`counterAxis=CENTER`，垂直居中

## 用法

最小：
```html
<span class="pi-item" data-state="active"></span>
<span class="pi-item" data-state="default"></span>
```

容器组合：
```html
<ul class="pi-list" aria-label="页码">
  <li><span class="pi-item" data-state="active" aria-current="true"></span></li>
  <li><span class="pi-item" data-state="default"></span></li>
  <li><span class="pi-item" data-state="default"></span></li>
</ul>
```

## State 切换

`.pi-item` 通过 `data-state="active | default"` 切换；也兼容 `.is-active` class。

## 锚点回查

- 真值出处：`325:13044`（item 子件 dot ELLIPSE 12×12）+ `325:13051`（容器 gap=12）
- token：`--color-text-primary` / `--color-bg-item-tertiary`
- 不引图标，纯纯 ELLIPSE
