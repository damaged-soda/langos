# 基础规范（conventions）

本文件定义 `workspace-docs` 的基础规范，适用于：

- 人类在这里新增或修改文档；
- AI 在这里读取、生成、修改文档；
- 未来希望长期复用的所有“知识”与“协议”。

可以把这里看作本知识库的“写作规范 + 命名规范 + AI 行为底线”。

---

## 1. 通用原则

1. **优先保证可读性与一致性**  
   - 让未来的自己和 AI 都能快速理解；一旦形成习惯（命名、结构），尽量在同类文档中保持一致。
2. **人类和 AI 共用一套规范**  
   - 自己写文档也按此执行；让 AI 写文档时可明确要求“按 `langos/meta/conventions.md` 来”。
3. **可以迭代，但要有意识地迭代**  
   - 发现规则不合适可以修改本文件；大改动建议说明原因或写 ADR，避免日后困惑。

---

## 2. 语言与编码

1. **默认语言**  
   - 文档默认使用简体中文；专有名词、代码、API 名等保持英文原样；对话全程中文（代码/必要引用除外）。
2. **编码**  
   - 文件统一使用 UTF-8；换行统一使用 LF（`\n`）。
3. **中英文混排**  
   - 中英文之间尽量加空格，例如：`AI 协议`、`Redis key`；
   - 文件名中的英文与数字统一使用小写和连字符（`-`）或下划线（`_`）。

---

## 3. 目录与文件类型

1. **统一使用 Markdown**  
   - 文档统一使用 `.md`；配置/协议类使用 `.yaml`（例如 `langos/runtime/protocols/*.yaml`）。
2. **目录职责**  
   - 顶层目录含义以 `workspace-docs/README.md` 为准；子目录按语义清晰拆分（如 `repo1/`、`trading/`、`cross-repo/` 等）。
3. **不要在同一层放过多文件**  
   - 若某目录下文件超过 ~20 个，考虑再按子主题拆分；如 `workspace/specs/repo1/` 可按年份或模块再拆。

---

## 4. 命名规范

### 4.1 日期与时间

1. **日期格式**：统一使用 `YYYY-MM-DD`（如 `2025-12-10`）。  
2. **时间与时区**：使用 24 小时制，如 `2025-12-10 14:30`；涉及时区时显式写出，如 `(Asia/Shanghai)`。
3. **文件名中的日期**：需要按时间排序时使用 `YYYYMMDD-` 前缀，如 `20251210-repo1-architecture-update.md`。

### 4.2 文档文件名

1. **规格文档（spec）**  
   - 路径示例：`workspace/specs/repo1/202512-feature-x-spec.md`  
   - 格式：`YYYYMM[-optional]-short-name-spec.md`，例如 `202512-feature-x-spec.md`、`20251210-order-refactor-spec.md`。
2. **ADR 文档**  
   - 路径示例：`workspace/adr/0001-use-redis-for-bundle-storage.md`  
   - 格式：`NNNN-short-name.md`，其中 `NNNN` 为递增编号（从 `0001` 开始）。
3. **项目文档**  
   - 路径示例：`workspace/projects/project-trading-bot/overview.md`  
   - 常用文件名：`overview.md`、`timeline.md`、`repos.md`、`tasks.md`。
4. **repo 说明文档**  
   - 路径示例：`workspace/repos/repo1/overview.md`  
   - 建议文件：`overview.md`（用途/模块/依赖）、`architecture.md`（架构与组件）、`data-flow.md`（数据/事件流）、`ai-notes.md`（AI 注意事项）。

### 4.3 AI 协议与索引

1. **协议文件名**  
   - 路径：`langos/runtime/protocols/<id>.yaml`；`<id>` 使用小写字母、数字和下划线，语义清晰，如 `add_protocol`、`add_repo_index` 等。
2. **协议索引**  
   - 路径：`langos/runtime/protocols/index.yaml`；所有协议必须在索引中注册，才能被自然语言调用。

---

## 5. 文档结构与写作方式

1. **标题层级**：`#` 主标题（仅一个），`##` 一级小节，`###` 二级小节，原则上不超过 4 级。
2. **段落与列表**：简短段落、一段一事；步骤/要点优先用列表。
3. **代码与配置片段**：使用代码块，例如：
   ```bash
   mkdir -p workspace-docs/langos/runtime/protocols
   ```
   ```yaml
   id: add_protocol
   ...
   ```
4. **引用与备注**：需要强调时可用 `>` 引用；避免冗长口语化吐槽。
5. **语言风格**：简洁、明确；对 AI 重要的约束可用“必须”“不得”表述。

---

## 6. AI 行为相关约定（基础层）

> 任务级流程由 `langos/runtime/protocols/*.yaml` 定义；这里是所有场景通用的基础约定。

1. **确认优先**：破坏性或大改前先复述理解并征求确认，典型包括重构大模块、重写核心文档、设计/修改协议。
2. **不编造业务逻辑**：边界不清时先提问；必须假设时标注“假设”并提示确认。
3. **文档优先，但不盲信**：发现文档与代码不一致要指出，并与用户决定更新文档还是调整代码；必要时执行相关协议（如 `refresh_repo_docs`）。
4. **遵守目录与命名规范**：新增文档优先放入约定目录（如 `workspace/specs`、`workspace/repos` 等），命名遵守本节规则。
5. **协议调用规则**：不得要求用户提供协议路径；应通过 `index.yaml` 将意图映射到协议 id，确认后再执行。

---

## 7. 版本与演进

1. **修改本规范的流程**：觉得某条不适用即可修改；若影响较大（如命名规则变化），建议在文末添加变更记录，或写 ADR（放在 `workspace/adr/`）说明原因。
2. **变更记录（示例）**  
   - 2025-12-10：创建 `conventions.md` 初版，定义基本命名规则与 AI 行为约定。

---

## 8. 实际使用建议

- 自己写文档：对照本文件逐步养成习惯，无需一次做到完美。
- 让 AI 写文档：可以先声明“请遵守 `workspace-docs/langos/meta/conventions.md` 来写”。
- 发现常见重复任务：除了写文档规范外，可考虑用 `add_protocol` 将流程固化到 `langos/runtime/protocols/`。

这样，`conventions.md` 提供基础规则，`protocols/*.yaml` 提供具体流程，两者配合使用。

---

## 9. workspace-docs 仓库的迭代规范

当需要调整 `workspace-docs` 本身的结构、规范或协议时，参考以下约定，避免越改越乱。

### 9.1 基本原则

1. 像维护代码一样维护文档：每个改动有明确目的；小步快跑，避免大爆炸式重构。
2. 不破坏已有约定，或明确说明破坏点：变更路径/命名时同步更新引用（README、索引、协议等），并说明破坏性影响。
3. 能增补就不硬重写：优先加小节/示例；只有结构完全不适配时才整体重写，并配套 ADR 说明。
4. AI 只能建议，最终决策在你：AI 可给出分析与方案，但是否实施由人类决定。

### 9.2 常见改动类型

**新增文档/目录**  
- 判断顶层归属（`workspace/repos` / `workspace/projects` / `workspace/domain` / `workspace/specs` / `workspace/adr` 等）。  
- 命名按本文件；有索引则同步更新（如 `workspace/repos/INDEX.md`、`protocols/index.yaml`）。

**修改现有文档（非结构性）**  
- 小改可直接改；语义变化建议在文中或变更记录说明背景；若影响其他文档，至少提示影响范围。

**结构调整/目录重构**  
- 先给出重构前后对比草案，确认后再移动文件。  
- 同步更新：`workspace-docs/README.md`、相关索引（如 `workspace/repos/INDEX.md`、`protocols/index.yaml`）、其它引用。  
- 较大调整建议写 ADR（如 `workspace/adr/00xx-refactor-workspace-docs-structure.md`）。

**协议与索引演进**  
- 协议：`langos/runtime/protocols/<id>.yaml`；索引：`langos/runtime/protocols/index.yaml`。  
- 新增协议优先走 `add_protocol` 流程；修改协议视为升级，调整 `version` 并说明主要变化；删除需谨慎，可先标记 `deprecated`。

**规范（conventions）变更**  
- 尽量向后兼容：新增规则 > 微调措辞 > 重写规则。  
- 影响广的改动可在文末记录，必要时写 ADR 说明原因。  
- 新文档遵守最新规范；旧文档可在演进中逐步对齐，无需一次性迁移。

### 9.3 AI 参与迭代的标准流程

1. 信息收集：询问痛点（难查？难改？AI 用不顺？），阅读 `workspace-docs/README.md`、`langos/meta/*`、`langos/runtime/*` 等。
2. 现状总结：简要总结当前结构/规范状态，与痛点关联。
3. 备选方案：给出几种路径（小增补/轻度重组/大重构），说明优缺点与影响范围。
4. 用户选择：用户定方案；不确定时建议从最小可行改动开始。
5. 变更计划：列出需新增/移动/重命名的文件/目录，及要更新的索引/协议/规范。
6. 分步落地：每类改动前先给预期变更，确认后执行；避免黑箱式一键全改。

---

遵守以上规范，可让 `workspace-docs` 成为一个可持续演进、可控复杂度的知识库，人类和 AI 都能高效协作。
