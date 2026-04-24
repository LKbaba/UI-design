# agent-ui-design

> 让任何编程智能体（Claude Code / Codex / Cursor / Aider…）都能帮你设计和实现 UI 界面的通用 Skill。

**一句话**：clone 到 agent 的 skills 目录 → 在任何项目里说"帮我做一个落地页"就能触发 → 四阶段工作流（需求澄清 → 可见计划 → 构建 → 自检）自动跑起来。

---

## 它解决什么问题

AI 做 UI 的老毛病：

- 上来就写代码，需求还没搞清楚
- 套路化产物：紫→粉→蓝 mesh 渐变 + Emoji 图标 + 三栏 features + 评价轮播 + CTA
- 技术栈乱跳（上一轮 Next.js，下一轮又换 Vue）
- 没有交接契约，多轮对话后设计系统漂移

**agent-ui-design** 用四个机制解决：

| 问题 | 机制 |
|-----|------|
| 上来就写代码 | **discover 阶段** 先把需求问清楚，产物写进 `DESIGN.md` |
| 套路化产物 | **anti-slop 硬禁区清单** + 每次构建前强制加载 |
| 技术栈漂移 | 由用户在 discover 阶段指定，写进 `DESIGN.md`，此后所有构建都遵守 |
| 设计系统漂移 | `DESIGN.md` 作为 **single source of truth**，阶段间交接 |

---

## 安装

### Claude Code

```bash
# 全局安装（任何项目都能触发）
git clone https://github.com/<你的用户名>/UI-design.git ~/.claude/skills/agent-ui-design

# 或者项目级安装
git clone https://github.com/<你的用户名>/UI-design.git <你的项目>/.claude/skills/agent-ui-design
```

### Codex

```bash
git clone https://github.com/<你的用户名>/UI-design.git ~/.codex/skills/agent-ui-design
```

### 其他 agent（Cursor / Aider / …）

复制 `SKILL.md` 的 description 和内容到对应 agent 的 Skill / Rule 配置位置。`references/` 和 `templates/` 目录保持相对路径即可。

---

## 使用方法

安装完后，在任何项目里对 agent 说：

```
帮我做一个 SaaS 落地页
设计一个仪表盘
这个 hero 区改得高级一点
做一个 login 页面
给我 3 个方向探索一下首页
```

Skill 会自动触发，走四阶段工作流：

```
discover  →  plan  →  build  →  verify
   ↓          ↓         ↓         ↓
 问需求     出计划     写代码     自检
 填 DESIGN  等批准    半程汇报   三阶段
   .md
```

### 第一次启动一个新 UI 项目

你可以直接让 agent 套用模板：

```
把 templates/DESIGN.md.template 复制到项目根，我们走 discover 阶段
```

或者更简单，直接说"帮我启动一个新 UI 项目"，agent 会自己建立 `DESIGN.md`。

---

## 四阶段工作流详解

### 1. Discover（需求澄清）

agent 会用 `AskUserQuestion` 逐条问：受众？输出形状？技术栈？硬约束？参考源？验收标准？

答完填入 `DESIGN.md`。信息密集的 brief（你一次把上下文全给了）可以跳过这步。

### 2. Plan（可见计划）

agent 写出执行计划，停下等你批准：

```markdown
## Plan
- Goal: ...
- Confirmed facts: ...
- Assumptions: ...
- First artifact: ...
- Variation axes: ...
- Verification: ...

请确认此方案，或告诉我要改哪里。
```

**沉默不等于同意**。你不说话 agent 就不往下走。

### 3. Build（构建）

计划批了再写代码。半程会主动让你看一次，不会闷头做到死。多变体用 **Tweaks 系统** 暴露（同一文件切换），不会生成一堆 `-v1.tsx` `-v2.tsx`。

### 4. Verify（自检）

交付前自动三阶段自检：
- **结构层** — 类型检查、控制台、响应式溢出
- **视觉层** — 桌面 + 窄屏各看一眼，对比度 ≥ 4.5:1
- **设计卓越** — 层级、间距、颜色、情绪

通过 Playwright MCP 自动截图验证。

---

## 项目结构

```
UI-design/                           # 仓库根 = Skill 本体
├── SKILL.md                         # 核心 skill 定义（<250 行）
├── load-manifest.json               # 渐进加载路由映射
├── VERSION                          # 版本号
├── README.md                        # 使用者指南（本文件）
├── AGENTS.md                        # 贡献者指南（agent-neutral）
├── CLAUDE.md                        # 同上，Claude Code 兼容副本
├── references/                      # 按需加载的技术/设计约束
│   ├── workflow.md                  # 四阶段详解
│   ├── design-context.md            # discover 阶段问题清单
│   ├── anti-slop.md                 # 反 AI 套路硬禁区
│   ├── typography-spacing.md        # 排版间距基线
│   ├── brand-tokens-template.md     # 品牌 tokens 填空模板
│   ├── verification-protocol.md     # 三阶段自检
│   └── tweaks-system.md             # 变体探索
├── templates/                       # 可复制到使用者项目的模板
│   ├── DESIGN.md.template           # 需求→实现交接契约
│   └── AGENTS.md.template           # 使用者项目的 agent 入口
└── reference/                       # 灵感来源（cc-design 等，只读）
```

---

## 与其他 Skill 的关系

| Skill | 定位 | 与 agent-ui-design 关系 |
|-------|------|------------------------|
| **cc-design** | 大而全的 HTML 设计 Skill（50+ references） | agent-ui-design 借鉴了其渐进加载架构，做了精简 |
| **jiehe-web-design** | 为某个具体项目定制的 Skill | agent-ui-design 是通用版；如果你想为自己的项目做定制版，可以 fork 本仓库并硬编码品牌 tokens |
| **spec-flow** | 规划 / PRD / 任务拆分 | 协作：spec-flow 生成 PRD → agent-ui-design 的 discover 阶段读 PRD 填 `DESIGN.md` |

---

## 贡献

见 [`AGENTS.md`](./AGENTS.md) 或 [`CLAUDE.md`](./CLAUDE.md)。

---

## License

MIT
