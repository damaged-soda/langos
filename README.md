# langos

自然语言驱动的编程“操作系统”内核，提供愿景/规范（meta）和对话运行时/协议（runtime），供人类与 AI 以自然语言协作开发。

> 受众：人类 + AI（根目录共读）

---

## 1. 它是什么

- 用自然语言作为操作界面，协议作为“系统调用”，覆盖需求梳理 → 方案 → 编码 → 评审四环节。  
- 目标：可控（关键节点需确认）、可审计（输入/输出/决策可追溯）、可复用（协议/模板沉淀）。  
- 本仓只包含 OS 内核，不含业务文档。业务知识库（如 `workspace-docs`）可独立存在，并按需与 langos 协作。

---

## 2. 目录结构

```txt
langos/
  README.md
  meta/                 # 愿景、规范、迭代方式
    README.md
    conventions.md
    vision.md
  runtime/              # 行为规范、运行时说明、协议
    guidelines.md
    runtime.md
    protocols/
      index.yaml        # 协议索引（必须）
      add-protocol.yaml
      add-repo-index.yaml
      add-requirement.yaml
      refresh-repo-docs.yaml
```

---

## 3. 快速使用（在 `work/` 目录）

```bash
git clone <LANGOS_REPO_URL> langos
# 可再准备/指定业务文档目录（例如 workspace-docs/workspace/），供业务任务时按需读取
```

AI 初始化时应先加载 langos 基础运行包：
- `langos/runtime/guidelines.md`
- `langos/runtime/runtime.md`
- `langos/runtime/protocols/index.yaml`

当意图是修改/理解 OS 自身（规范/协议/分层）时，再阅读治理包：
- `langos/meta/vision.md`
- `langos/meta/conventions.md`
- `langos/meta/README.md`（可选）

业务任务时，按需阅读你指定的业务文档目录（例如 `workspace-docs/workspace/` 下的 repos/specs/projects 等），缺什么再读什么。

---

## 4. 按需阅读路径（节省注意力）

- 任何任务（AI 必读）：`runtime/guidelines.md`、`runtime/runtime.md`、`runtime/protocols/index.yaml`
- 改 OS/协议/规范：再读 `meta/vision.md`、`meta/conventions.md`、`meta/README.md`
- 业务任务：读取你指定的业务文档目录（仓库索引 → 目标仓库文档 → 相关需求/项目），langos 本身不带业务内容。

---

## 5. AI 调用协议的约定

1) 解析意图并在 `runtime/protocols/index.yaml` 匹配 1–3 个候选；无匹配则说明走普通对话。  
2) 用户确认后执行对应协议，遵循步骤，不跳过 ASK/CONFIRM/AWAIT。  
3) 信息不足先问，不编造；必要假设需标注并复述确认。  
4) 收尾总结达成与未决。  
5) 禁止要求用户提供协议路径。

---

## 6. 业务知识库协作方式（示例）

- 将 langos 放在 `work/langos/`；业务文档可在同级目录下的其它仓库（如 `work/workspace-docs/`）。  
- AI 工作流程：先加载 langos 基础运行包 → 检测/询问业务文档目录 → 业务任务时按需读取。  
- 若需要自定义业务协议，可：  
  - 提 PR 到 langos（推荐统一管理）；或  
  - 在业务仓单独维护扩展协议/索引，并声明优先级与路径。

---

## 7. 迭代与版本

- 建议用标签发布（如 `v0.1.0`），业务仓可固定到特定 tag/commit。  
- 协议/规范变更时：先复述意图，调整对应文件，必要时 bump 版本并记录主要变化。  
- 受众标记仅适用于 Markdown；YAML/配置不加受众标识。
