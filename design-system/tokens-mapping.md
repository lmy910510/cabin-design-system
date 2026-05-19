# Figma ↔ CSS Token 对照表

> 给 AI（和人）查的字典：在 Figma 看到的变量名 / 样式名，到 HTML 里到底叫什么。
> **强约束**：写 HTML/CSS 时只能用本表里出现过的 CSS 变量；不在本表里的颜色、间距、字体不允许凭感觉写。
> 所有数值均通过 `figma_execute` 直读 `figma.variables.getVariableByIdAsync` 解析得到，与 Figma 真实值 1:1 对齐。

---

## 1. 颜色 - Palette（基础色板）

> 业务不要直接用 Palette；只在组件库内部、或语义层暂未覆盖时使用。
> 命名约定：`--palette-{family}-{step}`，`step ∈ {00,0..10,transparent[-N]}`。
> `white/black` 的 0~9 是不同 alpha 阶梯，10 是不透明纯色，00 是 0 alpha；`coldGray-alpha-{light|dark}` 是带色调的半透明灰阶。

### 1.1 中性色（white / black / coldGray / coldGray-alpha-light/dark）

| Figma 名 | CSS 变量 | 值 |
|---|---|---|
| `white/00..10` | `--palette-white-00` ~ `--palette-white-10` | `rgba(255,255,255,0)` 0% / 6% / 8% / 20% / 35% / 42% / 54% / 60% / 70% / 80% / 90% / `#ffffff` |
| `black/00..10` | `--palette-black-00` ~ `--palette-black-10` | `rgba(0,0,0,0)` 0% / 4% / 8% / 14% / 30% / 42% / 50% / 60% / 70% / 80% / 90% / `#000000` |
| `coldGray/0..10` | `--palette-cold-gray-0` ~ `-10` | `#f7f8fa` `#eaedf2` `#dee2e8` `#d0d5dc` `#b4bac5` `#969eab` `#828997` `#697181` `#444c5c` `#1f2229` `#16181d` |
| `coldGray-alpha-light/00..10` | `--palette-cold-gray-alpha-light-{00,0..10}` | 基色 `#dee0e8` × {0, 4%, 8%, 14%, 30%, 42%, 50%, 60%, 70%, 80%, 90%, 100%} |
| `coldGray-alpha-dark/00..10` | `--palette-cold-gray-alpha-dark-{00,0..10}` | 基色 `#4b526b` × {0, 4%, 8%, 14%, 30%, 42%, 50%, 60%, 70%, 80%, 90%, 100%} |

### 1.2 彩色族（11 档主色 + transparent 收尾）

| Figma 族 | CSS 前缀 | 0 | 5 (主色) | 10 |
|---|---|---|---|---|
| `blue/0..10` | `--palette-blue-{0..10}` | `#eff6ff` | `#367bf6` | `#071854` |
| `teal/0..10` | `--palette-teal-{0..10}` | `#effefc` | `#4fe7ec` | `#064558` |
| `red/0..10` | `--palette-red-{0..10}` | `#fff0f1` | `#ff293b` | `#3d0005` |
| `green/0..10` | `--palette-green-{0..10}` | `#f0fff4` | `#00b83d` | `#134228` |
| `orange/0..10` | `--palette-orange-{0..10}` | `#ffefe5` | `#fa6800` | `#5c1d00` |
| `yellow/0..10` | `--palette-yellow-{0..10}` | `#fff9e0` | `#ffd000` | `#4d2f00` |
| `indigo/0..10` | `--palette-indigo-{0..10}` | `#e5e5ff` | `#3636f5` | `#05054d` |
| `purple/0..10` | `--palette-purple-{0..10}` | `#f0e9fd` | `#7729ff` | `#170040` |
| `magenta/0..10` | `--palette-magenta-{0..10}` | `#ffe8f8` | `#e525a5` | `#451735` |
| `pink/0..10` | `--palette-pink-{0..10}` | `#ffe8f0` | `#f5276c` | `#5c0522` |
| `lime/0..10` | `--palette-lime-{0..10}` | `#f8ffe8` | `#96d443` | `#1e2f0a` |

> 中间档省略，详见 `tokens.css`。

### 1.3 transparent 变体

| Figma 名 | CSS 变量 | 值 |
|---|---|---|
| `blue/transparent` | `--palette-blue-transparent` | `rgba(54,123,246,0.12)` |
| `teal/transparent` | `--palette-teal-transparent` | `rgba(79,231,236,0.12)` |
| `red/transparent0` | `--palette-red-transparent-0` | `rgba(255,41,59,0.12)` |
| `red/transparent1` | `--palette-red-transparent-1` | `rgba(255,41,59,0.30)` |
| `green/transparent` | `--palette-green-transparent` | `rgba(0,184,61,0.12)` |
| `orange/transparent` | `--palette-orange-transparent` | `rgba(250,104,0,0.12)` |
| `yellow/transparent` | `--palette-yellow-transparent` | `rgba(255,208,0,0.12)` |
| `indigo/transparent` | `--palette-indigo-transparent` | `rgba(54,54,245,0.12)` |
| `purple/transparent` | `--palette-purple-transparent` | `rgba(119,41,255,0.12)` |
| `magenta/transparent` | `--palette-magenta-transparent` | `rgba(229,37,165,0.12)` |
| `pink/transparent` | `--palette-pink-transparent` | `rgba(245,39,108,0.06)` |
| `lime/transparent` | `--palette-lime-transparent` | `rgba(150,212,67,0.12)` |

> red 有两档（`-0` 12% 用于背景，`-1` 30% 用于失能警告文字）；其它族只有一档 12%。

---

## 2. 颜色 - Semantic（语义色，业务唯一入口）

> 业务用色一律走 Semantic 层。Light/Dark 自动切换，写法：`color: var(--color-text-primary);`。
> 切主题：在 `<html>` 上加 `data-theme="light"` 或 `data-theme="dark"`，默认是 light。

### 2.1 Text 文字色

| Figma 名 | CSS 变量 | Light 来源 | Dark 来源 |
|---|---|---|---|
| `text/primary` | `--color-text-primary` | coldGray-9 `#1f2229` | white-9 |
| `text/secondary` | `--color-text-secondary` | coldGray-6 `#828997` | white-6 |
| `text/tertiary` | `--color-text-tertiary` | coldGray-4 `#b4bac5` | white-4 |
| `text/primary_inverse` | `--color-text-primary-inverse` | white-9 | coldGray-9 |
| `text/secondary_inverse` | `--color-text-secondary-inverse` | white-6 | coldGray-6 |
| `text/tertiary_inverse` | `--color-text-tertiary-inverse` | white-4 | coldGray-4 |
| `text/warning` | `--color-text-warning` | red-5 `#ff293b` | red-4 |
| `text/link` | `--color-text-link` | blue-5 `#367bf6` | blue-4 |
| `text/price` | `--color-text-price` | orange-5 `#fa6800` | orange-4 |
| `text/positive` | `--color-text-positive` | green-6 `#0da841` | green-5 |
| `text/disabled` | `--color-text-disabled` | coldGray-3 | (Dark 未单独定义) |
| `text/disabled_inverse` | `--color-text-disabled-inverse` | white-8 | (Dark 未单独定义) |
| `text/disabled_warning` | `--color-text-disabled-warning` | red-transparent-1 (red-5 30%) | (Dark 未单独定义) |

### 2.2 Bg 背景色

| Figma 名 | CSS 变量 | Light 来源 | Dark 来源 |
|---|---|---|---|
| `bg/page_primary` | `--color-bg-page-primary` | white-10 `#fff` | coldGray-10 `#16181d` |
| `bg/page_secondary` | `--color-bg-page-secondary` | coldGray-0 `#f7f8fa` | coldGray-9 `#1f2229` |
| `bg/container_primary` | `--color-bg-container-primary` | white-10 | coldGray-9 |
| `bg/container_secondary` | `--color-bg-container-secondary` | coldGray-0 | (未单独定义) |
| `bg/card_primary` | `--color-bg-card-primary` | white-10 | coldGray-9 |
| `bg/card_secondary` | `--color-bg-card-secondary` | coldGray-0 | coldGray-8 |
| `bg/item_primary` | `--color-bg-item-primary` | **coldGray-9** `#1f2229`（深色，强调按钮/Switch on track）| **white-9** |
| `bg/item_secondary` | `--color-bg-item-secondary` | coldGray-2 | (未单独定义) |
| `bg/item_tertiary` | `--color-bg-item-tertiary` | coldGray-1 `#eaedf2` | (未单独定义) |
| `bg/item_quaternary` | `--color-bg-item-quaternary` | coldGray-0 | (未单独定义) |
| `bg/item_disabled` | `--color-bg-item-disabled` | coldGray-2 `#dee2e8` | (未单独定义) |
| `bg/item_elevated` | `--color-bg-item-elevated` | white-10 | (未单独定义) |
| `bg/overlay` | `--color-bg-overlay` | black-7 (70%) | black-7 (70%) |
| `bg/transparent` | `--color-bg-transparent` | white-5 (54%) | (未单独定义) |
| `bg/warning` | `--color-bg-warning` | red-5 | red-5 |
| `bg/warning_tansparent` | `--color-bg-warning-transparent` | red-transparent-0 (red-5 12%) | red-transparent-0 |
| `bg/link` | `--color-bg-link` | blue-transparent (12%) | blue-transparent |
| `bg/price` | `--color-bg-price` | orange-5 | orange-4 |
| `bg/price_transparent` | `--color-bg-price-transparent` | orange-transparent (12%) | orange-transparent |
| `bg/positive` | `--color-bg-positive` | green-transparent (12%) | green-transparent |

> Figma 原变量名拼成 `bg/warning_tansparent`（缺一个 r），CSS 端规范化为 `--color-bg-warning-transparent` 以可读为准。

### 2.3 Border 描边色

| Figma 名 | CSS 变量 | Light 来源 | Dark 来源 |
|---|---|---|---|
| `border/primary` | `--color-border-primary` | coldGray-3 | white-3 |
| `border/secondary` | `--color-border-secondary` | coldGray-2 | white-2 |
| `border/tertiary` | `--color-border-tertiary` | coldGray-1 | white-1 |
| `border/active` | `--color-border-active` | coldGray-9 | white-9 |
| `border/disabled` | `--color-border-disabled` | coldGray-2 | (未单独定义) |
| `border/elevated` | `--color-border-elevated` | white-10 | (未单独定义) |
| `border/warning` | `--color-border-warning` | red-5 | red-4 |
| `border/link` | `--color-border-link` | blue-5 | blue-4 |
| `border/price` | `--color-border-price` | orange-5 | orange-4 |
| `border/positive` | `--color-border-positive` | green-6 | green-5 |

> "未单独定义"= Figma 在 Dark 模式下没给该变量配 alias，组件渲染时实际取上层 fallback；CSS 这边不复写，让它自动继承 Light 的引用。

---

## 3. 间距 - Spacing

| Figma 名 | CSS 变量 | 值 (px) |
|---|---|---|
| `space/none` | `--space-none` | 0 |
| `space/hairline` | `--space-hairline` | 3 |
| `space/xxs` | `--space-xxs` | 6 |
| `space/xs` | `--space-xs` | 12 |
| `space/sm` | `--space-sm` | 18 |
| `space/md` | `--space-md` | 24 |
| `space/lg` | `--space-lg` | 30 |
| `space/xl` | `--space-xl` | 36 |
| `space/2xl` | `--space-2xl` | 42 |
| `space/3xl` | `--space-3xl` | 48 |
| `space/4xl` | `--space-4xl` | 54 |
| `space/5xl` | `--space-5xl` | 60 |
| `space/6xl` | `--space-6xl` | 66 |
| `space/7xl` | `--space-7xl` | 72 |
| `space/8xl` | `--space-8xl` | 78 |
| `space/9xl` | `--space-9xl` | 84 |
| `space/10xl` | `--space-10xl` | 90 |
| `space/11xl` | `--space-11xl` | 96 |
| `space/12xl` | `--space-12xl` | 114 |
| `space/13xl` | `--space-13xl` | 126 |
| `space/14xl` | `--space-14xl` | 162 |
| `space/15xl` | `--space-15xl` | 192 |
| `space/16xl` | `--space-16xl` | 240 |
| `space/full` | `--space-full` | 999（用于胶囊圆角）|

> 规范：6 的倍数为基础步进（hairline=3 是细线特例），AI 在写代码时如果对间距数值有犹豫，向 6 的倍数对齐基本不会错。

---

## 4. 字体 - Text Styles

> 详细 class 名和字体属性见 `text-styles.css`，本节只列对照。命名 = `t-{family}-{size}-{weight}[-折行]`。
> 字体族：`Noto Sans SC`（中文）/ `DIN`（数字、英文）。

| Figma 名 | CSS class | 字号 | 行高 | 字重 |
|---|---|---|---|---|
| `Noto Sans SC / 72 Bold` | `.t-cn-72-bold` | 72 | 78 | 700 |
| `Noto Sans SC / 60 Bold` | `.t-cn-60-bold` | 60 | 66 | 700 |
| `Noto Sans SC / 48 Bold` | `.t-cn-48-bold` | 48 | 54 | 700 |
| `Noto Sans SC / 42 Bold` / `Medium` | `.t-cn-42-{bold,medium}` | 42 | 48 | 700/500 |
| `Noto Sans SC / 42 Bold _折行` | `.t-cn-42-bold-wrap` | 42 | 54 | 700 |
| `Noto Sans SC / 36 Bold` / `Medium` | `.t-cn-36-{bold,medium}` | 36 | 42 | 700/500 |
| `Noto Sans SC / 32 {Bold,Medium,Regular}` | `.t-cn-32-{bold,medium,regular}` | 32 | 36 | 700/500/400 |
| `Noto Sans SC / 32 Medium _折行` | `.t-cn-32-medium-wrap` | 32 | 48 | 500 |
| `Noto Sans SC / 28 {Bold,Medium,Regular}` | `.t-cn-28-{bold,medium,regular}` | 28 | 32 | 700/500/400 |
| `Noto Sans SC / 28 Regular _折行` | `.t-cn-28-regular-wrap` | 28 | 36 | 400 |
| `Noto Sans SC / 24 {Bold,Medium,Regular}` | `.t-cn-24-{bold,medium,regular}` | 24 | 28 | 700/500/400 |
| `Noto Sans SC / 24 Regular _折行` | `.t-cn-24-regular-wrap` | 24 | 32 | 400 |
| `Noto Sans SC / 20 {Bold,Medium,Regular}` | `.t-cn-20-{bold,medium,regular}` | 20 | 24 | 700/500/400 |
| `Noto Sans SC / 18 {Medium,Regular}` | `.t-cn-18-{medium,regular}` | 18 | 22 | 500/400 |
| `Noto Sans SC / 16 {Medium,Regular}` | `.t-cn-16-{medium,regular}` | 16 | 20 | 500/400 |
| `Noto Sans SC / 14 {Medium,Regular}` | `.t-cn-14-{medium,regular}` | 14 | 18 | 500/400 |
| `Noto Sans SC / 12 {Medium,Regular}` | `.t-cn-12-{medium,regular}` | 12 | 16 | 500/400 |
| `Noto Sans SC / 10 {Medium,Regular}` | `.t-cn-10-{medium,regular}` | 10 | 14 | 500/400 |
| `DIN / {72,60,48,42,36,32,28,24,20,18,16,14,12,10}` × `{Bold,Medium,Regular}` | `.t-num-{size}-{weight}` | 同左 | 与中文档同步 | 700/500/400 |

> `_折行` = 用于多行段落的版本，行高被加大 ~1.28x；单行 UI 文案用普通版即可。

---

## 5. 阴影 - Effect Styles

| Figma 名 | CSS 变量 | 备注 |
|---|---|---|
| `Shadow / Light` | `--shadow-light` | 单层，y6 r18 black 5% |
| `Shadow / Medium` | `--shadow-medium` | 双层，y6 r30 6% + y6 r6 2% |
| `Shadow / Heavy` | `--shadow-heavy` | 双层，y18 r78 6% + y6 r18 4% |

详细参数见 `shadows.css`。

---

## 6. 命名规则速记

```
颜色（基础）   --palette-{family}-{step}                e.g. --palette-blue-5
颜色（语义）   --color-{role}-{variant}                 e.g. --color-text-primary
间距           --space-{size}                           e.g. --space-md
字体           .t-{cn|num}-{size}-{weight}[-wrap]       e.g. .t-cn-32-medium
阴影           --shadow-{light|medium|heavy}            e.g. --shadow-medium
```

只要 AI 写代码时遵守这五条命名规则、并对照本表挑值，HTML 端的输出就基本不会偏离 Figma 规范。

---

## 7. 校准记录（重要）

本表 v2 与 v1 相比有以下关键修正（v1 是 AI 推断、v2 是 Figma 真值）：

- `coldGray` 全系列：v1 推断的 hex 不准确，v2 已用真值（`-9 = #1f2229`、`-10 = #16181d` 等）。
- `bg/item_primary` Light：v1 写成 white-0，v2 真值是 **coldGray-9（深色）**。这意味着主按钮/Switch on 状态背景在浅色主题下是深色块，符合车机 HMI 习惯。
- `bg/page_primary` Light：v1 写成 coldGray-0，v2 真值是 **white-10（纯白）**。
- `bg/item_disabled` Light：v1 写成 coldGray-1，v2 真值是 **coldGray-2（#dee2e8）**。
- `bg/link` / `bg/positive` Light：v1 写成实色（red-5、green-5），v2 真值是对应 transparent (12%)。
- `text/disabled_warning`：v1 推断为 red-2，v2 真值是 red-transparent-1 (red-5 30% alpha)。
- `border/active`：v1 写成 blue-5，v2 真值是 **coldGray-9（中性强调）**。
- `border/elevated`：v1 用 alpha-light，v2 真值是 white-10。
- `red-5`：v1 `#e02d2d`，v2 `#ff293b`。`green-5`：v1 `#1f9854`，v2 `#00b83d`。等等。

⚠️ AI 在写组件时一切以 v2 (本表) 为准。
