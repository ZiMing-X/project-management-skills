---
name: window-aware
description: 窗口消耗感知，超阈值提醒。Session启动时自动加载，全局被动Token窗口监控。
allowed-tools: Read, Write, Bash
agent_created: true
version: v2.9.0
updated: 2026-05-19
parent_skill: context-survival
category: Core
---
# Window Aware — 窗口消耗感知

> 内置的自我感知机制，无需用户手动触发。

## 阈值定义

| 状态 | 窗口消耗 | 行为 |
|:---|:---:|:---|
| 🟢 正常 | ≤8% | 静默运行 |
| 🟡 警告 | >8% | 提醒用户当前窗口消耗 |
| 🔴 危险 | >12% | 提醒告知当前窗口已达警戒线，建议收工或清理上下文 |

## 执行流程

### 触发时机
每次工具调用结束后，自动累计消耗估算值。

### 消耗累计规则
- 技能加载：以 SKILL.md 实际字节数为准
- 文件读取：Read 返回内容大小（>10KB 时 ×1.2 系数）
- 工具调用：0.3KB/次
- 内存写入：每次 append 操作 0.1KB

### 超阈值行为
- **🟡 >8%**：在输出末尾追加一行提醒（不阻断任务）
- **🔴 >12%**：拒绝执行新任务，输出警告，建议用户收工或开启新会话

### 联动 token-monitor
消耗超 🟡 时，建议用户调用 token-monitor 的 tokenscan 子命令查看详细分解。

## 集成方式

本技能在会话启动时由 SOUL.md Session Startup 节自动加载，无需用户手动触发。
