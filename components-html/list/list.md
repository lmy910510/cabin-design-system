# list（列表行 - 整行组合）

> 1:1 映射自 Figma「🤖座舱组件库-AI版 / list」Component `388:17634`

list 是**整行**组件，本身**只负责行级布局 + 分隔线**。中、左、右三个槽各自由独立组件提供视觉与交互，list 不重写子槽任何样式：

```
.list
 ├─ .list-leading   ← 由 list-leading.css 提供（icon / checkbox / none）
 ├─ .list-content   ← 由 list-content.css 提供（title / subtitle）
 └─ .list-action    ← 由 list-action.css 提供（arrow / button / switch）
```

---

## 行级规格（Figma 真值）

| 项目              | 值                                            |
| ----------------- | --------------------------------------------- |
| 布局              | 水平 flex / `align-items:center`              |
| 槽间 gap          | **36px**                                      |
| 上下 padding      | **42px**                                      |
| 中槽宽度          | `flex:1 1 auto`（占剩余宽度）                 |
| 左/右槽宽度       | `flex:none`（hug 内容）                       |
| 底部分隔线        | `1px solid --color-border-secondary`          |
| 末行              | 通过 `:last-child` 或 `.list--last` 去掉分隔线 |
| 整行可点击（可选）| `.list--clickable` → hover/active 反馈        |
| 整行禁用（可选）  | `.list--disabled` → 全部子槽变灰 + 关闭交互   |

> 行高度由子槽 hug：典型最低高度 = 60(icon) + 84(padding 42×2) ≈ 144px。

---

## 用法

### 单行

```html
<div class="list">
  <span class="list-leading list-leading--icon">
    <svg viewBox="0 0 48 48" fill="currentColor">
      <use href="#icon/wifi--fill" />
    </svg>
  </span>
  <div class="list-content list-content--title">
    <div class="list-content__title-row">
      <div class="list-content__title-group">
        <span class="list-content__title">WLAN</span>
      </div>
      <span class="tag tag--large tag--tinted">5G</span>
    </div>
  </div>
  <span class="list-action list-action--arrow">
    <span class="list-action__subtitle">已连接</span>
    <span class="list-action__arrow" aria-hidden="true">
      <svg fill="currentColor"><use href="#icon/enter--line" /></svg>
    </span>
  </span>
</div>
```

### 列表组（推荐 ul 语义化）

```html
<ul class="list-group">
  <li class="list">...</li>
  <li class="list">...</li>
  <li class="list">...</li>     <!-- 末行自动去掉分隔线 -->
</ul>
```

### 整行可点 / 禁用

```html
<li class="list list--clickable">...</li>
<li class="list list--disabled">...</li>
```

---

## 三槽组合矩阵

| 场景               | leading            | content    | action     |
| ------------------ | ------------------ | ---------- | ---------- |
| 设置项跳转         | icon               | title      | arrow      |
| 带描述的设置项     | icon               | subtitle   | arrow      |
| 开关项             | icon               | title      | switch     |
| CTA 行（系统更新） | icon               | subtitle   | button     |
| 多选               | checkbox           | title      | （无）     |
| 危险操作（恢复出厂）| none（省略）      | title      | arrow      |
| 状态展示           | icon               | title      | arrow（仅副标题） |

---

## 自检清单

- [x] 行 hbox / gap=36 / padding=42 0 / align=center —— 与 Figma 1:1
- [x] 中槽 `flex:1 / min-width:0`，左/右槽 `flex:none`
- [x] 底部 1px `--color-border-secondary` 分隔线，末行去掉
- [x] **不重写**子槽任何视觉（颜色 / 字号 / 内边距）—— 全部来自三个独立槽组件
- [x] 颜色仅使用语义 token，不引用 palette
- [x] 提供 `.list-group` / `.list--clickable` / `.list--disabled` / `.list--last` 行级修饰符

---

## 与子槽的关系

| 子槽          | 文件                              | 角色                                           |
| ------------- | --------------------------------- | ---------------------------------------------- |
| list-leading  | `list-leading/list-leading.css`   | 左槽：icon / checkbox / none                    |
| list-content  | `list-content/list-content.css`   | 中槽：title / subtitle，含 Tag / Badge 布尔位   |
| list-action   | `list-action/list-action.css`     | 右槽：arrow / button / switch                  |
| icon          | `icon/icon.svg` + `icon/icon.css` | 图标视觉真源（sprite + currentColor），左槽 icon 与右槽 chevron 通过 `<use href="#icon/<name>--<form>" />` 引用 |

**list 不写**任何 icon、checkbox、文字、按钮、开关样式。子槽改了就直接生效，不存在分叉。所有真业务图标（左槽 icon、右槽 chevron）的 path 真源都在 `icon` 组件里，本组件只通过 sprite 引用。

---

## 变更日志

| 日期       | 变更 | 说明                                    |
| ---------- | ---- | --------------------------------------- |
| 2026-05-18 | 首版 | 纯组合三槽：行级 flex + 36 gap + 42 上下 padding + 1px 分隔线 |
| 2026-05-19 | icon 解耦 | 左槽 icon / 右槽 chevron 改为引用独立 [`icon`](../icon/icon.md) 组件 sprite，消除 list 与上层组件之间手画 svg 的分叉；左槽 icon 默认用 `--fill` 形态、chevron 用 `enter--line`（视觉真源 = Figma 楔形 polygon，与旧版本手画 stroke chevron 存在视觉差异，属修正） |
