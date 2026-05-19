# Figma 规范库出图经验总结

## 背景

本项目目标是：先理解座舱组件库规范，再根据需求通过 Figma Console MCP 在目标 Figma 画布中生成符合规范的设计稿。

本次实践中，第一次出图虽然视觉上参考了组件库，但没有严格复用组件库的组件、styles 和 variables。经用户指出后，进行了补救绑定，并形成以下经验，作为后续出图前的强制检查清单。

---

## 已暴露的问题

### 1. 只做了“视觉参考”，没有做到“规范绑定”

第一次生成车机设置页时，部分区域是用基础图层手绘完成的：

- TopBar
- SideMenu
- list 主体
- 标题、描述、标签、分割线、背景卡片等

这些节点使用了硬编码颜色、字号、描边、阴影，没有绑定规范库中的 variable 和 style。

### 2. 组件复用不充分

第一次只成功复用了：

- Button
- Switch
- Pagination

但以下内容没有直接复用组件实例，或没有在手绘 fallback 后补齐规范绑定：

- TopBar
- SideMenu
- SideMenu / Item
- list

如果组件不能跨文件导入，也不能直接手绘完就结束，必须说明原因，并用规范变量和样式重建。

### 3. 没有先拉取并核对规范资源

第一次出图前没有完整确认规范库资源，后续才发现组件库中实际存在：

- variables：259 个
- variable collections：
  - 🎨 Palette
  - 🌈 Semantic
  - 📐 Spacing
- styles：65 个，包括文字样式和阴影样式

正确流程应当是在出图前先确认并使用这些资源。

### 4. Figma API 使用细节踩坑

在绑定过程中发现：

- `textStyleId` 在 dynamic-page 环境下不能直接同步赋值，需要使用 `node.setTextStyleIdAsync(...)`。
- `fills` / `strokes` 不能直接用 `node.setBoundVariable('fills', variable)` 绑定颜色变量。
- 正确做法是使用 `figma.variables.setBoundVariableForPaint(paint, 'color', variable)`，再把 paint 写回 `node.fills` 或 `node.strokes`。
- 组件实例内部可能仍存在硬编码子层，但不应随意 detach 修改组件内部结构，除非用户明确要求。

---

## 后续一次性出图 SOP

### Step 1：确认目标文件和目标画布

出图前必须确认：

- 当前活动文件是否是用户指定的目标文件。
- 当前 page 是否正确。
- 目标画布是否已有内容，避免覆盖已有设计。

### Step 2：读取规范库组件

根据用户需求，先搜索并确认相关组件是否可用：

- 主框架组件，如 TopBar、SideMenu、AppFrame
- 内容组件，如 list、Input、Button、Switch、Pagination
- 图标组件
- 组件 description 中的使用规则、变体、结构、尺寸、状态说明

如果组件可导入，优先实例化组件。

如果组件不可跨文件导入，必须：

1. 说明不可直接导入的原因。
2. 按组件 description 和视觉结构重建。
3. 对重建节点绑定规范 style 和 variable。

### Step 3：读取规范库 styles 和 variables

出图前必须检查并使用：

- Semantic 颜色变量：
  - text/primary
  - text/secondary
  - text/tertiary
  - text/primary_inverse
  - bg/page_primary
  - bg/container_primary
  - bg/card_primary
  - bg/item_primary
  - bg/item_secondary
  - bg/item_disabled
  - border/primary
  - border/secondary
  - border/tertiary
  - border/active
  - border/disabled
- Palette 颜色变量：
  - orange/6 等品牌或功能色
- Spacing 变量：
  - 如后续布局需要，应优先使用 spacing token
  - **重要**：自我发挥布局（页面级排版、标题区、卡片内边距、模块间距等）时，间距也必须遵循组件库 📐 Spacing variable 提供的规范取值，不要凭 web/移动端直觉给数字。具体每个 token 对应的使用场景以 Spacing variable 在 Figma 中的 description 为准（用户会持续补充），出图前要去 variable 里读最新的 description 再决定用哪一档。
- Text styles：
  - 普通字体_Notosans/大字号03_48/bold
  - 普通字体_Notosans/中字号01_42/bold / medium / regular
  - 普通字体_Notosans/中字号03_32/bold / medium / regular
  - 普通字体_Notosans/中字号04_28/medium / regular
  - 普通字体_Notosans/中字号05_24/medium / regular
  - 小字号相关样式
- Effect styles：
  - Shadow/Light
  - Shadow/Medium
  - Shadow/Heavy

### Step 4：生成设计稿

生成时遵循优先级：

1. 优先复用组件库组件实例。
2. 组件不可导入时，按 description 重建结构。
3. 所有自绘节点必须绑定 variable / style。
4. 不允许用硬编码颜色、字号、阴影作为最终结果。
5. 图层命名需要可读，方便后续自查和调整。

### Step 5：绑定规范资源

自绘节点必须绑定：

- 背景色：绑定 bg/* 变量
- 文本颜色：绑定 text/* 变量
- 描边：绑定 border/* 变量
- 强调色：绑定功能色或语义色变量
- 文本样式：绑定 text style
- 阴影：绑定 effect style

Figma Console MCP 中推荐实现方式：

```js
// 文本样式绑定
await node.setTextStyleIdAsync(style.id);

// 颜色变量绑定
const boundPaint = figma.variables.setBoundVariableForPaint(
  paint,
  'color',
  variable
);
node.fills = [boundPaint];
```

### Step 6：出图后强制自查

回复用户“已完成”前，必须跑自查：

- 当前活动文件是否正确。
- 生成 frame 是否存在。
- 自绘节点硬编码文字样式是否为 0。
- 自绘节点硬编码颜色/描边是否为 0。
- 自绘节点硬编码阴影是否为 0。
- 文字是否都绑定 text style。
- 文字颜色是否都绑定 variable。
- 背景、卡片、分割线、状态色是否都绑定 variable。
- 是否仍有组件实例内部硬编码，如果有，区分是组件内部还是自绘节点。
- 截图检查是否有重叠、截断、错位、视觉异常。

### Step 6.5：图标调用 SOP

调用图标时（无论是替换占位矩形还是新增图标），按以下顺序执行，避免无效搜索和清单穷举：

1. **先查仓库里的对照表**：`/图标语义对照表.md`
   - 业务场景 → 推荐图标的映射在这里维护
   - 命中 → 直接用，跳到第 4 步
2. **未命中 → 同义词扩展并行搜**
   - 不要一个关键词一个关键词试
   - 一次性想好 5~10 个同义词（中英文混合）并行调用 `figma_search_components`
   - 例：搜"驾驶辅助"时，同时搜 `驾驶 / 辅助 / ADAS / autopilot / sensor / fusion / drive / pilot / lane / cruise`
3. **仍未命中 → 才退回 findAll 拉清单**
   - 切到组件库文件，`figma.root.findAll(n => n.type==='COMPONENT_SET' && n.name.startsWith('icon/'))`
   - **只取 name**，不要一把梭取 description/key（391 个图标全字段返回会膨胀到 ~14k token，只取 name 约 2k token）
   - 在本地（对话上下文里）做语义匹配，挑出 5~10 个候选
   - 再用 `figma_get_component_details` 或二次 findAll 拉这几个候选的 description + variant key
4. **形态选择**
   - 默认用「形态=面形」variant（list row 主图标、CTA、远观识别）
   - 弱信息层（分组标题、辅助操作）用「形态=线形」
5. **回填对照表**
   - 调用完成后，把本次确认的"业务场景 → 图标"映射回填到 `/图标语义对照表.md`
   - 若是"靠近似图标顶替"的情况，同步登记到对照表的"缺失项"区
6. **批量执行**
   - 多个图标替换合并成一次 `figma_execute`，记录每个占位元素的 x/y/w/h/name/parent，替换后还原

### 为什么知识沉淀用 markdown 而不是 Figma Frame

图标对照表、业务场景知识、决策规则等"AI 要反复读"的结构化文本，**统一沉淀为仓库里的 markdown 文件**，不要做成 Figma 文件里的封面 Frame。原因：

| 维度 | markdown | Figma Frame |
|---|---|---|
| 工具调用次数 | 1 次 `read_file` | 3~5 次（navigate + findAll + 解析 text 节点 + 可能截图兜底） |
| Token 消耗 | 表本身字符数（~1.5k token） | 节点元数据膨胀（~10k+ token） |
| 内容结构 | 天然结构化（标题/表格/列表） | 离散 text 节点，AI 要重建结构 |
| 跨文件 | 直接读 | 要 navigate 切文件 |
| 版本管理 | git 可追溯 | 无 |
| AI 自动维护 | 可直接 read → 改 → 写回 | 改文字要逐节点定位，复杂 |
| 跨项目复用 | 拷文件即可 | Figma 文件内嵌，难迁移 |

**结论**：AI 读取 markdown 比读取 Figma Frame 效率高 5~10 倍。Figma 文件本身只承载视觉资源（组件、variable、style），不承载"给 AI 读的知识表"。

例外：如果某张表**主要是给团队设计师人眼看的**，再考虑在 Figma 里做一份"展示用"镜像（人眼看 Figma，AI 读 markdown）。当前工作流是用户 + AI 直接协作，**单 markdown 一份即可**，避免双份同步成本。

### Step 7：向用户说明结果

回复时应明确说明：

- 生成在哪个文件、哪个 page、哪个 frame。
- 复用了哪些组件实例。
- 哪些组件因跨文件不可导入而按规范重建。
- 自查结果：硬编码情况、style / variable 绑定情况、截图检查结果。
- 如果还有未同步或未验证的部分，必须明确说出来。

---

## 本次修正后的达标状态

修正后，对 `车机设置页 - 组件库生成` 的自查结果：

- 自绘节点硬编码文字样式：0
- 自绘节点硬编码颜色 / 描边：0
- 自绘节点硬编码阴影：0
- 文字节点：28 / 28 均绑定规范文字 style
- 文字颜色：28 / 28 均绑定规范文本颜色 variable
- 规范变量绑定节点：已覆盖背景、卡片、文本、边框、标签、菜单状态等
- 规范样式绑定：已覆盖标题、正文、说明文字和 Shadow/Medium
- 仍存在的硬编码 paint：来自 Pagination 组件实例内部图标/栅格子层，不属于手绘节点，未 detach 修改

---

## 后续改进目标

后续继续与用户交流时，要逐步做到：

1. 出图前自动建立组件、style、variable 使用计划。
2. 尽量减少 fallback 手绘。
3. fallback 时也要做到变量和样式全绑定。
4. 每次出图都先自查再汇报。
5. 形成稳定的一次性出图能力，让用户不需要反复提醒“要用组件和规范资源”。
