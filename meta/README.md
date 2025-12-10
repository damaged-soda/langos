# langos/meta

`langos/meta` 存放这套自然语言编程 OS 的“基础规范”和“元信息”，是人类和 AI 在使用自选业务文档库前必须先了解的内容。

它回答的问题：

- 这套知识库/文档库（以及“自然语言编程操作系统”）的愿景、原则与分层是什么？
- 文档应该如何命名、编写、放置？
- AI 在这里工作时，有哪些通用的行为约定与确认要求？

简言之：**业务知识库目录由你指定，`langos/meta` 是使用手册和规则说明。**

---

## 快速导航

- 顶层目录地图与最小使用路径：见 `langos/README.md`（说明 meta / runtime 分层、业务文档不在本仓）。
- AI 初始化必读：`langos/runtime/guidelines.md`、`langos/runtime/runtime.md`、`langos/runtime/protocols/index.yaml`。
- 启动对话提示：`langos/runtime/startup.md`，按模式（学习/治理/业务开发）切入。
- 业务资产位置：由你指定的业务文档目录/仓库（例：单独 docs 仓、工作区的 `workspace/` 目录等），与本仓分离。

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

> “在这次对话里，请你遵守 `langos/meta/conventions.md` 的约定来行动。”

---

## 3. 与其它目录的关系

- `langos/meta/`：规则与约定，定义“怎么玩”。
- `langos/runtime/`：接口与执行层，定义对话规范、路由与协议（“怎么玩”的具体流程）。
- 业务资产层：你指定的业务文档目录/仓库（如 `workspace/`、`docs/` 或独立 repo），包含仓库说明、项目、需求规格、领域知识、ADR、归档等。

其中，**`langos/meta` 和 `langos/runtime` 对 AI 影响最大**：
- `meta` 提供通用规范（写文档、命名、提问、确认等）；
- `runtime` 提供具体操作协议（某一类任务的标准流程）。

---

## 4. AI 初始化时的阅读要求（概览）

AI 初始化时，通常按以下顺序阅读（若存在对应文件）：  
1. 业务文档库的 README（若有），理解整体结构与分层。  
2. 本文件 `langos/meta/README.md`，理解 `meta` 的定位。  
3. `langos/meta/conventions.md`，掌握基础规范。  
4. 视需要阅读：  
   - 业务文档库中的仓库索引（如 `workspace/repos/INDEX.md` 等）  
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

如果你刚搭好业务文档库，建议：

1. 保留本文件：`langos/meta/README.md`。
2. 按实际习惯补充/调整：`langos/meta/conventions.md`（基础规范）和 `vision.md`（愿景与分层）。
