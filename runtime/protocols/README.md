# 协议与指令系统说明（runtime/protocols）

本目录存放 **langos 运行时的协议定义**，可以视为一组“自然语言指令”的结构化实现。  
每个 `*.yaml` 描述一条可复用的对话/协作流程（协议），由运行时根据自然语言请求进行匹配与执行。

核心目标：

- 用协议（YAML）来描述固定流程，而不是散落的对话习惯；
- 通过索引文件 `index.yaml` 接收自然语言请求并路由到合适的协议；
- 让人类只用说“我要做什么”，而不用记协议文件路径或技术细节。

---

## 1. 目录结构

典型结构如下：

```text
runtime/protocols/
├─ index.yaml                ← 协议索引与触发表达
├─ dispatch-request.yaml     ← 请求路由协议
├─ add-repo-index.yaml       ← 添加新项目索引
├─ add-requirement.yaml      ← 添加新需求规格文档
└─ refresh-repo-docs.yaml    ← 更新项目说明与修订文档
```

文件说明：

- `index.yaml`
  - 列出所有可用协议的基本信息：id、name、file、description、triggers 等。
  - 给出每个协议的触发意图与示例表达（例如“添加新需求”“这个 repo 还没在文档里建说明”）。
  - 运行时只需要读取 `index.yaml`，就能通过自然语言找到候选协议。
- 其他 `*.yaml`
  - 每个文件定义一个协议，包含 id、name、version、description。
  - `triggers`：触发意图或关键短语。
  - `preconditions`：前置条件。
  - `outputs`：协议执行的预期输出。
  - `steps`：按顺序执行的步骤（例如 ASK_USER / ANALYZE_DOCS / PLAN_DOCS / DRAFT_DOCS / CONFIRM_AND_SUMMARIZE 等）。
  - `safety`：安全约束与行为底线。

协议的具体语义和约束由各自的 YAML 文件定义；本 README 只描述整体规则与使用方式。

---

## 2. 自然语言 → 协议的路由方式

运行时不会要求人类提供“协议路径”或“协议 id”，而是通过自然语言来路由：

1. 人类发出请求（中文），例如：
   - “帮我写一个需求 spec”
   - “这个 repo 还没在文档里建说明”
   - “这个项目的文档可能过时了”
2. 运行时使用 `index.yaml` 中的 `triggers` 与示例表达，匹配 1–3 个候选协议。
3. 运行时向人类展示候选协议列表（id / name / 简述），并询问是否选用其中某一个。
4. 若人类选择某个协议，则进入对应协议文件（例如 `add-requirement.yaml`）的 `steps` 流程。
5. 若没有匹配协议或人类不想使用协议，则回退为普通对话，但仍遵守 `runtime/guidelines.md` 中的行为约束。

特别强调：

- 不要要求人类输入 YAML 路径或协议文件名。
- 优先使用意图识别 + `index.yaml` 的触发表达来路由。
- 匹配不确定时，应向人类展示候选列表并征求选择，而不是自作主张执行。

当前已存在的协议包括（非穷举）：

- `dispatch_request`：请求路由，判断是“信息咨询”还是“命令/请求”，并决定是否调用协议。
- `add_repo_index`：为新出现的仓库添加文档索引与说明。
- `add_requirement`：将关于新功能/新需求的对话整理为结构化的规格文档。
- `refresh_repo_docs`：对比现有文档与代码，帮助更新项目文档。

### 协议角色（role）

- 目的：避免将不同层级的流程混在一起，明确哪些协议是内核链路，哪些是用户可选的治理/业务协议。
- 角色枚举（当前）：
  - `core`：核心运行时链路（例如 `dispatch_request`），由运行时自动执行，不作为候选协议展示。
  - `governance`：治理/文档/配置相关协议（如 init_doc_root / add_requirement / add_repo_index / refresh_repo_docs），可被自然语言触发并出现在候选列表。
  - 预留 `feature`/`business`/`experimental` 等，供未来扩展。
- 路由规则：匹配候选协议时仅考虑 `role != core`（可选再加 `exposed: true`）；`role = core` 的协议按运行时内置链路执行，不向用户展示。
- 声明位置：每个协议的 `*.yaml` 中填写 `role`；`index.yaml` 可镜像 `role` 便于路由或分组，权威以协议文件为准。

---

## 3. 协议的典型结构（简介）

一个典型协议（例如 `add-requirement.yaml`）通常包含以下部分：

- `id` / `name` / `version`：协议标识信息。
- `description`：协议的大致用途和场景。
- `triggers`：用于匹配的意图与语句片段。
- `preconditions`：可执行前需要满足的条件。
- `outputs`：协议完成后应给出哪些产出。
- `steps`：核心流程，常见类型包括：
  - ASK_USER：向人类提问或澄清信息。
  - ANALYZE_REQUEST：分析当前请求内容。
  - ANALYZE_DOCS：根据已有文档进行盘点或比对。
  - PLAN_DOCS：规划要创建/更新的文档与结构。
  - DRAFT_DOCS：起草文档内容或更新片段。
  - PLAN_INDEX_UPDATE：规划索引更新片段。
  - CONFIRM_AND_SUMMARIZE：复述变更与输出，总结达成与未决事项。
- `safety`：约束行为的规则（例如“全程中文”“未经确认不得直接写入文件”“缺信息先问、不编造”等）。

运行时在执行协议时，应严格按照 `steps` 顺序推进，不跳过 ASK_* / CONFIRM_* 类步骤。

---

## 4. 如何新增或修改协议（给维护者看的简版流程）

当你希望为 langos 增加新的能力（“指令”）或修改现有协议时，推荐遵循如下流程：

1. 在 docs 仓写清需求与设计（推荐）
   - 在 `../langos-docs/specs/langos/` 下新增或更新相关规格，例如：
   - 描述新协议的背景、目标、输入输出、步骤要点、确认点与安全约束。
   - 或对现有协议进行重构/拆分/合并的需求说明。
   - 如变更较大，可以配套一条 ADR 记录决策过程。
2. 在 `runtime/protocols/` 下新增或修改 `*.yaml`
   - 新增时：复制一个结构类似的协议文件作为模板，修改 `id` / `name` / `description` / `triggers` / `steps` / `safety` 等字段，确保步骤名和约定的类型一致（如 ASK_USER / DRAFT_DOCS 等）。
   - 修改现有协议时：对照规格和 ADR，说明变更原因，调整字段时注意向后兼容性与已有使用习惯。
3. 更新协议索引 `index.yaml`
   - 为新协议添加一条条目，包含：`id`（与协议文件中的 id 一致）、`name`（简短中文名）、`file`（对应的 YAML 文件名）、`description`（简要描述用途）、`triggers.intent` 与 `triggers.utterance_examples`（用于匹配的意图与示例表达）。
   - 对已有协议的 `utterance_examples` 进行微调时，注意不要“抢占”不适合该协议的表达。
4. 在对话中演练关键路径
   - 使用接近真实的自然语言请求，验证是否会路由到正确的协议、步骤顺序与提问是否合理、安全约束是否得到遵守（例如不会在未确认前写入内容）。
   - 如发现问题，再迭代协议定义和规格。
5. 必要时更新文档
   - 在 `../langos-docs/specs/langos/` 中更新相关规格。
   - 视情况在 docs 仓的 `projects/` 或 `adr/` 中记录此次调整。

---

## 5. 面向 AI 的使用提示

- 收到“命令/请求”类输入且不确定用什么流程时，可优先通过 `dispatch_request` 协议来分析意图并检索候选协议；若没有合适的协议，则回退普通对话，但仍遵守 `runtime/guidelines.md` 的行为底线。
- 不要要求用户提供协议文件路径或记住某个 YAML 的名称；协议路由应始终基于自然语言意图与 `index.yaml` 的触发表达；匹配不确定时，应展示候选列表并向用户确认。
- 在执行协议前后，尤其是新建或修改文档、执行危险操作之前，先复述预期变更并征求确认。
- 如信息不足，先提问澄清，不编造业务或实现细节。
- 只有在用户确认后，才视为协议步骤可以“落盘”或进一步执行。
