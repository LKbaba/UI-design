# AGENTS.md · 贡献者指南

这份文件是 **agent-ui-design** 项目的 agent-neutral 约定，面向任何在这个仓库里工作的编程智能体（Claude Code、Codex、Cursor、Aider 等）。

> 使用者（把这个 Skill 安装到自己项目里的人）**不读这份文件**。他们看 `README.md`。
> 这份文件是给**贡献者**和**在这个仓库里做修改的智能体**看的。

---

## 这个仓库是什么

一个可以被任何编程智能体加载的 UI 设计 Skill。交付物：一份 `SKILL.md` + 13 份 `references/` + 2 份 `templates/` + 渐进加载映射 `load-manifest.json`。

使用者通过 `git clone` 把整个仓库放到自己的 skills 目录（见 README），之后在任何项目里说"帮我设计 UI"就会触发本 Skill。

---

## 开发环境

- **OS**：Windows 10（主要维护环境）
- **Shell**：Git Bash
- **路径格式**：Git Bash 下用正斜杠
- **文件系统**：大小写不敏感
- **行尾**：CRLF（`git config core.autocrlf true`）

---

## 修改约定

### 修改 SKILL.md

- **硬限制 <250 行**。新增内容先考虑能否放到 `references/` 里渐进加载。
- 修改 description 字段时，保证使用者常用说法能触发：设计、UI、界面、原型、页面、design、landing page、mockup、dashboard 等。
- 技术栈**不写死在 SKILL.md 里**。由 discover 阶段问出来，写入使用者项目的 `DESIGN.md`。

### 新增 reference

1. 放到 `references/` 目录下
2. 在 `SKILL.md` 的「路由表」里新增一行
3. 在 `load-manifest.json` 对应位置注册
4. 更新 `README.md`「项目结构」章节（如果结构有变）
5. 写清楚这份 reference 的「加载时机」和「加载原因关键词」

### 新增 template

1. 放到 `templates/` 目录，文件名带 `.template` 后缀
2. 在 `load-manifest.json.templates` 下注册 `copy_to` 目标路径
3. 模板里用 `{{PLACEHOLDER}}` 标记让 Skill 在 discover 阶段填空

### 修改 load-manifest.json

保证结构：`defaults` + `taskTypes` + `checkpoints` + `templates` 四个顶级键不能改名。每个条目都要有 `group` / `reason` / `references` 三个字段，便于 Skill 加载时用 `Load: because=<reason> loaded=<paths>` 宣告。

---

## Release Rules

发新版本时**同一次改动**里必须：

1. 更新根目录 `VERSION`
2. 如果触发行为（SKILL.md 的 description 或工作流）变了，同步更新 `README.md` 和 `references/workflow.md`
3. 如果路由（`load-manifest.json` 或路由表）变了，同步更新 `SKILL.md` 的路由表章节
4. 如果 release 流程本身变了，同步更新本文件

`VERSION` 不升，使用者端的升级提示看不到变化。

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
| 修改 discover 阶段的问题清单 | `references/design-context.md` |
| 修改反 AI slop 的硬禁区清单 | `references/anti-slop.md` |
| 查看按场景的布局范例 | `references/design-patterns.md` |
| 查看代码级好坏对比 | `references/good-vs-bad.md` |
| 修改排版/间距基线 | `references/typography-spacing.md` |
| 修改渐进加载的路由 | `load-manifest.json` + `SKILL.md` 路由表 |
| 修改使用者项目的设计契约模板 | `templates/DESIGN.md.template` |
| 修改设计卓越/视觉层级指南 | `references/design-excellence.md` |
| 修改色彩系统指南 | `references/color-system.md` |
| 修改字体推荐指南 | `references/font-recommendations.md` |
| 修改设计哲学/判断力框架 | `references/design-philosophy.md` |
