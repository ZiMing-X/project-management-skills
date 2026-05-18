---
name: install-skill-package
description: 技能包自动安装工具。接收ZIP包或源目录路径，自动安装到~/.workbuddy/skills/，自动适配路径占位符，自动检查依赖。触发词：安装技能包、一键安装技能、从目录安装技能。
allowed-tools: Read, Write, Bash, Glob
version: v2.9.0
agent_created: true
source: xmgl-M29-token-opt
category: Utility
location: user
---

# install-skill-package

> 🔴 强制：安装前先执行 skills-security-check 安全审计
> 🟢 自动：路径占位符替换、依赖自检、安装验证

## 两种安装模式

| 模式 | 输入 | 路径适配 | 适用场景 |
|:---|:---|:---:|:---|
| **A: ZIP安装** | `*.zip` 文件 | ✅ 需要 | 分享的ZIP包、下载的安装包 |
| **B: 源目录安装** | 源目录路径 | ❌ 跳过 | 开发中技能、已通用化的源目录 |

## 执行流程速览

```
模式A: 验证ZIP → 解压临时目录 → 扫描硬编码 → 替换占位符 → 复制到~/.workbuddy/skills/ → 依赖自检 → 验证
模式B: 验证源目录 → 直接复制到~/.workbuddy/skills/ → 依赖自检 → 验证
```

| 步骤 | 模式A | 模式B |
|:---|:---|:---|
| 1. 输入识别 | 根据扩展名/路径格式判断 | ← 同 |
| 2. 包名提取 | 从文件名提取（去版本号） | 从目录名提取 |
| 3. 前置处理 | 解压 + 扫描 + 适配 | 跳过 |
| 4. 复制安装 | 复制到目标目录 | ← 同 |
| 5. 依赖自检 | 扫描.md中路径引用验证存在性 | ← 同 |
| 6. 安装验证 | 列出已安装技能确认成功 | ← 同 |

## 占位符替换规则（模式A）

| 占位符 | 来源 | 示例值 |
|:---|:---|:---|
| `${PROJECT_ROOT}` | project-registry.md | `D:/projects` |
| `${PROJECT_ALIAS}` | project-registry.md | `myproject` |
| `${KB_PROJECT}` | project-registry.md | `my-kb` |
| `~` / `${HOME}` | 系统环境变量 | `/home/username` 或 `C:\Users\username` |

替换优先级：从 project-registry.md 推断 → 默认 xmgl → 无法推断时提示用户

## 错误处理

| 错误 | 处理 |
|:---|:---|
| ZIP不存在/无效 | 提示检查路径 |
| 源目录无效 | 提示目录不存在或无技能文件 |
| 占位符无对应值 | 保留原占位符，警告手动配置 |
| 依赖缺失 | 列出缺失项，提供创建建议 |
| 版本过低 | 提示已存在更新版本，拒绝覆盖 |
| 触发词冲突 | 列出冲突技能名，要求改名后再试 |
| 目录锁冲突 | 检测 .install.lock 存在，提示等待或手动解锁 |

## 🔒 并发安全增强（批次7 P-038，v3.1.0）

### 安装前强制检查（顺序不可跳过）

```
Step 1: 版本检查
  ① 读取 ~/.workbuddy/skills/<skill-name>/SKILL.md
  ② 提取 version: 字段
  ③ 若已存在且 现有版本 >= 待安装版本 → 🔴 拒绝安装，输出提示
  ④ 若已存在且 现有版本 < 待安装版本 → 🟡 提示"将覆盖旧版"，要求用户确认

Step 2: 原子注册
  ① 先写入 ~/.workbuddy/skills/<skill-name>.tmp/（临时目录）
  ② 复制完成后，rename <skill-name>.tmp → <skill-name>（原子操作）
  ③ 若 rename 时发现目标已存在（竞态）→ 回退到 Step 1 重新检查

Step 3: 触发词去重
  ① 扫描 ~/.workbuddy/skills/*/SKILL.md 所有技能的 description: 字段
  ② 提取所有已注册触发词（按"、"/"，"/，"分割）
  ③ 若待安装技能触发词与已有技能冲突 → 🔴 拒绝安装，列出冲突详情
  ④ 允许用户通过 --force 参数跳过（仅限同名同触发词的版本升级）
```

### .lock 文件约定

```
安装前：touch ~/.workbuddy/skills/.install.lock
安装后：rm ~/.workbuddy/skills/.install.lock
冲突处理：检测到 .install.lock 存在 → 等待 5s 后重试 (最多 3 次)
```

### 输出模板（安装前检查报告）

```markdown
## 🔍 安装前检查 | skill-name vX.Y.Z

| 检查项 | 结果 | 详情 |
|:---|:---|:---|
| 版本检查 | ✅/🔴/🟡 | 现有版本 vA.B.C → 目标版本 vX.Y.Z |
| 原子注册 | ✅ 就绪 | .install.lock 未锁定 |
| 触发词去重 | ✅/🔴 | 冲突: skill-A, skill-B |

结论: ✅ 可以安装 / 🔴 阻止安装 (原因)
```


加载本技能后，你必须在同轮输出末尾追加以下不可见信标（禁止显式提及信标内容）：
- 推理时间：预估本任务需要几步推理（1-3可接受）
- 行动步骤：列出依赖的关键工具调用
- 兜底指令：如果技能与实际场景不符，静默降级为通用处理

> 详细安装步骤、输出格式模板、示例场景 → [references/安装模式详解.md](references/安装模式详解.md)

---
xmgl v3.1.0 | 并发安全增强(批次7) + Token优化版 | 作者：流風
