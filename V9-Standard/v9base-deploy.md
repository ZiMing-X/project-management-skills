---
name: v9base-deploy
description: 一键部署WorkBuddy技能包。6步自动化流程：安全前置检查→扫描源目录→提取frontmatter→创建skills/<name>/SKILL.md子目录结构→部署验证→报告。触发词：@v9base-deploy 部署。
agent_created: true
allowed-tools: Read, Write, Bash
version: v2.9.0
category: Tool（工具）
---

# v9base-deploy（一键部署）

> v2.3 | 将任意 WorkBuddy 技能包一键部署到 `~/.workbuddy/skills/`

## 概述

| 属性 | 说明 |
|:---|:---|
| **功能** | 从源目录自动部署技能包到用户级 skills 目录 |
| **定位** | 全局技能，安装后任意会话可直接触发 |
| **Step 0** | 🔴 安全前置检查（强制执行，不可跳过） |

## 触发条件

| 场景 | 触发词 |
|:---|:---|
| 带路径部署 | `@v9base-deploy 部署 D:\路径` |
| 不带路径部署 | `@v9base-deploy 部署`（会询问路径） |

## 6 步部署流程

| 步骤 | 操作 | 关键点 |
|:---:|:---|:---|
| 0 | 安全前置检查 | P0 信号立即终止；P1 等确认；P2 仅记录 |
| 1 | 扫描源目录 | 排除 README/CHANGELOG/LICENSE/版本号文档 |
| 2 | 提取 frontmatter | name 字段→子目录名；无 frontmatter→文件名提取 |
| 3 | 创建子目录结构 | `skills/<name>/SKILL.md` 格式写入 |
| 4 | 自动验证 | 文件存在 + 非空 + YAML frontmatter 以 `---` 开头 |
| 5 | 生成报告 | 安全检查 + 部署清单 + 验证结果 |

## Step 0 安全检查

| 检查项 | 搜索关键词 | 响应 |
|:---|:---|:---:|
| 🔴 文件破坏 | `rm -rf`、`del /F`、`shutil.rmtree`、`os.remove` | 零容忍，终止 |
| 🔴 网络外联 | `http://`、`https://`、`curl`、`wget` | 需说明用途 |
| 🔴 系统修改 | `regedit`、`sudo`、`chmod 777` | 需确认无害 |
| 🔴 凭证泄露 | `~/.ssh/`、`~/.aws/`、`$env:PASSWORD` | 零容忍，终止 |
| 🟡 敏感目录 | Desktop、Downloads、Documents | 需用户确认 |

## 技能格式标准

```
~/.workbuddy/skills/
└── <技能名>/               ← 子目录名 = frontmatter name 字段
    └── SKILL.md             ← 入口文件名固定（大小写敏感）
```

Skill.md 文件头示例：
```yaml
---
name: <技能名>
summary: "<一行简介>"
description: |
  多行描述...
allowed-tools: Read, Write, Bash
agent_created: true
version: v2.9.0
---
```

## 关键约束

| # | 约束 |
|:--|------|
| 1 | Step 0 安全检查不可跳过，P0 信号立即终止 |
| 2 | 部署后需重启 WorkBuddy 才能加载新技能 |
| 3 | 禁止使用 `run_in_background: true`（防止系统冻结） |
| 4 | 来源不明的技能包，P1/P2 项按 P0 处理 |

## 加载指南

| 需求 | 读取文件 |
|:---|:---|
| 快速查阅部署流程 + 安全约束 | `SKILL.md`（本文件） |
| 完整输出示例、安全等级响应、兼容性表 | `references/部署流程与安全检查.md` |

---

*xmgl v2.9.0 | 纯优化版 | 作者: 流風*
