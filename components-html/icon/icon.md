# icon（座舱图标系统）

> HTML 端统一图标方案。视觉真源为 Figma 「🤖座舱组件库-AI版」中的 `icon/<语义名>` component set，每个 set 含 `形态=线形 / 面形` 两个 variant。HTML 端用 SVG sprite 承载，零构建依赖、零运行时开销，主题色通过 `currentColor` 自动继承。

---

## 方案选型（Why SVG sprite）

| 方案 | 主题色继承 | 静态站点可用 | 缓存 | 评价 |
| --- | --- | --- | --- | --- |
| `<img src=".svg">` | ❌（不能 currentColor） | ✓ | ✓ | 否 |
| Inline svg（每处 path 复制） | ✓ | ✓ | ❌ | 当前的「分叉」做法，要消除 |
| CSS `mask-image` | △（必须单色） | ✓ | ✓ | 强制单色，多色 icon 无法处理 |
| **SVG sprite（`<symbol>` + `<use>`）** | ✓ | ✓ | ✓ | **当前选用** |
| Icon font | ✓ | ✓ | ✓ | 可访问性差，已被业界淘汰 |

选 sprite 的关键理由：
1. **零构建**：项目目前是纯静态 HTML，不引入打包工具
2. **真源唯一**：所有 icon 都在 `icon.svg` 一个文件里，更新只改一处
3. **主题色继承自然**：`<symbol>` 内 path 不写 fill，由外层 `.icon { fill: currentColor }` 接管
4. **同源浏览器缓存**：第一次加载后所有页面命中缓存

---

## 用法

### 1. 引入

```html
<!-- 在 <head> 引入样式 -->
<link rel="stylesheet" href="../icon/icon.css" />

<!-- 在 <body> 顶部加载 sprite loader（同步、无 defer） -->
<script src="../icon/icon-sprite-inline.js"></script>
```

> **为什么不直接 `<use href="../icon/icon.svg#…"/>`**
> 跨文件 SVG sprite 引用在 `file://` 协议下（macOS iCloud 路径含中文/空格的尤其严重）会被浏览器以安全策略拒绝加载，导致图标"看不见"。
> 改为：在页面里通过 `<script src="…icon-sprite-inline.js"></script>` 把 sprite 内容注入到当前文档 `<body>` 顶部一个 `<div style="display:none">` 中，
> 然后所有引用都用同文档锚点 `<use href="#icon/<name>--<form>"/>` —— 这是最稳的零构建方案。

### 2. 写法

```html
<!-- 默认尺寸 36×36 -->
<svg class="icon" aria-hidden="true">
  <use href="#icon/wifi--line"/>
</svg>

<!-- 24×24（tag 前置 icon） -->
<svg class="icon icon--sm" aria-hidden="true">
  <use href="#icon/star--fill"/>
</svg>

<!-- 42×42（input / tooltip 用） -->
<svg class="icon icon--lg" aria-hidden="true">
  <use href="#icon/refresh--line"/>
</svg>
```

### 3. id 命名规则

```
icon/<语义名>--<form>
       └── 等于 Figma component set 名
              └── line | fill（对应 Figma 「形态=线形 | 形态=面形」）
```

---

## 尺寸（icon.css 内 modifier）

| 类名 | 实际尺寸 | 典型场景 | 来源 |
| --- | --- | --- | --- |
| `.icon`（默认） | 36 × 36 | list-leading icon 槽 / list-action chevron | Figma list 36×36 / list-action 36×36 |
| `.icon--sm` | 24 × 24 | tag 前置 icon / Rating-item | Figma tag 24×24 |
| `.icon--xs` | 16 × 16 | （预留） | — |
| `.icon--lg` | 42 × 42 | input 内清除 / 操作 / tooltip 内 icon/info | Figma input/tooltip 42×42 |
| `.icon--xl` | 48 × 48 | sprite 原始 viewBox 尺寸（调试用） | — |

> SVG `viewBox` 一律是 `0 0 48 48`（对齐 Figma 真值）；通过 `width/height` 缩放显示尺寸不会失真。

---

## 形态选择默认规则（与「图标语义对照表」一致）

- **`fill`（面形）**：list row 主图标、CTA 按钮、需要远观识别的强信息层（**默认选这个**）
- **`line`（线形）**：分组标题、辅助操作、密集图标网格中的弱信息层

---

## 当前 sprite 内置图标清单（MVP，12 × 2 = 24 symbol）

> 选取依据：项目当前代码用到的 + 高频通用 icon。后续按需扩充。

| 语义名 | line | fill | Figma node | 业务场景 |
| --- | --- | --- | --- | --- |
| `wifi` | ✓ | ✓ | `60:1376` | WLAN / 无线网 |
| `star` | ✓ | ✓ | `60:575` | 收藏 / 评分（注：Tag 默认 icon 也是 star） |
| `add` | ✓ | ✓ | `60:2812` | 添加 / 加号（线/面同源） |
| `refresh` | ✓ | ✓ | `60:496` | 刷新 / 自动同步 / 数据同步 |
| `close` | ✓ | ✓ | `60:540` | 关闭 / × 按钮（线/面同源） |
| `back` | ✓ | ✓ | `60:830` | 返回 / 上一步（pagination-prev 用） |
| `enter` | ✓ | ✓ | `60:519` | 进入箭头（**= chevron_right**，list-action arrow 用） |
| `down` | ✓ | ✓ | `60:526` | 向下 / 折叠（线/面同源） |
| `up` | ✓ | ✓ | `60:533` | 向上 / 展开（线/面同源） |
| `setting` | ✓ | ✓ | `60:603` | 设置 / 通用 |
| `list` | ✓ | ✓ | `60:2846` | 列表 / 菜单（含定位形 + 三横线，区别于 hamburger） |
| `info` | ✓ | ✓ | `60:3126` | 信息提示 / tooltip 前置 / brand-tag 得来速「车道取餐 无须下车」 |

> Figma `icon/<name>` 的 `形态=面形` ↔ HTML id `icon/<name>--fill`；`形态=线形` ↔ `icon/<name>--line`。

### 库里同形说明

`add` / `close` / `down` / `up` 在 Figma 库里 line 与 fill **路径完全相同**，但本 sprite 仍保留两个 symbol（id 不同），以确保调用方按"形态选择默认规则"语义化引用，将来 Figma 真分形态时只改 sprite 内部 path 即可，调用方代码无需改动。

---

## 与 Figma 的关系 / 一致性策略

- 视觉真源：Figma 「🤖座舱组件库-AI版」 icon component set
- 拉取方式：`figma.exportAsync({format:'SVG_STRING'})` 直接拿矢量
- 一致性保证：
  - viewBox 完全保留 Figma 真值（`0 0 48 48`）
  - path d 属性原样保留
  - **去掉硬编码 fill `#1F2229`**——交给 `currentColor` 继承
  - 一旦 Figma 修订 icon，sprite 重跑同一个 figma_execute 脚本即可整体同步

---

## 缺失项（业务出现需求时补）

> 这一节用来记录「业务用过、库里也有，但 sprite 还没收录」的 icon。

- `icon/feedback`（60:475）反馈
- `icon/clear`（60:482）清除
- `icon/person`（60:489）用户
- `icon/remind`（60:505）提醒
- `icon/search`（60:547）搜索
- `icon/more`（60:554）更多
- `icon/lock`（座舱图标语义对照表已记录）儿童安全锁
- `icon/safety`（同上）隐私保护 / 安全防护
- `icon/voice` 语音助手
- `icon/sensor_fusion` 驾驶辅助 / ADAS
- `icon/fail` 输入框清除 / 失败提示

> 完整库 ~390 个语义 icon，业务侧"需要哪个补哪个"，避免 sprite 体积无谓膨胀。

---

## 与上层组件的关系

| 上层组件 | 用法 |
| --- | --- |
| `list-leading`（type=icon） | 60×60 容器内嵌 `.icon`（36×36），主题色继承 |
| `list-action`（type=arrow） | `chevron_right` → 用 `icon/enter--line` 36×36 |
| `tag`（show icon） | 24×24 前置 icon → `.icon--sm` |
| `button`（icon-position） | 36×36 leading/trailing icon → `.icon` |
| `input`（show clear / show action） | 42×42 → `.icon--lg`（待 input 组件落地） |
| `tooltip`（show icon） | 42×42 → `.icon--lg`（待 tooltip 组件落地） |

---

## 自检清单

- [x] sprite 单文件承载所有 symbol
- [x] symbol id 命名 `icon/<name>--<form>`，与 Figma component set 名一致
- [x] path 不带 fill，由外层 `currentColor` 接管 → 主题色继承
- [x] viewBox 完全对齐 Figma 真值（48×48）
- [x] 4 档尺寸 modifier 覆盖 Figma 全部固定档位（24 / 36 / 42 / 48）
- [x] 装饰性 icon 用 `aria-hidden="true"`；纯 icon 按钮用 `role="img" aria-label="…"`

---

## 与「图标语义对照表」的关系

工作区根目录的 `图标语义对照表.md` 仍是**业务场景 → 图标语义名**的最终查表入口。本组件文档负责回答**如何把语义名渲染出来**。两份文档配合使用：

1. 业务出现新场景 → 查 `图标语义对照表.md`，命中得到"图标语义名"
2. 把语义名套进本文档的写法 → `icon/<语义名>--<form>`
3. 如果 sprite 里还没收录 → 加到「缺失项」清单 + 用 figma_execute 拉真值补进 `icon.svg`，回填本文档的"内置清单"

---

## 文件结构与真源同步

```
components-html/icon/
├── icon.svg                  ← 视觉真源：sprite，22 个 symbol
├── icon-sprite-inline.js     ← icon.svg 的 inline 副本（运行时使用），由脚本生成
├── icon.css                  ← .icon / .icon--xs/sm/lg/xl 尺寸 modifier
├── icon.html                 ← 静态片段示例
└── icon.preview.html         ← 预览页
```

### 真源同步（重要）

`icon-sprite-inline.js` 是 `icon.svg` 内 `<svg>…</svg>` 主体的 JS 内联副本。
**一旦改了 `icon.svg`（增/删/改 symbol），必须立刻重新生成 inline 文件**，命令：

```bash
cd "<工作区根>" && python3 - <<'PY'
import re
p='components-html/icon/icon.svg'
src=open(p,'r',encoding='utf-8').read()
inner=re.search(r'<svg [^>]*>([\s\S]*)</svg>',src).group(1).strip()
inner_js=inner.replace('\\','\\\\').replace('`','\\`').replace('${','\\${')
js=open('components-html/icon/icon-sprite-inline.js').read()
# 替换 ICON_SPRITE_INNER 模板：见现有文件首部脚本，重新写一遍即可
PY
```

> 也可以直接重跑当初的生成命令（见对话记录）。校验方法：
> `grep -o 'icon/[a-z]*--[a-z]*' components-html/icon/icon-sprite-inline.js | sort -u | wc -l`
> 输出应等于 `icon.svg` 内 `<symbol>` 数量。

---

## 变更日志

| 日期 | 变更 | 说明 |
| --- | --- | --- |
| 2026-05-19 | 首版 | 建立 SVG sprite + .icon 容器；MVP 收录 11 个语义 icon × 2 form = 22 symbol；从 Figma `figma_execute` 直读真值，去掉硬编码 fill 改用 currentColor |
| 2026-05-19 | 增 sprite loader | 新增 `icon-sprite-inline.js`，把 sprite 内联到当前文档 `<body>`，所有引用改为同文档锚点 `<use href="#icon/…"/>`，规避 `file://` 跨文件 use 在 macOS iCloud 中文路径下加载失败的问题 |
| 2026-05-19 | button 接入 sprite | button.preview.html / button.html / button.md 全部 leading/trailing/icon-only demo 从手画 svg 迁到 sprite 引用；删除（trash）与分享（share）暂保留临时手画并标注 TODO，等 sprite 收录后再迁 |
| 2026-05-19 | 增 info | 收录 `icon/info`（`60:3126`，**12 × 2 = 24 symbol**），从 Figma `exportAsync(SVG_STRING)` 直读 48×48 真值；fill 形态用 `fill-rule="evenodd"` 实现「实心圆 + 镂空 i」的 boolean subtract 效果。brand-tag 得来速 variant 由原本的「内联 #brand-info-icon 22×22 一次性 symbol」迁到统一调用 `#icon/info--line`（线形、中性提示语义），去掉了一次分叉 |
