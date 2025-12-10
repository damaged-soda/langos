# langos/meta

> 受众：人类优先；AI 仅在修改/理解“OS”治理与规范时阅读。

`langos/meta` 存放整个 `workspace-docs` 的“基础规范”和“元信息”，是人类和 AI 在使用本知识库前必须先了解的内容。

它回答的问题：

- 这个知识库（以及“自然语言编程操作系统”）的愿景、原则与分层是什么？
- 文档应该如何命名、编写、放置？
- AI 在这里工作时，有哪些通用的行为约定与确认要求？

简言之：**`workspace-docs` 是业务知识中心，`langos/meta` 是使用手册和规则说明。**

---

## 1. 目录内容

建议至少包含以下文件：

- `langos/meta/README.md`（本文件）  
  说明 `langos/meta` 的定位，并给出整体规范的概览。

- `langos/meta/conventions.md`  
  详细的基础规范，包括：
  - 文件与目录的命名约定；
  - 文档内容的结构和写作风格；
  - 时间和版本号的表示方式；
  - 对 AI 的通用要求与行为约束。

- `langos/meta/vision.md`  
  自然语言编程“操作系统”的愿景、分层定位与迭代方式。

根据需要可以逐步增加更多元信息文件（如术语、流程说明等）。

---

## 2. 面向对象：人类 + AI

`langos/meta` 既是给人看的，也是给 AI 看的：

- 对人类：提供写文档的统一规范和目录约定，便于自用或协作。
- 对 AI：定义基础行为约束（如何提问、何时需要确认、如何命名/落盘），在初始化时应优先阅读并遵守。

可在对话中明确要求 AI：

> “在这次对话里，请你遵守 `workspace-docs/langos/meta/conventions.md` 的约定来行动。”

---

## 3. 与其它目录的关系

- `langos/meta/`：规则与约定，定义“怎么玩”。
- `langos/runtime/`：接口与执行层，定义对话规范、路由与协议（“怎么玩”的具体流程）。
- `workspace/`：业务资产层，包含仓库说明、项目、需求规格、领域知识、ADR、归档等。

其中，**`langos/meta` 和 `langos/runtime` 对 AI 影响最大**：
- `meta` 提供通用规范（写文档、命名、提问、确认等）；
- `runtime` 提供具体操作协议（某一类任务的标准流程）。

---

## 4. AI 初始化时的阅读要求（概览）

详细清单在 `workspace-docs/README.md`，这里强调与 `langos/meta` 的关系：

1. 阅读 `workspace-docs/README.md`，理解整体结构与分层。
2. 阅读本文件 `langos/meta/README.md`，理解 `meta` 的定位。
3. 阅读 `langos/meta/conventions.md`，掌握基础规范。
4. 视需要阅读：  
   - `workspace/repos/INDEX.md`（仓库列表）  
   - `langos/runtime/guidelines.md`  
   - `langos/runtime/protocols/index.yaml`

---

## 5. 如何演进 `langos/meta`

`langos/meta` 不需要一开始就写得非常完整，可以逐步演进：

1. 先完善核心文件：`langos/meta/README.md`、`conventions.md`、`vision.md`。
2. 使用过程中发现空白再补充/调整：
   - 常见命名/写法/流程说明可沉淀到 `conventions.md`；
   - AI 易踩坑点可写在 `conventions.md` 或 `langos/runtime/guidelines.md`。
3. 如规范变动较大，可在文末增加变更记录，或在 `workspace/adr/` 写 ADR 说明原因。

---

## 6. 接下来要做什么

如果你刚搭好 `workspace-docs`，建议：

1. 保留本文件：`workspace-docs/langos/meta/README.md`。
2. 按实际习惯补充/调整：`workspace-docs/langos/meta/conventions.md`（基础规范）和 `vision.md`（愿景与分层）。
