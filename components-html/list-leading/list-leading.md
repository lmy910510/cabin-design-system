# list-leading（列表行左侧装饰槽）

> HTML 端独立左槽组件。Figma 当前 `list` 主组件把左侧 icon 直接画在内部（60×60 线形 icon），尚未拆成独立 component set；HTML 端先把它独立出来，与 `list-content` / `list-action` 同等地位。后续 Figma 补充 component set 后，本组件的 type 维度直接对齐即可，无需返工。

list-leading 是 list 行的**左侧装饰槽**，承载图标、复选框、单选、头像、序号等"行首附件"。它本身只是 60×60 居中容器，不带背景 / padding / 圆角，**视觉真源来自被它内嵌的真组件**（如 `.checkbox`、未来的 `.radio` / `.avatar`）。

---

## 四个 Type（当前支持）

| Type     | 容器尺寸 | 内容                                                                  | 典型用途                       |
| -------- | -------- | --------------------------------------------------------------------- | ------------------------------ |
| none     | 0 × 0    | 不渲染（纯文字行）                                                      | 简单设置项（"语言"、"时区"）   |
| icon     | 60 × 60  | 36×36 sprite icon（真源在 [`icon`](../icon/icon.md)）                  | 设置/功能列表（"WLAN"、"通用"）|
| checkbox | 60 × 60  | 内嵌独立 `.checkbox` 实例（视觉真源在 checkbox.css）                    | 多选列表                       |
| radio    | 60 × 60  | 内嵌独立 `.radio` 实例（视觉真源在 radio.css）                          | 单选列表（同 `name` 互斥）     |

> 后续可扩展：`avatar` / `dot` 等，结构相同（外层 list-leading 提供 60×60 占位 + 内嵌真组件）。

---

## 关键尺寸 / 颜色

| 项目        | 值                                                |
| ----------- | ------------------------------------------------- |
| icon 容器   | 60 × 60                                           |
| icon 本体   | 36 × 36，由独立 `.icon` 组件提供（sprite, viewBox 0 0 48 48） |
| icon 颜色   | `currentColor` = `--color-text-primary`           |
| checkbox 容器 | 60 × 60（内嵌真组件，自身不写视觉规则）         |
| radio 容器  | 60 × 60（内嵌真组件，自身不写视觉规则）           |

> icon 本体的所有视觉规格、可用语义清单、形态切换（line / fill）见 [`icon.md`](../icon/icon.md)。
> checkbox / radio 的方框（外圆）/ 选中底色 / 勾（圆点）色等视觉规格，全部见 [`checkbox.md`](../checkbox/checkbox.md) / [`radio.md`](../radio/radio.md)。

---

## 用法

```html
<!-- icon：内嵌独立 .icon 组件（sprite use） -->
<!-- 需要同时引入 ../icon/icon.css -->
<span class="list-leading list-leading--icon">
  <svg class="icon" aria-hidden="true">
    <use href="#icon/wifi--fill"/>
  </svg>
</span>

<!-- checkbox：list-leading 提供 60×60 占位，.checkbox 提供方框/勾/状态 -->
<!-- 需要同时引入 ../checkbox/checkbox.css -->
<span class="list-leading list-leading--checkbox">
  <label class="checkbox">
    <input type="checkbox" />
    <span class="checkbox__box" aria-hidden="true">
      <svg viewBox="0 0 36 36" fill="none">
        <path d="M9 18l6 6 12-12" stroke="currentColor" stroke-width="3.5"
              stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </span>
  </label>
</span>

<!-- radio：list-leading 提供 60×60 占位，.radio 提供外圆/圆点/状态 -->
<!-- 需要同时引入 ../radio/radio.css -->
<span class="list-leading list-leading--radio">
  <label class="radio">
    <input type="radio" name="row-theme" checked />
    <span class="radio__box" aria-hidden="true">
      <span class="radio__dot"></span>
    </span>
  </label>
</span>

<!-- none：直接不渲染 .list-leading 节点即可 -->
```

---

## 自检清单

- [x] 容器 `display:inline-flex` / `align-items:center` / `justify-content:center` / `flex:none`
- [x] 容器自身无背景 / 无圆角 / 无 padding
- [x] icon type 60×60 容器，内部使用独立 `.icon` 组件（sprite, 36×36）
- [x] checkbox type 仅写 60×60 占位，**不重复**写勾/方框样式
- [x] radio type 仅写 60×60 占位，**不重复**写外圆/圆点样式
- [x] none type 占 0×0，可直接省略节点
- [x] 颜色仅使用语义 token，不引用 palette

---

## 与上层组件的关系

| 上层使用方     | 用法                                              |
| -------------- | ------------------------------------------------- |
| `list`（行项） | 列表行 row 的左槽（与 list-content / list-action 同级）|
| 其它          | 暂未直接使用                                      |

---

## 与 Figma 的关系

- Figma 当前 `list` 主组件（388:17634）左侧 icon 是**固定** 60×60 线形 icon，未做 component set 拆分。
- HTML 端的 `list-leading` 是**前置抽象**：把左侧位独立成槽，预留 type 维度。
- 后续 Figma 把左槽拆成 component set 后，HTML 这里的 type 直接 1:1 对齐即可。

---

## 变更日志

| 日期       | 变更                                                          | 说明                                              |
| ---------- | ------------------------------------------------------------- | ------------------------------------------------- |
| 2026-05-18 | 首版                                                          | 三个 type：none / icon / checkbox（内联实现）     |
| 2026-05-18 | checkbox 解耦                                                  | checkbox 视觉真源迁至独立 `checkbox.css`，本组件仅 60×60 占位，消除分叉 |
| 2026-05-18 | 新增 radio type                                                | 复用相同结构（60×60 占位 + 内嵌独立 `.radio`），视觉真源在 `radio.css` |
| 2026-05-19 | icon 解耦                                                       | icon 视觉真源迁至独立 `icon.svg` sprite + `icon.css`（共 11 个语义 × 2 form），消除手画 svg 分叉；本组件仍只负责 60×60 占位 |
