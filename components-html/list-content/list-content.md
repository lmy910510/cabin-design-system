# list-content（列表行中槽内容）

> 1:1 映射自 Figma「🤖座舱组件库-AI版 / list-content」Component Set `388:17491`

list-content 是 list 行的**中间内容槽**，承载主标题、未读红点、标签和解释信息。它本身只是垂直 flex 容器，不带背景 / padding / 圆角；高度由子内容 hug，宽度由父级 list 行 fill。

---

## 两个 variant

| Type     | gap  | 子节点                                  | 典型用途                              |
| -------- | ---- | --------------------------------------- | ------------------------------------- |
| Title    | 18px | title-row                               | 单行标题项（"WLAN"、"蓝牙"）          |
| Subtitle | 12px | title-row + description                 | 带解释的项（"通用：版本、关于本机"）   |

---

## 两个布尔属性（与 Figma 对齐）

| Figma boolean | HTML 控制                                                                |
| ------------- | ------------------------------------------------------------------------ |
| `Show Badge`  | 是否在标题旁内嵌 `<span class="badge badge--circle">`（独立 [badge](../badge/badge.md) 组件，12×12 红点） |
| `Show Tag`    | 是否在标题行右侧内嵌 `<span class="tag …">`（独立 [tag](../tag/tag.md) 组件）|

> 直接增删对应节点即可，与 Figma boolean=true/false 等价。
> Tag / Badge 视觉真源都已抽到独立组件，本组件只负责把它们放在标题行的位置上。

---

## 关键尺寸 / 颜色（Figma 真值）

| 项目              | 值                                                                |
| ----------------- | ----------------------------------------------------------------- |
| 容器 gap (Title)  | 18px                                                              |
| 容器 gap (Sub)    | 12px                                                              |
| title-row gap     | 12px（title-group 与 tag 之间）                                   |
| 标题字号          | 32 / 32 / Medium（Noto Sans SC）                                  |
| 标题色            | `--color-text-primary` = `#1f2229`                                |
| Badge             | 内嵌独立 `.badge.badge--circle` 组件（12×12，背景 `--color-bg-warning` = red-5），视觉真源在 [`badge.md`](../badge/badge.md) |
| Tag               | 内嵌独立 `.tag` 组件（默认 Large/Tinted），视觉真源在 [`tag.md`](../tag/tag.md) |
| description 字号  | 28 / 36 / Regular                                                 |
| description 色    | `--color-text-secondary` = `#828997`                              |

---

## 用法

```html
<!-- Title：标题 + Badge + Tag（需同时引入 tag.css） -->
<div class="list-content list-content--title">
  <div class="list-content__title-row">
    <div class="list-content__title-group">
      <span class="list-content__title">标题文本</span>
      <span class="badge badge--circle" aria-hidden="true"></span>
    </div>
    <span class="tag tag--large tag--tinted">标签文案</span>
  </div>
</div>

<!-- Subtitle：标题 + Badge + Tag + 解释信息 -->
<div class="list-content list-content--subtitle">
  <div class="list-content__title-row">
    <div class="list-content__title-group">
      <span class="list-content__title">标题文本</span>
      <span class="badge badge--circle" aria-hidden="true"></span>
    </div>
    <span class="tag tag--large tag--tinted">标签文案</span>
  </div>
  <p class="list-content__description">解释信息……</p>
</div>
```

---

## 自检清单

- [x] 两个 variant 容器 gap 分别为 18 / 12 —— 与 Figma 1:1
- [x] title-row hbox / gap=12 / align=center —— 与 Figma 1:1
- [x] title 32/32 Medium `--color-text-primary` —— 与 Figma 1:1
- [x] Badge 12×12 圆角 6 `--palette-red-5` —— 与 Figma 1:1
- [x] Tag 内嵌独立 `.tag` 组件，本组件不重复维护 tag 视觉规则
- [x] description 28/36 Regular `--color-text-secondary` —— 与 Figma 1:1
- [x] description 仅出现在 Subtitle variant —— 与 Figma 1:1
- [x] 容器自身无背景 / 无圆角 / 无 padding —— 与 Figma 1:1

---

## 与上层组件的关系

| 上层使用方     | 用法                                                     |
| -------------- | -------------------------------------------------------- |
| `list`（行项） | 列表行 row 的中间槽（`flex:1`，左右分别是 leading/action）|
| 其它          | 暂未直接使用                                             |

---

## 变更日志

| 日期       | 变更 | 说明                                                  |
| ---------- | ---- | ----------------------------------------------------- |
| 2026-05-18 | 首版 | 1:1 还原 Figma `list-content` 两个 variant + 两个布尔 |
| 2026-05-19 | tag 解耦 | tag 视觉真源迁至独立 `tag.css`（含 6 个 variant），本组件标题行右侧改为内嵌 `<span class="tag …">`；旧 `.list-content__tag` 保留为 Large/Tinted 别名（已修正 Tinted 底色为语义 token，dark 模式正确反相）|
