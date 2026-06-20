# Jouleverse 浏览器 UI 组件库与设计系统建设计划

文档日期：2026-06-19  
文档用途：内部汇报 / 技术选型 / 设计系统规划 / 后续实施依据  
项目范围：`explorer-v2` 后续新功能页面  

> 本文档只描述计划与规范，不代表已经实施代码改造。

## 0. 汇报摘要

<mark>核心结论：后续新功能建议统一使用 Naive UI 作为基础组件库，并在其上建立 Jouleverse 自有设计系统。</mark>

<mark>实施策略：旧功能不主动重构，新功能强制使用设计系统，避免一次性重写带来的风险。</mark>

<mark>视觉方向：可信、清晰、数据密集、轻 Web3，不走重霓虹、重赛博、重营销视觉。</mark>

<mark>关键建设内容：浅色 / 暗色主题、颜色 token、J Logo loading、空状态、异常状态、点击反馈、W3C / WCAG 可访问性、团队接入规范。</mark>

## 1. 项目现状

### 1.1 技术栈

- Vue 3
- Vite
- TypeScript
- Pinia
- Vue Router
- Tailwind CSS 4.2.2
- ethers / viem / wagmi 等链上交互依赖

### 1.2 样式现状

- 当前页面主要依赖自定义 `scoped CSS`。
- 页面中按钮、卡片、表单、状态提示、加载提示存在重复实现。
- `package.json` 中已经声明 Tailwind CSS，但当前入口主要导入 `src/style.css`，Tailwind 的实际使用程度有限。
- 项目缺少统一的浅色 / 暗色主题规范。
- 项目缺少统一的空状态、异常状态、加载状态和链上操作反馈规范。

### 1.3 本轮设计系统目标

- 不主动改旧页面。
- 后续新页面使用统一设计系统。
- 建立一套可复制、可维护、可审查的 UI 规范。
- 降低后续页面开发时重复写 CSS 的成本。
- 提升用户对点击、加载、错误、成功状态的感知。

## 2. 前期 UI 组件库选型

### 2.1 推荐方案

<mark>推荐使用 Naive UI。</mark>

### 2.2 选择 Naive UI 的原因

| 维度 | 结论 |
| --- | --- |
| Vue 兼容性 | Naive UI 面向 Vue 3，适合当前项目。 |
| TypeScript | 组件类型支持完善，适合当前严格 TypeScript 配置。 |
| 视觉风格 | 默认风格简洁，适合区块浏览器这类数据工具。 |
| 组件覆盖 | Button、Input、Card、Tabs、DataTable、Descriptions、Tag、Alert、Dialog、Message、Skeleton、Empty 等能力完整。 |
| 主题能力 | 可通过 `NConfigProvider` 做浅色 / 暗色 / 品牌色主题覆盖。 |
| 渐进接入 | 新页面可以先用，不需要旧页面立刻重写。 |
| 维护成本 | 比纯自定义 CSS 更低，比 headless 方案更省开发时间。 |

### 2.3 对比其他方案

| 方案 | 优点 | 不足 | 是否推荐 |
| --- | --- | --- | --- |
| Naive UI | Vue 3 友好、组件完整、风格轻、主题能力好 | 需要建立品牌 token，避免默认风格过强 | <mark>推荐</mark> |
| Element Plus | 成熟、文档多、后台组件丰富 | 视觉偏传统后台，品牌感较弱 | 不作为首选 |
| shadcn-vue / Reka UI | 自定义能力强，适合高控制力设计系统 | 需要团队维护大量组件样式，初期成本高 | 暂不作为首选 |
| 继续自定义 CSS | 短期无新增依赖 | 长期重复、难维护、状态不统一 | 不推荐 |

## 3. 整体视觉风格

### 3.1 关键词

<mark>可信、清晰、稳定、数据密集、轻 Web3。</mark>

### 3.2 设计原则

- 页面以信息阅读效率为优先。
- 色彩主要用于品牌、状态、点击反馈和重点操作。
- 地址、哈希、金额、交易状态必须易读。
- 不做过度装饰。
- 不做大面积渐变背景。
- 不做复杂营销页构图。
- 不让用户猜测哪里可以点击。

### 3.3 新旧页面策略

<mark>旧功能页面不主动重构。</mark>

后续如果旧页面发生业务修改，再按”触达即迁移”的方式逐步替换局部组件。例如修改某个交易详情页时，优先迁移该页面的按钮、状态标签、空状态、错误提示，而不是一次性全站重写。

### 3.4 样式技术方案策略

<mark>新功能页面：全面使用 Naive UI 组件 + `Jv*` 业务组件 + 设计系统 token，禁止使用 Tailwind CSS 类。</mark>

旧页面：现有 Tailwind 用法冻结保留，触达时随业务修改替换为 Naive UI 组件。

长期目标：全站逐步迁移至 Naive UI，Tailwind 依赖最终退出项目。

## 4. 颜色系统

说明：色块使用 Markdown 内嵌 HTML 渲染，便于汇报时直观看到颜色。

### 4.1 品牌色

| 用途 | 色块 | 色值 | 使用建议 |
| --- | --- | --- | --- |
| Jouleverse Red / 主色 | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#EB1727;vertical-align:middle;"></span> | `#EB1727` | 主按钮、品牌重点、关键操作 |
| 主色 Hover | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#D41422;vertical-align:middle;"></span> | `#D41422` | 主按钮 hover |
| 主色 Pressed | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#B91C1C;vertical-align:middle;"></span> | `#B91C1C` | 主按钮 pressed |
| 主色浅背景 | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#FEF2F2;vertical-align:middle;"></span> | `#FEF2F2` | 浅色主题下的提示背景、选中背景 |

### 4.2 浅色主题

| Token | 色块 | 色值 | 用途 |
| --- | --- | --- | --- |
| `--jv-bg-page` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#F8FAFC;vertical-align:middle;"></span> | `#F8FAFC` | 页面背景 |
| `--jv-bg-surface` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#FFFFFF;vertical-align:middle;"></span> | `#FFFFFF` | 卡片、弹窗、表格背景 |
| `--jv-bg-subtle` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#F1F5F9;vertical-align:middle;"></span> | `#F1F5F9` | 哈希背景、弱区块背景 |
| `--jv-text-primary` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#0F172A;vertical-align:middle;"></span> | `#0F172A` | 主文字 |
| `--jv-text-secondary` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#475569;vertical-align:middle;"></span> | `#475569` | 次级文字 |
| `--jv-text-muted` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#64748B;vertical-align:middle;"></span> | `#64748B` | 弱文字、说明 |
| `--jv-border` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#E2E8F0;vertical-align:middle;"></span> | `#E2E8F0` | 边框 |
| `--jv-link` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#2563EB;vertical-align:middle;"></span> | `#2563EB` | 地址、哈希、跳转链接 |

### 4.3 暗色主题

| Token | 色块 | 色值 | 用途 |
| --- | --- | --- | --- |
| `--jv-bg-page` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#0E0F12;vertical-align:middle;"></span> | `#0E0F12` | 页面背景 |
| `--jv-bg-surface` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#17191D;vertical-align:middle;"></span> | `#17191D` | 卡片、弹窗、表格背景 |
| `--jv-bg-subtle` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#202329;vertical-align:middle;"></span> | `#202329` | 哈希背景、弱区块背景 |
| `--jv-bg-hover` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#262A31;vertical-align:middle;"></span> | `#262A31` | hover 背景 |
| `--jv-text-primary` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#F4F4F5;vertical-align:middle;"></span> | `#F4F4F5` | 主文字 |
| `--jv-text-secondary` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#D4D4D8;vertical-align:middle;"></span> | `#D4D4D8` | 次级文字 |
| `--jv-text-muted` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#A1A1AA;vertical-align:middle;"></span> | `#A1A1AA` | 弱文字、说明 |
| `--jv-border` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#2F343D;vertical-align:middle;"></span> | `#2F343D` | 边框 |
| `--jv-link` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#7DD3FC;vertical-align:middle;"></span> | `#7DD3FC` | 地址、哈希、跳转链接 |

### 4.4 状态色

| 状态 | 浅色主题色块 | 浅色值 | 暗色主题色块 | 暗色值 | 用途 |
| --- | --- | --- | --- | --- | --- |
| 成功 | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#16A34A;vertical-align:middle;"></span> | `#16A34A` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#4ADE80;vertical-align:middle;"></span> | `#4ADE80` | 成功、已确认、在线 |
| 警告 | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#D97706;vertical-align:middle;"></span> | `#D97706` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#FBBF24;vertical-align:middle;"></span> | `#FBBF24` | 等待、风险提示 |
| 错误 | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#DC2626;vertical-align:middle;"></span> | `#DC2626` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#F87171;vertical-align:middle;"></span> | `#F87171` | 失败、错误、拒绝 |
| 信息 | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#2563EB;vertical-align:middle;"></span> | `#2563EB` | <span style="display:inline-block;width:18px;height:18px;border-radius:4px;border:1px solid #CBD5E1;background:#60A5FA;vertical-align:middle;"></span> | `#60A5FA` | 信息、处理中 |

### 4.5 可读性要求

<mark>正文文本对比度至少满足 WCAG AA 的 4.5:1，大字号至少 3:1，图标、边框、焦点态等非文本视觉元素至少 3:1。</mark>

状态不能只依赖颜色表达，必须配合文字、图标或结构提示。例如交易失败不能只显示红色，还需要明确显示“交易失败”。

## 5. J Logo Loading 动效计划

### 5.1 目标

<mark>使用 Jouleverse 的 “J” 符号建立统一 loading 记忆点。</mark>

### 5.2 “涨水”动效设想

J 图形作为容器，内部红色或亮红色液位从底部逐渐上涨，水面可以有轻微波浪。外层可加一圈低透明度能量环，表示链上数据加载或交易处理中。

### 5.3 技术方案

| 方案 | 实现方式 | 复杂度 | 推荐程度 |
| --- | --- | --- | --- |
| 简单版 | J 图形呼吸 / 旋转 | 低，约 `0.5-1 天` | 可作为兜底 |
| 中等版 | SVG mask + CSS 水位上涨 | 中，约 `1-2 天` | <mark>推荐第一期</mark> |
| 复杂版 | Canvas / Lottie 液体粒子 | 高，约 `3-5 天` | 不建议第一期 |

### 5.4 推荐落地

<mark>第一期采用“中等版”：SVG mask + CSS animation，实现 J 内部涨水效果。</mark>

原因：

- 不依赖大型动画库。
- 体积可控。
- 方便适配浅色 / 暗色。
- 可使用 `prefers-reduced-motion` 提供静态兜底。
- 适合按钮、局部加载、全页加载多个尺寸复用。

### 5.5 使用场景

- 首屏初始化。
- 区块列表加载。
- 搜索中。
- 钱包连接中。
- 等待用户签名。
- 交易广播中。
- 交易确认中。

## 6. 交互反馈规范

### 6.1 核心要求

<mark>处处点击有回应。</mark>

用户点击任何按钮、卡片、链接、地址、哈希、分页、Tab、钱包操作，都必须立即看到反馈。

### 6.2 可点击区域状态

| 状态 | 要求 |
| --- | --- |
| 默认态 | 能看出是可点击对象 |
| Hover | 背景、边框、颜色或阴影变化 |
| Pressed | 颜色加深或轻微下压 |
| Focus Visible | 出现清晰焦点环 |
| Disabled | 降低视觉权重，禁止 hover 动效 |
| Loading | 显示 J loading 或 Naive UI loading，禁止重复提交 |

### 6.3 点击区域尺寸

- 桌面端按钮高度建议 `36-40px`。
- 移动端触控目标建议不小于 `44px`。
- 卡片可点击时，整张卡片应有明确 hover / focus 样式。
- 地址和哈希可点击时，应使用链接色、等宽字体、hover 下划线或背景变化。

### 6.4 链上操作反馈链路

链上操作必须展示完整状态：

1. 钱包未连接。
2. 等待钱包签名。
3. 用户取消签名。
4. 交易广播中。
5. 链上确认中。
6. 交易成功。
7. 交易失败。
8. RPC 超时或网络异常。

## 7. 空状态与异常状态

### 7.1 统一状态组件

建议建设 `JvPageState`，覆盖：

- 空搜索结果。
- 空交易列表。
- 空区块列表。
- 未拥有 Core ID。
- 钱包未连接。
- 钱包地址不匹配。
- RPC 网络异常。
- 合约调用失败。
- 交易失败。

### 7.2 插画规范

<mark>插画优先使用本地 SVG，不依赖远程图片。</mark>

**分层策略：**

- **通用空列表、空搜索结果**：直接使用 Naive UI `NEmpty` 组件的内置默认图，无需自定义插画。
- **有明确语境的场景**（网络异常、钱包未连接、交易失败等）：使用 AI 生成的专属 SVG，通过 `NEmpty` 的 `icon` 插槽注入。

```vue
<NEmpty description="网络异常，请稍后重试">
  <template #icon>
    <NetworkErrorSvg />
  </template>
</NEmpty>
```

视觉建议：

- 风格简洁通用，线条清晰。
- 中性灰为主体，少量 Jouleverse 红作为强调。
- 不在插画内部写文字。
- 浅色 / 暗色主题都要可见（使用 `currentColor` 或主题变量）。

### 7.3 第一批插画清单

| 插画 | 用途 | 优先级 | 来源 |
| --- | --- | --- | --- |
| 空搜索 | 搜索无结果 | 高 | Naive UI NEmpty 默认图 |
| 空列表 | 暂无交易、暂无区块 | 高 | Naive UI NEmpty 默认图 |
| 网络异常 | RPC / WebSocket 异常 | 高 | AI 生成 SVG |
| 钱包未连接 | 钱包相关功能入口 | 高 | AI 生成 SVG |
| 地址不匹配 | 当前钱包与页面地址不一致 | 中 | AI 生成 SVG |
| 交易失败 | 合约调用失败或交易 revert | 高 | AI 生成 SVG |

## 8. 中期设计系统指南

### 8.1 设计 Token

需要定义：

- 颜色 token。
- 字体 token。
- 间距 token。
- 圆角 token。
- 阴影 token。
- 动效 token。
- z-index token。
- 状态 token。

### 8.2 基础组件

建议第一批封装：

| 组件 | 用途 |
| --- | --- |
| `JvLoading` | J Logo loading |
| `JvPageState` | 空状态 / 异常状态 |
| `JvHashText` | 地址 / 哈希展示、复制、跳转 |
| `JvActionButton` | 带 loading、防重复提交、成功失败反馈的按钮 |
| `JvClickableCard` | 统一卡片点击反馈 |
| `JvStatusTag` | 交易、网络、钱包、合约状态标签 |
| `JvAmount` | 金额展示、单位、精度控制（见下方说明） |

#### JvAmount 说明

`JvAmount` 是专门展示链上金额的封装组件。区块链浏览器中金额显示涉及多个细节问题，如果不统一封装，各页面自行处理容易出现精度不一致、单位展示混乱的问题。

该组件需覆盖以下能力：

- **精度控制**：超小数值（如 `0.000000123 JVB`）的截断位数，避免展示过长或精度丢失。
- **单位跟随**：支持 JVB、GWEI、ETH 等多种单位配置，单位紧跟数值后展示。
- **大数格式化**：三位一组加千分符（如 `1,234,567.89`），提升可读性。
- **主题一致**：数值与单位颜色跟随浅色 / 暗色主题 token，不硬编码颜色。

Naive UI 无内置金额组件，需自行封装。

### 8.3 Naive UI Provider

后续实施时建议统一在应用根部接入：

- `NConfigProvider`
- `NMessageProvider`
- `NDialogProvider`
- `NNotificationProvider`
- `NLoadingBarProvider`

主题模式建议支持：

- 浅色。
- 暗色。
- 跟随系统。

## 9. W3C / WCAG 标准

<mark>新功能页面应按 W3C / WCAG 2.2 AA 作为基础验收标准。</mark>

### 9.1 语义化结构

- 页面主区域使用 `main`。
- 导航区域使用 `nav`。
- 列表使用合适的列表结构。
- 数据详情优先使用描述列表或 `NDescriptions`。
- 表单字段必须有明确 label。

### 9.2 键盘可访问

- 所有可点击控件必须可通过键盘聚焦和触发。
- Tab 顺序应符合视觉顺序。
- 弹窗、下拉、菜单优先使用 Naive UI，避免手写复杂焦点管理。
- `focus-visible` 必须清晰。

### 9.3 状态提示

- 异步操作必须有明确状态。
- 错误提示必须说明原因和下一步。
- 表单错误应绑定字段，不只弹全局提示。
- loading 不能导致页面长时间空白。

参考：

- [WCAG 2.2](https://www.w3.org/TR/WCAG22/)
- [Naive UI](https://github.com/tusen-ai/naive-ui)
- [Tailwind CSS Vite 安装说明](https://tailwindcss.com/docs/installation/using-vite)

## 10. 后期落地实施路线

### 10.1 第 0 阶段：现状冻结与基线确认

预计时间：`0.5-1 天`

工作内容：

- 明确旧页面不主动迁移。
- 记录当前页面样式问题。
- 记录当前构建问题和依赖状态。
- 明确后续新功能必须接入设计系统。

产出：

- 现状检查记录。
- 设计系统接入范围说明。

### 10.2 第 1 阶段：基础依赖与主题接入

预计时间：`1-2 天`

工作内容：

- 安装 Naive UI。
- 建立 `theme.ts`。
- 建立 `tokens.css`。
- 接入 Naive UI Provider。
- 建立浅色 / 暗色 / 跟随系统模式。

产出：

- 主题基础配置。
- 全局 Provider。
- 颜色 token。

### 10.3 第 2 阶段：基础业务组件

预计时间：`3-5 天`

工作内容：

- `JvLoading`
- `JvPageState`
- `JvHashText`
- `JvActionButton`
- `JvClickableCard`
- `JvStatusTag`

产出：

- 可被新功能复用的业务组件。
- 基础示例页面。

### 10.4 第 3 阶段：J Loading 与插画资产

预计时间：`2-4 天`

工作内容：

- 实现 J 涨水 loading。
- 增加 reduced-motion 兜底。
- 设计第一批空状态 / 异常状态 SVG。
- 适配浅色 / 暗色。

产出：

- `JvLoading` 动效。
- 状态插画资产。

### 10.5 第 4 阶段：新功能试点

预计时间：`3-5 天`

工作内容：

- 选择一个新页面完整使用设计系统。
- 验证浅色 / 暗色。
- 验证移动端。
- 验证 loading、empty、error、success。
- 验证键盘可访问。

产出：

- 新功能样板页面。
- 设计系统接入示例。

### 10.6 第 5 阶段：团队接入与治理

预计时间：`1-2 天`

工作内容：

- 输出开发指南。
- 输出 PR checklist。
- 输出组件使用示例。
- 明确禁止继续复制旧页面大段 scoped CSS。

产出：

- `docs/design-system.md`
- `docs/ui-development-guide.md`
- PR 检查清单。

## 11. 风险与控制

| 风险 | 影响 | 控制方案 |
| --- | --- | --- |
| 新旧页面视觉不一致 | 用户看到旧页面和新页面风格不同 | 明确旧页面冻结，新功能统一，后续触达即迁移 |
| Naive UI 默认感过强 | 品牌识别弱 | 使用 Jouleverse token 覆盖主题 |
| 文件体积增加 | 首屏变慢 | 按需引入，避免全量图标库，构建后做体积分析 |
| 动效复杂导致性能问题 | 低端设备卡顿 | J loading 第一版用 SVG + CSS，不用 Canvas / Lottie |
| 暗色模式可读性不足 | 用户阅读困难 | 色彩 token 做对比度验收 |
| 团队绕过设计系统 | 维护成本回升 | 建立 PR checklist 和组件使用规范 |
| 旧 CSS 继续扩散 | 样式债务增加 | 新功能禁止复制旧页面大段 scoped CSS |

## 12. 文件大小与性能治理

### 12.1 文件大小风险

引入 UI 组件库后，构建体积一定会增加。核心不是完全不增加，而是避免不可控增加。

<mark>第一期目标：只引入必要组件，构建后用体积分析确认增量。</mark>

### 12.2 控制策略

- 不全局注册所有 Naive UI 组件。
- 新业务组件内部按需 import。
- 图标库避免全量导入。
- 插画使用本地 SVG。
- J loading 使用 SVG + CSS。
- 第一版不引入 Lottie。
- 构建后使用体积分析工具检查产物。

### 12.3 建议验收指标

- 新增依赖后构建成功。
- 首页首屏无明显变慢。
- 样式和交互正常 tree-shaking。
- 大型图标、插画、动画资源不进入首屏主包。

## 13. 后期开发团队接入说明

### 13.1 新功能开发必须遵守

- 使用设计系统 token。
- 使用 Naive UI 或 `Jv*` 业务组件。
- 同时验证浅色 / 暗色。
- 提供 loading、empty、error、success 状态。
- 所有点击都有反馈。
- 所有链上操作都有明确状态链路。
- 移动端触控区域不小于建议值。

### 13.2 禁止项

- 新页面复制旧页面大段 scoped CSS。
- 只用颜色表达状态。
- 点击后无反馈。
- loading 期间允许重复提交。
- 暗色模式只做背景变黑，不检查文字对比度。
- 插画使用远程不可控链接。

### 13.3 PR Checklist

- 是否使用设计系统组件？
- 是否使用颜色 token？
- 是否支持浅色 / 暗色？
- 是否有空状态？
- 是否有异常状态？
- 是否有 loading？
- 点击是否有 hover / pressed / focus-visible？
- 链上操作是否有完整状态？
- 移动端是否可点击、可阅读？
- 是否满足基础 W3C / WCAG 要求？

## 14. 建议文件规划

后续实施时建议新增：

```text
src/design-system/
  theme.ts
  tokens.css
  components/
    JvLoading.vue
    JvPageState.vue
    JvHashText.vue
    JvActionButton.vue
    JvClickableCard.vue
    JvStatusTag.vue
  assets/
    empty-search.svg
    empty-list.svg
    network-error.svg
    wallet-disconnected.svg
    address-mismatch.svg
    transaction-failed.svg

docs/
  design-system.md
  ui-development-guide.md
```

## 15. 汇报重点

<mark>重点 1：Naive UI 是基础能力，不是最终视觉；最终视觉由 Jouleverse 设计系统决定。</mark>

<mark>重点 2：旧功能不主动迁移，降低上线风险。</mark>

<mark>重点 3：后续新功能统一设计系统，逐步形成稳定资产。</mark>

<mark>重点 4：J Logo 涨水 loading 技术上可实现，建议第一期用 SVG + CSS 做中等复杂度版本。</mark>

<mark>重点 5：浅色、暗色、空状态、异常状态、点击反馈、W3C / WCAG 标准必须一次性纳入规范。</mark>

<mark>重点 6：文件大小需要治理，不追求零增长，但要避免不可控增长。</mark>

<mark>重点 7：后期团队接入必须靠文档、组件封装和 PR checklist 约束。</mark>

