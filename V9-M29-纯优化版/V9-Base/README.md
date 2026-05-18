# xmgl V9-Base — AI辅助项目治理框架（纯优化版）

> **版本**：v2.9.0
> **定位**：最小可行集（5技能，纯优化，无时间信标）
> **作者**：流風

---

## 快速开始

1. **新手首次使用** → 阅读 [docs/快速入门.md](docs/快速入门.md)
2. **零依赖部署** → 参考根目录 [零依赖一键部署说明.txt](零依赖一键部署说明.txt)
3. **了解框架** → 参阅 V9-Standard 或 V9-Full 的框架总览文档

---

## 技能清单（5个核心）

| # | 技能 | 触发词 | 作用 |
|:--|:---|:---|:---|
| 01 | `project-tracker` | 记录事件、周报、待办 | 项目事件追踪 |
| 02 | `project-manager` | 初始化项目、任务汇报 | 项目文档体系管理 |
| 03 | `shutdown-protocol` | 收工、shutdown | 会话结束持久化协议 |
| 04 | `session-guard` | 会话守护、状态审计 | 会话启动状态纠偏 |
| 05 | `checkpoint-guard` | 任务门禁、断点续传 | 复杂任务强制断点 |

---

## 目录结构

```
V9-Base/
├── README.md                  ← 你在这里
├── STANDARD.md                ← 版本定位与技能清单
├── CHANGELOG.md               ← 版本变更记录
├── config.env.example         ← 环境变量模板
├── 零依赖一键部署说明.txt      ← 零依赖部署说明
├── checkpoint-guard.md         ← 技能文件 ×5
├── project-manager.md
├── project-tracker.md
├── session-guard.md
├── shutdown-protocol.md
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
  ├── V9-Standard (20) ── 日常推荐
  └── V9-Base (5) ── 最小可行集 ← 你在这里
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

> **提示**：Base 版仅含项目治理骨架，日常使用请升级至 [V9-Standard](../V9-Standard/)（20技能）。首次使用请先阅读 [docs/快速入门.md](docs/快速入门.md)。

---

## 联系方式

- 作者：流風
- 邮箱：s59401080@163.com
- GitHub：https://github.com/ZiMing-X/project-management-skills

## 商业授权

本项目的 V9-Base 和 V9-Standard 版本采用 Apache-2.0 许可证，可自由使用、修改和分发。

如需商业授权、企业定制部署、培训等服务，请通过邮箱联系作者。

> V9-Full 及更高版本暂不公开，商业使用请联系授权。
