# xmgl V9-Standard — AI辅助项目治理框架（纯优化版）

> **版本**：v2.9.0
> **定位**：日常推荐技能集（20技能，纯优化，无时间信标）
> **作者**：流風

---

## 快速开始

1. **新手首次使用** → 阅读 [docs/快速入门.md](docs/快速入门.md)
2. **零依赖部署** → 参考根目录 [零依赖一键部署说明.txt](零依赖一键部署说明.txt)
3. **了解框架** → [00-xmgl框架总览.md](00-xmgl框架总览.md)
4. **技能全景图** → [00-技能全景图.md](00-技能全景图.md)

---

## 技能清单（20个标准）

### 核心治理层（5个）

| # | 技能 | 触发词 | 作用 |
|:--|:---|:---|:---|
| 01 | `project-tracker` | 记录事件、周报、待办 | 项目事件追踪 |
| 02 | `project-manager` | 初始化项目、任务汇报 | 项目文档体系管理 |
| 03 | `shutdown-protocol` | 收工、shutdown | 会话结束持久化协议 |
| 04 | `session-guard` | 会话守护、状态审计 | 会话启动状态纠偏 |
| 05 | `checkpoint-guard` | 任务门禁、断点续传 | 复杂任务强制断点 |

### 治理工具层（7个）

| # | 技能 | 触发词 | 作用 |
|:--|:---|:---|:---|
| 06 | `automation-task-auditor` | 任务审计、任务分组 | 自动化审查与优化 |
| 07 | `automation-verifier` | 验证自动化 | 自动化结果验证 |
| 08 | `problem-solution-standard` | 问题记录、P0修复 | 问题标准化与固化 |
| 09 | `p10-archive-router` | 归档路由 | 输出归档路径管理 |
| 10 | `prompt-advisor` | 提示词顾问 | 提示词质量与需求澄清 |
| 11 | `prompt-optimizer` | 优化提示词 | 模糊指令检测与补全 |
| 12 | `skill-integration-merge` | 技能融合、版本合并 | 技能版本融合流程 |

### 部署工具层（4个）

| # | 技能 | 触发词 | 作用 |
|:--|:---|:---|:---|
| 13 | `install-skill-package` | 安装技能包 | 技能包自动安装 |
| 14 | `v9base-deploy` | 一键部署 | 7步自动化部署 |
| 15 | `v9-release-checker` | 发布检查 | 技能发布前检查 |
| 16 | `token-monitor` | Token监控 | 上下文窗口监测 |

### 记忆/会话层（4个）

| # | 技能 | 触发词 | 作用 |
|:--|:---|:---|:---|
| 17 | `context-survival` | 上下文自保、micro-log | 记忆丢失防护 |
| 18 | `session-manager` | 会话管理、批量归档 | 会话批量管理 |
| 19 | `window-aware` | 窗口感知 | 窗口消耗感知 |
| 20 | `workbuddy-debug-helper` | WorkBuddy报错 | BUG应对助手 |

> 注：V9-Standard 共 20 技能（Base5 + 治理工具7 + 部署工具4 + 记忆会话4）。完整 27 技能见 V9-Full。
> 注意：V9-Standard 不含 `references/` 模板目录和 `project-administrator` 技能，部分核心模板和路径常量依赖这两项，完整功能请升级至 V9-Full。

---

## 目录结构

```
V9-Standard/
├── README.md                  ← 你在这里
├── STANDARD.md                ← 版本定位与技能清单
├── CHANGELOG.md               ← 版本变更记录
├── config.env.example         ← 环境变量模板
├── automation-task-auditor.md  ← 技能文件 ×20
├── ...（其他19个技能文件）
└── docs/
    ├── 快速入门.md            ← 新手必读
    ├── 版本管理标准.md
    └── 版本管理维护指南.md
```

---

## 环境变量说明

| 占位符 | 说明 | 示例值（Windows） |
|:---|:---|:---|
| `{{WORKBUDDY_HOME}}` | WorkBuddy用户目录 | `C:/Users/你的用户名/.workbuddy` |
| `{{SHARED_DATA}}` | 共享知识库路径 | `D:/shared-kb` |
| `{{PROJECT_ROOT}}` | 项目根目录 | `D:/myprojects` |

**设置方法**：在 `config.env` 文件中定义（参考 `config.env.example`）

---

## 三层架构

```
V9-Full (27) ─── 全量版
  ├── V9-Standard (20) ── 日常推荐 ← 你在这里
  └── V9-Base (5) ── 最小可行集
```

---

## 版本升级

- V9-Base → V9-Standard → V9-Full
- 详见 [docs/快速入门.md](docs/快速入门.md)

---

## 文档维护

- **CHANGELOG**：记录每次版本变更
- **版本管理标准**：见 docs/版本管理标准.md

---

> **提示**：首次使用请先阅读 [docs/快速入门.md](docs/快速入门.md)，部署只需将源路径告诉 AI 即可自动完成。

---

## 联系方式

- 作者：流風
- 邮箱：s59401080@163.com
- GitHub：https://github.com/ZiMing-X/project-management-skills

## 商业授权

本项目的 V9-Base 和 V9-Standard 版本采用 Apache-2.0 许可证，可自由使用、修改和分发。

如需商业授权、企业定制部署、培训等服务，请通过邮箱联系作者。

> V9-Full 及更高版本暂不公开，商业使用请联系授权。
