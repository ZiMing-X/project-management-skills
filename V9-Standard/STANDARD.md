# V9-Standard 标准版

> **版本标识**: V9-Standard | v2.9.0
> **版本类型**: 标准版（日常推荐）
> **技能数量**: 20
> **发布日期**: 2026-05-16
> **更新日期**: 2026-05-18
> **上级版本**: 从 V10-Standard (v10.0.2) 转化

---

## 版本定位

V9-Standard 是 xmgl 治理框架的**标准版**。包含 20 个自研技能（Base 5 + 通用工具 15），基于 V10-Standard 最新版本转化，剥离全部时间信标，保留所有优化内容和辅助目录。

**核心差异（vs V10）**：
- V9-Standard：纯优化文本，无推理指令
- V10-Standard：V9 技能集 + 全员 L3 时间信标

**适用场景**：
- 日常项目管理使用者
- 偏好简洁无附加指令的技能体系
- 跨会话持久化的项目追踪

---

## 三层架构

```
V9-Full (27)
  ├── V9-Base (5)      → 最小可行集
  ├── V9-Standard (20) → 日常推荐
  └── V9-Full (27)     → 全部
```

### Base 层（5技能）— 项目生命周期骨架

| # | 技能 | 定位 |
|:--|:---|:---|
| 1 | project-tracker | 事件日志/周报/待办 |
| 2 | project-manager | 项目文档体系管理 |
| 3 | shutdown-protocol | 收工持久化协议 |
| 4 | session-guard | 会话启动守护 |
| 5 | checkpoint-guard | 复杂任务门禁 |

### Standard 层（20技能）— Base + 15通用工具

Base(5) + automation-verifier, automation-task-auditor, context-survival, install-skill-package, problem-solution-standard, prompt-advisor, prompt-optimizer, session-manager, skill-integration-merge, token-monitor, window-aware, workbuddy-debug-helper, p10-archive-router, v9base-deploy, v9-release-checker

### Full 层（27技能）— Standard + 7领域/批处理

Standard(20) + acp-proxy-bypass, db-change-analyzer, m24-b6-collection, m29-b6-collection, plugin-kb-insight, v8.1-batch, v9-feedback-optimizer

---

## 技能清单（20个标准）

| # | 技能 | 分类 |
|:--|:---|:---|
| 01 | checkpoint-guard | 门禁 |
| 02 | project-manager | 项目管理 |
| 03 | project-tracker | 项目追踪 |
| 04 | session-guard | 会话守护 |
| 05 | shutdown-protocol | 收工协议 |
| 06 | automation-task-auditor | 治理工具 |
| 07 | automation-verifier | 治理工具 |
| 08 | problem-solution-standard | 标准化 |
| 09 | p10-archive-router | 归档路由 |
| 10 | prompt-advisor | 提示词 |
| 11 | prompt-optimizer | 提示词 |
| 12 | skill-integration-merge | 技能治理 |
| 13 | install-skill-package | 部署工具 |
| 14 | v9base-deploy | 部署工具 |
| 15 | v9-release-checker | 发布检查 |
| 16 | token-monitor | 部署工具 |
| 17 | context-survival | 记忆自保 |
| 18 | session-manager | 会话管理 |
| 19 | window-aware | 窗口感知 |
| 20 | workbuddy-debug-helper | 调试 |

---

## 框架文档

| 文件名 | 说明 |
|:---|:---|
| `00-xmgl框架总览.md` | 框架顶层设计，含架构总览和4条铁律 |
| `00-技能全景图.md` | 27技能详解、触发词、版本信息 |

## 配套文档（docs/）

| 文件名 | 说明 |
|:---|:---|
| `快速入门.md` | V9-Standard 上手指南 |
| `版本管理标准.md` | 版本号规范 |
| `版本管理维护指南.md` | 版本检测/升级/快照流程 |

## 环境配置

| 文件名 | 说明 |
|:---|:---|
| `config.env.example` | 环境变量配置模板 |

---

## 版本历史

| 版本 | 日期 | 说明 |
|:---|:---|:---|
| v2.9.0 | 2026-05-17 | V9-Standard：20技能标准版，从V10-Standard v10.0.2转化 |

---

## 升级路径

```
V9-Base (5)  →  V9-Standard (20)  →  V9-Full (27)
```

---

*xmgl V9-Standard v2.9.0 | M29批次 | 作者：流風*
