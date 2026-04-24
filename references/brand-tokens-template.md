# 品牌 Tokens 填空模板

> 什么时候读这份：discover 阶段用户提到品牌色 / 字体 / token 时。用 `AskUserQuestion` 让用户填空，然后把结果写入 `DESIGN.md`。

---

## 使用方法

1. 把下面的模板发给用户，让他们填写已知项
2. 不知道的标 `TBD`，agent 会在 plan 阶段处理
3. 填完后写入 `DESIGN.md` 的"品牌 Tokens"章节
4. 后续 build 阶段只用 `DESIGN.md` 里定义的 tokens，不编造

---

## 模板

```markdown
## 品牌 Tokens

### 颜色

| Token 名称 | HEX 值 | 用途 |
|-----------|--------|------|
| brand-primary | #______ | 主品牌色（CTA、链接、重点强调） |
| brand-secondary | #______ | 辅助品牌色（次要按钮、标签） |
| brand-accent | #______ | 点缀色（徽章、通知、高亮） |
| bg-primary | #______ | 主背景色 |
| bg-secondary | #______ | 次要背景色（卡片、区块交替） |
| text-primary | #______ | 主文字色 |
| text-secondary | #______ | 次要文字色（描述、辅助信息） |
| text-muted | #______ | 弱化文字（占位符、禁用态） |
| border-default | #______ | 默认边框色 |
| success | #______ | 成功状态 |
| warning | #______ | 警告状态 |
| error | #______ | 错误状态 |

### 字体

| 用途 | 字体族 | 备注 |
|------|--------|------|
| 标题 | __________ | |
| 正文 | __________ | |
| 代码 | __________ | 等宽字体 |

### 间距偏好

- [ ] 紧凑（适合信息密集型界面，如仪表盘）
- [ ] 标准（适合大多数网站）
- [ ] 宽松（适合品牌展示、落地页）

### 圆角

- [ ] 直角（0px）
- [ ] 小圆角（4px）
- [ ] 中圆角（8px）
- [ ] 大圆角（12-16px）
- [ ] 全圆（pill / 999px）

### 阴影风格

- [ ] 无阴影（扁平设计）
- [ ] 微妙阴影（subtle shadow）
- [ ] 明显阴影（elevated）
- [ ] 品牌色阴影（colored shadow）

### 其他

- 暗色模式：[ ] 需要  [ ] 不需要  [ ] 以后再说
- 动效偏好：[ ] 最少  [ ] 适度  [ ] 丰富
- 参考产品风格：__________（如 Linear、Stripe、Notion 等）
```

---

## 填空规则

### 用户全部填完

直接写入 `DESIGN.md`，进入 plan 阶段。

### 用户部分填写

- 已填项 → 写入 `DESIGN.md`
- 未填项 → 标为 `ASSUMPTION: [agent 的假设]`
- 在 plan 阶段口头复述假设，让用户确认或推翻

### 用户说"自由发挥"

使用中性默认值：

```markdown
| Token 名称 | HEX 值 | 用途 |
|-----------|--------|------|
| brand-primary | #18181B | 主品牌色 |
| bg-primary | #FFFFFF | 主背景色 |
| bg-secondary | #F4F4F5 | 次要背景色 |
| text-primary | #18181B | 主文字色 |
| text-secondary | #71717A | 次要文字色 |
| text-muted | #52525B | 弱化文字（zinc-600，白底对比度 7.2:1） |
| border-default | #E4E4E7 | 默认边框色 |
| accent | #2563EB | 强调色（链接、tag 标签、焦点环、代码高亮） |
```

字体：`system-ui, -apple-system, sans-serif`
间距：标准
圆角：中圆角（8px）
阴影：微妙阴影

**注意**：中性默认值是临时的，明确告知用户后续可替换。不要把默认值当最终方案。

### Accent 色使用策略（70-20-10 法则）

accent 色**不是**到处都用——滥用 = 无强调。遵循比例：

| 层级 | 占比 | 用途 |
|------|------|------|
| 中性色（bg + text） | 70-80% | 背景、正文、边框——用户不应注意到 |
| 结构色（border + secondary） | 15-20% | 分隔线、次要文字、卡片背景 |
| 强调色（accent） | 5-10% | 主 CTA、链接、tag 标签、焦点环、徽章 |

**斜眼测试**：眯着眼看你的页面，如果能瞬间找到 3 个 accent 元素 → 正确。找到 8 个 → 太多了。

accent 色推荐使用场景：
- Section 的 tag 标签（`开源 SKILL`、`工作流` 等）
- 链接和内联代码高亮
- `:focus-visible` 焦点环
- 主 CTA 按钮（如果品牌色不是深色）
- 徽章 / 通知点

accent 色禁止场景：
- 大面积背景
- 正文文字
- 多个按钮同时用（只有主 CTA 用）

---

## 对比度验证

填完 tokens 后，立即验证关键对比度组合：

| 组合 | 最低要求 |
|------|---------|
| text-primary / bg-primary | ≥ 4.5:1（WCAG AA） |
| text-secondary / bg-primary | ≥ 4.5:1 |
| text-muted / bg-primary | ≥ 3:1（大字可放宽） |
| brand-primary / bg-primary | ≥ 3:1（用于大号文字/图标） |
| 按钮文字 / brand-primary | ≥ 4.5:1 |

不达标 → 在 plan 阶段提出，和用户商量调整。
