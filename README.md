# langos

由于AI注意力不足，本项目失败了

> 给 AI 的启动入口：
> - `runtime/startup.md`：加载后模式选择与对话入口。
> - `runtime/guidelines.md`：对话行为底线。
> - `runtime/runtime.md`：运行时流程、确认闸门。
> - `runtime/protocols/index.yaml`：协议索引与触发例句。
> - `runtime/conventions.md`：命名、写作、落盘规范。

langos 是一个以自然语言作为操作界面的“编程操作系统”内核：用**协议（YAML）**将协作流程结构化，使 AI 在「需求 → 方案 → 编码 → 评审」的闭环里保持**可控、可审计、可复用**。

- 本仓提供：运行时最小集（启动提示、对话规范、协议索引与协议定义）。
- 可选文档库（设计背景/治理蓝图等）通常放在同一工作区的其他 repo（例如 `langos-docs/`），运行时不依赖它也能工作。

## 推荐工作区入口
建议把 langos 和你要治理/开发的所有 repo 放在同一个 `work/` 下：

```text
work/
├─ langos/            ← 本仓：运行时与协议
├─ repo1/             ← 你要开发/治理的目标仓库
├─ repo2/
└─ langos-docs/       ← 可选：高层背景/蓝图/治理文档
```

这样你可以在 work/ 作为工作区根目录启动 Codex，使其具备跨 repo 的读写与对照能力。

## 快速上手
```shell
cd work
git clone <langos 仓库地址> langos
codex
> 加载当前目录下的langos
```

## 文档库
langos已实现自举，如果需要开发/了解设计思路，参考 https://github.com/damaged-soda/langos-docs
