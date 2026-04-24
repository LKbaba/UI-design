> **本文件是 [`AGENTS.md`](./AGENTS.md) 的 Claude Code 兼容副本**——Claude Code 默认读这份 `CLAUDE.md`，Codex / Cursor / Aider 等读 `AGENTS.md`。两份文件内容保持一致，修改时请同步更新。

# 贡献者指南（Claude Code 版）

这是 **agent-ui-design** 项目的贡献者约定，面向在这个仓库里工作的编程智能体。

> 使用者（把这个 Skill 安装到自己项目里的人）**不读这份文件**，他们看 `README.md`。

---

## 开发环境

- **OS**：Windows 10.0.19045
- **Shell**：Git Bash
- **路径格式**：Git Bash 下用正斜杠
- **文件系统**：大小写不敏感
- **行尾**：CRLF（`git config core.autocrlf true`）

---

## 修改约定

### 修改 SKILL.md

- **硬限制 <250 行**。新增内容先考虑能否放到 `references/` 里渐进加载。
- 修改 description 字段时，保证使用者常用说法能触发：设计、UI、界面、原型、页面、design、landing page、mockup、dashboard 等。
- 技术栈**不写死**在 SKILL.md 里。由 discover 阶段问出来，写入使用者项目的 `DESIGN.md`。

### 新增 reference

1. 放到 `references/` 目录
2. 在 `SKILL.md` 的「路由表」里新增一行
3. 在 `load-manifest.json` 注册
4. 更新 `README.md`「项目结构」章节（如结构有变）

### 新增 template

1. 放到 `templates/` 目录，文件名带 `.template` 后缀
2. 在 `load-manifest.json.templates` 下注册 `copy_to` 目标路径
3. 模板里用 `{{PLACEHOLDER}}` 标记，Skill 在 discover 阶段填空

---

## Release Rules

发新版本时**同一次改动**里必须：

1. 更新根目录 `VERSION`
2. 如果触发行为变了，同步更新 `README.md` 和 `references/workflow.md`
3. 如果路由变了，同步更新 `SKILL.md` 的路由表和 `load-manifest.json`
4. 同步更新 `AGENTS.md`（和本文件保持一致）

---

## Codex MCP 使用指南

Codex 是 OpenAI 的自主编码 agent，通过 MCP 集成。

**工作流**：Claude 规划架构 → 把范围明确的任务交给 Codex → 回审结果

- `codex` 工具：开启新会话，参数 prompt / sandbox / approval-policy
- `codex-reply` 工具：按 threadId 续接多轮任务
- 通过 `developer-instructions` 参数传项目上下文
- 推荐配置：`sandbox='workspace-write'`、`approval-policy='on-failure'`

**前置**：`npm i -g @openai/codex`，配置 `OPENAI_API_KEY`。

---

## 快速导航

| 想做什么 | 读哪里 |
|---------|--------|
| 了解本 Skill 对使用者的行为契约 | `SKILL.md` |
| 了解使用者怎么安装和触发 | `README.md` |
| 修改 discover 阶段问题清单 | `references/design-context.md` |
| 修改反 AI slop 硬禁区 | `references/anti-slop.md` |
| 查看按场景的布局范例 | `references/design-patterns.md` |
| 查看代码级好坏对比 | `references/good-vs-bad.md` |
| 修改排版/间距基线 | `references/typography-spacing.md` |
| 修改渐进加载路由 | `load-manifest.json` + `SKILL.md` 路由表 |
| 修改使用者项目的设计契约模板 | `templates/DESIGN.md.template` |
| 修改设计卓越/视觉层级指南 | `references/design-excellence.md` |
| 修改色彩系统指南 | `references/color-system.md` |
| 修改字体推荐指南 | `references/font-recommendations.md` |
| 修改设计哲学/判断力框架 | `references/design-philosophy.md` |

## Development Environment
- OS: Windows 10.0.19045
- Shell: Git Bash
- Path format: Windows (use forward slashes in Git Bash)
- File system: Case-insensitive
- Line endings: CRLF (configure Git autocrlf)
