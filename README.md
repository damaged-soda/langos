# langos

自然语言驱动的编程“操作系统”内核，提供对话规范与协议；业务文档不在此仓。

## 目录地图（分层与职责）
```
langos/                 ← 本仓，仅存治理/运行时/协议
├─ meta/                ← 治理层：愿景、规范、迭代方式
│  ├─ README.md         ← meta 定位说明
│  ├─ conventions.md    ← 基础规范（命名/写作/AI 底线）
│  └─ vision.md         ← 愿景与分层定位
├─ runtime/             ← 接口与执行层：对话约束、运行时、协议
│  ├─ guidelines.md     ← 对话流程与行为约束
│  ├─ runtime.md        ← 运行时流程（路由、确认闸门）
│  ├─ startup.md        ← 加载后给人/AI 的启动提示
│  └─ protocols/        ← 协议及索引（index.yaml、各 *.yaml）
└─ （无业务文档）      ← 业务资产放在你指定的业务文档目录/仓库（可自定义）
```

## 最小使用路径（人/AI 都可参考）
1) 获取本仓：`git clone <LANGOS_REPO_URL> langos`（在 `work/` 目录）。
2) AI 初始化必读：`runtime/guidelines.md`、`runtime/runtime.md`、`runtime/protocols/index.yaml`。
3) 人/AI 对话入口：按 `runtime/startup.md` 先选模式（学习/治理/业务开发）。
4) 业务开发时：指定业务文档目录/仓库（可自选，例如已有的 docs 仓或 `workspace/` 目录）；按协议执行需求→方案→编码→评审，关键节点必须确认。
5) 改规范/协议：阅读 `meta/*` 与目标协议，修改后同步 `runtime/protocols/index.yaml`。

## 其他提示
- 业务知识/需求/仓库说明请放在独立的业务文档目录/仓库（路径可自定义，保持与代码仓分离即可）。
- 自然语言协议检索入口：`runtime/protocols/index.yaml`；新增协议需先登记。***
