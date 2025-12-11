# langos

自然语言驱动的编程“操作系统”内核代码仓，整体即运行时（协议 + 启动提示 + 必读规范）。高层文档与设计背景在 `../langos-docs/`。

## 快速入口（AI 必读）
- `runtime/startup.md`：加载后模式选择与对话入口。
- `runtime/guidelines.md`：对话行为底线。
- `runtime/runtime.md`：运行时流程、确认闸门。
- `runtime/protocols/index.yaml`：协议索引与触发例句。
- `runtime/conventions.md`：命名、写作、落盘规范。

## 文档库
- 代码外的愿景/治理/自举说明等背景文档已迁移到 `../langos-docs/`（例如 `repos/langos/overview.md`、`vision.md`、`governance.md`、`specs/README.md`）。
- 如需自举或理解设计思路，先查阅文档库；执行/落盘时按本仓协议与规范操作，改协议前先明确需求与方案。
