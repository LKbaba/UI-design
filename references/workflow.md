# 四阶段工作流详解

> 什么时候读这份：每次新任务启动前。它是 SKILL.md 里四阶段工作流的详细说明和执行清单。

---

## 总览

```
┌──────────────┐   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│  discover    │──▶│  plan        │──▶│  build       │──▶│  verify      │
│  需求澄清     │   │  可见计划    │   │  构建        │   │  自检        │
│              │   │              │   │              │   │              │
│ 填 DESIGN.md │   │ 停下等批准   │   │ 半程汇报     │   │ 三阶段自检   │
└──────────────┘   └──────────────┘   └──────────────┘   └──────────────┘
```

**single source of truth**：`DESIGN.md` 在项目根目录，discover 阶段建立，后续所有阶段都以它为准。

---

## 阶段 1 · discover（需求澄清）

### 触发条件

- 新任务
- 或现有任务但上下文明显不足
- 或用户明确说"我们先理清需求"

### 跳过条件

- 信息密集的 brief（用户一次把受众/输出/栈/约束/参考/验收全说清了）→ 直接进 plan
- 局部微调（改文案、调单色）→ 直接改，不走 discover
- 已批准任务的后续迭代 → 引用上次的 `DESIGN.md`，不重问

### 动作

1. **宣告加载**：
   ```text
   Load: because=before-discover loaded=references/design-context.md
   ```

2. **检查 `DESIGN.md` 是否存在**：
   - 存在 → 问用户 append / merge / overwrite，不要静默覆盖
   - 不存在 → 从 `templates/DESIGN.md.template` 复制一份，准备填空

3. **按 `references/design-context.md` 的问题清单逐条问**：
   - 用 `AskUserQuestion`（或平台等效工具），每次 1 个问题、2-4 个选项 + freeform
   - 优先问"会改变路由"的问题：输出类型、技术栈、任务状态、可用上下文、主要设计风险
   - 再问"会改变实现"的问题：品牌 tokens、信息架构、动效预算、响应式范围

4. **把答案落到 `DESIGN.md`**：
   - 每回答一个问题就更新对应章节
   - 未解决的字段标成 `ASSUMPTION: ...`，不藏在脑子里

5. **口头复述对齐**：
   ```markdown
   好，我确认一下我读到的需求：
   - 受众: ...
   - 技术栈: ...
   - 范围: ...
   - 硬约束: ...
   - 验收: ...
   如果有偏差，告诉我；没问题我进入 plan 阶段。
   ```

### 完成标志

`DESIGN.md` 的以下字段都有值（或显式 `ASSUMPTION`）：受众 / 输出形状 / 技术栈 / 硬约束 / 参考源 / 验收标准 / 品牌 tokens / 信息架构（如需要）。

---

## 阶段 2 · plan（可见计划）

### 触发条件

- discover 完成
- 或 brief 本身已足够详细

### 动作

1. **生成 plan**（在 agent 的回复里直接输出，不写到文件）：
   ```markdown
   ## Plan
   - **Goal**: 要建什么、给谁用（一句话）
   - **Confirmed facts**: 从 DESIGN.md / 项目代码 / 参考源里确认的事实
   - **Assumptions**: 未解决项的显式假设
   - **Tech stack**: 技术栈版本号（如 Next.js 15.1 + Tailwind v4.0 + shadcn/ui）
   - **First artifact**: 第一个要交付的文件/组件（路径 + 名字）
   - **Variation axes**: 几条探索维度（若有，用 Tweaks 暴露）
   - **Verification**: 验收方式（截图 / typecheck / 对比度 / 响应式断点）

   请确认此方案，或告诉我要改哪里。
   ```

2. **停下来等批准**。不要：
   - 把沉默当同意
   - 继续往下写代码
   - 擅自扩大范围

3. **跳过批准的三种情况**：
   - 用户明确说"不用计划直接做"
   - 已批准任务的后续迭代
   - 局部微调

### 完成标志

用户明确说"同意"、"开始"、"go"、"OK" 或其他肯定回应。

---

## 阶段 3 · build（构建）

### 触发条件

plan 已批准。

### 动作

1. **宣告加载**：
   ```text
   Load: because=before-build loaded=references/typography-spacing.md,references/anti-slop.md
   ```
   有变体探索：追加 `references/tweaks-system.md`。
   有动效：追加 `references/verification-protocol.md`（读动效硬规则章节）。

2. **先说布局再写代码**：
   ```markdown
   布局策略：
   - 容器: max-w-[1280px] mx-auto
   - 栅格: 12 列 + 24px gutter
   - Section 节奏: py-32（桌面）/ py-16（移动）
   - 首屏密度: 60% 留白
   确认后我写第一版。
   ```

3. **写代码**。规则：
   - 颜色只用 `DESIGN.md` 里的 tokens，不编造
   - 间距只用 4/8/12/16/24/32/48/64/96/128 刻度
   - 字重只用 2-3 个
   - 图标库统一（Lucide / Heroicons / Radix Icons 选一套）
   - 无真实素材时用灰色占位块，不编造

4. **半程汇报**（每完成 30-50% 就停一次）：
   ```markdown
   第一版 Hero 完成，截图在这里 [...]。
   要继续下一个 section 吗？还是先调这里？
   ```

5. **变体用 Tweaks 暴露**。多文件爆炸是反模式。见 `references/tweaks-system.md`。

### 完成标志

所有 plan 里列的 artifact 都写完、半程汇报都过了、用户同意进入 verify。

---

## 阶段 4 · verify（自检）

### 触发条件

build 自认完成。

### 动作

1. **宣告加载**：
   ```text
   Load: because=before-delivery loaded=references/verification-protocol.md
   ```

2. **三阶段自检**（详见 `verification-protocol.md`）：

   **结构层**
   - [ ] 类型检查通过（`pnpm typecheck` / `tsc --noEmit`）
   - [ ] 浏览器控制台无报错
   - [ ] 响应式无溢出

   **视觉层**
   - [ ] 桌面（1440）截图正常
   - [ ] 窄屏（375）截图正常
   - [ ] 正文对背景对比度 ≥ 4.5:1（WCAG AA）
   - [ ] 所有改动过的 section 都亲自看了一眼（不只第一屏）

   **设计卓越**
   - [ ] 视觉层级清晰
   - [ ] 间距有节奏
   - [ ] 颜色和谐
   - [ ] 情绪贴合 brief

3. **Playwright 使用判断**：
   - ✅ 需要：Hero 动效、新页面首次完成、深度评审、复杂交互
   - ❌ 不需要：纯文案改、单色微调、已验证页面的小迭代

4. **失败处理**：回阶段 3 重建，**不要**降低验收标准。

### 完成标志

三阶段全通过。

---

## 交付（简短收尾）

```markdown
✅ [完成的东西]，关键能力：[Tweaks / 响应式 / 动效 / …]
DESIGN.md: [已更新到 vN]
提示: [需要用户后续提供的素材，或未解决问题]
Next: [一个明确的下一步]
```

**不要**：
- 列每个文件
- 解释技术栈
- 自吹"按最佳实践"
- 道歉式结尾

---

## 常见偏离与纠正

| 偏离 | 表现 | 纠正 |
|------|------|------|
| 跳过 discover | 接到需求直接写代码 | 至少问清楚受众 + 技术栈 + 第一个交付物 |
| plan 太虚 | Goal 写"做一个好看的页面" | 必须具体到"给 B 端 SaaS 决策者的落地页，Hero + AI Gap + Services 三个 section" |
| build 闷头做 | 一次性交付 800 行代码 | 强制半程汇报，30-50% 就停一次 |
| verify 只看第一屏 | 只截图了 Hero | 所有改动过的 section 都要看 |
| 变体生 N 个文件 | `Hero-v1.tsx`、`Hero-v2.tsx`、`Hero-v3.tsx` | 用 Tweaks 在同一文件内切换 |
