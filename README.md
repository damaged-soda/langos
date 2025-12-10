# langos

自然语言驱动的编程“操作系统”内核，提供对话规范与协议；业务文档不在此仓。

## 做什么
- 用自然语言驱动需求→方案→编码→评审，关键节点需确认，过程可审计、可复用。
- 内含治理层（愿景、规范）和接口层（行为规范、运行时、协议索引与协议）。
- 业务知识/需求/仓库说明请放在独立的业务文档目录/仓库。

## 快速开始（在 `work/` 目录）
```bash
git clone <LANGOS_REPO_URL> langos
```
AI 初始化时先读取：
- `runtime/guidelines.md`
- `runtime/runtime.md`
- `runtime/protocols/index.yaml`

按需再读：
- 改 OS/规范/协议：`meta/vision.md`、`meta/conventions.md`、`meta/README.md`
- 业务任务：读取你指定的业务文档目录（例如 `workspace-docs/workspace/` 下的 repos/specs/projects），缺什么再读什么。

## 目录提示
- `meta/`：愿景、分层、规范、迭代方式（人类优先；AI 仅在调整/理解 OS 时读）
- `runtime/`：行为规范、运行时、协议与索引（AI 优先；人类可读）

## 自然语言协议入口
使用 `runtime/protocols/index.yaml` 做协议检索；新增协议也需在此注册后才能自然语言调用。***
