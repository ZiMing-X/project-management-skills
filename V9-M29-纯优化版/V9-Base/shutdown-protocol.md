---
name: shutdown-protocol
description: 会话收工协议。会话结束前执行，自动更新所有变更项目的 state.md + project.md + TRUTH-SOURCE.md + 日记忆汇总。触发词：收工、shutdown、wrap up、结束会话。
agent_created: true
allowed-tools: Read, Write, Bash
version: v2.9.0
updated: 2026-05-16
source: xmgl-M29-token-opt
---

# shutdown-protocol — 会话收工协议

记忆自保体系**写入端**。每次会话结束前执行，将变更持久化到权威文件。Session Guard 为读取端，两者配合形成自愈循环。

## 🔴 B6 防御铁律

| # | 铁律 | 违规则 |
|:--|:---|:------|
| 1 | **写后必须回读验证** — Write 后立即 Read 确认 | 标记"乐观完成"，状态回滚 |
| 2 | **Checkpoint 指针贯穿全程** — 起始写入 `shutdown_ptr: START`，每步更新 | 断联后无法恢复 |
| 3 | **单文件失败不阻断全局** — 记录错误码，跳过继续 | 阻塞将导致大面积不一致 |
| 4 | **收工未完成→下次启动告警** — TRUTH-SOURCE 残留 `shutdown_ptr != COMPLETED` | Session Guard 发出告警 |

## 触发条件

用户说：**"收工" / "shutdown" / "wrap up" / "结束会话"**

## 执行协议（7步）

```
Pre-Step → Step1 → Step2 → Step3 → Step4 → Step5 → Step6 → Step7
写入指针    扫描变更  更新      更新      更新      记录插件  写入记忆  输出摘要
             memory   state.md  project.md TRUTH-             收工标记  清除指针
```

| 步骤 | 操作 | 指针状态 | 说明 |
|:---|:-----|:--------|:-----|
| Pre | 写入 checkpoint 起始标记 → 回读确认 | `START` | 失败→B6-001阻断 |
| 1 | 扫描 memory/YYYY-MM-DD.md，提取变更项目 | `SCANNED` | |
| 2 | 逐项目更新 state.md → Write → Read 回读 | `STATE_UPDATED` | 失败→B6-002 |
| 3 | 逐项目更新 project.md → Write → Read 回读 | `PROJECT_UPDATED` | 失败→B6-003 |
| 4 | 更新 TRUTH-SOURCE.md → Write → Read 回读 | `TRUTH_UPDATED` | 失败→B6-004阻断 |
| 5 | 记录本次使用插件 | — | 可选 |
| 6 | 写入日记忆收工标记 → 回读 | `MEMORY_WRITTEN` | 失败→B6-005 |
| 7 | 输出收工摘要 + 清除 checkpoint 指针 | `COMPLETED`→清除 | 失败→B6-006 |

> **详细步骤说明** → `references/执行协议详情.md`

## 错误码表

| 错误码 | 说明 | 严重度 | 处理 |
|:------|:-----|:------:|:-----|
| 🔴 B6-001 | Checkpoint 起始标记写入失败 | 阻断 | 停止收工，输出警告 |
| 🟡 B6-002 | state.md 写入回读失败 | 非阻断 | 记入异常列表，跳过 |
| 🟡 B6-003 | project.md 写入回读失败 | 非阻断 | 记入异常列表，跳过 |
| 🔴 B6-004 | TRUTH-SOURCE 写入回读失败 | 阻断 | 输出警告，不继续 |
| 🟡 B6-005 | 记忆收工标记写入失败 | 非阻断 | 记入异常列表 |
| 🟡 B6-006 | Checkpoint 清除失败 | 非阻断 | 下次 Session Guard 告警 |

## 执行铁律

1. **增量更新不覆盖** — state.md 已有历史信息保留
2. **缺失不阻塞** — 某项目 state.md 不存在则记录跳过
3. **TRUTH-SOURCE 最后更新** — 所有 state.md 完成后才更新
4. **必输出摘要** — 收工摘要必须展示，让用户确认
5. **时间戳精确到分钟** — 格式 `YYYY-MM-DDTHH:MM`
6. **绝对路径** — 项目文件必须用 `E:\...` 或 `C:\Users\...` 绝对路径
7. **写后必回读** — 每次 Write 后必须 Read 确认（B6 #1）
8. 🚫 **禁止 `run_in_background`** — 已知 BUG #07 导致系统冻结，收工阶段使用前台 Agent 或直接工具调用


> ⚠️ **强制执行**：加载本技能时，**必须**完成以下推理并在收工摘要中输出：
> 1. 读取日记忆 → **按时间戳排序所有事件** → 标注操作先后顺序
> 2. 对比本次会话前后的 state.md 差异 → 识别**状态迁移点**（🔴→🟢 或 🟢→🔴）
> 3. 对每个迁移点 → **回溯原因链**："状态X变化是因为操作Y"
> 4. 收工摘要中显式包含**状态迁移诊断**段落

## 加载指引

- 详细步骤 → `references/执行协议详情.md`
- 错误码详情 → `references/B6错误码表.md`
- Checkpoint 状态机 + Session Guard 关系 → `references/checkpoint状态机.md`

---

*xmgl v2.9.0 | 纯优化版 | 作者: 流風*
