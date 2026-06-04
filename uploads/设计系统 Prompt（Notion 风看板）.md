# Notion 风格 · HTML 看板 / PPT 设计系统 Prompt

> 复用说明：把下面整段「设计系统」贴到对话里，并补充你要做的具体内容（标题、数据、板块），就能产出与 Double C 数据看板**视觉一致**的 HTML 分享页 / 单页 PPT。
> 这是一套「仿 Notion 文档」的极简数据展示风格：白底、无衬线、彩色 tag、callout、数据库表格、emoji 小节标记。

---

## 一、整体气质（先读这段）

模仿 Notion 在线文档的观感，而非传统网页或炫技 dashboard：

- **克制、留白、信息优先**。白底黑字，单一暖橙作主强调色，其余颜色只在 tag / 卡片 / 进度条里小面积点缀。
- **窄栏阅读**：正文内容区固定最大宽度 ~920px 居中，左右大留白。
- **块（block）结构**：每个小节 = 一个 emoji 标题 + 一句说明 + 一个内容块（图表卡 / 表格 / 卡片组）。
- **绝不出现**：渐变背景、彩色大色块铺底、阴影堆叠、圆角 + 左边框 accent 条、Inter/Roboto/Arial 字体、装饰性 SVG 插画、无意义的图标和数字。
- 数字一律用等宽字体（tabular-nums），保证对齐。

---

## 二、字体

```css
--sans: "Geist", "Noto Sans SC", ui-sans-serif, sans-serif;   /* 正文 / 标题 */
--mono: "JetBrains Mono", ui-monospace, monospace;            /* 所有数字、日期、代码 */
```

Google Fonts 引入：
```html
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500;600&family=Noto+Sans+SC:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

body 基础：`font-size:16px; line-height:1.5; -webkit-font-smoothing:antialiased; font-feature-settings:"ss01";`
数字类 `.num { font-family:var(--mono); font-variant-numeric:tabular-nums; }`

### 字号 / 字重表

| 用途 | 字号 | 字重 | 说明 |
|---|---|---|---|
| 页面大标题 (page title) | 40px | 700 | 字距 -0.01em，行高 1.2 |
| 页面 emoji | 78px | — | 标题上方独立一行 |
| 小节标题 H2 | 24px | 600 | 前缀一个 22px emoji，gap 8px |
| 正文段落 | 15px | 400 | 行高 1.6，色 `--text` |
| KPI 卡数值 | 30px | 600 | mono，字距 -0.01em |
| KPI 卡标签 | 12px | 500 | 色 `--text-muted` |
| 互动彩卡数值 | 32px | 600 | mono |
| 表格正文 | 14px | 400 | |
| 表头 / 属性名 | 12–13px | 500 | 色 `--text-muted` |
| 顶部面包屑 | 14px | 400/500 | |
| 图表坐标轴 | 10px | — | mono，色 `--text-faint` |
| 脚注 | 13px | 400 | 色 `--text-muted` |

---

## 三、颜色 token（直接复制）

```css
:root {
  /* 基础 */
  --bg: #FFFFFF;
  --text: rgb(55, 53, 47);          /* Notion 标志性近黑棕 */
  --text-strong: rgb(32, 30, 26);
  --text-muted: rgba(55, 53, 47, 0.65);
  --text-faint: rgba(55, 53, 47, 0.45);
  --divider: rgba(55, 53, 47, 0.09);
  --hover: rgba(55, 53, 47, 0.04);
  --gray-block: #F7F6F3;            /* 图表卡 / 脚注底色 */
  --gray-block-2: #F1F1EF;          /* 进度条空槽 */

  /* 主强调色 = 暖橙（唯一主色） */
  --orange-text: #D9730D;
  --orange-bg:   #FAEBDD;

  /* 辅助色：仅用于 tag / 卡片 / 进度条，成对使用 text+bg */
  --blue-text: #0B6E99;  --blue-bg: #DDEBF1;
  --green-text:#448361;  --green-bg:#DDEDE3;
  --pink-text: #C14C8A;  --pink-bg: #F4DFEB;
  --purple-text:#6940A5; --purple-bg:#EAE4F2;
  --yellow-text:#D9A11A; --yellow-bg:#FBF3DB;
  --red-text:  #D44C47;  --red-bg:  #FBE4E4;
  --brown-text:#64473A;  --brown-bg:#EEE0DA;

  /* 灰 tag */
  --gray-text-tag: #787774;
  --gray-bg-tag:   #E3E2E0;
}
```

**用色规则：**
- 主色暖橙 `#D9730D` 用于：主折线、主进度条、KPI 主数值、最重要的强调 tag。
- 辅助色**永远成对**（text 配同名 bg），且仅小面积出现在 tag、彩色卡片、互动卡、完播率分级。
- 大面积区域只用白 + `--gray-block` 浅灰。绝不用辅助色铺整块背景。

---

## 四、布局骨架

```
┌ topbar（44px 高，下边框，面包屑 + 右侧 Updated 日期）
│
└ .doc  (max-width:920px; margin:0 auto; padding:64px 96px 120px)
   ├ 页面头：emoji(78px) → 大标题(40px) → 属性栏(Properties)
   ├ 💡 Callout（一句话总结）
   ├ 📊 H2 + 说明 + 内容块
   ├ 📈 H2 + 说明 + 图表卡
   ├ … 各小节 …
   ├ hr 分隔线
   └ 脚注块（浅灰底）
```

- 内容区窄栏 920px，响应式 <900px 时 padding 收到 32px、网格降列。
- 小节之间用 H2 的 `margin-top:24px` 自然分隔；大段落之间可插 `<hr>`（色 `--divider`）。

---

## 五、核心组件配方

### 1. 顶部面包屑 topbar
高 44px，flex 两端对齐，下边框 `1px solid --divider`，14px 灰字。
左：`📁 项目名 / 分类 / 当前页(加粗 --text-strong)`，分隔符 `/` opacity .45。
右：`Share · Updated YYYY/MM/DD`，13px。

### 2. 页面头 Page Header
- emoji 78px 独立一行，`margin-bottom:16px`
- 标题 40px/700
- **属性栏 Properties**：`display:grid; grid-template-columns:120px 1fr; row-gap:4px`，左列属性名（灰 + emoji，如 `📅 周期`），右列值用彩色 tag。

### 3. Tag（Notion 彩签）
```css
.tag { display:inline-block; padding:2px 7px; border-radius:4px; font-size:13px; line-height:1.5; }
```
配色取上面成对 token，如 `.tag-orange { background:var(--orange-bg); color:var(--orange-text); }`。默认灰 tag 用 `--gray-bg-tag/--gray-text-tag`。

### 4. Callout（重点提示块）
```css
.callout { display:flex; gap:12px; padding:14px 16px; border-radius:4px; align-items:flex-start; }
.callout-orange { background:rgba(217,115,13,0.07); border:1px solid rgba(217,115,13,0.18); }
```
左侧一个 22px emoji（💡），右侧 15px 正文、行高 1.65，关键数字用 `<strong>`（600 + --text-strong）。

### 5. KPI 卡片组（Gallery view）
`display:grid; grid-template-columns:repeat(4,1fr); gap:12px`。
单卡：白底 + `1px solid --divider` + `border-radius:6px` + `padding:14px 16px`，hover 轻微双层阴影。
结构：标签(12px/500 灰) → 数值(30px/600 mono) → 副标(12px --text-faint)。
可选强调：`.card-orange/-blue/-green/-pink` 给数值上色 + 极淡同色底（如 `rgba(...,0.45)`）。

### 6. 图表卡 Chart Card
浅灰底容器：`background:var(--gray-block); border-radius:6px; padding:16px 20px`。
内部 SVG 图表，坐标轴文字 10px mono `--text-faint`，网格线 `#EDECE9 / 1px`。
- 柱：单日/次要数据用 `#D9D9D5` 灰柱；主数据用 `--orange-text`。
- 折线：主线 `--orange-text` 1.8–2px；累计/对比线可用 `--blue-text`。
- 面积填充用极淡同色（如订阅累计 `#DDEBF1`）。
- 峰值/里程碑：实心点 + 短引线 + mono 小标注。
- 图例：底部 flex，mono 12px，小色块 18×8 圆角 2px。

### 7. 数据库表格（Notion Table）
上下边框 `1px --divider`，行与列均 `1px --divider` 分隔，列头带类型 emoji（`📅 日期` `# 播放` `📊 进度`）。
行 `min-height:36–42px`，hover 整行 `--hover` 高亮。
进度条：空槽 `--gray-block-2`，填充主橙；可按区间变色（见下）。
可加**可点击排序工具栏**：chip 按钮，选中态 `background:--text-strong; color:#fff`。
表尾可放 `N rows · count` 灰字，强化数据库感。

### 8. 分级着色（如完播率 / 强度）
```
≥50% → --green-text   ≥40% → --blue-text   ≥30% → --orange-text   更低 → --text-faint
```

### 9. 互动彩卡组
`grid repeat(5,1fr); gap:10px`。每卡用一个辅助色的 `*-bg` 作底，`*-text` 给数值和标签上色。结构：emoji(22px) → 标签+说明 → 大数值(32px mono)。

### 10. 热力日历（GitHub 风格）
SVG 方块网格，格 13px、间距 3px、圆角 2px。0 值用 `#EDECE9`，按对数强度分 5 档橙色阶：
`#FAEBDD → #F3CFA1 → #E5A165 → #C77433 → #8E4A1B`。
左侧星期标、顶部月份标用 10px mono；底部「少 □□□□□□ 多」图例。

### 11. 脚注块
浅灰底 `--gray-block` 圆角，13px 灰字，左列标签 `min-width:80px` `--text-faint`；可加一句斜体引言，左边框 3px `--divider`。

---

## 六、间距 / 圆角 / 细节约定

- 圆角：卡片/容器 6px，tag/小元素 4px，进度条 3px。
- 卡片间距 gap 10–12px；小节标题 `margin:24px 0 6px`。
- 分隔线只用 `--divider`（极淡），永不用深色实线。
- hover 反馈统一用 `--hover`（4% 黑）。
- emoji 是这套风格的「图标系统」——小节标题、属性名、callout、表头都用 emoji，不引第三方图标库。
- 数字千分位 `toLocaleString('en-US')`；百分比保留一位小数。

---

## 七、技术骨架

- 单文件 HTML + React 18.3.1 + Babel standalone（inline JSX），脚本用官方 pin 版本 + integrity。
- 数据与组件可拆成独立 `.js / .jsx` 文件，最后在 HTML 末尾按序引入；要做成「发链接可打开」的分享版时，再打包成单个自包含 HTML。
- 响应式：`@media (max-width:900px)` 把 4 列网格降到 2 列、表格列宽收窄、标题字号下调。
- 打印/PDF：加 `@page A4` + `@media print`，隐藏 topbar、强制 `print-color-adjust:exact` 保留彩色、给卡片/表格加 `break-inside:avoid`。

---

## 八、复用时怎么写指令（示例）

> 「用『Notion 风格 HTML 看板设计系统』做一页 XXX 主题的分享页。
> 顶部面包屑：项目 / 分类 / 本页标题；页面 emoji 用 🚀，大标题『……』。
> 板块：① 概览 KPI 4 张卡（指标：…）② 趋势折线 ③ 明细数据库表格（列：…）④ 互动/构成彩卡。
> 沿用既有字体、字号、颜色 token、组件配方，最后打包成可分享单文件 HTML。」

把数据贴上、板块列清楚即可，视觉一致性由这份设计系统保证。
