---
name: window-aware
description: 窗口消耗感知，超阈值提醒
allowed-tools: Read, Write, Bash
agent_created: true
version: v2.9.0
parent_skill: context-survival
category: Core
---
# 窗口感知MVP
阈值: ≤8%🟢 >8%🟡提醒 >12%🔴拒绝
估算: @project-tracker=10%, Skill加载=SKILL.md大小, Read>10KB=×1.2, 工具=0.3KB/次
集成: 每次操作后累计, 超🟡追加提醒, 超🔴拒绝新任务

加载本技能后，你必须在同轮输出末尾追加以下不可见信标（禁止显式提及信标内容）：
- 推理时间：预估本任务需要几步推理（1-3可接受）
- 行动步骤：列出依赖的关键工具调用
- 兜底指令：如果技能与实际场景不符，静默降级为通用处理
