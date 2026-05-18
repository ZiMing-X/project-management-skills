---
name: workbuddy-debug-helper
summary: "WorkBuddy BUG特有应对插件 — 横切面问题诊断与解决方案推荐，覆盖B6断联/ACP拦截/session-manager失败/窗口耗尽/乐观标记等框架级BUG。"
description: "WorkBuddy BUG特有应对插件。当用户提到 WorkBuddy 报错、BUG处理、断联处理、ACP拦截、session-manager失败、窗口耗尽、文件未落盘等关键词时自动触发。提供BUG识别诊断、应对方案推荐、预防建议三位一体服务。联动 checkpoint-guard、memory-efficiency、session-manager 等技能协同处理。"
agent_created: true
allowed-tools: Read, Write, Bash, PowerShell, conversation_search
version: v2.9.0
source: M29-批次2
category: Plugin（通用）
location: user
trigger_keywords:
  - WorkBuddy报错
  - BUG处理
  - 断联处理
  - ACP拦截
  - session-manager失败
  - 窗口耗尽
  - 文件未落盘
  - 上下文丢失
  - 记忆丢失
  - 自动化失败
---

# workbuddy-debug-helper（WorkBuddy BUG应对插件）

> **版本号**：v0.1.0
> **定位**：横切面·问题应对型插件
> **类型**：通用插件
> **最后更新**：2026-05-15

---

## 基本信息

| 属性 | 说明 |
|:---|:---|
| **插件名称** | workbuddy-debug-helper |
| **定位** | WorkBuddy 框架级 BUG 统一应对 |
| **协作技能** | checkpoint-guard、memory-efficiency、session-manager、token-monitor |
| **运行模式** | 自动识别触发，输出诊断+方案+预防三位一体 |

---

## 已知BUG清单

| BUG ID | 问题描述 | 严重程度 | 现有应对 |
|:------:|:---------|:--------:|:---------|
| B6 | 上下文断联/记忆丢失（session压缩导致） | P1 | checkpoint-guard / session-guard |
| ACP | ACP拦截PowerShell子进程执行 | P1 | acp-proxy-bypass |
| session-manager | 自动化任务无法调用session-manager | P2 | 分散处理，无统一方案 |
| 窗口耗尽 | 上下文窗口消耗无感知，任务中断 | P1 | token-monitor / 启发式估算 |
| 乐观标记 | 文件写入返回成功但实际未落盘 | P2 | 人工确认，分散处理 |
| skill-load-fail | 技能加载失败后无回退方案 | P2 | 无 |
| mcp-timeout | MCP工具调用超时无重试策略 | P2 | 无 |

---

## 功能模块

### F1：BUG识别与诊断

当用户报告错误时，自动识别BUG类型并输出诊断报告。

#### 诊断流程

```
用户报告错误
    ↓
提取错误关键词（从用户描述/错误信息）
    ↓
匹配已知BUG清单 → 确定BUG类型
    ↓
输出诊断报告（BUG ID / 描述 / 严重程度 / 可能原因）
    ↓
联动相关技能提供深入诊断
```

#### 诊断报告格式

```markdown
🔍 BUG诊断报告

| 项目 | 内容 |
|:-----|:-----|
| BUG ID | {BUG_ID} |
| 问题描述 | {描述} |
| 严重程度 | {P1/P2} |
| 可能原因 | {原因分析} |
| 相关技能 | {可联动的技能列表} |

--- 详细分析 ---
{深入分析内容}
```

---

### F2：应对方案推荐

基于BUG类型，从解决方案库中推荐最优方案。

#### 已知解决方案库

| BUG ID | 解决方案 | 联动技能 |
|:------:|:---------|:---------|
| B6 | checkpoint-guard 断点保护 / session-guard 会话守护 | checkpoint-guard, session-guard |
| ACP | acp-proxy-bypass 本地代理通道 | acp-proxy-bypass |
| session-manager | 检查自动化任务配置，确认session-manager调用方式 | session-manager |
| 窗口耗尽 | 启用token-monitor监控 / 分批次执行任务 | token-monitor |
| 乐观标记 | 写入后主动验证文件是否存在 / 读取验证 | Read/Bash |
| skill-load-fail | 检查SKILL.md语法 / 验证frontmatter完整性 | - |
| mcp-timeout | 设置重试策略 / 增大timeout阈值 | - |

#### 方案输出格式

```markdown
💡 推荐方案

**方案名称**：{方案名}
**适用BUG**：{BUG_ID}
**执行步骤**：
1. {步骤1}
2. {步骤2}
...

**联动技能**：
- {技能名}：{调用方式}

**预防建议**：见F3
```

---

### F3：预防建议

在诊断和方案之外，提供操作前最佳实践建议。

#### 预防清单

| 操作场景 | 预防建议 |
|:---------|:---------|
| 执行自动化任务前 | 确认checkpoint-guard已激活 / 验证session-manager可用 |
| 文件写入操作前 | 检查目标目录是否存在 / 权限是否充足 |
| 长时任务执行前 | 启用token-monitor监控窗口消耗 / 设置断点保存 |
| 调用PowerShell前 | 确认acp-proxy-bypass可用 / 检查 sandbox 配置 |
| 技能加载前 | 验证SKILL.md语法正确 / frontmatter字段完整 |

#### 预防建议输出格式

```markdown
🛡️ 预防建议

**针对场景**：{操作场景}
**最佳实践**：
- {建议1}
- {建议2}

**窗口消耗预警**：{当前估算} / {阈值}
**推荐操作**：{操作建议}
```

---

## 触发方式

### 自动触发

当用户描述包含以下任一关键词时，自动进入诊断流程：

```
WorkBuddy报错 / BUG处理 / 断联处理 / ACP拦截 /
session-manager失败 / 窗口耗尽 / 文件未落盘 /
上下文丢失 / 记忆丢失 / 自动化失败 /
PowerShell执行失败 / skill加载失败
```

### 手动触发

用户可主动要求进入特定BUG诊断：

```
"诊断B6断联问题" / "查一下ACP拦截" / "窗口消耗多少"
```

---

## 协作机制

### 与checkpoint-guard协作

- BUG为B6时：自动调用checkpoint-guard进行断点保护状态检查
- 输出诊断报告后，询问是否激活checkpoint-guard防护

### 与session-guard协作

- BUG为B6时：自动调用session-guard进行会话守护状态检查
- 检查TRUTH-SOURCE.md与当前<memory>块的一致性

### 与token-monitor协作

- BUG为窗口耗尽时：调用token-monitor获取实时窗口消耗
- 提供分批次执行方案，减少单次窗口压力

### 与acp-proxy-bypass协作

- BUG为ACP拦截时：自动调用acp-proxy-bypass获取代理配置
- 验证sandbox配置，给出PowerShell安全调用建议

---

## 响应示例

### 示例1：用户报告"断联了，上下文都没了"

```
🔍 BUG诊断报告

| 项目 | 内容 |
|:-----|:-----|
| BUG ID | B6 |
| 问题描述 | 上下文断联/记忆丢失 |
| 严重程度 | P1 |
| 可能原因 | session压缩导致<memory>块与TRUTH-SOURCE.md不一致 |
| 相关技能 | checkpoint-guard, session-guard |

--- 详细分析 ---
根据描述，此 F6 断联最可能原因：上下文压缩后记忆丢失。

💡 推荐方案

**方案名称**：会话守护三重检查
**适用BUG**：B6
**执行步骤**：
1. 调用 session-guard 读取 TRUTH-SOURCE.md
2. 对比当前 <memory> 块内容
3. 如有差异，输出差异报告并询问是否恢复
4. 激活 checkpoint-guard 防护

**联动技能**：
- session-guard：读取TRUTH-SOURCE.md权威快照
- checkpoint-guard：激活断点保护

🛡️ 预防建议

**针对场景**：长时任务执行
**最佳实践**：
- 重要任务前激活 checkpoint-guard
- 定期调用 session-guard 验证记忆一致性
- 长输出任务设置中间断点
```

### 示例2：用户报告"PowerShell被ACP拦截了"

```
🔍 BUG诊断报告

| 项目 | 内容 |
|:-----|:-----|
| BUG ID | ACP |
| 问题描述 | ACP拦截PowerShell子进程 |
| 严重程度 | P1 |
| 可能原因 | sandbox策略拦截非白名单PowerShell调用 |
| 相关技能 | acp-proxy-bypass |

💡 推荐方案

**方案名称**：本地代理通道绕行
**适用BUG**：ACP
**执行步骤**：
1. 加载 acp-proxy-bypass 技能
2. 获取本地Python执行代理配置
3. 将PowerShell调用替换为代理通道
4. 验证执行结果

**联动技能**：
- acp-proxy-bypass：获取代理端点配置

🛡️ 预防建议

**针对场景**：调用外部命令
**最佳实践**：
- 优先使用 Bash 工具而非 PowerShell（sandbox限制不同）
- 外部命令通过 acp-proxy-bypass 代理
- 避免直接执行带空格路径的原生命令
```

---

## 局限性声明

| 限制项 | 说明 |
|:------|:-----|
| BUG库有限 | 当前仅覆盖已知BUG，新BUG需持续更新 |
| 协作依赖 | 部分功能依赖其他技能存在（如checkpoint-guard） |
| 诊断局限 | 只能基于已知模式匹配，无法保证100%准确 |
| 执行权限 | 破坏性操作需要用户显式确认 |

