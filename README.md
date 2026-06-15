<h1 align="center">大威天龙画布</h1>

<p align="center">属于你自己的桌面 AI 视觉创作工作台</p>

<p align="center">
  <img alt="React" src="https://img.shields.io/badge/React-19.2-149ECA?logo=react&logoColor=white" />
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white" />
  <img alt="Vite" src="https://img.shields.io/badge/Vite-7-646CFF?logo=vite&logoColor=white" />
  <img alt="Tailwind CSS" src="https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?logo=tailwindcss&logoColor=white" />
  <img alt="License" src="https://img.shields.io/badge/License-MIT-green" />
</p>

一个以无限画布为载体的本地 AI 视觉创作工作台：把图片、文字、音频、视频和生成结果组织成可连线的节点图，由节点连线驱动生成，并由一个可审批、可续接的画布导演 Agent 帮你把创作意图落实成画布成果。

所有画布数据、渠道配置与 API Key 都保存在你自己的浏览器本地，除了你主动配置的模型接口之外不经过任何第三方服务器。

## 功能

### 画布引擎

- **无限创作画布**：基于 `@xyflow/react` 的节点图，组织图片、文字、音频、视频和生成结果，节点连线驱动生成。
- **图片节点操作**：裁剪、局部重绘（蒙版）、拆分、放大、角度调整，以及 AI 超分（仅增强清晰度，结果生成到新的已连线节点）。
- **涂鸦节点**：内置画板（画笔/橡皮/多色/撤销），手绘草图或构图参考，连接到图片节点后作为参考图参与生成。
- **资源引用**：节点间通过 `@1`/`@2` 形式的提及引用参考图，配套预告与提及输入组件。
- **项目与资产管理**：多项目管理、剪贴板、画布导出、渐进式图片预览（含 Web Worker 解码）。

### 分镜系统

- **分镜视表**：手动添加空白镜头，为每个镜头挑选参考图（从画布图片节点选择或本地上传），编写视频提示词（可用 `@1`/`@2` 引用参考图），逐镜头或一键用视频模型生成分镜视频；时长/比例/模型可编辑。
- **分镜图表**：宫格编排分镜提示词，宫格列数、分辨率（Auto/1K/2K/4K）、画面比例、模型自由选择；每格可单独生成，一键生成全部分镜图并按宫格排布到画布；每格显示生成预览（点击定位节点）。

### 画布导演 Agent

- **任务面板与工具循环**：右侧任务面板，支持资产规划、稳定 ID 分镜编辑、镜头媒体准备、图片/视频生成和实际图片读取。可增量编排导演台人物、布景、机位、相机路径及物体动画；任务步骤、执行结果与待验收项随项目保存，支持显式继续。
- **执行确认与验收**：已有节点的修改范围、删除、全画布重排及媒体预算由用户确认。图片初始预算为 4 张，首次提交、变更参数、再次生成同一节点或超额均需确认；视频逐次确认。生成返回实际结果节点与成功/失败数量，结束前检查画布与图片验收状态。
- **Skill 库**：可上传 Markdown/文本文件，或让导演把方法沉淀成 Skill。启用技能以目录形式提供、按需读取；Agent 新建的 Skill 默认停用，待用户审核启用。
- **运行时边界**：每轮任务最多 24 次模型调用、80 次工具调用；附件最多 8 个，单文件 200 KiB / 24,000 字符，总正文 96,000 字符。附件正文是数据，不是操作授权。

### 导演台（MONOFORM 三维白模预演）

内置 MONOFORM 3D 白模预演工作台，摆机位、调动作，导出 PNG/MP4 直接回到画布：

- 人物 WASD 游戏式行走（自动转向 + 行走/奔跑循环动画，Shift 奔跑，松键回站立）
- 机位 WASD 观察者式飞行（沿视线方向，Q/E 升降），14 种预设机位（自动识别人物正反面朝向）
- 场景道具白模库（椅/桌/沙发/拱门/楼梯/门/窗/树/车辆/屋顶/植物/建筑/道路）
- 动作预设选中即实时循环预演，关键帧时间轴，工具切换快捷键 1/2/3/4

### 模型接入

- **多渠道配置**：配置 OpenAI 兼容接口及自定义中转 API，支持 Gemini 原生格式。
- **模型脚本**：自定义协议可通过模型脚本配置，内置模板转发尺寸、质量及对应接口参数；已有用户脚本不会被覆盖。自定义脚本可使用 `params.resolution`、`params.ratio` 和 `params.geminiImageConfig`。
- **提示词库与视觉资产**：内置提示词卡片、生成记录与本地备份。

#### 图片自动尺寸

- OpenAI 格式适配采用像素尺寸提交。尺寸为 Auto 且明确选择分辨率时，1K / 2K / 4K 分别提交 `1024x1024` / `2048x2048` / `3840x2160`；4K Auto 的回退构图为横屏，面板显示对应像素值。分辨率也为 Auto 时才采用服务端默认尺寸。
- Gemini 原生适配对支持分辨率的模型保留自动比例，并独立提交 `imageSize`。手动填写的像素尺寸保留原值。

## 技术栈

| 层次 | 选型 |
| --- | --- |
| 框架 | React 19.2 · TypeScript 5 · Vite 7 |
| 画布 | `@xyflow/react` 12 · `@dagrejs/dagre` 自动布局 |
| 状态 | Zustand 5（11 个独立 store：画布、Agent 面板、配置、资产、Skill、生成历史等） |
| UI | Tailwind CSS 4 · antd 6 · Radix UI · lucide-react · Motion |
| 数据 | TanStack Query 5 · axios · localforage（本地持久化） |
| Agent | 自研工具循环（Responses / Gemini 双格式）、工具注册与校验、预算与审批闸门 |
| 导演台 | 独立 Vite + three.js 子工程（`web/monoform-studio`），构建后嵌入主应用 |
| i18n | i18next · react-i18next（zh-CN / en-US） |

## 目录结构

```text
web/src/components/canvas/         画布与节点组件（50 个文件，约 10,200 行）
web/src/components/canvas/nodes/   节点实现：分镜、涂鸦、RunningHub 工作流
web/src/pages/canvas/project.tsx   画布工程主页面
web/src/lib/canvas/                画布与 Agent 核心逻辑
web/src/stores/                    Zustand 状态层
web/src/services/api/              模型与服务接入（图片/视频/音频/脚本/RunningHub）
web/monoform-studio/               三维白模预演导演台（独立子工程）
web/scripts/                       构建、启动与回归测试脚本
```

核心 Agent 逻辑：

```text
web/src/lib/canvas/agent-tools.ts        工具定义与执行器（含 skill / 读节点工具）
web/src/lib/canvas/agent-execution.ts    工具循环、去重、停止与结束验收
web/src/lib/canvas/agent-run.ts          任务状态、修改范围与媒体预算
web/src/lib/canvas/agent-context.ts      历史、附件、技能目录与上下文边界
web/src/lib/canvas/director-agent.ts     导演台增量场景与动画编辑
web/src/components/canvas/canvas-agent-panel.tsx  任务面板、审批、附件与显式继续
```

## 快速开始

### Windows 一键启动

双击仓库根目录的 `start.vbs`，服务会在后台启动，页面就绪后直接打开默认浏览器，全程不显示命令窗口。`start.bat` 保留为兼容入口，会转交同一启动流程，但 Windows 可能短暂显示初始命令窗口。

脚本会自动寻找常见 Node.js 安装位置；首次启动会自动安装前端依赖（需要网络），然后打开：

```text
http://localhost:3001/
```

重复启动会复用已有的画布服务。后台日志位于 `web/launcher.log`、`web/server.log` 和 `web/server-error.log`；启动失败时会显示错误提示。

### 手动启动

需要 Node.js LTS：

```powershell
cd web
npm install --legacy-peer-deps --include=optional
npm run dev
```

默认端口为 `3001`，开发服务器和预览服务器共用同一端口。需要临时更换端口时，可设置 `APP_PORT`，启动脚本和 Vite 会同步读取。

### 构建导演台

导演台的三维预演界面需要先构建一次（`start.vbs` 会在检测到构建标记缺失时自动执行）：

```powershell
cd web
npm run build:monoform
```

该命令构建 `web/monoform-studio` 并把产物同步到 `web/public/monoform`。**这部分产物不入库**（见 `.gitignore`），克隆仓库后需要执行一次本命令，否则导演台页面会空白。

## 使用说明

1. 右上角配置：添加 API 地址和 Key，为渠道拉取模型，分别选择图片模型和文本模型（Agent 用）。
2. 工具栏「分镜」按钮创建分镜节点，点击节点打开编排面板，填主题和镜头后点「生成分镜画面」。
3. 右下角机器人按钮打开画布导演，直接下指令，例如："把这份剧本编成 4 个分镜，先准备画面节点"；检查规划后再要求生成并确认预算。
4. 配置、画布、资产默认保存在当前浏览器本地。执行模型请求时，相关文本、图像及 API 鉴权信息会发送到用户配置的接口。

首次使用时渠道和默认模型为空，请添加自己的渠道、API Key 和模型。渠道设置使用本应用独立的浏览器存储；删除全部渠道后，关联的 API Key 和默认模型也会清空。

### Agent 边界

- 工具调用接口支持 Responses 与 Gemini 格式；视觉验收需要当前文本模型同时支持图像输入。
- 普通对话模式不提供工具。
- 失败媒体请求也计入本轮额度；停止等待不保证已提交的远端请求停止处理或计费。
- 继续任务会重新读取画布，保留步骤和待验收项，重新申请修改权限与媒体预算。现有画布撤销仍按画布历史工作，不是跨画布、导演台与远端生成的原子回滚。
- RunningHub 当前支持入口和状态读取，远端执行需使用工作流节点面板；导演台录制、导出仍在导演台界面操作。
- 自动验收检查结构、生成状态和图片是否送入视觉模型；构图、连续性及创作质量仍需要模型判断和用户复核。

## 测试

```powershell
cd web
npm run typecheck        # tsc --noEmit
npm run test:agent       # Agent 工具循环回归
npm run test:image-settings
npm run test:api-proxy
npm run test:runninghub
```

测试使用隔离存储、模拟模型和媒体结果，不提交真实媒体请求。浏览器测试页位于 `web/scripts/fixtures/`，其中 Agent 导演台测试页仅允许在独立的本地端口 `3011` 运行，以隔离测试配置。

CI 见 [`.github/workflows/desktop-build.yml`](.github/workflows/desktop-build.yml)：在 `web` 下执行 `npm ci` → `typecheck` → `build`。

## 数据与隐私

- 画布、资产、历史记录与渠道配置默认保存在当前浏览器本地（localStorage / localforage），不随源码目录复制，也不会上传。
- API Key 在软件的渠道设置中填写，不写入仓库文件。
- `infinite-canvas-config.json` 和 `infinite-canvas-backup-*.zip` 等导出文件包含 API 配置，应作为私密文件保管。
- 执行模型请求时，相关文本、图像及鉴权信息会发送到你自己配置的接口。

## 许可与致谢

本项目基于 [GuiYi-Xi](https://github.com/GuiYi-Xi) 的 **infinite-atelier** 项目（MIT 许可）二次开发与定制，在此基础上重构了画布引擎、新增了分镜系统与画布导演 Agent。

原始 MIT 许可与版权声明保留在 [`LICENSE`](LICENSE) 中，请一并保留。导演台模型与动画素材的来源说明见 `web/monoform-studio/public/models/ANIMATION_SOURCES.md`。

发布构建产物前请检查图片、模型、提示词与素材授权，并确认提交内容中不含个人路径、密钥与邮箱。
