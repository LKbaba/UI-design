# 排版与间距速查

> 什么时候读这份：每次设计任务启动前（基础必载），以及 build 阶段写代码时。

---

## 字号基线

| 用途 | Web | 幻灯片 / 大屏 |
|------|-----|---------------|
| 正文 Body | ≥ 16px | ≥ 24px |
| 小字 Caption | ≥ 12px | ≥ 16px |
| 按钮文字 | ≥ 14px | ≥ 18px |
| 移动端正文 | ≥ 16px | — |

**硬规则**：正文 < 16px → 不合格，不要用。

---

## 字阶（Type Scale）

推荐使用 Major Third（1.25）比例：

```
xs:   12px   — caption / 辅助信息
sm:   14px   — 小号正文 / 标签
base: 16px   — 正文基线
lg:   20px   — 小标题
xl:   25px   — 区块标题
2xl:  31px   — 页面标题
3xl:  39px   — Hero 副标题
4xl:  49px   — Hero 主标题
5xl:  61px   — 展示文字
```

响应式：移动端字阶下调一级（如桌面 4xl → 移动端 3xl）。

推荐用 `clamp()` 做流体字号：
```css
/* 示例：Hero 主标题 */
font-size: clamp(2rem, 1.5rem + 2.5vw, 3.75rem);
```

---

## 行高

| 用途 | 行高 | 备注 |
|------|------|------|
| 正文 | 1.5 – 1.6 | 可读性最优区间 |
| 展示文字（≥ 32px） | 1.1 – 1.2 | 大字需要收紧 |
| 标题（20-31px） | 1.2 – 1.3 | 介于正文和展示之间 |
| CJK 正文 | 1.6 – 1.8 | 中日韩文字需要更大行高 |
| CJK 标题 | 1.3 – 1.5 | 同上，但不如正文夸张 |

**Bringhurst 公式**：`行高 ≈ 字号 × (1 + 2 / 字号)`（px 单位）

---

## 间距刻度（Spacing Scale）

**唯一允许的间距值**（8px 网格系统）：

```
4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 · 96 · 128
```

| 间距 | 常用场景 |
|------|---------|
| 4px | 图标与文字间距、紧凑元素内间距 |
| 8px | 相关元素组内间距、列表项间距 |
| 12px | 表单字段间距、紧凑卡片内间距 |
| 16px | 段落间距、卡片内间距、按钮内间距 |
| 24px | 区块内元素组间距、栅格 gutter |
| 32px | Section 内部分隔 |
| 48px | Section 间距（移动端） |
| 64px | Section 间距（桌面端，紧凑） |
| 96px | Section 间距（桌面端，标准） |
| 128px | Section 间距（桌面端，宽松） |

**硬规则**：
- 不允许使用刻度之外的值（如 5px、10px、15px、20px、36px）
- 垂直间距必须是 8 的倍数（4 和 12 作为微调例外）
- 水平间距可以用 4 的倍数

---

## 字重

**最多使用 3 个字重**：

| 字重 | 用途 |
|------|------|
| 400 (Regular) | 正文 |
| 600 (Semibold) | 强调、小标题、按钮 |
| 700 (Bold) | 大标题 |

**禁止**：
- 一个页面用 4 种以上字重
- 用 100/200（太细，可读性差）
- 用 800/900（太重，除非品牌字体要求）

---

## 容器与栅格

```css
/* 推荐容器宽度 */
--container-max: 1280px;   /* 标准 */
--container-narrow: 768px;  /* 文章/博客 */
--container-wide: 1440px;   /* 仪表盘/全宽 */

/* 栅格 */
--grid-columns: 12;
--grid-gutter: 24px;        /* 桌面 */
--grid-gutter-mobile: 16px; /* 移动 */
--page-margin: 24px;        /* 移动端页面边距 */
```

---

## CSS Variables 模板

可直接复制到项目中使用：

```css
:root {
  /* 字阶 */
  --text-xs: 0.75rem;    /* 12px */
  --text-sm: 0.875rem;   /* 14px */
  --text-base: 1rem;     /* 16px */
  --text-lg: 1.25rem;    /* 20px */
  --text-xl: 1.5625rem;  /* 25px */
  --text-2xl: 1.9375rem; /* 31px */
  --text-3xl: 2.4375rem; /* 39px */
  --text-4xl: 3.0625rem; /* 49px */

  /* 行高 */
  --leading-tight: 1.15;
  --leading-snug: 1.3;
  --leading-normal: 1.5;
  --leading-relaxed: 1.6;
  --leading-cjk: 1.75;

  /* 间距 */
  --space-1: 0.25rem;  /* 4px */
  --space-2: 0.5rem;   /* 8px */
  --space-3: 0.75rem;  /* 12px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  --space-8: 2rem;     /* 32px */
  --space-12: 3rem;    /* 48px */
  --space-16: 4rem;    /* 64px */
  --space-24: 6rem;    /* 96px */
  --space-32: 8rem;    /* 128px */

  /* 字距（Tracking） */
  --tracking-tighter: -0.05em;  /* 展示文字 48px+ */
  --tracking-tight: -0.025em;   /* 标题 24-48px */
  --tracking-normal: 0;         /* 正文 */
  --tracking-wide: 0.025em;     /* 小字 */
  --tracking-wider: 0.05em;     /* 全大写标签 */

  /* 字重 */
  --font-normal: 400;
  --font-semibold: 600;
  --font-bold: 700;

  /* 容器 */
  --container-max: 80rem;    /* 1280px */
  --container-narrow: 48rem; /* 768px */
}
```

---

## 行宽（Measure）

| 用途 | 理想行宽 | 上限 |
|------|---------|------|
| 正文 | 45–75 字符 | 80 字符 |
| 标题 | 15–40 字符 | — |
| CJK 正文 | 25–35 字符 | 40 字符 |

实现：
```css
/* 正文容器 */
max-width: 65ch; /* 约 45-75 字符 */
```

---

## 响应式断点

```css
/* 推荐断点 */
--bp-sm: 640px;   /* 大手机 */
--bp-md: 768px;   /* 平板 */
--bp-lg: 1024px;  /* 小桌面 */
--bp-xl: 1280px;  /* 标准桌面 */
--bp-2xl: 1536px; /* 大桌面 */
```

必须检查的四个宽度：**375px**（手机）、**768px**（平板）、**1024px**（小桌面）、**1440px**（桌面）。

### 断点行为指南

| 断点 | 典型变化 | 常见陷阱 |
|------|---------|---------|
| ≤ 640px | 单列、汉堡菜单、全宽卡片 | 固定宽度元素溢出 |
| 768px | 双列网格、侧栏浮现 | 卡片过窄文字挤压 |
| 1024px | 多列布局稳定、导航展开 | 从 2 列到 3 列的尴尬中间态 |
| ≥ 1280px | 容器居中、侧边留白 | 内容拉伸没有 max-width |

**1024px 过渡要点**（最容易被忽略的断点）：
- 3 列网格在此处可能需要退回 2 列
- 侧栏 + 内容区总宽度需要验证
- 导航菜单的桌面/移动切换点通常在这里

### 文字换行

```css
/* 标题——平衡两端，避免孤行 */
h1, h2, h3 { text-wrap: balance; }

/* 正文——优化最后一行，避免单独一个字 */
p { text-wrap: pretty; }
```

`text-wrap: balance` 让标题的每行长度趋于一致，消灭"长+短"的尴尬换行。
`text-wrap: pretty` 避免段落最后一行只剩一两个字。

### CJK 排版补充

- **不要**对中文/日文标题使用负字距（`tracking-tight/tighter`）——CJK 字符间距已经是光学均匀的
- CJK 正文行高 ≥ 1.6（已列在行高表中）
- CJK 和拉丁混排时，浏览器会自动加小间距，不需要手动干预
- 中文标点使用 `font-feature-settings: "halt"` 可以做半宽标点（酌情使用）

---

## 8px 网格 Debug 工具

开发时可临时加上，交付前必须删除：

```css
/* 8px 网格辅助线——仅开发用，交付前删除 */
body {
  background-image:
    linear-gradient(rgba(255, 0, 0, 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 0, 0, 0.05) 1px, transparent 1px);
  background-size: 8px 8px;
}
```

---

## 交付前核查

- [ ] 正文 ≥ 16px
- [ ] 移动端触控目标 ≥ 44px
- [ ] 只用了 2-3 个字重
- [ ] 间距全部来自 4·8·12·16·24·32·48·64·96·128 刻度
- [ ] 行高在推荐区间内
- [ ] 容器有 max-width 限制
- [ ] CJK 文本（如有）行高 ≥ 1.6
- [ ] 无 debug 网格残留
