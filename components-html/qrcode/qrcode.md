# QRCode · 二维码块

> Figma 节点：`330:13207`（QRCode ComponentSet）
> 8 个 variant：`Type={Circle, Square} × State={Default, Inactive, Loading, Expired}`

## 一句话定位
扫码上车 / 扫码登录用的二维码容器；外框 360×360，padding=24，分圆/方两形态，4 种业务状态。

## 视觉真值

| 维度 | 值 | token / 备注 |
| --- | --- | --- |
| 外框尺寸 | 360 × 360 | — |
| 内边距 | 24 | `--space-md` |
| 内容区 | 312 × 312 | 360 - 24×2 |
| Circle 圆角 | 999 / `full` | `border-radius: 999px` |
| Square 圆角 | 36 / `xl` | `border-radius: 36px` |
| Default 边框 | 3px `border/tertiary` | `--color-border-tertiary`（仅 Default 态有） |
| Default 背景 | `bg/container_primary` | `--color-bg-container-primary`（白） |
| 非 Default 背景 | `bg/item_quaternary` | `--color-bg-item-quaternary` |
| Inactive 文案 | "请勾选下方隐私协议"，32 Medium | `--color-text-primary` |
| Inactive 占位 | 168×168 | `--color-bg-item-secondary`，opacity 0.6 |
| Loading 主体 | 120×120 旋转环 | 纯 CSS（`@keyframes qrcode-spin`） |
| Expired 主体 | 120×120 icon | sprite `#icon/refresh--line` |

## 用法

属性：`data-type`、`data-state` 决定 8 个 variant。

```html
<!-- 真二维码：把内部 .qrcode__pattern 换成 <img> 即可 -->
<div class="qrcode" data-type="square" data-state="default">
  <img class="qrcode__pattern" alt="二维码" src="qr.png" />
</div>

<!-- 加载中 -->
<div class="qrcode" data-type="circle" data-state="loading">
  <div class="qrcode__loading" role="status" aria-label="加载中"></div>
</div>

<!-- 已过期：点击刷新 -->
<div class="qrcode" data-type="circle" data-state="expired">
  <button class="qrcode__refresh" type="button" aria-label="刷新二维码">
    <svg viewBox="0 0 48 48"><use href="#icon/refresh--line"/></svg>
  </button>
</div>

<!-- 未授权（缺隐私协议勾选等） -->
<div class="qrcode" data-type="square" data-state="inactive">
  <p class="qrcode__hint">请勾选下方隐私协议</p>
  <div class="qrcode__placeholder"></div>
</div>
```

## 取舍说明

- Figma Default 态自带的 logo（多层渐变 brand-icon）属于品牌资产，没在通用组件中还原；推荐外接 `<img>` 提供完整二维码图，也可在 `.qrcode__pattern` 上叠 logo
- Loading 不依赖 sprite（库里没注册 `loading--line` symbol），改用 CSS 旋转 ring；视觉效果与 Figma 的 icon/loading 一致
- Default 态的 strokeWeight 在 boundVariable 是 `hairline`(0.5)，但 Figma 显式覆盖为 3px——这里取 **3px**

## 锚点回查

- 真值出处：`330:13207` 8 个 variant 节点（Circle/Default 330:13205、Circle/Inactive 337:13455、Circle/Loading 337:13503、Circle/Expired 337:13740、Square/Default 330:13206、Square/Inactive 337:13494、Square/Loading 337:13630、Square/Expired 337:13746）
- token：`--color-bg-container-primary` / `--color-bg-item-quaternary` / `--color-bg-item-secondary` / `--color-bg-item-tertiary` / `--color-text-primary` / `--color-border-tertiary` / `--space-md`
- 图标：sprite `#icon/refresh--line`
