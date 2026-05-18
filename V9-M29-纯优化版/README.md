# xmgl V9 — AI辅助项目治理框架（纯优化版）

> **版本**：v2.9.0  
> **定位**：无时间信标，最小 Token 消耗，即插即用  
> **发布日期**：2026-05-18

## 什么是 V9-M29 纯优化版

V9 是 xmgl 技能体系的纯优化版本——剥离了所有时间信标（L3 信息），专注于：

- **最小 Token 消耗**：去除冗余元数据，加载即用不浪费窗口
- **清晰三层架构**：Base → Standard → Full，按需选择
- **即插即用**：所有技能文件可直接部署到 `~/.workbuddy/skills/`

## 三层架构

```
┌──────────────────────────────────────┐
│ V9-Full (27技能)                      │
│ Base + Standard + 8个高级模块         │
│ 含DB变更分析/知识库插件/反馈优化等     │
├──────────────────────────────────────┤
│ V9-Standard (20技能) ← 日常推荐       │
│ Base + 15个扩展                       │
│ 含自动化审计/部署/上下文管理等         │
├──────────────────────────────────────┤
│ V9-Base (5技能) ← 最小内核            │
│ 门禁/项目管理/会话守护/收工协议       │
└──────────────────────────────────────┘
```

| 版本 | 技能数 | 定位 | 适用场景 |
|:---|:---:|:---|:---|
| **V9-Base** | 5 | 最小内核 | 只需基本项目管理能力 |
| **V9-Standard** | 20 | 日常推荐 | 完整日常工作效率套装 |
| **V9-Full** | 27 | 完整功能 | 全场景覆盖（含DB/插件） |

## V9-Base（5个核心技能）

| # | 技能 | 作用 |
|:--|:---|:---|
| 1 | checkpoint-guard | 复杂任务门禁 |
| 2 | project-manager | 项目文档管理 |
| 3 | project-tracker | 事件追踪报告 |
| 4 | session-guard | 会话状态守护 |
| 5 | shutdown-protocol | 收工持久化 |

## 快速开始

### 部署任意版本

```bash
# 以 V9-Base 为例，复制到 skills 目录
cp V9-Base/*.md ~/.workbuddy/skills/
```

或使用一键部署：

```bash
# 使用 v9base-deploy 自动化部署
cd V9-Standard
python scripts/deploy.py --target ~/.workbuddy/skills/
```

## 目录结构

```
V9-M29-纯优化版/
├── README.md               ← 你在这里
├── V9-Base/                ← 5技能最小内核
│   ├── checkpoint-guard.md
│   ├── project-manager.md
│   ├── project-tracker.md
│   ├── session-guard.md
│   └── shutdown-protocol.md
├── V9-Standard/            ← 20技能日常套装
│   ├── README.md
│   └── *.md (20技能)
├── V9-Full/                ← 27技能完整版
│   ├── docs/
│   ├── scripts/
│   ├── release/
│   └── *.md (32文件含框架文档)
└── release/                ← ZIP 分发包
    ├── V9-Base-v2.9.0-5技能-纯优化版.zip      (11.2 KB)
    ├── V9-Standard-v2.9.0-20技能-纯优化版.zip  (含全配套)
    └── V9-Full-v2.9.0-27技能-纯优化版.zip      (173.4 KB)
```

## 与 V10 的关系

| 维度 | V9 | V10 |
|:---|:---|:---|
| 信标 | 无时间信标 | 含 L3 时间信标 |
| 技能数 | 27 | 28（+doc-generator） |
| Token | 最小 | 标准 |
| 适用 | 稳定生产环境 | 测试/开发环境 |

## 变更历史

- 2026-05-17：v9-p1-init 初始化，v9-p2-strip-skills 去信标，v9-p3-strip-meta 元文档去信标
- 2026-05-18：v9-p4-package 完成三子集生成 + 打包
