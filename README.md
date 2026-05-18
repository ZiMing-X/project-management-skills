# xmgl — AI辅助项目治理框架

> **GitHub 仓库**：[github.com/ZiMing-X/project-management-skills](https://github.com/ZiMing-X/project-management-skills)
> **作者**：流風 | **邮箱**：s59401080@163.com
>
> **许可策略**：三级分层许可（详见 [docs/三级许可策略.md](docs/三级许可策略.md)）

---

## 这是什么

一套基于 Markdown 文件的项目治理方法，让 AI 能够在每次新会话中自动理解项目状态、追踪任务进展、执行复杂任务门禁。

## 核心思想

1. **文档即架构** — 项目的一切规则、状态、边界都沉淀在 Markdown 文件中
2. **约束即自由** — 明确的项目边界和白名单让 AI 专注不越界
3. **技能即资产** — 可复用的 AI 技能文档，积累形成组织级知识库

---

## 版本选择（三级许可模式）

| 版本 | 技能数 | 许可证 | 适用场景 |
|:---|:--:|:---|:---|
| **V9-Standard** | 20 | **AGPL-3.0** | 日常推荐（← 推荐从这里开始） |
| V9-Base | 5 | **Apache-2.0** | 最小可行集，轻量入门 |
| V8-legacy | 6 | CC BY-NC 4.0 | 旧版参考，含 project-workspace |
| V10-Full | 27 | 商业授权 | 完整治理体系（不公开，[联系作者](mailto:s59401080@163.com)授权） |

> **许可证说明（GitHub 检测器参考）**：
> 本仓库采用目录级物理隔离策略。根目录 LICENSE 适用于 `V9-Base/` 目录（Apache-2.0）。
> 其他子目录（`V9-Standard/`、`V8-legacy/`）包含各自独立的许可证文件，
> 详见各子目录下的 `LICENSE` 文件。

### 三级许可设计要点

- **L1 — V9-Base（Apache-2.0）**：宽松许可，最大化社区传播，可自由使用、修改、再分发
- **L2 — V9-Standard（AGPL-3.0）**：强 Copyleft，"网络即分发"，防止云厂商不贡献代码直接商业化
- **L3 — V10-Full（商业闭源）**：不在此公开仓库中，闭源交付给企业客户

⚠️ **兼容性警告**：AGPL-3.0 与 Apache-2.0 **不兼容**，两个版本的代码必须物理隔离，不得互相引用。

---

## 快速开始

1. **选择版本** — 推荐从 `V9-Standard/` 开始
2. **阅读快速入门** — 打开对应目录下的 `docs/快速入门.md`
3. **零依赖部署** — 参考各版本根目录下的 `零依赖一键部署说明.txt`
4. **开始使用** — 部署后输入 `会话守护` 激活 session-guard

---

## 零依赖部署（核心优势）

无需安装任何技能或运行任何脚本。

**部署命令（直接复制粘贴给 AI）：**

```
请帮我把以下目录的技能文件部署到 ~/.workbuddy/skills/
源路径：{此处替换为你的版本目录绝对路径}
```

AI 会自动读取所有技能文件并安装到正确位置。部署后重启 WorkBuddy 即可。

详见各版本目录下的 `零依赖一键部署说明.txt`。

---

## 目录结构

```
project-management-skills/
├── README.md                   ← 本文件
├── LICENSE                     ← Apache-2.0（适用于 V9-Base/）
├── OWNERSHIP.md               ← 原创声明
│
├── V9-Base/                   ← L1：5技能，Apache-2.0
│   ├── LICENSE                ← Apache-2.0 全文
│   ├── README.md
│   └── （技能文件…）
│
├── V9-Standard/               ← L2：20技能，AGPL-3.0
│   ├── LICENSE                ← AGPL-3.0 全文
│   ├── README.md
│   └── （技能文件…）
│
├── V8-legacy/                 ← 旧版参考，CC BY-NC 4.0
│   ├── LICENSE                ← CC BY-NC 4.0 全文
│   └── （技能文件…）
│
└── docs/
    └── 三级许可策略.md         ← 许可架构设计文档
```

（V10-Full 企业版不在此公开仓库中）

---

## 许可证详情

### V9-Base — Apache License 2.0

- 可自由使用、修改、再分发
- 只需保留版权声明
- 适合个人或公司集成到产品中

### V9-Standard — GNU AGPL-3.0

- **"网络即分发"**：通过网络服务（SaaS、API）使用修改版，必须开源修改部分的源代码
- 有效防止云厂商在不贡献代码的前提下直接商业化
- [查看完整许可证](V9-Standard/LICENSE)

### V8-legacy — CC BY-NC 4.0

- 署名 + 非商业性使用
- [查看完整许可证](V8-legacy/LICENSE)

### V10-Full — 商业授权

- 暂不公开，商业使用请联系 [s59401080@163.com](mailto:s59401080@163.com) 授权

---

## 联系方式

- **作者**：流風
- **邮箱**：s59401080@163.com
- **GitHub**：[github.com/ZiMing-X/project-management-skills](https://github.com/ZiMing-X/project-management-skills)

如需商业授权、企业定制部署、培训等服务，请通过邮箱联系作者。

---

*最后更新：2026-05-19（新增三级许可策略）*
