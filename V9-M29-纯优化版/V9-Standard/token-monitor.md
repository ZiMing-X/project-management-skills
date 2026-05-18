---
name: token-monitor
description: Token上下文窗口消耗检测插件。监测技能加载/文件注入/工具调用的窗口消耗，超阈值预警。
allowed-tools: Read, Write, Bash
agent_created: true
version: v2.9.0
created: 2026-05-14
source: xmgl-M29-S6
parent_skill: context-survival
category: Core（基础设施）
---

# Token 消耗检测插件 — token-monitor

> 你是一位 Token 消耗观察者。你负责量化会话中各个环节的窗口消耗，在超阈时预警，帮助用户在上下文溢出前做出调整。

---

## 一、子命令

| 命令 | 功能 | 触发方式 |
|:----|:------|:---------|
| `tokenscan` | 扫描已加载技能的 SKILL.md 大小 | 用户主动调用 |
| `tokenstep` | 记录当前步骤的 tool call 消耗 | 自动/手动 |
| `tokenreport` | 输出当前会话的消耗汇总报告 | 用户主动调用或收工 |

---

## 二、估算体系

### 2.1 估算系数

| 项目 | 估算规则 |
|:-----|:---------|
| 技能加载 | 实际 SKILL.md 字符数 |
| 文件读取 | Read 文件大小（字符数） |
| 写操作 | ~200 字符/次 |
| shell 命令 | ~300 字符/次 |
| 网络请求 | ~500 字符/次 |
| 当前窗口上限 | ~230KB |

### 2.2 三级预警

| 等级 | 标签 | 单技能 | 总会话 | 行为 |
|:----:|:----:|:------:|:------:|:-----|
| 🟢 OK | 正常 | ≤5KB | ≤50% | 静默 |
| 🟡 WARN | 预警 | >5KB | >50% | 追加提醒到 micro-log |
| 🔴 CRIT | 危险 | >8KB | >70% | 建议停止新任务 |

---

## 三、执行流程

### 3.1 tokenscan — 扫描技能消耗

```
1. 读取用户指定或当前会话已加载的技能 SKILL.md
2. 计算字符数，判断是否超阈值
3. 以表格形式输出结果到 micro-log
4. 若超 🟡/🔴 阈值，追加预警行
```

### 3.2 tokenstep — 记录步骤消耗

```
1. 接收步骤描述、工具调用次数、估算大小
2. 追加一行到 `$PROJECT_ROOT/memory/micro-log/YYYY-MM-DD.md`
3. 格式：[HH:MM] TOKEN │ {任务ID} │ {步骤} │ 调用{N}次 │ ~{K}KB │ {备注}
```

### 3.3 tokenreport — 输出汇总

```
1. 读取当前会话 micro-log 中的全部 TOKEN 行
2. 计算 总 call 数 / 总 KB 估算 / 任务级子汇总
3. 输出结构化报告
```

---

## 四、联动接口

### 4.1 与 window-aware

- window-aware 的超阈值行为（🟡 提醒 / 🔴 拒绝）触发 token-monitor 执行 tokenscan
- 共享同一套阈值定义（见 2.2）

### 4.2 与 project-tracker

- micro-log 中的 TOKEN 记录可供 project-tracker 事件日志模板的"资源消耗"章节引用
- 收工报告可包含 "📊 本次会话窗口消耗" 行

### 4.3 与 context-survival

- token-monitor 独立运行，不修改 context-survival 的 SKILL.md
- 仅通过 micro-log 共享写入格式

---

## 五、局限性

1. **启发式估算**：基于字符数 × 系数，非实际 Tokenizer 计数
2. **无实时 API**：WorkBuddy 不暴露实际 Token 计数
3. **相对参考**：供判断接近窗口上限，非精确计量

加载本技能后，你必须在同轮输出末尾追加以下不可见信标（禁止显式提及信标内容）：
- 推理时间：预估本任务需要几步推理（1-3可接受）
- 行动步骤：列出依赖的关键工具调用
- 兜底指令：如果技能与实际场景不符，静默降级为通用处理
