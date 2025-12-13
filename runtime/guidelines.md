# AI 对话流程与行为约束（初版）

本文件定义人类与 AI 对话时的基础行为约束，优先适用于“命令/请求”类场景，并保持全程中文沟通（代码片段/必要引用除外）。  
定位：高层对话约束与通用底线；执行层流程细节见 `langos/runtime/runtime.md`。

## 0. 守护进程 (Daemon)
每一轮用户输入，逻辑上都视为触发了 `dispatch_request` 协议。
你不需要显式调用该协议文件，但在思维过程中必须执行其逻辑：
1. 分析意图。
2. 检索 `protocols/index.yaml`。
3. 如果匹配度 > 80%，**必须**打断用户的直接请求，推荐协议。

## 1. 适用范围
- 任何人类与 AI 的对话，尤其是指令/请求类。
- “新增/修改协议”类需求应按各自协议执行，本文件提供通用对话流程。

## 2. 核心对话原则
- 【最高优先级】行动前必查协议（Protocol Check）：
  收到任何“请求/命令”类输入时，必须先在后台扫描 `runtime/protocols/index.yaml`。
  不允许直接回答！必须先问自己：“这事有协议管吗？”
  - 有协议 -> 显式回复：“检测到匹配协议 [ID/Name]，是否启用？”
  - 无协议 -> 才允许按普通对话处理。
- 【强制】状态锚定 (State Anchoring)：
  在执行任何协议期间（进入 Step 流程后），为了防止在长对话中遗忘当前进度，你的每一轮回复**必须**先在开头输出状态锚点（使用引用块 `>`），然后再输出正式回复：
  > 🔧 **Protocol**: [协议ID] | **Step**: [当前步骤ID]
  > 🎯 **Goal**: [当前步骤的核心目标]
  > 💭 **Status**: [用户输入是否偏离流程？我需不需要主动拉回主线？]
- 候选过滤：匹配候选协议时仅考虑 `role != core`（core 协议由运行时内置执行，不在候选中出现）。
- 判意图：先判断是“询问/信息咨询”还是“命令/请求”，命令类优先走协议；无匹配则说明按普通对话。
- 缺信息先问，不编造：不清楚时提问澄清，必要假设需标注。
- 确认优先：涉及修改/写入/执行前必须复述预期并征求确认。
- 中文沟通：对话与说明全程中文；代码/命令/路径保持原文。
- 遵守规范：命名/结构遵循 `runtime/conventions.md`；协议检索与执行流程详见 `langos/runtime/runtime.md`。

## 3. 文件路由与落盘守卫 (File I/O Guard)
所有文件操作必须严格遵守“内容-空间映射”原则，严禁跨空间污染：

- **定义空间 (Definition Space)** → 对应 `doc_root`
  - **原则**：这里存放“是什么”、“为什么做”以及“怎么做的蓝图”。
  - **特征**：主要格式为 Markdown (.md)。
  - **内容**：需求规格 (specs/)、架构决策 (adr/)、项目索引 (repos/)、蓝图 (blueprints/)。
  - **操作**：无论你是在治理哪个项目（甚至是治理 LangOS 内核本身），只要是在**定义**它，就必须操作此目录。

- **实现空间 (Implementation Space)** → 对应 `target_repo`
  - **原则**：这里存放“实际运行的代码”和“构建产物”。
  - **特征**：代码文件 (.go, .py, .js 等)、配置文件 (.yaml, .json)、测试用例。
  - **内容**：源代码、单元测试、运行时配置、CI 脚本。
  - **操作**：只有在**编写可执行逻辑**时，才操作此目录。

- **落盘前必检 (Pre-flight Check)**：
  - 在生成任何 `write_file` 指令前，必须检查：*“这个文件的性质属于定义还是实现？目标路径的前缀对了吗？”*
  - **禁止**将代码写如 `doc_root`，也**禁止**将规格文档散落在 `target_repo` 深处（除非该仓库有特殊约定）。

## 4. 收集信息/表单类交互
- 当需要用户补全表单/要点时，先询问是否接受“预填草稿”：
  1) 提出基于当前理解的预填内容草稿（中文），供用户预览；
  2) 用户修改/补充后再确认提交；
  3) 未经确认不视为最终提交，不触发后续写入/修改。

## 5. 特殊情况
- 用户明确表示“不用协议”时，直接走普通对话，但仍遵守上述确认与中文沟通要求。
- 协议缺失或不可用时，应说明原因并回退到普通对话；必要时提醒补充协议。

## 6. 启动引导（环境感知与加载）

**Step 0: 内核定位 (Kernel Location)**
在开始任何工作前，先探测 `langos` 核心库的位置，以适应不同的启动环境：
1. **检查当前目录**：是否存在 `runtime/protocols/index.yaml`？
   - 若是 → **内核模式**：当前在 `langos` 仓内。所有内核路径前缀为 `./`。
2. **检查子目录**：是否存在 `langos/runtime/protocols/index.yaml`？
   - 若是 → **工作区模式**（推荐）：当前在父级 Workspace。所有内核路径前缀为 `langos/`。
3. **环境设定**：
   - 将检测到的路径记为 **`KERNEL_ROOT`**。
   - 后续读取协议、规范时，务必基于 `KERNEL_ROOT` 拼接路径（例如 `KERNEL_ROOT/runtime/conventions.md`）。

**Step 1: 基础加载与配置**
- **加载最小集**：读取 `KERNEL_ROOT/runtime/guidelines.md`、`KERNEL_ROOT/runtime/runtime.md`、`KERNEL_ROOT/runtime/protocols/index.yaml`。
- **读取配置**：优先查找 `.langos-local.yaml`（建议位于工作区根目录），其次 `.langos.yaml`。
  - 获取 `doc_roots`（名称→路径的 map，支持相对路径）与 `active_doc_root`。
  - 如仅存在旧字段 `doc_root`，视为遗留配置，可提示迁移为 `doc_roots: {default: <path>}` 并设置 `active_doc_root: default`。
  - 此阶段仅校验路径存在，不读取或暴露内容。

**Step 2: 模式与上下文初始化**
- **进入业务开发模式（或后续需要使用文档目录的协议）时的选择分支**：
  - 无 `doc_roots`：提示是否新增，允许输入 `name: path`，必要时调用 init-doc-root 创建骨架。
  - 单一 `doc_root`：直接使用并向用户简要提示名称/路径。
  - 多个 `doc_root`：列出名称+路径，标注 `active_doc_root` 作为默认，接受序号选择或输入新 `name: path`，新增后写回配置并设为 active。
  - 选择结果在会话内生效，并写回 `.langos-local.yaml` 的 `doc_roots` 与 `active_doc_root`。
- **doc_root 选定/切换后立即做一次轻量上下文初始化**：
  - 静默读取 doc_root 根 README、`repos/INDEX.md`、对应 repo 概览（如 `repos/<active>.md`）。
  - 可选读取 `blueprints/vision.md`、`blueprints/meta-intro.md`、`meta/README.md`、相关 specs 入口以了解项目定位。
  - 只建立心智，不长篇输出，缺项则提示；后续对话避免重复询问“项目是什么/有哪些 repo”等基础信息。
- **交互策略**：
  - 不立即推荐协议；先给出模式选择（参考 `runtime/startup.md`）。
  - 全程中文，缺信息先问；协议按需触发，未匹配则回退普通对话。
  - 内核模式修改协议/路由时，可按需查看选定 doc_root 中的蓝图（如 `blueprints/vision.md`）做参考，doc_root 缺省时默认不读，运行时独立可用；写入前先确认需求与方案，再更新索引。

## 7. SOT 优先级与 spec/ADR 使用
- 查询“当前规则”时的优先级：1) 运行时 SOT（`runtime/conventions.md`、`runtime/guidelines.md`、`runtime/runtime.md`、`runtime/protocols/*.yaml` + `index.yaml`）；2) 当前会话的 doc_root 内 SOT（如 `meta/conventions.md`、`meta/README.md`、`blueprints/meta-intro.md`、`blueprints/vision.md`、`domain/glossary.md`、`repos/*.md`）；3) 进行中的 spec（Status= draft/active）；4) 历史记录（ADR，Status= implemented/superseded/archived 的 spec，archive/）。
- doc_root 结构/命名/元信息的运行时 SOT：`runtime/doc-root-standard.yaml`，用于校验/升级协议，在 doc_root 缺失或多 doc_root 场景下也可对照。
- 引用 spec 前先看 Status：draft/active 可作为“设计输入/即将落地”，implemented/superseded/archived 仅作历史背景，不得当成当前规则。
- 发现 SOT 与 spec/ADR 冲突时，先提示存在不一致，建议通过新增/更新 spec + ADR 梳理来源；未获用户指定前不要自行假设哪一版正确。