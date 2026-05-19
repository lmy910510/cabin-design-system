# list-action（列表行右侧操作区）

> 1:1 映射自 Figma「🤖座舱组件库-AI版 / list-action」Component Set `388:17408`

list-action 是一个**轻量包装器**，统一 list-row 右侧操作区的三种形态：箭头进入、按钮、开关。它本身只负责水平布局与 12px 间距，**真正的视觉来自内部嵌入的 Button / Switch 组件**。

---

## 三种 variant

| Type    | 子内容              | 容器高度 | 典型用途                                  |
| ------- | ------------------- | -------- | ----------------------------------------- |
| Arrow   | subtitle + 右箭头   | 42       | 跳转下级页面（"通用 → 关于本机"）         |
| Button  | 单个 `.btn`         | 60+      | 行内 CTA（"立即更新"、"立即续费"）        |
| Switch  | 单个 `.switch`      | 60       | 开关项（"WLAN"、"蓝牙"）                  |

> 容器自身无背景、无圆角、无内边距；高度由内部子组件 hug。

---

## 关键尺寸 / 颜色（Figma 真值）

| 项目           | 值                                                         |
| -------------- | ---------------------------------------------------------- |
| 子项间 gap     | **12px**（spacing token VariableID 20:491）                 |
| 主轴对齐       | flex-start                                                 |
| 交叉轴对齐     | center                                                     |
| 副标题字号     | 28 / 28 / Regular（Noto Sans SC）                           |
| 副标题色       | `--color-text-secondary` = `#828997`                       |
| 箭头容器尺寸   | 42 × 42                                                    |
| 箭头颜色       | 与副标题同 `--color-text-secondary`                         |
| 箭头形态       | 线形（视觉真源 = 独立 [`icon`](../icon/icon.md) 组件 sprite `icon/enter--line`，1:1 Figma component set `icon/enter` 形态=线形） |

---

## 用法

```html
<!-- Arrow：跳转 -->
<span class="list-action list-action--arrow">
  <span class="list-action__subtitle">已连接</span>
  <span class="list-action__arrow" aria-hidden="true">
    <svg fill="currentColor"><use href="#icon/enter--line" /></svg>
  </span>
</span>

<!-- Button：行内 CTA -->
<span class="list-action list-action--button">
  <button class="btn btn--secondary btn--xs">按钮</button>
</span>

<!-- Switch：开关 -->
<span class="list-action list-action--switch">
  <label class="switch">
    <input type="checkbox" checked />
    <span class="switch-track"></span>
    <span class="switch-thumb"></span>
  </label>
</span>
```

### 默认子组件 variant（与 Figma instance 一致）

| Type    | 默认嵌入                                          |
| ------- | ------------------------------------------------- |
| Arrow   | subtitle + sprite [`icon/enter--line`](../icon/icon.md) 42×42 |
| Button  | `Variant=Secondary, Size=Compact-60, Icon-Position=None, State=Default` |
| Switch  | `On=False, Disabled=False`                        |

业务可在保持容器结构不变的前提下，把内嵌的 `.btn` / `.switch` 换成任意合法 variant（例如 Primary 大按钮、disabled switch 等）。

---

## 自检清单

- [x] 容器 `display:inline-flex` + `align-items:center` + `gap:12px` —— 与 Figma `lm=HORIZONTAL, gap=12, ca=CENTER` 1:1
- [x] 容器无背景 / 无圆角 / 无 padding —— 与 Figma `fills=[]`、`pad=0` 1:1
- [x] 副标题 28/28 Regular `--color-text-secondary` —— 与 Figma 真值 1:1
- [x] 箭头 42 × 42、线形、`currentColor=text/secondary` —— 1:1
- [x] Button 默认 Secondary Compact-60 —— 与 Figma instance `mc=6:57` 1:1
- [x] Switch 默认 Off / 启用 —— 与 Figma instance `mc=257:5526` 1:1

---

## 与上层组件的关系

| 上层使用方     | 用法                                                          |
| -------------- | ------------------------------------------------------------- |
| `list`（行项） | 列表行 row 的右侧固定槽位（行内 `flex-end` 对齐）              |
| TopBar         | 不直接使用；TopBar 右侧用独立 `.btn btn--icon-only btn--md`   |
| SideMenu       | 不使用                                                        |

---

## 变更日志

| 日期       | 变更 | 说明 |
| ---------- | ---- | ---- |
| 2026-05-18 | 首版 | 1:1 还原 Figma `list-action` 三 variant；复用已落地的 `.btn` 与 `.switch` |
| 2026-05-19 | icon 解耦 | Arrow chevron 视觉真源改为独立 [`icon`](../icon/icon.md) 组件 sprite `icon/enter--line`（Figma 真值是楔形实心 polygon，与旧版本手画的 `stroke` 折线 chevron 存在视觉差异，属向 Figma 真源对齐的修正） |
