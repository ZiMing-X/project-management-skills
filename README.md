# xmgl — AI辅助项目治理框架

> **GitHub 仓库**：[github.com/ZiMing-X/project-management-skills](https://github.com/ZiMing-X/project-management-skills)
> **许可证**：Apache-2.0（V9-Base / V9-Standard）| 商业授权（V9-Full 及以上）
> **作者**：流風 | **邮箱**：s59401080@163.com

---

## 这是什么

一套基于 Markdown 文件的项目治理方法，让 AI 能够在每次新会话中自动理解项目状态、追踪任务进展、执行复杂任务门禁。

## 核心思想

1. **文档即架构** — 项目的一切规则、状态、边界都沉淀在 Markdown 文件中
2. **约束即自由** — 明确的项目边界和白名单让 AI 专注不越界
3. **技能即资产** — 可复用的 AI 技能文档，积累形成组织级知识库

---

## 版本选择

| 版本 | 技能数 | 适用场景 | 许可证 |
|:---|:--:|:---|:---|
| **V9-Standard** | 20 | 日常推荐（← 推荐从这里开始） | Apache-2.0 |
| V9-Base | 5 | 最小可行集，轻量入门 | Apache-2.0 |
| V8-legacy | 6 | 旧版参考，含 project-workspace | CC BY-NC 4.0 |
| V9-Full | 27 | 完整治理体系（含数据库分析、批次调度） | 商业授权，不公开 |

> V9-Full 及更高版本暂不公开，商业使用请联系作者授权。

---

## 快速开始

1. **选择版本** — 推荐从 `V9-M29-优化版/V9-Standard/` 开始
2. **阅读快速入门** — 打开对应目录下的 `docs/快速入门.md`
3. **零依赖部署** — 参考各版本根目录下的 `零依赖一键部署说明.txt`
4. **开始使用** — 部署后输入 `会话守护` 激活 session-guard`

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
├── V9-M29-优化版/
│   ├── V9-Base/          ← 5技能，最小可行集
│   └── V9-Standard/     ← 20技能，日常推荐
├── V8-legacy/            ← 旧版V8（6技能），仅供参考
├── LICENSE                ← Apache-2.0 全文
├── OWNERSHIP.md          ← 原创声明
└── README.md             ← 本文件
```

---

## 许可证说明

- **V9-Base / V9-Standard**：采用 [Apache-2.0](LICENSE) 许可证，可自由使用、修改和分发。
- **V8-legacy**：采用 CC BY-NC 4.0，署名-非商业性使用。
- **V9-Full 及更高版本**：暂不公开，商业使用请联系授权。

---

## 联系方式

- **作者**：流風
- **邮箱**：s59401080@163.com
- **GitHub**：[github.com/ZiMing-X/project-management-skills](https://github.com/ZiMing-X/project-management-skills)

如需商业授权、企业定制部署、培训等服务，请通过邮箱联系作者。
